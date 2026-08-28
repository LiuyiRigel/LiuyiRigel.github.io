---
title: '[研究笔记] Paderborn 主线（一）：7 臂方法对比与表示选择规律'
date: 2026-08-28
permalink: /posts/2026/08/bearing-06-paderborn-7arm/
tags:
  - ResearchNote
  - Bearing-Fault-Diagnosis
  - Paderborn
  - Domain-Generalization
  - Representation-Learning
  - Mamba
mathjax: true
image: /images/bearing_research/fig18_pad_scoreboard_web.jpg
---


<div align="center">
    <img src="/images/bearing_research/fig18_pad_scoreboard_web.jpg" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

本系列第 6 篇，正式进入主线。我们在 Paderborn（32 设备）上做的第一轮系统实验是**7 臂方法对比**：5 种表示 × 2 种骨干，在"留一轴承 × 跨转速"协议下，回答一个前置问题——**到底哪种输入表示最能跨设备迁移？**

---

## 1. 实验设置

### 1.1 协议：留一轴承 × 跨转速二分类

- **数据**：Paderborn 32 设备，健康 vs 损伤（二分类）；
- **协议**：每次留出一个轴承作测试，其余 31 台训练；
- **偏移**：训练在 1500 rpm，测试在 900 rpm（换设备 + 换转速双重偏移）；
- **评估**：32 folds 池化，窗口级精度 + 轴承级 recall/FPR + AUC。

### 1.2 七个臂

| 臂 | 表示 | 骨干 |
|---|---|---|
| raw | 原始波形（SSST 时频图） | BiMamba |
| env | 包络（Hilbert） | BiMamba |
| ord | 阶比重采样 | BiMamba |
| raw+env | 原始 + 包络 堆叠 | BiMamba |
| raw+ord | 原始 + 阶比 堆叠 | BiMamba |
| raw_cnn | 原始波形 | CNN |
| env_cnn | 包络 | CNN |

**注意**：这里的"raw"指 SSST 时频图（后续统一 Benchmark 里 raw1d 才是纯原始波形）。7 臂阶段的核心问题是**表示（含预处理）**对跨设备迁移的影响。

---

## 2. 窗口级精度：表面上看不出问题

| 臂 | 精度 mean±std | n |
|---|---:|---:|
| raw | 82.1±27.5 | 64 |
| env | 81.1±38.9 | 64 |
| raw+env | 81.5±26.3 | 64 |
| ord | 80.8±38.0 | 64 |
| raw+ord | 82.6±35.7 | 64 |
| raw_cnn | 76.4±29.0 | 32 |
| env_cnn | 81.2±39.0 | 32 |

**如果只看 Accuracy，所有臂都在 76–83%，似乎"表示不重要"。但这是假象**——把精度按"健康折 vs 损伤折"拆开，真相立刻暴露。

---

## 3. 拆分健康折/损伤折：两种失败模式

| 臂 | 健康折 | 损伤折 |
|---|---:|---:|
| raw | 69.0 | 85.1 |
| env | **0.0** | 99.8 |
| raw+env | 46.5 | 89.5 |
| ord | **1.9** | 99.0 |
| raw+ord | 9.8 | 99.4 |
| raw_cnn | 38.3 | 85.2 |
| env_cnn | **0.0** | 100.0 |

**两个极端失败模式**：

1. **env/ord 把健康全报成损伤**：健康折精度 0.0–1.9%，损伤折 99–100%——模型学到的是"有没有冲击能量"，对健康设备（跨设备后有能量波动）全部误报；
2. **raw 相对平衡**：健康折 69.0%、损伤折 85.1%，虽不完美但没有系统性崩坏。

---

## 4. 轴承级 FPR：env/ord 的"全报警"性质

| 臂 | 阈值 | Recall% | FPR% |
|---|---|---|---|
| raw | 0.3 | 92.3 | 33.3 |
| env | 0.3 | 100.0 | **100.0** |
| ord | 0.3 | 100.0 | **100.0** |
| raw+ord | 0.3 | 100.0 | **100.0** |

env/ord 在任何阈值下 FPR 都是 100%——**12 个健康轴承全部误报为损伤**。这不是调阈值能救的，是表示本身丢失了区分"健康 vs 损伤"的跨设备信息。

**物理机制**：包络/阶比解调把信号"压扁"成能量包络，丢掉了**载波/共振频带**信息。而跨设备时，恰恰是载波/共振（受设备个体影响）与冲击结构（故障信息）的组合才可分健康与损伤。解调表示只剩"有没有冲击"——健康设备在陌生工况下也有冲击性噪声，于是全报损伤。

---

## 5. AUC：raw 唯一显著领先

| 臂 | AUC |
|---|---:|
| raw | **0.864** |
| env | 0.797 |
| raw+env | 0.837 |
| ord | 0.624 |
| raw+ord | 0.700 |
| raw_cnn | 0.680 |
| env_cnn | 0.756 |

AUC 不受阈值影响，是比 Accuracy 更诚实的排序：

> **raw（SSST）0.864 > raw+env 0.837 > env 0.797 > env_cnn 0.756 > raw+ord 0.700 > raw_cnn 0.680 > ord 0.624**

- SSST 时频图是唯一 AUC > 0.85 的表示；
- 堆叠（raw+env / raw+ord）**拖后腿**——噪声通道稀释了判别信息；
- Mamba 骨干全面优于 CNN（raw 0.864 vs raw_cnn 0.680）。

---

## 6. 分损伤类型 recall（raw 臂）

| 损伤类型 | recall |
|---|---|
| EDM trench（刻槽） | 4/4 |
| drilling（钻削） | 4/6 |
| electric engraving（电蚀） | 12/14 |
| fatigue pitting（疲劳点蚀） | 22/22 |
| plastic indentation（塑性压痕） | 6/6 |

raw 表示对多数损伤类型 recall 高；弱项是 drilling（4/6），与损伤形态的稳定性有关。

---

## 7. 结论：论文级表示选择规律

7 臂实验给出了可写进论文的**三条规律**：

1. **载波/共振是跨设备可迁移信息**：保留原始时频结构的 SSST 表示（raw）是唯一可迁移且平衡的；env/ord 解调表示跨设备把健康误报为损伤（FPR 100%）。
2. **堆叠表示拖后腿**：raw+env / raw+ord 不如 raw 单独用——把"噪声通道"堆进输入会稀释判别信息。
3. **Mamba > CNN**：同表示下 Mamba 骨干跨设备 AUC 全面占优（0.864 vs 0.680）。

**对后续统一 Benchmark 的直接影响**：
- 表示必须包含**原始时频结构**（raw/SSST），env/ord 作为对照臂保留（用于展示"精度假象"）；
- 放弃堆叠表示作为主配置；
- Mamba 谱系作为主模型家族。

<div align="center">
    <img src="/images/bearing_research/fig20_pad_anatomy_web.jpg" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

*Paderborn 表示解剖：不同表示在不同折叠下的行为差异。*

---

## 8. 诚实性备注

- 7 臂阶段是**32 折池化、多 seed 独立计数**，但基线（CNN）的 fold 数是主模型的一半（32 vs 64），后续统一 Benchmark 已统一到 32 设备 × 3 seeds 全量；
- env/ord 的"FPR 100%"结论在 Batch1 全量中再次确认（见第 7 篇），两条证据线互相印证；
- 7 臂的 raw 是 SSST 图；纯原始波形的 raw1d 表现完全不同（单转速下漏报而非误报）——这本身就是最重要的发现之一。

下一期：统一 Benchmark 设计 + Batch1 全量结果——跨转速盲点、多转速修复与精度假象。
