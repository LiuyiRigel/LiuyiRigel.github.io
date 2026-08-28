---
title: '[研究笔记] 实验数据集全景：五类轴承数据的角色与探查'
date: 2026-08-28
permalink: /posts/2026/08/bearing-04-datasets/
tags:
  - ResearchNote
  - Bearing-Fault-Diagnosis
  - Dataset
  - Paderborn
  - XJTU-SY
  - CWRU
mathjax: true
image: /images/bearing_research/fig17_md_results_web.jpg
---


<div align="center">
    <img src="/images/bearing_research/fig17_md_results_web.jpg" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

本系列第 4 篇。梳理研究用到的**五类轴承数据集**：各自的特点、在本研究中的角色（主线/辅线/对照）、下载渠道，以及首轮探查的关键结论。所有数据均来自合法公开渠道（官网 / Mendeley / IEEE Dataport）。

---

## 1. 数据集总览

| # | 数据集 | 角色 | 设备/样本规模 | 关键特性 |
|---|---|---|---|---|
| 1 | **Paderborn**（KAt 数据中心） | **主线基准** | 32 台轴承 | 真实+人工损伤、4 工况、64 kHz |
| 2 | **XJTU-SY**（加速寿命） | 跨轴承/跨工况验证 | 15 台轴承 | 全寿命记录、25.6 kHz |
| 3 | **CWRU**（凯斯西储） | 同分布对照 | 4 负载 × 多故障 | 经典基准、12/48 kHz |
| 4 | **Multi-domain**（Mendeley） | 跨几何类型 | 3 种轴承型号 | 深沟球/圆柱/圆锥滚子 |
| 5 | **变速振动**（Huang & Baddour） | 连续变转速验证 | 4 转速剖面 | 段内变速、200 kHz |
| 6 | **AE-2013 台架**（自有） | 辅线 | 每类 1 轴承 | 声发射、跨转速状态识别 |

---

## 2. Paderborn：主线实验基准

**Paderborn University KAt Bearing Data Center** 是我们当前论文主实验（统一 Benchmark）的唯一数据来源。

- **32 台轴承**：6 健康 + 12 人工损伤 + 14 真实损伤（覆盖 EDM 刻槽 / 电蚀 / 钻削 / 疲劳点蚀 / 塑性压痕等）；
- **4 种工况**：转速 900/1500 rpm × 负载 0.7/0.1 Nm × 径向力 1000/400 N；
- **多通道**：振动 + 电流 + AE，振动采样 64 kHz；
- **规模**：每工况每轴承 20 个 4 秒记录。

**为什么它适合做跨设备主线？** 32 个设备个体让我们可以严谨地做**留一设备（leave-one-bearing-out）**协议——每次用 31 台训练、1 台测试，这正是"未见设备泛化"的黄金标准评估方式。

- 官网下载：https://groups.uni-paderborn.de/kat/BearingDataCenter/ （32 个 RAR，约 5.4 GB）
- 文献：Lessmeier et al., *Condition monitoring of bearing damage in electromechanical drive systems...*, PHM Europe 2016

---

## 3. XJTU-SY：跨轴承/跨工况验证（代理主线）

**XJTU-SY 滚动轴承加速寿命数据集**（西安交通大学）记录了 15 台轴承从健康到失效的全寿命过程。

- **3 种工况**：2100 rpm/12 kN、2250 rpm/11 kN、2400 rpm/10 kN；
- 振动双通道 25.6 kHz；
- 本研究用它验证：早期/晚期故障识别、**跨轴承（同型号多个体）泛化**、物理先验边界（提示卡 +4.8pp，置换检验 p=0.0038，见第 5 篇）。

官网下载：https://biaowang.tech/xjtu-sy-bearing-datasets/ （备用：IEEE Dataport）
文献：王彪 等，《XJTU-SY滚动轴承加速寿命试验数据集解读》，机械工程学报, 2019。

---

## 4. CWRU：经典对照基准

**CWRU Bearing Data Center** 是故障诊断领域最经典的公开数据，几乎每篇论文都有它。

- SKF/ND 轴承，4 种负载（0–3 hp）；
- 故障：内圈/外圈/滚动体/保持架 × 多种损伤尺寸；
- 采样 12/48 kHz。

