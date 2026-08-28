---
title: '[研究笔记] Paderborn 主线（二）：统一 Benchmark 设计与 Batch1 全量结果'
date: 2026-08-28
permalink: /posts/2026/08/bearing-07-paderborn-benchmark/
tags:
  - ResearchNote
  - Bearing-Fault-Diagnosis
  - Paderborn
  - Benchmark
  - Cross-Speed
  - Domain-Generalization
mathjax: true
image: /images/bearing_research/fig23_exp_comparison_web.jpg
---


<div align="center">
    <img src="/images/bearing_research/fig23_exp_comparison_web.jpg" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

本系列第 7 篇，论文主实验。7 臂对比（第 6 篇）回答了"表示怎么选"，但还缺一个**受控、全量、统计严谨**的基准来支撑论文。这一篇讲统一 Benchmark 的设计与 Batch1 全量结果——包括三个核心发现：**跨转速盲点、多转速修复、精度假象**。

---

## 1. 为什么需要"统一 Benchmark"？

7 臂阶段的局限：
- 基线（CNN）只有主模型一半的 fold 数，主表不对称；
- 没有 IID 空白对照（不知道"完全无偏移"的上限）；
- 没有跨负载、跨设备（不跨转速）的协议；
- 统计检验不完整（只有部分置换检验）。

统一 Benchmark 的目标：**把 Paderborn 做成论文主实验台**，覆盖 8 种表示 × 7 种模型 × 3 种子 × 4 类协议，输出可直接进论文的表格和统计。

---

## 2. 设计：三因素 × 四协议

### 2.1 三因素（延续第 1 篇框架）

| 因素 | 变量 |
|---|---|
| R1 呈现形式 | raw1d / stft / ssst / env / ord / cwt / ssst_env / ssst_ord |
| R2 训练覆盖 | 单转速 vs 多转速训练 |
| R3 模型结构 | WDCNN / CNN2D / ResNet / TCN / LSTM / Transformer / VanillaMamba / BiMamba1D / BiMamba |

### 2.2 四个协议（偏移类型阶梯）

| 协议 | 训练 | 测试 | 偏移类型 |
|---|---|---|---|
| IID（Batch2） | 全设备同转速 80/20 | 同上 | 无（空白对照） |
| LOB_same_speed（Batch2） | 31 设备 1500rpm | 留出设备 1500rpm | 换设备 |
| **LOB_cross_speed（Batch1）** | 31 设备 1500rpm | 留出设备 900rpm | **换设备+换转速** |
| **LOB_multispeed（Batch1）** | 31 设备 900+1500rpm | 留出设备 900rpm | 换设备（含目标转速训练） |
| LOB_cross_load（Batch2） | 31 设备 0.7Nm | 留出设备 0.1Nm | 换设备+换负载 |

标签：三分类（健康/人工/真实损伤）+ 二分类（健康/损伤）。每配置 n = 96（32 设备 × 3 seeds），全部逐折持久化、断点续跑、可复现。

### 2.3 评价口径（对齐 MSSP 级标准）

- 主指标：**准确率 ± 标准差**（32 轴承 × 3 种子 = 96 折观测）；
- 补充：宏 F1、逐类 F1、one-vs-rest AUC、混淆矩阵、FPR；
- 显著性：**配对置换检验**（2 万次置换）；
- 指标对照与数据处理细节见报告 35（MSSP 论文指标逐项对齐）。

---

## 3. Batch1 全量结果

### 3.1 三分类（健康/人工/真实）：很难

| 表示 | 模型 | 单转速 Acc% | 多转速 Acc% |
|---|---:|---:|---:|
| raw1d | BiMamba1D | 38.6 ± 47.6 | **71.9 ± 38.0** |
| raw1d | VanillaMamba | 38.3 ± 47.2 | 71.5 ± 37.9 |
| raw1d | WDCNN | 47.6 ± 49.8 | 67.8 ± 42.1 |
| stft | BiMamba | 43.2 ± 31.7 | 49.3 ± 32.6 |
| ssst | BiMamba | 47.6 ± 33.5 | 46.9 ± 30.7 |
| env | BiMamba | 42.6 ± 46.4 | 45.4 ± 37.1 |
| ord | BiMamba | 43.7 ± 49.1 | 43.9 ± 40.5 |

**三分类结论**：跨设备+跨转速下三分类非常难；多转速训练只对 raw1d 类模型有大提升；人工 vs 真实损伤的分类跨设备不稳定——不是好的论文主任务。

### 3.2 二分类（健康/损伤）：核心战场

**单转速训练（LOB_cross_speed）：**

| 表示 | 模型 | Acc% | AUC | FPR% |
|---|---:|---:|---:|---:|
| env | BiMamba | 79.9 | 0.489 | 99.9 |
| ord | BiMamba | 81.3 | 0.621 | 100.0 |
| ssst | BiMamba | 77.9 | 0.793 | 56.0 |
| stft | BiMamba | 70.6 | 0.768 | 37.8 |
| raw1d | BiMamba1D | 44.8 | 0.747 | 0.0 |
| raw1d | VanillaMamba | 44.5 | 0.749 | 0.0 |
| raw1d | WDCNN | 54.7 | 0.770 | 0.0 |

