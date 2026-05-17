---
title: "VisualPRM: An Effective Process Reward Model for Multimodal Reasoning"
authors: "Weiyun Wang, Zhangwei Gao, Lianjie Huang, Zhe Chen, Jinguo Zhu, Wenhai Wang, Jifeng Dai, Yu Qiao, Xizhou Zhu"
venue: "arXiv 2025"
year: 2025
arxiv_id: "2503.10291"
reading_depth: 精读
status: 未读
tags:
  - "#多模态/推理/PRM"
  - "#多模态/推理/测试时计算"
  - "#多模态/推理/数学推理"
prerequisites:
  - "[[M2-10]]"
  - "[[M2-11]]"
---

## 核心问题

> 如何构建有效的多模态过程奖励模型（PRM），并用其在测试时计算（Best-of-N）中显著提升多模态推理性能？

## 动机

- AtomThink（[[M2-10]]）和 LLaVA-CoT（[[M2-09]]）证明过程监督有价值，但缺乏规模化的多模态 PRM
- 文本领域的 PRM 依赖大量人工过程标注（如 PRM800K），这在多模态领域成本更高
- 需要自动化的多模态过程监督数据构建方法

## 方法

### 总体框架

自动化构建 VisualPRM400K 过程监督数据集，训练 VisualPRM-8B，支持 Best-of-N（BoN）和 Tree Search 两种测试时计算策略，覆盖多模态数学和科学推理。

### 关键模块

1. **VisualPRM400K 数据集构建**（对应论文 Sec.3）：
   - 从多个多模态数学数据集（MathVista、MathVerse、GeoQA 等）收集问答对
   - 用多个 MLLM 生成多条推理链（蒙特卡洛采样）
   - 蒙特卡洛估计步骤正确性：$Q(s_k) = \frac{\text{# correct final answers when starting from } s_k}{\text{# total rollouts from } s_k}$
   - 按 $Q(s_k)$ 阈值标注每步为正确（+）或错误（-）

2. **VisualPRM-8B 训练**：
   - 基于 InternVL 2.5-8B 微调
   - 输入：图像 + 问题 + 前 $k$ 步推理链
   - 输出：步骤 $k$ 正确性的 binary 分类

3. **测试时计算策略**：
   - **Best-of-N**：生成 $N$ 个完整推理链，用 PRM 对每链打总分（步骤分数乘积），选最高分
   - **Beam Search with PRM**：逐步扩展，PRM 在每步对候选评分

### 关键公式 / 算法

蒙特卡洛步骤质量估计（对应论文 Eq.1）：

$$q(s_k) = \frac{1}{M}\sum_{m=1}^{M} \mathbb{1}[f(\text{rollout}_m(s_k)) = \text{GT}]$$

PRM 训练目标：$\mathcal{L} = -\sum_{k} [c_k \log p_\phi(+|s_k) + (1-c_k) \log p_\phi(-|s_k)]$

## 关键实验

| 实验 | 数据集 / 设置 | 主要结论 |
|------|--------------|---------|
| BoN（N=64）| MathVista, MathVerse | VisualPRM 引导 BoN 超越单次生成约 12-15% |
| 跨模型泛化 | InternVL2.5 / Qwen2-VL | PRM 对不同基础模型均有正向提升 |
| PRM vs ORM | 对比实验 | PRM 在过程较长的复杂推理上显著优于 ORM |
| VisualProcessBench | 步骤正确性评测 | VisualPRM 在步骤识别准确率上达到 SOTA |

**最重要的一个数字结果**：使用 VisualPRM + BoN（N=64），在 MathVista 上将基础模型精度从 ~65% 提升至 ~77%，提升约 12 个百分点。

## 局限

- 蒙特卡洛标注需要大量推理链采样（每个步骤 50+ rollouts），计算成本高
- PRM 只在有 GT 答案的数学/科学题上可靠训练，开放域任务难以扩展
- 大 N（N=64）的 BoN 推理成本显著增加

## 个人思考

**方法的核心 insight 是**：蒙特卡洛步骤质量估计是自动化构建多模态过程标注的关键——无需人工逐步标注，只需让多个模型在该步骤后继续推理，统计最终答案正确率即可估计该步骤质量。

**与我研究方向的关联**：VisualPRM 代表了测试时计算扩展（test-time scaling）在多模态推理中的最成熟实现。理解 PRM 的训练和应用对于理解"为什么更多计算 = 更好推理"这一核心趋势至关重要。

**延伸问题 / 可能的改进方向**：
- PRM 训练数据能否通过 RL（[[M2-11]] Vision-R1）在线生成？
- 如何将 VisualPRM 用于 RLHF 训练中的过程奖励（而非仅测试时使用）？

## 与其他论文的联系

- 继承自：[[M2-10]] AtomThink（多模态 PRM 概念），PRM800K（文本 PRM 数据构建思路）
- 相关：[[M2-07]] XComposer-Reward（ORM vs PRM 对比），[[M2-11]] Vision-R1（RL 推理）
- 同期对比：[[M2-11]] Vision-R1 的 GRPO（在线 RL vs 离线 PRM 搜索）
