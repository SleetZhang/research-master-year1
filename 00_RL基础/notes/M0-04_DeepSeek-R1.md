---
title: "DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning"
authors: "DeepSeek-AI"
venue: arXiv
year: 2025
arxiv_id: "2501.12948"
reading_depth: 粗读
status: 待读
tags:
  - RL基础/GRPO
  - 后训练/推理增强
prerequisites:
  - "[[M0-02_InstructGPT]]"
estimated_hours: 3
---

# DeepSeek-R1 粗读笔记

## 核心贡献（3 句话）

1. 提出 GRPO（Group Relative Policy Optimization），通过 group-relative advantage 估计替代 PPO 的 value function（critic），大幅降低推理计算开销。
2. 结合冷启动（少量长 CoT 数据 SFT）+ GRPO RL 训练，在数学、代码等可验证奖励任务上达到 OpenAI o1 水平。
3. GRPO 去掉了 critic 网络，使 RL 训练只需 actor + reference model，工程上更简洁。

## GRPO 与 PPO 的核心差异

| 组件 | PPO | GRPO |
|------|-----|------|
| Critic 网络 | 需要（与 actor 同规模） | **不需要** |
| Advantage 估计 | GAE（需要 value function）| group-relative：组内归一化 |
| 采样方式 | 单次采样 | 同一 prompt 采 G 个输出 |
| 适用场景 | 通用 RL | 有可验证奖励的推理任务 |

**Group-relative advantage 计算**：对 prompt $x$ 采样 $G$ 个输出 $\{y_i\}_{i=1}^G$，各自获得 reward $\{r_i\}$：

$$\hat{A}_i = \frac{r_i - \text{mean}(\{r_j\})}{\text{std}(\{r_j\})}$$

不需要 value function，直接用同组其他输出做基线（baseline）。

## 与主线的关系

- 本文的 GRPO 是 [[M2-11_Vision-R1]] 和 [[M2-12_Visual-RFT]] 的算法基础
- 读本文只需要掌握 GRPO 原理（论文前 2-3 节），不需要深入 DeepSeek 全套训练细节
- GRPO 的可验证奖励范式（correctness reward + format reward）直接被多模态论文借用

## 值得继续精读吗？

**否**。粗读掌握 GRPO 原理即可，全文技术细节（蒸馏策略、scaling 实验）对多模态后训练研究意义有限。GRPO 的多模态应用见 [[M2-11_Vision-R1]]（更贴近主线）。

## 快速记录

<!-- PPO 为什么需要 critic？GAE（Generalized Advantage Estimation）的计算过程 -->
<!-- GRPO 的 group 大小 G 一般取多少？对 reward variance 的影响 -->
<!-- DeepSeek-R1 的冷启动策略：为什么不能直接从 base model 做 RL？-->
