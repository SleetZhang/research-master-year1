---
title: "MRAG-Bench: Vision-Centric Evaluation for Retrieval-Augmented Multimodal Models"
authors: "Hu et al."
venue: ICLR
year: 2025
arxiv_id: "2410.08182"
reading_depth: 粗读
status: 待读
tags:
  - 多模态/Agent/RAG
  - 多模态/评测/RAG基准
prerequisites:
  - "[[M3-06_VisRAG]]"
  - "[[M3-10_ColPali]]"
estimated_hours: 2.5
---

# MRAG-Bench 粗读笔记

## 核心贡献（3 句话）

1. 提出 MRAG-Bench：第一个视觉中心的多模态 RAG 评测基准，专门测试模型在检索到视觉知识后能否正确利用（而非仅评测文本RAG），涵盖 16 个场景、1,375 个问题。
2. 核心发现：现有 MLLM 即使检索到了正确的视觉信息，利用率仍不高（不善于利用检索图像来增强答案），揭示了多模态 RAG 的关键瓶颈不在检索，而在"视觉知识利用"。
3. ICLR 2025，为 ColPali、VisRAG 等检索方法提供了统一的评测标准，推动了视觉 RAG 评测的标准化。

## 与主线的关系

- **多模态 RAG 方向的评测基准**，作为 VisRAG（[[M3-06_VisRAG]]）和 ColPali（[[M3-10_ColPali]]）的评测工具
- 关键发现"检索对了但没用好"对后续研究有重要指导：说明视觉知识的理解和整合是核心问题，与后训练（多模态 SFT/RLHF）直接相关
- 理解 MRAG-Bench 有助于识别多模态 RAG 研究的真正瓶颈

## 值得继续精读吗？

**否**。评测基准论文，粗读了解评测维度和核心发现即可。

## 快速记录

<!-- MRAG-Bench 的 16 个场景具体是什么类型？（知识密集型 vs 推理密集型 vs 感知密集型）
     "视觉知识利用率"如何量化？与纯文字 RAG 的性能差距？
     哪些模型在 MRAG-Bench 上表现最好？闭源模型（GPT-4V）vs 开源模型的差距？ -->
