---
title: "Large Multimodal Agents: A Survey"
authors: "Junlong Li, Shicheng Liu, MohammadSaleh Pezeshkpour, Xinrui He, Yizheng Sun, Namo Bhansali, Kai Zhang, Hao Su, Sixin Xu, Hao Guo, Mingjie Sun, Boming Yang, Shengding Hu, Bowen Jin, Zhuoran Yang, ..."
venue: "Visual Intelligence (Springer) 2025"
year: 2025
arxiv_id: "N/A"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/Agent/综述"
  - "#多模态/Agent/工具调用"
  - "#多模态/Agent/记忆"
prerequisites:
  - "[[M1-06]]"
---

## 核心贡献（3 句话）

1. **做了什么**：系统综述多模态 Agent 领域，涵盖感知、规划、记忆、工具使用和执行等核心能力维度。
2. **怎么做的**：对 100+ 多模态 Agent 工作进行分类和对比，梳理从早期工具调用到具身 Agent 的演化路径。
3. **效果如何**：提供多模态 Agent 全景地图，是快速了解 Agent 领域结构的最佳综述之一（2025）。

## 与主线的关系

> Agent 模块的起点论文。先读综述建立 Agent 的整体框架认知（记忆/工具/RAG 的位置和关系），再选择性深入 [[M3-02]]~[[M3-07]] 的具体工作。

- 相关：[[M3-02]] MLLM-Tool，[[M3-05]] MemVerse，[[M3-06]] VisRAG

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：综述论文，快速建立地图即可，不需要深入每个方向

## 快速笔记

- Agent 核心能力框架：感知（Perception）→ 规划（Planning）→ 记忆（Memory）→ 工具（Tool Use）→ 执行（Execution）
- 与主线相关的子方向：工具调用（Function Calling）、外部记忆（RAG/Memory）
- **本学习计划排除**：具身 Agent（Embodied）、GUI Agent——请重点关注工具调用和记忆章节
