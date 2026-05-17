---
title: "LLaVA-OneVision: Easy Visual Task Transfer"
authors: "Bo Li, Yuanhan Zhang, Dong Guo, Renrui Zhang, Feng Li, Hao Zhang, Kaichen Zhang, Yanwei Li, Ziwei Liu, Chunyuan Li"
venue: "arXiv 2024"
year: 2024
arxiv_id: "2408.03326"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/架构/架构演进"
  - "#多模态/SFT/指令微调"
prerequisites:
  - "[[M1-03]]"
---

## 核心贡献（3 句话）

1. **做了什么**：整合 LLaVA-NeXT 系列多项 blog 工作，构建统一的 LLaVA-OneVision 模型，同时支持单图、多图、视频三类场景，是 LLaVA 系列的集大成之作。
2. **怎么做的**：使用 SigLIP 视觉编码器 + AnyRes（任意分辨率切片）技术处理高分辨率图像 + Qwen-2 LLM；通过单图→多图→视频的渐进式迁移训练策略实现跨场景泛化。
3. **效果如何**：在多个单图和视频 benchmark 上达到开源 SOTA，是理解 LLaVA 系列发展路径的终点站。

## 与主线的关系

> LLaVA 系列架构演进的终点，展示了"先单图 SFT → 再迁移到多图/视频"的训练策略。对于后训练研究者：了解这个基座模型的能力边界，才能理解后续偏好对齐工作在什么基础上做改进。

- 依赖于：[[M1-03]] LLaVA-1.5（基础架构）
- 同期对比：[[M1-07]] Qwen2-VL，[[M1-09]] InternVL 2.5

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：架构层面在 LLaVA-1.5 基础上无根本性突破，主要是数据和训练策略的工程改进

## 快速笔记

- AnyRes：将高分辨率图像切分为多个 patch + 低分辨率缩略图，是高分辨率处理的工程方案
- SigLIP 替代 CLIP：更好的图文对齐能力
- 迁移策略：单图 SFT 充分后，只需少量多图/视频数据即可迁移