在我们的研究中，CWRU 主要用作**方法对比的同分布上限**（如 DIS 复现 96.8%），以及早期原型验证。它是"IID 接近满分"叙事的来源之一。

官网下载：https://engineering.case.edu/bearingdatacenter
文献：Smith & Randall, *Rolling element bearing diagnostics using the Case Western Reserve University data*, MSSP, 2015。

---

## 5. Multi-domain：跨几何类型难度顶端

**Multi-domain 轴承故障数据集**（Mendeley）包含**多种几何类型**的轴承（深沟球 6205-2RS、圆柱滚子 NU205、圆锥滚子 30204 等）。

<div align="center">
    <img src="/images/bearing_research/fig17_md_results_web.jpg" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

*多域数据集跨轴承类型实验结果：跨几何类型的泛化是所有偏移中最难的（~40%）。*

**探查结论**：跨几何类型泛化精度仅 ~40%，处于难度阶梯顶端——因为轴承几何参数（节圆直径、滚动体数）改变后，故障特征阶次 FCO 都变了，物理不变量几乎消失（与第 2 篇理论呼应）。

下载：子集 3（圆锥滚子 30204）https://data.mendeley.com/datasets/7trwzz77xh；子集 1/2 DOI 10.17632/2cygy6y4rk

---

## 6. 变速振动（Huang & Baddour）：连续变速验证

**Bearing vibration data under time-varying rotational speed**（渥太华大学）用于验证"连续变速"这一最贴近工业现实的场景。

- SpectraQuest MFS-PK5M 试验台，ER16K 轴承；
- **段内连续变速**（约 700–1740 rpm），4 种转速剖面（升速/降速/升-降/降-升）；
- 200 kHz 采样（振动 + 编码器双通道）；
- 官方 3 类（健康/内圈/外圈）× 12 文件。

它的价值在于：**转速不是离散的几档，而是连续变化**——这是检验"软对齐 vs 硬对齐"的理想场景（结果见第 5 篇：soft 92.5% > base 90.9% > hard 77.2%）。

下载：Data in Brief 文章页（含 Mendeley 链接）https://doi.org/10.1016/j.dib.2018.04.034
文献：Huang & Baddour, *Bearing vibration data collected under time-varying rotational speed conditions*, Data in Brief, 2018。

---

## 7. AE-2013 台架（自有）：辅线的诚实边界

声发射（AE）台架数据是我们自有数据。**关键限制**：每个故障类型只有一个轴承——样本身份与故障类型完全混杂，因此：

- ❌ "故障类型跨轴承泛化"不可做（身份混淆）；
- ✅ 可做"**同轴承跨转速状态识别**"（12 方向矩阵均值约 93.2%）；
- 定位：辅线，为论文提供"范式对比"（per-hit 双模态 vs 参数序列）与机理追查素材（详见第 5 篇）。

---

## 8. 数据集使用的总体策略

```
主线：Paderborn（32 设备，留一设备 × 跨转速/跨负载，论文全部主结果）
  └─ 全部 Batch1/2/3 实验
辅线：XJTU-SY（跨轴承/跨工况 + 物理先验）
     变速振动（连续变速软/硬对齐）
     多域（跨几何类型难度顶端）
     AE-2013（范式对比 + 机理）
对照：CWRU（同分布上限 + DIS 复现）
```

这个结构让我们既能讲"受控基准"的严谨故事（主线），又有"难度谱"的广度（辅线），还能用 CWRU 对接文献基线——论文的证据体系因此是**多数据集、多层次**的。

---

## 9. 小结

- 主线 **Paderborn**：32 设备个体 → 严谨的留一设备跨域评估；
- **XJTU-SY / 变速振动 / 多域 / AE**：分别覆盖跨轴承、连续变速、跨几何类型、跨模态四个辅线维度；
- **CWRU**：对接文献的同分布对照；
- 每个数据集都对应一个明确的"难度维度"，共同构成论文的泛化难度谱。

下一期：辅线实验的具体结果——XJTU-SY 物理先验、AE 跨转速、变速振动软硬对齐与多域跨类型泛化。
