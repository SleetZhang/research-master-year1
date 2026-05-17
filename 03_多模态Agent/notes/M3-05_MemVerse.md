---
title: "MemVerse: Multimodal Memory for Lifelong Learning Agents"
authors: "Yequan Bie, Xinyu Zhu, Xintao Wang, Yuqing Yang, Weibin Li, Zhangwei Gao, Li Xue, Yang Song, Sibo Song, Ying Shan"
venue: "arXiv 2025"
year: 2025
arxiv_id: "2512.03627"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/Agent/记忆"
  - "#多模态/Agent/长期交互"
prerequisites:
  - "[[M3-01]]"
---

## 核心贡献（3 句话）

1. **做了什么**：提出 MemVerse，一个模型无关（model-agnostic）的多模态记忆框架，结合短期参数化记忆和长期层次化知识图谱记忆，支持多模态 Agent 的终身学习。
2. **怎么做的**：双层记忆架构：(1) 短期记忆：近期上下文的快速参数化召回；(2) 长期记忆：将多模态经历（图文）转化为结构化的层次知识图谱（entity-centric, multi-hop 可访问）；两层之间通过"重要性感知"的记忆迁移机制协作。
3. **效果如何**：在长期多模态对话和知识积累 benchmark 上超越基线记忆方法，在跨轮次信息回忆准确率上显著提升。

## 与主线的关系

> 多模态记忆机制的代表性工作，与 RAG（[[M3-06]]）形成互补——RAG 是推理时检索外部知识，Memory 是积累和管理对话历史/个性化知识。了解这个维度有助于思考组内工作与记忆/RAG 的结合点。

- 相关：[[M3-06]] VisRAG（外部知识检索 vs 内部记忆积累）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：记忆机制是次要方向，粗读了解框架设计即可

## 快速笔记

- 双层记忆：短期（近期上下文，快速） + 长期（历史经验，层次知识图谱）
- 知识图谱：以实体（entity）为中心，多模态属性（图像、文字）挂载在节点上
- 记忆迁移：根据信息重要性和访问频次，将短期记忆升级为长期记忆
