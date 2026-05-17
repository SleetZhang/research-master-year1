---
title: "Training language models to follow instructions with human feedback"
authors: "Ouyang et al."
venue: NeurIPS
year: 2022
arxiv_id: "2203.02155"
reading_depth: 精读
status: 待读
tags:
  - RL基础/RLHF
  - RL基础/PPO
  - 后训练/偏好对齐
  - 奖励模型
prerequisites:
  - "[[M0-01_PPO教程]]"
estimated_hours: 12
---

# InstructGPT 精读笔记

## 核心问题

> InstructGPT 如何用 RLHF + PPO 让语言模型"听指令"？四个组件（SFT / RM / actor / reference）各自的职责是什么？

## 动机与背景

<!-- 为什么需要 RLHF？预训练 LM 的 alignment 问题是什么？ -->

## 方法

### 三阶段流程

<!-- Stage 1: SFT（监督微调，人类示范数据）
     Stage 2: RM 训练（Bradley-Terry，偏好对数据）
     Stage 3: PPO 对 RM 进行 RL 优化 -->

### 四个组件职责

| 组件 | 初始化 | 是否更新 | 作用 |
|------|--------|---------|------|
| SFT model | 预训练 LM | 阶段 1 更新，之后固定 | 策略初始化 + reference model |
| Reward model | SFT model | 阶段 2 更新，之后固定 | 给每个 (prompt, response) 打分 |
| Actor (policy) | SFT model | 阶段 3 持续更新 | 被优化的目标模型 |
| Reference model | SFT model | 永远固定 | 提供 KL 基准，防止 reward hacking |

### 目标函数

$$r(x, y) = r_\phi(x, y) - \beta \cdot \text{KL}(\pi_\theta(y|x) \| \pi_\text{ref}(y|x))$$

<!-- β 的取值？KL 惩罚的必要性：防止 policy 脱离合理分布 -->

### Bradley-Terry 奖励模型

<!-- 偏好对 (y_w, y_l) 的损失函数形式 -->

$$\mathcal{L}_\text{RM} = -\mathbb{E}_{(x, y_w, y_l)}\left[\log \sigma(r_\phi(x, y_w) - r_\phi(x, y_l))\right]$$

## 关键实验

| 实验 | 结论 |
|------|------|
| 人类偏好评估 | 1.3B InstructGPT > 175B GPT-3 |
| Reward hacking | 无 KL penalty 时 RM 得分上升但人类评分下降 |
| Alignment tax | PPO-ptx（混合预训练数据）缓解 NLP benchmark 退化 |

## 局限

<!-- 偏好数据质量依赖标注者，RM 会被 hacking，PPO 训练不稳定 -->

## 个人思考

<!-- 与 DPO 对比：DPO 绕开了 RM 和 PPO，但本质上 InstructGPT 的四组件是理解 DPO 推导的前提 -->

## 与其他论文的联系

- [[M0-01_PPO教程]] — PPO 算法基础，必须先完成
- [[M0-03_DPO]] — DPO 是对本文 RLHF 流程的数学简化，从本文的优化目标推导
- [[M2-01_LLaVA-RLHF]] — 本文在多模态场景的直接扩展，加入 Factually Augmented RM
