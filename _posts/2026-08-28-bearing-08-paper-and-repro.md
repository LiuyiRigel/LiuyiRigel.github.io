---
title: '[研究笔记] 论文写作进展：从初稿 v0 到 v1，与 DIS 复现对照'
date: 2026-08-28
permalink: /posts/2026/08/bearing-08-paper-and-repro/
tags:
  - ResearchNote
  - Bearing-Fault-Diagnosis
  - Paper-Writing
  - DIS
  - Reproduction
  - MSSP
mathjax: true
image: /images/bearing_research/fig25_dis_repro_web.jpg
---


<div align="center">
    <img src="/images/bearing_research/fig25_dis_repro_web.jpg" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

本系列终篇。前面 7 篇讲了研究的背景、理论、模型、数据与实验；这一篇汇总**论文写作的进展**（从初稿 v0 到 v1 的结构演进）、**DIS 复现**（对照最接近我们定位的 MSSP 2026 论文），以及**下一步计划**。

---

## 1. 论文定位回顾：不造模型，造证据

第 1 篇讲过，我们最终选择"受控基准 + 可证伪规律"的定位。论文的骨架（报告 26）围绕**三条可证伪规律**展开：

1. **表示规律**：原始时频结构（raw/SSST）是唯一跨设备可迁移且平衡的表示；解调（env/ord）是"精度假象"；
2. **覆盖规律**：训练数据覆盖目标转速可修复跨转速盲点（+46.8pp）；数据覆盖 > 模型魔法；
3. **结构规律**：架构影响 FPR 上限，但带来的提升远小于数据覆盖。

这三条规律在 Batch1 全量中全部得到验证（第 7 篇），论文的故事线因此是"**证据驱动**"而非"方法驱动"。

---

## 2. 初稿 v0 → v1：主线收敛

### 2.1 v0（报告 30）：多线并进

初稿 v0 尝试同时承载：多数据集难度阶梯、软阶次对齐理论、AE 辅线、WaveBag 架构……内容全面但**主线不聚焦**。

### 2.2 v1（报告 36）：Paderborn 统一 Benchmark 主线

v1 做了三件关键的事：

1. **主线收敛**：全部主结果来自 Paderborn 受控基准（Batch1 全量回填）；多数据集难度阶梯、软对齐、AE 辅线**降级为辅助证据**（§4.7 与附录）；
2. **控制变量法叙事**：三个研究问题（R1 呈现 / R2 覆盖 / R3 结构）各自对应一个实验节，逐节给出配对置换检验；
3. **英文草稿先行**：标题、摘要均为英文草稿（如 *What Transfers Under Unseen Devices and Speeds?*），LaTeX 同步（`latex/main.tex` + 附录表 `appendix_tables.tex`）。

### 2.3 标题候选（v1）

- **A**: *What Transfers Under Unseen Devices and Speeds? A Controlled Study of Signal Representation, Training Coverage, and Architecture for Bearing Fault Diagnosis*
- **B**: *Data Coverage over Model Magic: A Controlled Benchmark of Cross-Device, Cross-Speed Bearing Fault Diagnosis*

---

## 3. DIS 复现：与最接近的 MSSP 论文对标

**DIS**（*Domain interference suppression for reliable fault diagnosis under unseen operating conditions*, MSSP 2026）是和我们定位最接近的论文：同样强调"跨未见工况的可靠性"。我们完整复现了它，作为方法层对照。

### 3.1 CWRU 复现（10 类，4 负载域，留一负载域）

| 域 | s0 | s1 | s2 | 均值 |
|---|---|---|---|---|
| 0hp | 93.4 | 91.1 | 95.2 | 93.2 |
| 1hp | 97.7 | 97.0 | 94.9 | 96.5 |
| 2hp | 100 | 100 | 100 | 100 |
| 3hp | 100 | 100 | 92.9 | 97.6 |
| **均值** | | | | **≈ 96.8%** |

### 3.2 SQV（清华变转速，7 类，6 域）

均值 **47.5%**（chance 14.3%）——变转速域泛化显著更难。

### 3.3 我们与 DIS 的关系（不是竞争，是互补）

| 层面 | DIS（方法路线） | 我们（数据/表示路线） |
|---|---|---|
| 操作对象 | 特征层（域干扰抑制） | 输入表示 + 训练配给 |
| 回答的问题 | 如何学更鲁棒的特征 | 什么表示/数据本身决定可靠性 |
| 结论互补性 | 特征层改进可叠加 | 表示层/数据层结论是方法的前置条件 |

