---
title: "Multimodal RewardBench: Holistic Evaluation of Reward Models for Vision Language Models"
authors: "Michihiro Yasunaga, Luke Zettlemoyer, Armen Aghajanyan"
venue: "arXiv 2025"
year: 2025
arxiv_id: "2502.14191"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/后训练/奖励模型"
  - "#多模态/评测/综合基准"
prerequisites:
  - "[[M2-07]]"
---

## 核心贡献（3 句话）

1. **做了什么**：构建 Multimodal RewardBench，首个全面评测多模态奖励模型（VLM-as-judge）质量的基准，覆盖 6 个域（通用正确性、偏好、知识、推理、安全、VQA）。
2. **怎么做的**：由领域专家人工标注偏好对，构建 ~4K 高质量多模态偏好判断样本；同时评测 20+ 现有奖励模型/judge 的性能。
3. **效果如何**：揭示当前最强模型（Gemini-1.5-Pro、Claude-3.5-Sonnet）在 Multimodal RewardBench 上仅达 72% 准确率，证明多模态奖励模型质量仍有很大提升空间。

## 与主线的关系

> Multimodal RewardBench 是理解当前奖励模型质量瓶颈的重要参考，特别是在读完 [[M2-07]] InternLM-XComposer2.5-Reward 后，用它来理解奖励模型的质量评估维度。

- 依赖于：[[M2-07]]（具体奖励模型），[[M2-01]]~[[M2-06]]（奖励模型的使用背景）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：benchmark 设计论文，了解评测维度和关键数字即可

## 快速笔记

- 6 个评测域：通用正确性 / 偏好 / 知识 / 推理 / 安全 / VQA
- 关键发现：推理和知识域最难，现有奖励模型在这两个域准确率最低
- 当前 gap：即使 GPT-4o 作为 judge，在多模态偏好判断上也有约 28% 的错误率
