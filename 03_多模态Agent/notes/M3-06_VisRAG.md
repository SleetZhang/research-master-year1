---
title: "VisRAG: Vision-based Retrieval-augmented Generation on Multi-modality Documents"
authors: "Shi Yu, Chaoyue Tang, Bokai Xu, Junbo Cui, Junhao Ran, Yukun Yan, Zhenghao Liu, Shuo Wang, Xu Han, Zhiyuan Liu, Maosong Sun"
venue: "ICLR 2025"
year: 2025
arxiv_id: "2410.10594"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/Agent/RAG"
  - "#多模态/Agent/检索增强"
prerequisites:
  - "[[M3-01]]"
  - "[[M1-07]]"
---

## 核心贡献（3 句话）

1. **做了什么**：提出 VisRAG，一个基于视觉的多模态 RAG 框架，在检索和生成两个阶段都直接使用原始视觉（图像）而非从文档中提取的文字，避免了 OCR 解析误差。
2. **怎么做的**：(1) 视觉检索：用 VLM 将文档页面作为图像进行嵌入和检索（无需 PDF→文字转换）；(2) 视觉生成：检索到的页面图像直接输入 MLLM，让模型在原始视觉信息基础上回答问题，保留图表、表格、公式的原始格式。
3. **效果如何**：ICLR 2025，在多模态文档 QA 任务上比传统文字 RAG 提升 20-40%，特别是包含图表、复杂排版的文档。

## 与主线的关系

> 视觉 RAG 的标志性工作，将 RAG 框架完全多模态化（从文字检索+文字生成 → 图像检索+图像生成）。对于组内工具调用/Agent 方向，理解 RAG 如何在多模态场景工作是重要基础知识。

- 相关：[[M3-07]] 多模态 RAG 综述（全景），[[M3-05]] MemVerse（记忆 vs 检索）

## 是否值得回头精读

- [x] 是，原因：如果组内有工作涉及多模态文档问答或视觉知识检索，这篇是必读的方法论基础
- [ ] 否，原因：

## 快速笔记

- 核心创新：RAG 流程完全保持视觉模态（不转文字），避免 OCR 错误尤其对复杂文档
- 视觉嵌入：用 VLM 的视觉编码器对文档页面图像生成向量，检索时比较向量相似度
- 关键结果：在含图表/排版的文档上 20-40% 提升；纯文字文档上提升较小
