---
title: "mDPO: Conditional Preference Optimization for Multimodal Large Language Models"
authors: "Fei Wang, Wenxuan Zhou, James Y. Huang, Nan Xu, Sheng Zhang, Hoifung Poon, Muhao Chen"
venue: "EMNLP 2024"
year: 2024
arxiv_id: "2406.11839"
reading_depth: 精读
status: 未读
tags:
  - "#多模态/后训练/DPO"
  - "#多模态/幻觉/缓解"
  - "#多模态/后训练/偏好对齐"
prerequisites:
  - "[[M2-02]]"
  - "[[M2-03]]"
---

## 核心问题

> 为什么直接将标准 DPO 应用于多模态 LLM 往往无法带来一致的性能提升？根本问题是什么？

## 动机

- 多个工作（Silkie、RLHF-V）将 DPO 应用于多模态，但效果不稳定，有时甚至下降
- 推测原因：多模态 DPO 存在一个文本 DPO 没有的独特问题
- 需要诊断问题并提出有针对性的解决方案

## 方法

### 总体框架

通过对照实验发现标准 DPO 在多模态场景的"无条件偏好问题"（unconditional preference），并提出 mDPO 通过图像条件约束和奖励锚定来解决这一问题。

### 关键模块

1. **问题诊断：Unconditional Preference Problem**（对应论文 Section 3）
   - 标准 DPO 的偏好优化主要基于文本信号，模型可能学会忽略图像条件
   - 测试：把 chosen 回答的图像替换为随机图像，DPO 优化后模型仍然偏好 chosen 回答
   - 结论：DPO 优化了"无条件文本偏好"而非"图像条件下的视觉忠实性"

2. **mDPO 解决方案**：
   - **图像条件约束**：同时优化图像偏好（chosen 图像 > rejected 图像），强制模型关注图像
   - **奖励锚定（Reward Anchor）**：添加约束确保 chosen 回答的奖励为正（防止 chosen 概率下降）

3. **mDPO 目标函数**（对应论文 Eq.5-7）

### 关键公式 / 算法

mDPO 在标准 DPO 基础上添加两项约束：

$$\mathcal{L}_\text{mDPO} = \mathcal{L}_\text{DPO}(y_c^t, y_r^t | x) + \lambda_1 \mathcal{L}_\text{image}(v_c, v_r | q, y) + \lambda_2 \mathcal{L}_\text{anchor}(y_c | x)$$

其中图像偏好 loss：$\mathcal{L}_\text{image} = -\log \sigma(r(q,v_c,y) - r(q,v_r,y))$

奖励锚定 loss：$\mathcal{L}_\text{anchor} = \max(0, \epsilon - r(q,v_c,y_c))$（确保 chosen 奖励 > $\epsilon$）

## 关键实验

| 实验 | 数据集 / 设置 | 主要结论 |
|------|--------------|---------|
| 问题诊断 | 替换图像实验 | 标准 DPO 在 image-swapped 设置下仍偏好 chosen，证明 unconditional preference 存在 |
| mDPO vs DPO | MMHal, POPE, MMStar | mDPO 在所有 benchmark 上超越标准 DPO |
| 消融 | 图像约束 vs 锚定 | 两者均重要，图像约束贡献更大 |
| 基座模型 | LLaVA-1.5-7B/13B | 在不同规模模型上效果一致 |

**最重要的一个数字结果**：在 POPE 幻觉基准上，mDPO 将幻觉率降低至 87.2%（标准 DPO 仅 85.6%，LLaVA 基线 86.5%）。

## 局限

- 图像偏好数据构建需要专门的负样本（随机图像/相似图像），增加数据准备复杂度
- 奖励锚定超参数 $\epsilon$ 的设定需要调优
- 仅验证了两个基座模型，泛化性有待更广泛验证

## 个人思考

**方法的核心 insight 是**：多模态 DPO 不能简单沿用文本 DPO——必须显式地让优化过程感知图像条件，否则模型会走"文本捷径"。这是多模态偏好学习独有的挑战。

**与我研究方向的关联**：mDPO 揭示了一个后续所有多模态偏好对齐工作都必须考虑的根本性问题。它改变了领域内对"直接用 DPO 做多模态对齐"的天真假设。很多后续工作（包括 RLAIF-V）都借鉴了这个视角。

**延伸问题 / 可能的改进方向**：
- 除了图像偏好约束，还有哪些方式让 DPO 感知多模态条件？
- 这个问题在推理增强场景（如 Visual-RFT）同样存在吗？→ 参见 [[M2-12]]

## 与其他论文的联系

- 继承自：DPO，[[M2-02]] RLHF-V，[[M2-03]] Silkie
- 被以下工作引用：[[M2-05]] RLAIF-V，[[M2-06]] Calibrated Self-Rewarding
- 同期对比：[[M2-03]] Silkie（标准 DPO 的局限性分析）
