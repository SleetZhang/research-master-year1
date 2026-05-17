---
title: "InstructBLIP: Towards General-purpose Vision-Language Models with Instruction Tuning"
authors: "Wenliang Dai, Junnan Li, Dongxu Li, Anthony Meng Huat Tiong, Junqi Zhao, Weisheng Wang, Boyang Li, Pascale Fung, Steven Hoi"
venue: "NeurIPS 2023"
year: 2023
arxiv_id: "2305.06500"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/架构/连接器"
  - "#多模态/SFT/指令微调"
prerequisites:
  - "[[BLIP-2]]"
---

## 核心贡献（3 句话）

1. **做了什么**：在 BLIP-2 基础上，系统整合 26 个公开视觉-语言数据集，并将其转换为指令微调格式，构建通用多模态指令跟随模型。
2. **怎么做的**：提出 Instruction-aware Q-Former，使 Q-Former 在提取视觉特征时感知文本指令，实现任务自适应的视觉特征压缩；同时使用平衡采样策略防止过拟合。
3. **效果如何**：在 13 个 held-out 数据集上实现零样本 SOTA，最小的 4B 模型超越 Flamingo-80B 平均 24.8%。

## 与主线的关系

> 连接器设计的重要里程碑：展示了 Q-Former 如何被指令感知化，是理解"如何让视觉编码器与 LLM 对齐"的典型案例。此后 LLaVA 系列转向更简单的 MLP 投影，对比阅读更有价值。

- 依赖于：[[BLIP-2]]（Q-Former 架构）
- 对照组：[[M1-03]]（MLP vs Q-Former 路线对比）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：Q-Former 路线在 LLaVA 1.5 之后基本被 MLP 路线取代，粗读了解其历史地位即可

## 快速笔记

（记录 1-3 个值得回忆的关键细节或数字）
- Instruction-aware Q-Former：让 Q-Former 的查询向量也 attend to 文本指令
- 26 个数据集 → 11 类视觉-语言任务，平衡采样防止大数据集主导训练
- 关键结论：多样化指令数据 + 指令感知特征 > 单任务专训
