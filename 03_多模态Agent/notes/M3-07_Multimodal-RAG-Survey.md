---
title: "A Survey of Multimodal Retrieval-Augmented Generation"
authors: "Yujian Betterest Li, Kai Liu"
venue: "arXiv 2025"
year: 2025
arxiv_id: "2504.08748"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/Agent/RAG"
  - "#多模态/Agent/综述"
prerequisites:
  - "[[M3-06]]"
---

## 核心贡献（3 句话）

1. **做了什么**：系统综述多模态 RAG 领域的最新进展，涵盖检索策略、生成策略、评测基准和应用场景。
2. **怎么做的**：对比文字 RAG vs 多模态 RAG 的差异，分析跨模态检索（文→图、图→文、图→图）的技术路线，综述代表方法（VisRAG、mR²AG、MRAMG 等）。
3. **效果如何**：2025年综述，提供多模态 RAG 的全景地图，是快速建立领域认知的最高效入口。

## 与主线的关系

> 与 [[M3-06]] VisRAG 配合读：VisRAG 是具体方法，本综述给出全景地图。在阅读 VisRAG 后读此综述，能了解 VisRAG 在整个 RAG 生态中的位置。

- 依赖于：[[M3-06]] VisRAG（具体方法理解）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：综述类论文，快速建立 RAG 地图即可

## 快速笔记

- 多模态 RAG 4 类检索：文本检索图像、图像检索文本、图像检索图像、跨模态混合检索
- 关键挑战：跨模态语义对齐（不同模态的向量空间不一致）
- 前沿趋势：Reflection 机制（按需检索，而非全文档检索）；图像-文本交织的生成（MRAMG）
