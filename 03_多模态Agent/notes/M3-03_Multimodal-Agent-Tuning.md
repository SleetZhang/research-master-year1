---
title: "Multi-modal Agent Tuning: Building a VLM-Driven Agent for Efficient Tool Usage"
authors: "Zhi Gao, Bofei Zhang, Pengxiang Li, Xiaojian Ma, Tao Yuan, Yuwei Wu, Yunde Jia, Song-Chun Zhu, Qing Li"
venue: "ICLR 2025"
year: 2025
arxiv_id: "2412.15606"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/Agent/工具调用"
  - "#多模态/后训练/SFT"
prerequisites:
  - "[[M3-02]]"
---

## 核心贡献（3 句话）

1. **做了什么**：提出多模态 Agent 微调方法（MAT），自动化生成多模态工具调用数据，并用于微调 VLM 作为 Agent 控制器，提升工具调用推理能力。
2. **怎么做的**：多阶段数据生成：先用 VLM 生成工具调用规划，再用外部 API 执行验证，保留成功的工具调用轨迹作为训练数据；最后 SFT 微调 VLM 学习工具选择和参数推理。
3. **效果如何**：在 ToolBench 和自建多模态工具基准上显著优于未微调的 VLM，证明自动化数据生成 + SFT 是使 VLM 具备工具调用能力的有效路径。

## 与主线的关系

> 展示了如何用 SFT 赋予 VLM 工具调用能力的工程范式，是 [[M3-04]] Visual-ARFT（RL路线）的对照参考。

- 依赖于：[[M3-02]] MLLM-Tool（工具调用基础框架）
- 对照：[[M3-04]] Visual-ARFT（SFT vs RL 路线）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：工程实现性论文，核心思路（自动数据生成+SFT）粗读已能理解

## 快速笔记

- 自动化数据生成：VLM 生成规划 → 外部执行 → 验证 → 保留成功轨迹
- 多模态场景：工具调用需要同时理解图像（如"识别图中的商品"）和文字指令
- 训练效率：自动生成数据比人工标注轨迹可扩展性更强
