---
title: "R1-Onevision: Advancing Generalized Multimodal Reasoning through Cross-Modal Formalization"
authors: "Yi Yang, Xiaoxuan He, Hongkun Hao, Yana Wei, Pu Zhao, Mingyang Li, Shengnan An, Hao Yu, Bing Duan, Fenghua Tong, Shengyu Zhang, Fei Wu"
venue: "ICCV 2025"
year: 2025
arxiv_id: "2503.10615"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/推理/RL强化学习"
  - "#多模态/推理/跨模态"
prerequisites:
  - "[[M2-11]]"
  - "[[M2-12]]"
---

## 核心贡献（3 句话）

1. **做了什么**：提出跨模态形式化（Cross-Modal Formalization）方法，将不同模态的推理问题（数学、图表、图像）统一转化为结构化形式，再用 GRPO 训练泛化推理能力。
2. **怎么做的**：用 LLM 将视觉推理问题形式化为标准数学或逻辑表达式，消除模态差异；再在此标准化表示上用 GRPO 训练跨模态推理模型，使同一模型能处理多类视觉推理任务。
3. **效果如何**：ICCV 2025，在多个多模态数学和推理 benchmark 上的跨任务泛化能力优于单任务特化方法。

## 与主线的关系

> R1-OneVision 代表了"跨模态泛化推理"的思路，与 Vision-R1（[[M2-11]]）的单任务优化形成对比。粗读了解跨模态形式化的核心思路及与主攻方向的关系即可。

- 依赖于：[[M2-11]] Vision-R1（GRPO 框架），[[M2-12]] Visual-RFT（视觉任务的 RL）
- 对照：[[M2-11]] Vision-R1（单任务 vs 跨模态泛化）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：跨模态形式化是工程思路，核心 RL 机制已在 [[M2-11]] 覆盖；对后训练主线贡献是 Vision-R1 的变体

## 快速笔记

- 跨模态形式化：把图表/图像中的信息提取为数学符号/逻辑公式，再进行推理
- 关键发现：形式化降低了模态差异，使同一 GRPO 模型能泛化到不同视觉推理类型
- 与 Vision-R1 的区别：前者关注单任务高精度，后者关注跨任务泛化
