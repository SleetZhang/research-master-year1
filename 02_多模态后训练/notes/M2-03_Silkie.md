---
title: "Silkie: Preference Distillation for Large Visual Language Models"
authors: "Lei Li, Zhihui Xie, Mukai Li, Shunian Chen, Peiyi Wang, Liang Chen, Yazheng Yang, Benyou Wang, Lingpeng Kong"
venue: "arXiv 2023"
year: 2023
arxiv_id: "2312.10665"
reading_depth: 精读
status: 未读
tags:
  - "#多模态/后训练/DPO"
  - "#多模态/数据/AI反馈"
  - "#多模态/后训练/RLAIF"
prerequisites:
  - "[[M2-02]]"
---

## 核心问题

> 能否用 AI 反馈（而非昂贵的人工标注）构建大规模多模态偏好数据集，并用 DPO 蒸馏出更好的视觉语言模型？

## 动机

- 人工偏好标注（如 RLHF-V 的 1.4K 样本）成本高，难以扩展
- GPT-4V 等强模型可以自动评估多模态输出的质量（帮助性、视觉忠实性、安全性）
- 多模态 AI feedback 方法（RLAIF）尚未在视觉语言领域系统探索

## 方法

### 总体框架

构建 VLFeedback 数据集：从 12 个开源 LVLM 采样 82K+ 多模态指令的多个回答，用 GPT-4V 对每个回答从三个维度打分，再将评分转化为偏好对，最终用 DPO 微调 Qwen-VL-Chat。

### 关键模块

1. **VLFeedback 数据集构建**：
   - 82,000+ 多模态指令（来自 OpenHermes、ShareGPT4V 等）
   - 12 个开源 LVLM 生成回答（Qwen-VL, InstructBLIP, mPLUG-Owl2 等）
   - GPT-4V 对每个回答从 3 维度打分：帮助性（1-10）、视觉忠实性（1-10）、伦理安全性（1-10）
2. **偏好对构建**：基于 GPT-4V 分数，选择得分最高 vs 最低的回答构成 (chosen, rejected) 对
3. **DPO 微调**：以 Qwen-VL-Chat 为基础模型，用标准 DPO 目标微调

### 关键公式 / 算法

标准 DPO 目标（无额外修改，核心贡献在数据）：

$$\mathcal{L}_\text{DPO} = -\mathbb{E}_{(x,y_c,y_r) \sim \mathcal{D}}\left[\log \sigma\left(\beta \log \frac{\pi_\theta(y_c|x)}{\pi_\text{ref}(y_c|x)} - \beta \log \frac{\pi_\theta(y_r|x)}{\pi_\text{ref}(y_r|x)}\right)\right]$$

## 关键实验

| 实验 | 数据集 / 设置 | 主要结论 |
|------|--------------|---------|
| 主实验 | MME benchmark | 感知 +6.9%，认知 +9.5% |
| 幻觉评测 | MMHal-Bench | 3.02 分（SOTA），显著降低幻觉 |
| AI feedback vs Human | 与 RLHF-V 对比 | AI feedback + 更多数据可达到相近效果 |
| 维度分析 | 3 维度消融 | 视觉忠实性 > 帮助性 > 安全性 对幻觉减少贡献 |

**最重要的一个数字结果**：Silkie 在 MMHal-Bench 上达到 3.02 分，是当时最低幻觉率的开源 LVLM。

## 局限

- VLFeedback 依赖 GPT-4V 打分，存在 GPT-4V 本身的偏见和错误
- DPO 偏好对的质量取决于采样模型的多样性
- 未解决多模态 DPO 中的 unconditional preference 问题（参见 [[M2-04]]）

## 个人思考

**方法的核心 insight 是**：多模态 AI feedback（RLAIF）可以规模化生成偏好数据，打破人工标注的瓶颈；GPT-4V 作为 judge 的多维度评分能有效区分幻觉和非幻觉回答。

**与我研究方向的关联**：Silkie 确立了"AI feedback + DPO"作为多模态对齐的主流廉价范式。理解这篇论文是理解 RLAIF-V（[[M2-05]]）和后续工作的重要基础。同时 VLFeedback 数据集本身也是很多后续工作的基准。

**延伸问题 / 可能的改进方向**：
- 如何让 AI feedback 更可靠（减少 GPT-4V 的偏见）？→ 参见 [[M2-05]] RLAIF-V（divide-and-conquer 评估）
- 标准 DPO 在多模态中是否有问题？→ 参见 [[M2-04]] mDPO

## 与其他论文的联系

- 继承自：[[M2-02]] RLHF-V（DPO框架），RLAIF（AI feedback 思想）
- 被以下工作改进：[[M2-05]] RLAIF-V（更好的自我反馈），[[M2-04]] mDPO（解决条件问题）
- 同期对比：[[M2-02]] RLHF-V（人工 vs AI feedback）
