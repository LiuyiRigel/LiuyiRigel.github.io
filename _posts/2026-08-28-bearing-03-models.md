---
title: '[研究笔记] 模型设计：SSST+轻量 Mamba 到 WaveBag 架构演进'
date: 2026-08-28
permalink: /posts/2026/08/bearing-03-models/
tags:
  - ResearchNote
  - Bearing-Fault-Diagnosis
  - Mamba
  - Synchrosqueezing
  - WaveBag
  - Architecture
mathjax: true
image: /images/bearing_research/ssst_demo.png
---


<div align="center">
    <img src="/images/bearing_research/ssst_demo.png" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

本系列第 3 篇。介绍研究的**模型设计演进**：从"SSST 时频前端 + 轻量双向 Mamba 骨干"的原型，到可解释的 WaveSeq/WaveBag 架构，以及过程中发现的模型分析结论。

---

## 1. 设计动机：为什么要轻量 + 可解释？

工业故障诊断模型的部署约束与学术基准不同：

- **算力受限**：边缘设备（传感器节点、工控机）无法跑大模型；
- **可解释性需求**：现场工程师需要知道"模型为什么报警"；
- **数据稀缺**：故障样本少，大模型容易过拟合。

因此我们设计了**参数极少、结构清晰、带物理先验**的原型：`AE_MambaNet`（约 55–62k 参数）。

---

## 2. SSST 时频前端

前端把一维振动/AE 信号变成二维时频图，是模型的"眼睛"。我们的实现（`ssst.py`）包含：

- **S 变换（Stockwell）**：频率自适应窗宽的时频分析，低频高分辨率；
- **同步挤压（SSST）**：能量重排锐化时频脊线；
- **同步提取（SET）**：同步挤压的进一步锐化变体；
- **物理先验通道**：基于故障特征阶次（FCO）生成的提示通道（见第 2 篇理论）。

<div align="center">
    <img src="/images/bearing_research/fig4_ae_params.png" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

*模型参数与结构分析：轻量双向 Mamba 骨干的设计权衡。*

### 前端验证（数值正确性）

| 测试 | 结果 |
|---|---|
| 纯音 100 kHz 自检 | 挤压峰值落点误差 ~1.4%（一个 FFT 箱内） |
| 合成 AE 冲击串（BPFO=107.2 Hz + 125 kHz 谐振） | 估计冲击重复率 107.8 Hz，谱峰 124 kHz |
| 噪声稳健性 | 统计谱峰用最大值投影（均值会被噪声高频翘曲干扰） |

---

## 3. 轻量双向 Mamba 骨干

### 3.1 为什么是 Mamba？

Mamba（状态空间模型）相比 Transformer 有两个关键优势：

1. **线性复杂度**：序列长度 $L$ 的计算量 $O(L)$，而注意力是 $O(L^2)$；
2. **长程依赖**：状态空间模型天然处理长序列，比 CNN 的感受野大得多。

对 224×224 时频图，Mamba 可以在**按频率行展开**后做序列建模，捕捉跨频率的脊线结构。

### 3.2 我们的实现细节

- 纯 PyTorch 实现，内置**并行选择性扫描**（Hillis–Steele 算法），不依赖 mamba-ssm C 扩展；
- 顺序参考 vs 并行实现的最大误差：CPU 2e-6 / GPU 6e-6（数值一致性验证通过）；
- 性能：前向 batch8 224×224 输入，CPU 194ms → GPU 7.3ms，**加速 26.7×**；
- 参数量 55.5k，FLOPs 64.3M（对比 CNN 85.5M）。

### 3.3 模型分析结论（报告 04/05）

七维评估（精度、效率、鲁棒性、可解释性、泛化、稳定性、复杂度）下的关键发现：

| 维度 | 结论 |
|---|---|
| 精度 | 同域 IID 接近 100%；跨工况先验通道 +7~8pp |
| 效率 | GPU 前向 7.3ms，55.5k 参数，适合边缘部署 |
| 鲁棒性 | 10dB 噪声 90.5% / 5dB 80.5% / 0dB 72.0%（跨轴承） |
| 可解释性 | Grad-CAM/显著性图与物理先验通道高度一致 |
| 泛化 | 跨轴承（同工况）86.8%±19.2，跨工况 80.3% vs 72.5%（+7.8pp） |

---

## 4. 架构演进：从 WaveSeq 到 WaveBag

在主线聚焦 Paderborn 之前，我们还探索了一条**波形嵌入序列**架构线（报告 13/16），最终简化收敛为 **WaveBag（波形形状袋）**：

### 4.1 设计思路

不把信号强行变成整幅时频图，而是：

1. **波形嵌入**：把信号切成短段，每段用轻量编码器嵌入；
2. **序列建模**：嵌入序列送入序列模型（双向 LSTM/Mamba）；
3. **袋模型简化**：实验发现序列建模的收益有限，简化为"波形形状袋"——对嵌入做聚合统计。

<div align="center">
    <img src="/images/bearing_research/fig16_wavebag_master_web.jpg" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

*WaveBag 总图：波形嵌入 → 形状袋聚合 → 分类，全程可解释。*

### 4.2 关键发现：序列位置信息不重要，波形形状才重要

WaveSeq（带序列建模）vs WaveBag（纯袋聚合）的对照实验显示：

- 性能几乎持平 → 故障判别信息主要承载在**波形形状**上，而不是冲击的先后顺序；
- 这符合物理直觉：故障特征（冲击重复率、谐波结构）本质上是"形状级"信息；
- 简化后模型更轻、更稳、更容易解释。

### 4.3 可解释性图

<div align="center">
    <img src="/images/bearing_research/fig12_interp.png" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

*可解释性分析：显著性图显示模型聚焦于与故障特征频率一致的时频区域。*

---

## 5. 从原型到 Benchmark：架构的角色定位

随着研究推进，Paderborn 统一 Benchmark（第 6–7 篇）成为主线。原型的定位随之明确：

- **AE_MambaNet / WaveBag**：轻量、可解释、物理先验注入的"研究载体"，验证了物理先验与软阶次对齐的可行性；
- **Benchmark 的 9 模型谱系**（WDCNN/TCN/LSTM/Transformer/VanillaMamba/BiMamba1D/CNN2D/ResNet/BiMamba）：在统一协议下系统回答"结构影响多大"；
- 重要教训（来自 7 臂分析）：**换模型带来的提升远小于补训练数据覆盖**（详见第 7 篇），这重新校准了架构创新的价值。

---

## 6. 小结

- **SSST 前端 + 轻量双向 Mamba**：55k 参数，跨工况 +7~8pp（先验通道），可解释性好；
- **WaveSeq → WaveBag 简化**：证明"波形形状 > 序列顺序"，架构更轻更稳；
- 架构在主线 Benchmark 中是**受控变量之一**，而不是主角——这本身就是方法论上的重要决定。

下一期：实验数据集全景——五类数据集的角色、下载与探查结论。
