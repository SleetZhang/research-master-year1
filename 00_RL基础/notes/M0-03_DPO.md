---
title: "Direct Preference Optimization: Your Language Model is Secretly a Reward Model"
authors: "Rafailov et al."
venue: NeurIPS
year: 2023
arxiv_id: "2305.18290"
reading_depth: 精读
status: 待读
tags:
  - RL基础/DPO
  - 后训练/偏好对齐
prerequisites:
  - "[[M0-02_InstructGPT]]"
estimated_hours: 12
---

# DPO 精读笔记

## 核心问题

> DPO 如何从 RLHF 优化目标用数学推导得出闭式 loss，从而绕开独立 RM 训练和 PPO 迭代？Bradley-Terry 模型和 KL 约束在推导中各自扮演什么角色？

## 动机与背景

<!-- RLHF 的三阶段流程繁琐，RM 训练和 PPO 都不稳定。能否直接从偏好数据一步到位训练 policy？ -->

## 方法：从 RLHF 目标到 DPO Loss 的完整推导

### Step 1：RLHF 优化目标

$$\max_\pi \mathbb{E}_{x \sim D, y \sim \pi}[r(x, y)] - \beta \cdot \text{KL}(\pi(y|x) \| \pi_\text{ref}(y|x))$$

<!-- 两项的含义：最大化奖励 vs 不要偏离 reference model 太远 -->

### Step 2：闭式最优解

$$\pi^*(y|x) = \frac{1}{Z(x)} \pi_\text{ref}(y|x) \exp\left(\frac{r(x,y)}{\beta}\right)$$

<!-- Z(x) 是归一化常数（配分函数），使 π* 成为合法概率分布 -->

### Step 3：从最优 policy 反解奖励函数

$$r(x, y) = \beta \log \frac{\pi^*(y|x)}{\pi_\text{ref}(y|x)} + \beta \log Z(x)$$

<!-- 关键：Z(x) 只依赖 x，对于同一个 prompt 的不同 y，Z(x) 相同 -->

### Step 4：代入 Bradley-Terry 模型，Z(x) 消去

Bradley-Terry 偏好概率：$p^*(y_w \succ y_l | x) = \sigma(r(x, y_w) - r(x, y_l))$

代入 Step 3 的反解表达式，$\beta \log Z(x)$ 相减消去：

$$\mathcal{L}_\text{DPO} = -\mathbb{E}\left[\log \sigma\left(\beta \log \frac{\pi_\theta(y_w|x)}{\pi_\text{ref}(y_w|x)} - \beta \log \frac{\pi_\theta(y_l|x)}{\pi_\text{ref}(y_l|x)}\right)\right]$$

**DPO 的优雅之处**：policy 自身就是隐式的奖励函数，不需要单独训练 RM。

### DPO vs RLHF 对比

| 方面 | RLHF (InstructGPT) | DPO |
|------|-------------------|-----|
| 需要 RM | 是 | 否（隐式） |
| 需要 PPO | 是 | 否 |
| On/Off policy | On-policy | Off-policy |
| 训练稳定性 | 低 | 高 |
| 理论保证 | 启发式 | 最优性有理论保证 |

## 关键实验

| 任务 | DPO vs PPO |
|------|-----------|
| 情感控制（IMDb）| DPO 相当或更好 |
| 摘要（TL;DR）| DPO 相当 |
| 对话（Anthropic HH）| DPO 相当 |

## 局限

<!-- Off-policy 问题：偏好数据由 π_ref 生成，当 π_θ 更新后可能与偏好数据分布不匹配 -->
<!-- 无法处理 length bias（倾向于选短的 y_l 或长的 y_w）-->
<!-- 多模态场景中的 unconditional preference 问题（见 [[M2-04_mDPO]]）-->

## 个人思考

<!-- 推导中最关键的洞察是什么？Z(x) 消去的条件（同 prompt 比较）意味着什么限制？ -->
<!-- DPO loss 的梯度方向：增大 chosen 的对数概率比，减小 rejected 的对数概率比 -->

## 与其他论文的联系

- [[M0-02_InstructGPT]] — DPO 从本文的 RLHF 目标推导而来，必须先读
- [[M2-02_RLHF-V]] — 多模态场景 DPO 的首个重要应用（dense segment-level DPO）
- [[M2-04_mDPO]] — 发现多模态 DPO 失效原因（unconditional preference）并修复
- [[M2-05_RLAIF-V]] — DPO 路线的 AI feedback 扩展
- [[M2-06_Calibrated-Self-Rewarding]] — 带视觉校准的 DPO 变体
