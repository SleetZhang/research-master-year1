---
title: "RLAIF-V: Open-Source AI Feedback Leads to Super GPT-4V Trustworthiness"
authors: "Tianyu Yu, Haoye Zhang, Yuan Yao, Yunkai Dang, Da Chen, Xiaoman Lu, Ganqu Cui, Taiwen He, Zhiyuan Liu, Tat-Seng Chua, Maosong Sun"
venue: "CVPR 2025"
year: 2025
arxiv_id: "2405.17220"
reading_depth: 精读
status: 未读
tags:
  - "#多模态/后训练/RLAIF"
  - "#多模态/后训练/DPO"
  - "#多模态/幻觉/缓解"
  - "#多模态/数据/AI反馈"
prerequisites:
  - "[[M2-02]]"
  - "[[M2-03]]"
  - "[[M2-04]]"
---

## 核心问题

> 能否让多模态模型在无需人工标注、无需依赖 GPT-4V 等闭源模型的情况下，通过自身生成的 AI feedback 实现对齐，甚至超越 GPT-4V 的可信度？

## 动机

- Silkie（[[M2-03]]）依赖 GPT-4V 作为 judge，成本高且存在商业依赖
- RLHF-V（[[M2-02]]）需要人工标注，难以规模化
- 能否让开源模型"自我对齐"（self-alignment）？

## 方法

### 总体框架

分治法（Divide-and-Conquer）评估：将复杂的整体回答评估分解为多个简单的单声明（claim）验证，每个声明的正确性更容易判断，从而使开源模型也能充当可靠的 judge。

### 关键模块

1. **回答分解（Decompose）**：将待评估的模型回答分解为多个独立的 atomic claims（事实声明）
2. **声明验证（Verify）**：用开源 MLLM 本身对每个 claim 逐一判断是否幻觉（Yes/No）；单个 claim 的判断比整体评估简单，准确率更高
3. **偏好对构建**：基于声明级别的幻觉评分，选择幻觉更少的回答作为 chosen
4. **DPO 迭代优化**：RLAIF-V 7B/12B 通过迭代 DPO 持续优化，每轮用当前模型重新生成偏好数据
5. **Best-of-N 测试时改进**：用自身生成多个候选答案，再用自身 judge 选最好的（推理时计算扩展）

### 关键公式 / 算法

分治 judge 评分（对应论文 Sec.3.2）：

$$\text{Score}(y | x, v) = 1 - \frac{1}{|C|}\sum_{c \in C} \mathbb{1}[\text{Hallucinate}(c, v)]$$

其中 $C$ 为 $y$ 分解出的 claims 集合，$\mathbb{1}[\text{Hallucinate}(c, v)]$ 为开源模型对单个 claim 的幻觉判断。

## 关键实验

| 实验 | 数据集 / 设置 | 主要结论 |
|------|--------------|---------|
| 主实验（7B）| Object HalBench | 幻觉减少 80.7%（基线 MLLM），整体减少 33.7% |
| 与闭源模型对比 | Object HalBench | RLAIF-V 12B 超越 GPT-4V 可信度 |
| 分治 vs 整体评估 | 消融 | 分治评估准确率显著高于整体评估（尤其是对开源 judge）|
| 迭代训练 | 3轮 vs 1轮 | 迭代后性能持续提升 |
| Best-of-N | 测试时 N=8 vs N=1 | 额外 ~10% 性能提升 |

**最重要的一个数字结果**：RLAIF-V 12B 在 Object HalBench 上超越 GPT-4V 的可信度，而无需任何人工标注或闭源模型 API 调用。

## 局限

- 分治评估增加推理成本（每个 claim 一次 MLLM 调用）
- 初始模型能力是上限——弱模型自我对齐的效果有限
- 主要针对幻觉问题，对其他对齐目标（安全性、帮助性）的效果未充分验证

## 个人思考

**方法的核心 insight 是**：将难题（整体回答评估）分解为简单子任务（单 claim 事实判断），使弱模型也能充当可靠 judge——这是 RLAIF 在多模态领域的重要突破，打破了"必须用强模型做 judge"的限制。

**与我研究方向的关联**：RLAIF-V 展示了"自我对齐"路线的可行性，这与 self-rewarding 系列工作（[[M2-06]]）形成有趣对比。同时分治评估框架对多模态奖励模型设计（[[M2-07]]）也有启发。

**延伸问题 / 可能的改进方向**：
- 分治评估的 claim 粒度如何影响效果？
- 能否将 RLAIF-V 的自我反馈思路扩展到推理增强（[[M2-09]] LLaVA-CoT 中的自我评估）？

## 与其他论文的联系

- 继承自：[[M2-02]] RLHF-V（dense feedback 理念），[[M2-03]] Silkie（AI feedback）
- 参考：[[M2-04]] mDPO（多模态 DPO 的条件问题）
- 同期对比：[[M2-06]] Calibrated Self-Rewarding（另一种自我奖励路线）