**多转速训练（LOB_multispeed）：**

| 表示 | 模型 | Acc% | AUC | FPR% |
|---|---:|---:|---:|---:|
| raw1d | BiMamba1D | **91.6** | **0.905** | 21.6 |
| raw1d | VanillaMamba | **91.6** | **0.908** | 20.5 |
| raw1d | WDCNN | 87.0 | 0.860 | 40.7 |
| ssst | ResNet | 83.7 | 0.890 | 29.9 |
| stft | ResNet | 83.3 | 0.872 | 38.2 |
| ssst | BiMamba | 80.2 | 0.807 | 60.8 |
| env | BiMamba | 78.9 | 0.601 | 89.4 |
| ord | BiMamba | 80.8 | 0.638 | 93.8 |

---

## 4. 三个核心结论

### 结论 1：跨转速盲点真实存在且非常严重

单转速训练下，raw1d 类模型在陌生转速上**把损伤大量漏报为健康**：

> BiMamba1D：Acc 44.8%、AUC 0.747、**FPR = 0%**

FPR = 0% 意味着**一个损伤都不报警**——"什么都不报"是最隐蔽的失败模式（健康看起来一切正常，实则已经失效）。这不是模型能力问题，而是**训练数据缺少目标转速覆盖**（物理根源见第 2 篇：绝对频率随转速变化）。

### 结论 2：多转速训练是最高性价比的修复手段

> raw1d + BiMamba1D：44.8% → **91.6%**（**+46.8pp**），AUC 0.747 → 0.905，配对置换 **p < 0.001**

效果跨模型一致：WDCNN、TCN、Transformer、VanillaMamba 等 raw1d 模型都因多转速训练大幅提升。**"补目标转速样本"带来的提升远大于换任何模型。**

### 结论 3：env/ord 是"精度假象"

- 单转速下 env/ord Acc 高达 80–81%，但 **AUC 仅 0.49–0.62、FPR ≈ 100%**；
- 多转速后 FPR 仍高达 89–94%，AUC 仅 0.60–0.64；
- 它们主要通过**把健康全报成损伤**来获得高 Accuracy。

**论文必须强调：只看 Accuracy 会被严重误导，必须同时看 AUC/FPR。**

### 结论 4（补充）：时频表示单转速下更稳健

单转速时 ssst/stft 的 AUC 0.77–0.79、FPR 38–56%，比 raw1d 的"漏报"和 env/ord 的"误报"都平衡；但多转速训练后 raw1d 上限更高。

---

## 5. 统计检验与实验完整性

- 配对置换检验（20,000 次置换）：
  - 三分类 raw1d 多转速 vs 单转速：**+33.3pp，p < 0.001**；
  - 二分类 raw1d：**+46.8pp，p < 0.001**；
- 全量配对数据已持久化（`summary/multispeed_diff.csv`、`perm_multispeed.csv`）；
- 混淆矩阵、逐类 F1、one-vs-rest AUC、逐轴承表已生成；
- 待补：Friedman + critical difference diagram、复杂度/延迟表（Batch2 阶段）。

---

## 6. 对论文的建议（来自全量证据）

1. **主任务用二分类（健康 vs 损伤）**——更贴近工程、更干净；
2. 三分类作为难度分析/补充实验；
3. 核心故事线：
   - 跨转速盲点（raw1d 漏报，FPR=0%）；
   - 多转速修复（+46.8pp，p<0.001）；
   - env/ord 精度假象（高 Acc 低 AUC 高 FPR）；
   - 时频表示抗干扰、raw1d 吃覆盖；
   - 架构影响 FPR 上限（LSTM FPR≈100% 不可用）；
4. 最佳配置：**raw1d + 多转速 + BiMamba1D/VanillaMamba**（91.6% / AUC 0.905）；ssst/stft + ResNet 作"低漏报"备选。

<div align="center">
    <img src="/images/bearing_research/fig24_film_web.jpg" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

*实验对照与覆盖分析：Batch1 各配置的完整性检查。*

---

## 7. 工程意义

| 发现 | 可操作结论 |
|---|---|
| 跨转速盲点（raw1d FPR=0%） | 部署前必须验证目标转速，否则模型"静默失效" |
| 多转速修复 +46.8pp | **采集目标工况样本 > 换模型**，成本最低的改进 |
| env/ord 精度假象 | 选型必须看 AUC/FPR，不能只看 Accuracy |
| ssst/stft 单转速稳健 | 数据覆盖不足时，时频表示是更安全的默认选择 |
| 轻量 Mamba 达标 | BiMamba1D/VanillaMamba 在最佳配置下优于重型模型 |

---

## 8. 诚实性备注

- Batch1 目前是**本地 RTX 4060 全量**（1392 fits，32 设备 × 3 种子）；论文定稿前需在服务器（A100）复跑确认；
- 基线第 3 个种子、Batch2（IID/同速/跨载 + cwt/组合表示）、Friedman 检验均为进行中状态；
- 所有数字以逐折持久化文件为准，可随时重新聚合复现。

下一期（系列终篇）：论文写作进展——从 v0 到 v1 的演进、DIS 复现对照与下一步计划。