论文 Discussion 将明确：**raw SSST + 多转速训练 + DIS 式特征层方法可以叠加**（作为未来工作验证）。

---

## 4. 诚实性工程：三个补缺实验

写论文前，我们还做了三个"自查"实验（报告 25），确保结论经得起审稿：

### 4.1 G2 会话身份混杂保险 ✅

变速振动 VS 实验中，"类识别是否依赖会话身份"曾是个疑点。严格协议（留出 (类, 试验) 全部剖面）下池化 99.5% ± 1.7——**类识别不依赖会话身份**，VS 近满分结果可信。

### 4.2 ord3 修正重验 ✅

多域跨类型实验中，旧 ord3 图因 nperseg 分辨率不足退化为常数（25.0%）。修正后 **32.9% ± 4.6**，仍输给含载波 SSST（39.9）约 7pp——"硬对齐丢失可迁移信息"经受住 bug 修正，措辞从"崩溃"升级为"量化落后 ~7pp"，更严谨。

### 4.3 Paderborn 3 类修正标签 ✅

修正 KA/KI 前缀与人工/真实标签的对应后：池化 **50.3% ± 34.3**（chance 33%），旧错误标签 44.8 → 修正后 +5.5pp。结论：**3 类损伤类型跨轴承迁移本质上难**（与二分类对照）。

> **方法论教训**：标签语义学影响结论——论文附上"错误标签对照"本身就是方法论贡献。

<div align="center">
    <img src="/images/bearing_research/fig26_gap_closing_web.jpg" 
         style="width: 85%; 
                border-radius: 5px; 
                box-shadow: 0 10px 20px rgba(0,0,0,0.2); 
                border: 1px solid #eee;">
</div>
<br>

*三个补缺实验：G2 会话混杂保险、ord3 重验、3 类修正标签。*

---

## 5. 实验覆盖度评估（报告 31）：我们敢说"系统性"

在写 v1 之前，我们先问自己：**"现在的测试量够不够、是否覆盖所有模型和表示？"** 诚实的回答是：不够——当时更适合叫"初步协议研究"，还不能叫"系统性 benchmark"。覆盖度评估给出了缺口清单和最低补实验方案，直接催生了统一 Benchmark（第 7 篇）与 Batch2 计划。

**方法论价值**：把"实验覆盖度评估"做成正式步骤，让论文的"系统性"有据可查。

---

## 6. 当前进度与下一步计划

### 已完成
- [x] Batch1 全量（32 设备 × 3 seeds，n=96，两个协议 × 5 表示 × 9 模型）；
- [x] 统计检验（配对置换检验）、混淆矩阵、逐类 F1、AUC/FPR；
- [x] 论文初稿 v1 回填 Batch1 全部真实数字；
- [x] LaTeX 主文件 + 附录表同步；
- [x] MSSP 指标对照（报告 35）。

### 进行中 / 待办
- [ ] 基线第 3 个种子补齐（当前基线 s0/s1）；
- [ ] **Batch2**：IID / 同速 / 跨载协议 + cwt / 组合表示；
- [ ] **Batch3（ART2REAL）**：纯人工损伤训练 → 真实损伤测试（探迁移边界）；
- [ ] Friedman + critical difference 图、复杂度/延迟表；
- [ ] Batch1 服务器（A100）复跑确认；
- [ ] 英文全文排版、参考文献、图注。

---

## 7. 系列总结

8 篇文章完整覆盖了这项研究：

| # | 主题 | 关键信息 |
|---|---|---|
| 1 | 总览与技术路线 | 为什么做跨域泛化、三因素框架 |
| 2 | 物理与数学 | FCO 不变量、SSST、软阶次对齐理论 |
| 3 | 模型设计 | SSST+Mamba（55k 参数）、WaveBag 演进 |
| 4 | 数据集 | 五类数据集的角色与探查 |
| 5 | 辅线实验 | XJTU-SY +4.8pp、AE 跨转速、软>硬、跨类型~40% |
| 6 | Paderborn 7 臂 | raw SSST 唯一可迁移、env/ord FPR 100% |
| 7 | 统一 Benchmark | 跨转速盲点 44.8→91.6、多转速修复、精度假象 |
| 8 | 论文与复现 | v0→v1 收敛、DIS 96.8%、三个补缺实验 |

**一句话结论**：跨设备跨转速的可靠诊断，主要取决于**训练数据覆盖**与**输入表示**，模型结构次之——这不是"模型魔法"的故事，而是"数据与表示"的故事。

后续进展（Batch2/3、论文投稿）会继续更新。感谢阅读。
