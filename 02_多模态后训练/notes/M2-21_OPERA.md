---
title: "OPERA: Alleviating Hallucination in Multi-Modal Large Language Models via Over-Trust Penalty and Retrospection-Allocation"
authors: "Huang et al."
venue: CVPR
year: 2024
arxiv_id: "2311.17911"
reading_depth: 粗读
status: 待读
tags:
  - 多模态/幻觉/解码时干预
  - 多模态/幻觉/注意力机制
prerequisites:
  - "[[M2-15_Hallucination-Survey]]"
  - "[[M2-20_VCD]]"
estimated_hours: 2.5
---

# OPERA 粗读笔记

## 核心贡献（3 句话）

1. 提出 OPERA（Over-Trust Penalty and Retrospection-Allocation）：通过惩罚解码过程中"过度信任"少数 summary token 的注意力聚合模式，并在检测到该模式时回溯重新分配注意力，从解码策略层面减少幻觉。
2. 核心洞察：幻觉往往发生在模型注意力高度集中于少数"summary token"（如句号、逗号）时，此时视觉信息利用不足，模型转而依赖语言先验生成。OPERA 在 beam search 期间检测并惩罚这种模式。
3. CVPR 2024 Highlight，在 CHAIR、POPE、MME 等基准上显著减少幻觉，无需额外训练，与所有基础模型兼容。

## 与主线的关系

- **"解码时干预"路线的另一代表作**，与 VCD（[[M2-20_VCD]]）形成对比：
  - VCD：通过对比扰动图像 logit 来放大视觉信号
  - OPERA：通过检测/惩罚注意力聚合模式来强制利用视觉信息
- 两篇合读可理解"解码时幻觉缓解"的两种思路（logit 层面 vs. 注意力层面）
- 与训练时方法（[[M2-02_RLHF-V]]、[[M2-04_mDPO]]）的比较是重要研究议题：测试时计算代价 vs. 训练成本

## 值得继续精读吗？

**否**。OPERA 的核心贡献是注意力模式的诊断和惩罚机制，粗读了解思路即可。若研究"解码时幻觉缓解"方向，OPERA 值得精读。

## 快速记录

<!-- "summary token"是什么？为什么会导致幻觉？
     Over-Trust Penalty 如何在 beam search 中实现？beam score 如何调整？
     Retrospection-Allocation 的回溯机制：回到什么位置？如何重新分配注意力？
     OPERA 与 VCD 是否可以结合使用？有没有组合实验？ -->
