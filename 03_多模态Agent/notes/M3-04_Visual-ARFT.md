---
title: "Visual Agentic Reinforcement Fine-Tuning"
authors: "Ziyu Liu, Yuhang Zang, Haodong Duan, Shengjie Wang, Wei Li, Bo Zhang, Xiaoyi Dong, Bing Su, Pan Zhang, Dahua Lin, Jiaqi Wang"
venue: "arXiv 2025"
year: 2025
arxiv_id: "2505.14246"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/Agent/工具调用"
  - "#多模态/推理/RL强化学习"
prerequisites:
  - "[[M2-12]]"
  - "[[M3-03]]"
---

## 核心贡献（3 句话）

1. **做了什么**：提出 Visual-ARFT，将强化微调（RFT）扩展到多模态 Agent 场景，让 VLM 通过 RL 学习推理分解、工具调用和结果验证的完整 Agent 行为。
2. **怎么做的**：定义 Agent 任务的可验证奖励（工具调用正确性 + 任务最终完成率），用 GRPO 框架训练 VLM 在完成长任务链的过程中进行工具选择和调用；同时引入 \<search\>、\<code\> 等特殊动作 token。
3. **效果如何**：在多步视觉 Agent 任务上显著优于 SFT 基线（Visual-RFT），特别是在复杂多步推理任务上体现出 RL 的优势。

## 与主线的关系

> Visual-ARFT 是 Visual-RFT（[[M2-12]]）在 Agent 场景的扩展，代表了"RL 赋能多模态 Agent"的路线。对于组内工具调用研究，了解 RL 如何用于 Agent 训练有重要参考价值。

- 依赖于：[[M2-12]] Visual-RFT（GRPO + 视觉奖励框架）
- 对照：[[M3-03]] Multi-modal Agent Tuning（SFT 路线 vs RL 路线）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：主要是 Visual-RFT 在 Agent 场景的应用扩展，核心 RL 机制已在 [[M2-12]] 覆盖

## 快速笔记

- 核心扩展：在 Visual-RFT 的视觉奖励之上，增加工具调用序列的过程奖励（每步工具是否正确调用）
- Agent 动作空间：\<think\> 推理、\<search\> 检索、\<code\> 执行代码、\<answer\> 最终答案
- 关键结果：RL 在长任务链（5+ 步）上的优势远大于短任务（SFT 在短任务上已经够用）
