---
title: "HippoRAG: Neurobiologically Inspired Long-Term Memory for LLMs"
authors: "Guo et al."
venue: NeurIPS
year: 2024
arxiv_id: "2405.14831"
reading_depth: 粗读
status: 待读
tags:
  - 多模态/Agent/记忆
  - 多模态/Agent/知识图谱
  - 记忆架构/海马体
prerequisites:
  - "[[M3-01_Large-Multimodal-Agents-Survey]]"
estimated_hours: 2.5
---

# HippoRAG 粗读笔记

## 核心贡献（3 句话）

1. 受海马体记忆机制启发，提出 HippoRAG：用知识图谱模拟海马体的"索引"功能，用 LLM 模拟皮层"处理"功能，实现跨文档的关联推理检索，而非传统 RAG 的局部向量匹配。
2. 核心创新：将文档分解为三元组（实体-关系-实体）构建知识图谱，检索时通过 Personalized PageRank 算法在图上传播，找到跨段落相关联的信息。
3. NeurIPS 2024，在多跳问答（MuSiQue, 2WikiMultiHopQA）等需要跨文档推理的任务上显著优于传统 RAG 和 GraphRAG。

## 与主线的关系

- **Memory 方向的基础架构论文**，为 MemVerse（[[M3-05_MemVerse]]）等多模态记忆工作提供架构参考
  - HippoRAG：LLM + 知识图谱记忆，主要针对文本
  - MemVerse：多模态双层记忆 + 层次知识图谱，扩展到图像/视频场景
- 理解 HippoRAG 的 Personalized PageRank 检索思路，有助于理解为什么多模态记忆需要图结构（而不是简单的向量检索）
- **注**：HippoRAG 是纯文本 RAG，与多模态场景有一定距离，读时注意关注"思想借鉴"而非直接方法迁移

## 值得继续精读吗？

**否**。HippoRAG 作为 Memory 方向的基础认知够用，粗读掌握海马体架构比喻和 Personalized PageRank 检索即可。若做 LLM 长期记忆研究，可精读。

## 快速记录

<!-- Personalized PageRank 在知识图谱上的传播过程（从检索节点出发，按图结构传播）
     三元组构建用的是 LLM 提取还是规则？准确率如何？
     与 GraphRAG（Microsoft）的区别：HippoRAG 更关注索引结构，GraphRAG 更关注社区摘要
     HippoRAG 2.0（如果有）是否扩展到多模态？ -->
