---
title: "Improved Baselines with Visual Instruction Tuning"
authors: "Haotian Liu, Chunyuan Li, Yuheng Li, Yong Jae Lee"
venue: "CVPR 2024"
year: 2024
arxiv_id: "2310.03744"
reading_depth: 精读
status: 未读
tags:
  - "#多模态/架构/连接器"
  - "#多模态/SFT/指令微调"
prerequisites:
  - "[[M1-02]]"
---

## 核心问题

> 在 LLaVA 框架下，哪些设计选择对性能提升最关键？能否以极低成本建立更强的多模态基线？

## 动机

- LLaVA 之后大量工作改动了多处组件，难以判断性能提升来自哪里
- 训练成本高昂：能否通过关键设计选择而非大规模扩展来提升性能？

## 方法

### 总体框架

在受控实验设置下系统比较各设计选择，最终确定：CLIP ViT-L-336px + 2层 MLP 投影 + Vicuna-13B + 混合学术 VQA 数据的组合效果最优。

### 关键模块

1. **视觉编码器升级**：CLIP ViT-L/14 → CLIP ViT-L-336px（更高分辨率）
2. **连接器升级**：线性层 → 2层 MLP（增强特征映射非线性表达）
3. **LLM 升级**：Vicuna-7B → Vicuna-13B
4. **数据升级**：仅合成对话 → 合成对话 + VQA 学术数据混合 + 格式化 prompt

### 关键公式 / 算法

（与 LLaVA 相同的训练目标，核心贡献在数据和架构选择上，非新公式）

## 关键实验

| 实验 | 数据集 / 设置 | 主要结论 |
|------|--------------|---------|
| 连接器消融 | 线性 vs MLP on VQAv2 | MLP +0.9% acc |
| 分辨率消融 | 224px vs 336px | 336px 在 TextVQA 上 +5%+ |
| 数据消融 | 合成 vs 合成+VQA | 加入学术VQA数据显著提升 |
| 主实验 | 11 个公开 benchmark | 以 1% GPU 训练成本超越同期所有开源模型 |

**最重要的一个数字结果**：LLaVA-1.5-13B 在 VQAv2 达到 80.0%，以极低训练成本超越当时开源 SOTA。

## 局限

- 仍受 CLIP 特征分辨率和能力限制（细粒度感知、密集场景）
- 未解决多图、视频等复杂场景
- MLP 连接器的信息压缩仍可能有损

## 个人思考

**方法的核心 insight 是**："更好的连接器 + 更高分辨率 + 混合数据"三个简单改动即可大幅提升性能，复杂架构并非必要。这是 Occam's Razor 在多模态领域的完美体现。

**与我研究方向的关联**：LLaVA-1.5 是后续几乎所有多模态后训练工作的基座模型，RLHF-V、mDPO、LLaVA-CoT 等均以此为基础。理解其训练流程是读后训练论文的必要前置。

**延伸问题 / 可能的改进方向**：
- MLP 是最优连接器吗？→ 参见 [[M1-05]] Cambrian-1
- 单一 CLIP 视觉编码器够用吗？→ 参见 [[M1-05]] Cambrian-1（多视觉编码器融合）

## 与其他论文的联系

- 继承自：[[M1-02]] LLaVA（整体框架）
- 被以下工作继承：[[M1-06]] LLaVA-OneVision，[[M2-01]] LLaVA-RLHF，[[M2-02]] RLHF-V
- 同期对比：[[M1-01]] InstructBLIP（Q-Former vs MLP）
