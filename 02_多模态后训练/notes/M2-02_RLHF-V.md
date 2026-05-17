---
title: "RLHF-V: Towards Trustworthy MLLMs via Behavior Alignment from Fine-grained Correctional Human Feedback"
authors: "Tianyu Yu, Yuan Yao, Haoye Zhang, Taiwen He, Yifeng Han, Ganqu Cui, Jinyi Hu, Zhiyuan Liu, Hai-Tao Zheng, Maosong Sun, Tat-Seng Chua"
venue: "CVPR 2024"
year: 2024
arxiv_id: "2312.00849"
reading_depth: 精读
status: 未读
tags:
  - "#多模态/后训练/DPO"
  - "#多模态/后训练/人类反馈"
  - "#多模态/幻觉/缓解"
prerequisites:
  - "[[M2-01]]"
  - "[[M1-03]]"
---

## 核心问题

> 如何用细粒度的人工纠错反馈（而非二选一偏好）来对齐多模态 LLM，用更少数据获得更好的幻觉减少效果？

## 动机

- LLaVA-RLHF（[[M2-01]]）需要 PPO + 完整奖励模型，训练成本高
- 粗粒度偏好标注（哪个答案更好）难以告诉模型"哪里错了"
- 幻觉往往是局部性的（某个词/片段），标注者能精确指出错误位置

## 方法

### 总体框架

收集段级别（segment-level）的人工纠错反馈，将其转化为 dense DPO 训练信号。与 LLaVA-RLHF 的对比：不需要 PPO，只用 DPO；但反馈更细粒度（段级别纠错 vs 整体偏好）。

### 关键模块

1. **细粒度纠错反馈收集**：标注者阅读模型生成的回答，直接在包含幻觉的句子/片段上修改为正确答案（而非选择好/坏）
2. **Dense DPO（DDPO）**：将纠错段落和原始段落组成 (chosen, rejected) 对，只在被纠错的位置计算 DPO loss（而非全序列）；正确段落用相同内容作为 chosen 和 rejected
   - Dense reward：$r(x,y_c,y_r) = \sum_{t \in \text{corrected}} \log \frac{\pi_\theta(y_c^t|ctx)}{\pi_\text{ref}(y_c^t|ctx)}$
3. **段级别对比**：只对幻觉段落计算 loss，保留非幻觉部分不变

### 关键公式 / 算法

Dense DPO 目标函数（对应论文 Eq.3-4）：

$$\mathcal{L}_\text{DDPO} = -\sum_{i=1}^{N} \sum_{t \in S_i^\text{corrected}} \log \sigma\left(\beta \log \frac{\pi_\theta(y_c^t|ctx_i)}{\pi_\text{ref}(y_c^t|ctx_i)} - \beta \log \frac{\pi_\theta(y_r^t|ctx_i)}{\pi_\text{ref}(y_r^t|ctx_i)}\right)$$

## 关键实验

| 实验 | 数据集 / 设置 | 主要结论 |
|------|--------------|---------|
| 幻觉评测 | Object HalBench | RLHF-V 减少幻觉 34.8%，仅用 1.4K 数据 |
| 与 LLaVA-RLHF 对比 | MMHAL-Bench | RLHF-V 用 1.4K 数据超越 LLaVA-RLHF（10K 数据）|
| 细粒度 vs 粗粒度 | 消融实验 | dense feedback 显著优于 segment-level 偏好标注 |
| 泛化性 | LLaVA-Bench，其他 | 不显著影响非幻觉能力 |

**最重要的一个数字结果**：1.4K 细粒度纠错样本使幻觉率降低 34.8%，比 LLaVA-RLHF（10K 样本 + PPO）更高效。

## 局限

- 细粒度纠错标注比偏好标注更耗时（每个样本需标注者更深度参与）
- DPO 的参考模型是固定的，多轮优化需重新构建偏好数据
- 仅在特定幻觉类型（对象存在性）上有显著效果，其他类型幻觉效果一般

## 个人思考

**方法的核心 insight 是**：细粒度纠错信号 > 粗粒度偏好信号，因为前者直接告诉模型"错在哪里"，而后者只说"这个不如那个"。这是信号密度（dense feedback）在对齐训练中的价值体现。

**与我研究方向的关联**：RLHF-V 开创了"用 DPO 替代 PPO 做多模态对齐"的路线，此后几乎所有多模态偏好对齐工作（mDPO、RLAIF-V、Silkie）都沿用 DPO 框架。同时"dense feedback"的思想也预示了过程奖励模型（PRM）的价值。

**延伸问题 / 可能的改进方向**：
- 能否自动生成纠错数据，减少人工标注成本？→ 参见 [[M2-05]] RLAIF-V
- DPO 在多模态中有独特问题吗？→ 参见 [[M2-04]] mDPO（unconditional preference problem）

## 与其他论文的联系

- 继承自：[[M2-01]] LLaVA-RLHF（多模态 RLHF 框架），DPO（优化算法）
- 被以下工作改进：[[M2-04]] mDPO（多模态 DPO 的条件问题），[[M2-05]] RLAIF-V（AI feedback 替代人工）
- 同期对比：[[M2-01]] LLaVA-RLHF（PPO vs DPO，粗粒度 vs 细粒度）
