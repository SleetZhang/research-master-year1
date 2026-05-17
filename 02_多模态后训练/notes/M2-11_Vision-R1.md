---
title: "Vision-R1: Incentivizing Reasoning Capability in Multimodal Large Language Models"
authors: "Wenyi Hong, Weihan Wang, Qingsong Lv, Ji Qi, Yan Wang, Zheng Zhang, Ji Qi, Shuai Bi, Lei Zhao, Xu Zhang, Wei Xu, Wendi Zheng, Yuxiao Dong, Jie Tang"
venue: "arXiv 2025"
year: 2025
arxiv_id: "2503.06749"
reading_depth: 精读
status: 未读
tags:
  - "#多模态/推理/RL强化学习"
  - "#多模态/推理/GRPO"
  - "#多模态/推理/o1风格"
prerequisites:
  - "[[M2-09]]"
  - "[[M2-10]]"
---

## 核心问题

> 如何将 DeepSeek-R1 风格的强化学习（GRPO + 可验证奖励）扩展到多模态推理，并解决多模态场景特有的训练挑战？

## 动机

- DeepSeek-R1 证明 RL + 可验证奖励可以在纯文本数学推理上超越 o1-mini，但多模态场景尚未探索
- 直接将文本 R1 方法搬到多模态面临独特挑战：(1) 多模态训练数据少；(2) 模型容易"过度思考"（冷启动问题）；(3) 视觉和语言推理需要协同
- 需要专门的多模态 CoT 数据和训练策略

## 方法

### 总体框架

两阶段训练：(1) 冷启动初始化（用 200K 多模态 CoT 数据 SFT）；(2) GRPO RL 训练（用 10K 可验证数学问答 + 规则奖励优化推理能力）。同时提出 PTST（Progressive Thinking Suppression Training）解决过度思考问题。

### 关键模块

1. **200K 多模态 CoT 数据集构建**：
   - 从现有 MLLM 和 DeepSeek-R1（文本推理模型）通过模态桥接生成多模态 CoT
   - 过滤：只保留推理链导向正确答案的数据
   - 格式：<think> ... </think> <answer> ... </answer>

2. **GRPO 训练**：
   - 对每个问题采样 $G$ 个推理链 $\{o_1, ..., o_G\}$
   - 奖励函数：规则验证答案正确性（格式奖励 + 正确性奖励）
   - 组相对优势：$A_i = (r_i - \bar{r}) / \text{std}(r)$

3. **PTST（Progressive Thinking Suppression Training）**：
   - 冷启动阶段模型可能生成过长的 \<think\> 段（"overthinking"）
   - 渐进式在训练后期惩罚过长思维链，鼓励简洁有效的推理

4. **可验证奖励**：
   - 数学题：答案是否与 GT 匹配（格式 + 数值）
   - 格式奖励：是否符合 \<think\>\</think\>\<answer\>\</answer\> 格式

### 关键公式 / 算法

GRPO 目标函数（对应论文 Eq.2）：

$$\mathcal{L}_\text{GRPO} = -\mathbb{E}_{q,\{o_i\}_{i=1}^G}\left[\frac{1}{G}\sum_{i=1}^G \frac{\pi_\theta(o_i|q)}{\pi_\text{ref}(o_i|q)} A_i - \beta \text{KL}(\pi_\theta || \pi_\text{ref})\right]$$

## 关键实验

| 实验 | 数据集 / 设置 | 主要结论 |
|------|--------------|---------|
| 主实验（7B）| MathVista | Vision-R1-7B 达 73.5%（vs OpenAI o1 的 73.9%）|
| 大模型 | MathVista (32B/72B) | Vision-R1-72B 达 78.2% |
| PTST 消融 | 思维链长度 vs 准确率 | PTST 减少无效推理，提升 efficiency |
| CoT 数据消融 | 有无冷启动 | 冷启动初始化是关键，无冷启动 GRPO 不稳定 |

**最重要的一个数字结果**：Vision-R1-7B 在 MathVista 上达到 73.5%，与 OpenAI o1（73.9%）持平，证明开源 RL 推理可以达到商业 o1 水平。

## 局限

- 当前仅在数学推理数据上验证，可验证奖励难以推广到开放域任务
- GRPO 训练稳定性对超参数敏感（组大小 G、KL 系数 β）
- 冷启动数据质量直接影响 RL 稳定性

## 个人思考

**方法的核心 insight 是**：冷启动（有质量的 CoT SFT）对于多模态 RL 推理至关重要——与文本 R1 不同，多模态模型从头用 RL 探索推理格式非常不稳定。"SFT 启动 + RL 精调"是更可靠的路径。

**与我研究方向的关联**：Vision-R1 是多模态 RL 推理增强的标志性工作，确立了"GRPO + 可验证奖励 + 冷启动"的三元素训练框架。后续 Visual-RFT（[[M2-12]]）、VisualPRM（[[M2-13]]）等都在此基础上扩展。

**延伸问题 / 可能的改进方向**：
- 如何将 GRPO 扩展到非数学、非验证型任务？→ 参见 [[M2-12]] Visual-RFT（视觉感知任务）
- 过程奖励 vs 结果奖励，哪个更有效？→ 参见 [[M2-13]] VisualPRM

## 与其他论文的联系

- 继承自：DeepSeek-R1（GRPO + 可验证奖励），[[M2-09]] LLaVA-CoT（CoT 数据格式）
- 被以下工作改进：[[M2-12]] Visual-RFT（扩展到感知任务），[[M2-14]] R1-OneVision（跨模态泛化）
- 同期对比：[[M2-10]] AtomThink（PRM 路线 vs GRPO 路线）
