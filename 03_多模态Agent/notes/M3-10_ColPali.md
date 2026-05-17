---
title: "ColPali: Efficient Document Retrieval with Vision Language Models"
authors: "Faysse et al."
venue: ICLR
year: 2025
arxiv_id: "2407.01449"
reading_depth: 粗读
status: 待读
tags:
  - 多模态/Agent/文档检索
  - 多模态/Agent/RAG
  - 架构/ColBERT风格
prerequisites:
  - "[[M3-06_VisRAG]]"
  - "[[M3-11_MRAG-Bench]]"
estimated_hours: 2.5
---

# ColPali 粗读笔记

## 核心贡献（3 句话）

1. 提出 ColPali：将 ColBERT 的后期交互（Late Interaction）检索范式扩展到多模态场景，用 PaliGemma（VLM）直接对文档页面图像生成多向量表示，实现高效且准确的视觉文档检索，无需 OCR 或图像解析预处理。
2. 核心创新：文档页面作为图像直接输入 VLM，生成 patch-level 多向量嵌入；查询也生成多向量；检索时用 MaxSim 操作（每个 query token 与所有 doc token 的最大相似度之和），兼顾精度和速度。
3. ICLR 2025，在 DocVQA、InfoVQA 等文档检索基准上显著超越 OCR+BM25 和单向量视觉检索，建立了新的视觉文档检索 SOTA。

## 与主线的关系

- **文档检索方向**，与 VisRAG（[[M3-06_VisRAG]]）形成对比：
  - ColPali：高效的 patch-level 多向量检索（ColBERT 风格），侧重检索效率和准确性
  - VisRAG：VLM 直接做页面级别的检索+生成，侧重端到端生成质量
  - 两者可以结合：ColPali 做检索，VisRAG 风格做生成
- MRAG-Bench（[[M3-11_MRAG-Bench]]）可以作为评测两种方法的统一基准
- ColPali 的 "文档图像 → patch 多向量" 思路对多模态 RAG 设计有启发

## 值得继续精读吗？

**否**。ColPali 的 ColBERT 扩展思路粗读即可理解；实现细节（PaliGemma 微调、MaxSim 加速）对当前阶段意义有限。

## 快速记录

<!-- ColBERT 的 Late Interaction 是什么？为什么比单向量检索准确？
     PaliGemma 如何对文档页面生成 patch 多向量？每页多少向量？
     检索速度：与 BM25、DPR 相比如何？能否做 ANN 加速？
     ColPali 是否处理了跨页检索的问题？ -->
