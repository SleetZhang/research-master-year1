---
title: "Hallucination Augmented Contrastive Learning for Multimodal Large Language Model"
authors: "Chaoya Jiang, Haiyang Xu, Mengfan Dong, Jiaxing Chen, Wei Ye, Ming Yan, Qinghao Ye, Ji Zhang, Fei Huang, Shikun Zhang"
venue: "CVPR 2024"
year: 2024
arxiv_id: "2312.06968"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/幻觉/缓解"
  - "#多模态/后训练/对比学习"
prerequisites:
  - "[[M2-15]]"
  - "[[M1-03]]"
---

## 核心贡献（3 句话）

1. **做了什么**：从表示学习角度分析幻觉成因，发现文本表示和视觉表示之间存在巨大 gap，且含幻觉和不含幻觉的文本表示纠缠在一起。
2. **怎么做的**：提出 HACL（Hallucination Augmented Contrastive Learning），将含幻觉的文本作为对比学习的"困难负样本"，拉近视觉表示与无幻觉文本表示，同时推远幻觉文本表示。
3. **效果如何**：CVPR 2024，在 MMHal-Bench 上比基线（MiniGPT-4/LLaVA）提升 34.66%/29.5%，是基于表示空间分析的幻觉缓解方法代表。

## 与主线的关系

> HACL 代表了"从训练阶段通过表示学习减少幻觉"的路线，与 DPO 路线（[[M2-02]]~[[M2-05]]）并行。了解这个方向有助于建立对幻觉缓解方法多样性的认识。

- 依赖于：[[M2-15]] 幻觉综述，[[M1-03]] LLaVA-1.5
- 对照：[[M2-02]] RLHF-V（DPO 路线 vs 对比学习路线）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：对比学习缓解幻觉的思路相对成熟，不是主攻后训练方向的核心

## 快速笔记

- 核心发现：视觉-文本表示空间存在系统性 gap，是幻觉的根本原因之一
- 对比学习设计：将同一图像的幻觉描述 vs 正确描述组成困难负正对，对比拉近/推远
- 实现：在 LLaVA 训练过程中添加对比损失项（不需要额外标注）
