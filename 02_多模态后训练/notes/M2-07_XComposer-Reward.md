---
title: "InternLM-XComposer2.5-Reward: A Simple Yet Effective Multi-Modal Reward Model"
authors: "Yuhang Zang, Xiaoyi Dong, Pan Zhang, Yuhang Cao, Ziyu Liu, Shengjie Wang, Haodong Duan, Jiaqi Wang, Dahua Lin"
venue: "arXiv 2025"
year: 2025
arxiv_id: "2501.12368"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/后训练/奖励模型"
  - "#多模态/推理/测试时计算"
prerequisites:
  - "[[M1-09]]"
  - "[[M2-05]]"
---

## 核心贡献（3 句话）

1. **做了什么**：基于 InternLM-XComposer2.5 构建多模态奖励模型，在 Multimodal RewardBench 上达到 SOTA，支持图文多模态输入的质量评分。
2. **怎么做的**：用多模态偏好数据对 InternVL 系基模型进行专项训练，将奖励模型的输出头替换为标量分数输出；同时支持 Best-of-N（BoN）测试时计算扩展。
3. **效果如何**：在多个多模态 benchmark 的 BoN 设置下，使用该奖励模型的测试时计算大幅提升基础模型性能（代替人工评分进行候选答案选择）。

## 与主线的关系

> 多模态奖励模型是连接"偏好对齐"和"推理增强"两个方向的桥梁——既是 DPO/RLHF 训练的信号来源，也是测试时 BoN 计算的评分器。这篇论文提供了工程实现的参考。

- 依赖于：[[M1-09]] InternVL 2.5（基座模型）
- 相关：[[M2-18]] Multimodal RewardBench（评测其质量）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：奖励模型构建方法较工程化，核心原理在 [[M2-01]]-[[M2-06]] 已充分覆盖

## 快速笔记

- 关键设计：在 InternVL 2.5 基础上只修改输出头（ORM）+ 专门的偏好数据微调
- BoN 应用：N=8 时，BoN + 该奖励模型可使 MathVista 提升约 7%
- 在 MultiModal RewardBench 覆盖的 6 个域（通用、偏好、知识、推理、安全、VQA）上均有竞争力
