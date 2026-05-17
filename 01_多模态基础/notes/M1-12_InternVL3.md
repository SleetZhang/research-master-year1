---
title: "InternVL3: Exploring Advanced Training and Test-Time Recipes for Open-Source Multimodal Models"
authors: "Chen et al."
venue: arXiv
year: 2025
arxiv_id: "2504.10479"
reading_depth: 粗读
status: 待读
tags:
  - 多模态基础/预训练
  - 多模态基础/Native-Multimodal
  - 模型架构/InternVL
prerequisites:
  - "[[M1-04_InternVL]]"
  - "[[M1-09_InternVL2.5]]"
estimated_hours: 2.5
---

# InternVL3 粗读笔记

## 核心贡献（3 句话）

1. 提出 Native Multimodal Pretraining（NMP）范式：在预训练阶段直接混合图文数据，打破 LLM 预训练和视觉对齐的两阶段割裂，从根本上提升视觉-语言融合能力。
2. 引入 Variable Visual Position Encoding（V2PE），支持任意分辨率图像和高分辨率文档的高效处理，比 InternVL2.5 的位置编码更灵活。
3. 在 OpenCompass 多模态榜单上超越同期闭源模型（GPT-4o、Claude 3.5）的若干子集，成为当时最强开源多模态 LLM 之一。

## 与主线的关系

- 代表 **native multimodal pretraining** 范式的代表作，与 M1 模块"预训练/连接器设计"主题高度相关
- 弥补了计划在"预训练范式"上的覆盖空白（此前 M1 以 alignment-stage 和 connector 设计为主）
- 与 [[M1-04_InternVL]] 和 [[M1-09_InternVL2.5]] 构成 InternVL 系列的完整演进链

**Native Multimodal Pretraining vs 传统两阶段**：

| 方案 | 预训练 | 视觉对齐 |
|------|--------|---------|
| 传统（LLaVA 范式）| 纯文本预训练 | 独立 vision-language alignment 阶段 |
| NMP（InternVL3）| 预训练直接混入图文数据 | 无需单独对齐阶段 |

## 值得继续精读吗？

**否**。NMP 的核心思想粗读已可理解；具体工程细节（数据配比、位置编码实现）对当前阶段意义有限。若后续研究预训练范式再返回精读。

## 快速记录

<!-- NMP 中图文数据的混合比例大概是多少？训练稳定性怎么保证？ -->
<!-- V2PE 与 RoPE、AnyRes 的关系？如何处理高分辨率切片的位置信息？ -->
<!-- InternVL3 与 Qwen2-VL 的 Naive Dynamic Resolution 有什么异同？ -->
