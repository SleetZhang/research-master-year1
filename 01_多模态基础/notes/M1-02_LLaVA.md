---
title: "Visual Instruction Tuning"
authors: "Haotian Liu, Chunyuan Li, Qingyang Wu, Yong Jae Lee"
venue: "NeurIPS 2023"
year: 2023
arxiv_id: "2304.08485"
reading_depth: 精读
status: 未读
tags:
  - "#多模态/架构/架构演进"
  - "#多模态/SFT/指令微调"
  - "#多模态/数据/合成数据"
prerequisites:
  - "[[CLIP]]"
  - "[[BLIP-2]]"
---

## 核心问题

> 如何构造多模态指令跟随数据，并用它微调出能理解图文交互指令的通用视觉语言助手？

## 动机

- 当时没有公开的多模态指令跟随数据集，模型缺乏对自然语言指令的通用响应能力
- 文本 GPT-4 虽然强大，但无法处理图像——能否利用已有的图像-文字对（图题/标注）让纯文本 GPT-4 生成对话数据？

## 方法

### 总体框架

将 CLIP ViT-L/14 视觉编码器与 Vicuna LLM 通过一个线性投影层（W）连接，用 GPT-4 生成的多模态指令数据进行两阶段训练。

### 关键模块

1. **视觉编码器**：CLIP ViT-L/14，冻结，提取图像特征
2. **线性投影层 W**：可训练，将视觉特征投影到 LLM embedding 空间（第一代用线性层，后续升级为 MLP）
3. **LLM（Vicuna）**：可训练，接受视觉 token + 文本 token 作为输入
4. **数据生成流水线**：用 COCO 图像的文字标注 → 文本化 → 喂给 GPT-4 → 生成对话/描述/推理三类指令数据

### 关键公式 / 算法

训练目标为 next-token prediction，仅对 assistant 回复部分计算 loss：

$$
\mathcal{L} = -\sum_{t} \log p_\theta(x_t^a | x_{<t}^a, x_v, x_q)
$$

其中 $x_v$ 为视觉 token，$x_q$ 为用户问题，$x^a$ 为 assistant 回复。

## 关键实验

| 实验 | 数据集 / 设置 | 主要结论 |
|------|--------------|---------|
| 主实验 | LLaVA-Bench（In-the-Wild）| GPT-4 打分 LLaVA 达到 GPT-4V 的 85.1% |
| 消融：数据类型 | 3类 GPT-4 数据 vs 只用图题 | 对话+描述+推理三者缺一不可 |
| 消融：连接器 | 线性 vs Q-Former | 简单线性层效果出人意料地好 |

**最重要的一个数字结果**：仅用 150K GPT-4 合成数据 + 515K VQA 数据，LLaVA 在 LLaVA-Bench 上达到 GPT-4V 的 85.1%。

## 局限

- 图像理解仅依赖 CLIP 特征，细粒度视觉感知能力有限
- 合成数据依赖 COCO 标注，不覆盖复杂场景
- 线性投影层限制了视觉和语言特征的对齐质量

## 个人思考

**方法的核心 insight 是**：多模态指令跟随的关键是数据，而非复杂架构；用纯文本 GPT-4 生成多模态指令数据是极具创意的 bootstrapping 方法。

**与我研究方向的关联**：这篇论文奠定了视觉 SFT 数据生成的范式——GPT-4 合成指令数据的思路被此后所有后训练工作继承。理解这篇论文是读后续 RLHF、DPO 工作的前提。

**延伸问题 / 可能的改进方向**：
- 合成数据质量和多样性能否进一步提升？→ 参见 [[M1-10]] ShareGPT4V
- 线性投影层是最优选择吗？→ 参见 [[M1-05]] Cambrian-1 的系统比较

## 与其他论文的联系

- 继承自：[[CLIP]]（视觉编码器）、[[BLIP-2]]（多模态对话范式）
- 被以下工作改进：[[M1-03]] LLaVA-1.5（MLP投影+更强LLM）
- 同期对比：[[M1-01]] InstructBLIP（Q-Former路线 vs 线性投影路线）
