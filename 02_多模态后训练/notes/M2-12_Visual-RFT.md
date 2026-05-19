---
title: "Visual-RFT: Visual Reinforcement Fine-Tuning"
authors: "Ziyu Liu, Zeyi Sun, Yuhang Zang, Wei Li, Bo Zhang, Xiaoyi Dong, Yuanhan Zhang, Shen Zhao, Haodong Duan, Jiaqi Wang"
venue: "ICCV 2025"
year: 2025
arxiv_id: "2503.01785"
reading_depth: 半精读
estimated_hours: 6
status: 未读
tags:
  - "#多模态/推理/RL强化学习"
  - "#多模态/推理/GRPO"
  - "#多模态/后训练/视觉感知"
prerequisites:
  - "[[M2-11]]"
---

## 阅读重点（半精读范围）

> 本文实验载体（细粒度分类、目标检测、OVD）属于传统 CV 任务，不是研究重点。只需吸收方法贡献，跳过实验细节。

- ✅ **Section 3 Method**：GRPO 在视觉感知任务的应用、VPO（视觉可验证奖励）设计（IoU、精确匹配）、训练流程
- ✅ **Ablation 结论**：100 样本 RL vs 500 样本 SFT 的数据效率对比（核心 takeaway）
- ❌ **跳过**：细粒度分类（CUB-200、Stanford Cars）/ 目标检测 / OVD 的具体实验数字和消融细节

## 核心问题

> 如何将 GRPO 风格的强化微调扩展到视觉感知任务（而非仅数学推理），并在极少样本的情况下优于监督微调（SFT）？

## 动机

- Vision-R1（[[M2-11]]）主要针对数学推理，其可验证奖励依赖精确数值答案
- 视觉感知任务（细粒度分类、目标检测、视觉推理定位）有可验证的视觉规则奖励（IoU、准确率等）
- SFT 在数据量少时容易过拟合，RL 能否提供更好的泛化？

## 方法

### 总体框架

将 GRPO 与视觉感知可验证奖励函数（VPO, Visual Perception Verifiable Reward）结合，针对细粒度分类、开放词汇检测、推理定位等任务设计特定奖励函数，实现数据高效的视觉强化微调。

### 关键模块

1. **视觉感知可验证奖励（VPO）**（对应论文 Sec.3.2）：
   - **细粒度分类**：答案字符串精确匹配奖励
   - **开放词汇检测**：预测框与 GT 框的 IoU 奖励（$r = \text{IoU}(b_{pred}, b_{gt})$）
   - **推理定位（Reasoning Grounding）**：先推理再给框，IoU + 格式奖励
   - 格式奖励：输出是否符合 <bbox> ... </bbox> 格式

2. **GRPO 训练**（沿用 Vision-R1 框架）：
   - 采样 $G=8$ 个候选推理链
   - 组相对奖励：$A_i = (r_i - \mu) / \sigma$
   - 目标：最大化正确答案的概率，最小化 KL 散度

3. **数据高效性**：
   - 只需极少 labeled 样本（如 100-500 样本），远少于 SFT 需求
   - 通过奖励信号的在线更新替代大规模 offline 数据

### 关键公式 / 算法

IoU 奖励函数（对应论文 Eq.4）：

$$r_\text{IoU}(b_\text{pred}, b_\text{gt}) = \frac{|b_\text{pred} \cap b_\text{gt}|}{|b_\text{pred} \cup b_\text{gt}|}$$

GRPO 目标与 [[M2-11]] 相同，核心差异在奖励函数设计。

## 关键实验

| 实验 | 数据集 / 设置 | 主要结论 |
|------|--------------|---------|
| 细粒度分类 | CUB-200, Stanford Cars | Visual-RFT 在 100 样本下超越 SFT（500 样本）|
| 目标检测 | Open-Vocab Detection | 少样本设置 Visual-RFT >> SFT |
| 推理定位 | GQA-Grounding | 视觉推理 + 定位组合任务 |
| 泛化性 | 跨数据集 zero-shot | RFT 泛化比 SFT 强 |

**最重要的一个数字结果**：在细粒度分类（CUB-200）上，用 100 个训练样本的 Visual-RFT 超过了用 500 样本 SFT 的基线，证明 RL 的数据效率优势。

## 局限

- 奖励函数设计依赖任务的可验证性（开放域生成任务难以设计可验证奖励）
- 当前仅验证分类和定位类任务，推理类任务的 VPO 设计更复杂
- GRPO 训练稳定性对 LR、$G$、$\beta$ 敏感

## 个人思考

**方法的核心 insight 是**：只要有可验证的奖励函数，GRPO 就比 SFT 更数据高效——因为 RL 通过在线探索学习，而 SFT 只能从固定 offline 数据拟合。视觉感知任务的几何/精确性奖励（IoU、精确匹配）天然满足这一条件。

**与我研究方向的关联**：Visual-RFT 将 RL 推理增强从数学域扩展到一般视觉感知，为后续在多模态后训练中使用 RL（不限于数学题）提供了方法论。同时它的少样本高效性对资源受限的研究场景很有价值。

**延伸问题 / 可能的改进方向**：
- 能否将 VPO 扩展到自由生成任务（用奖励模型代替规则奖励）？
- Visual-RFT + PRM = ？→ 参见 [[M2-13]] VisualPRM

## 与其他论文的联系

- 继承自：[[M2-11]] Vision-R1（GRPO 框架），DeepSeek-R1
- 相关：[[M2-13]] VisualPRM（过程奖励 vs 结果奖励）
- 同期对比：[[M2-14]] R1-OneVision（跨模态泛化 vs 任务专项）
