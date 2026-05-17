---
title: "BLIP-2: Bootstrapping Language-Image Pre-training with Frozen Image Encoders and Large Language Models"
authors: "Li et al. (Salesforce)"
venue: ICML
year: 2023
arxiv_id: "2301.12597"
reading_depth: 粗读
status: 待读
tags:
  - 多模态基础/预训练
  - 多模态基础/连接器设计
  - 架构/Q-Former
prerequisites:
  - "[[M1-01_InstructBLIP]]"
estimated_hours: 2.5
---

# BLIP-2 粗读笔记

## 核心贡献（3 句话）

1. 提出 Q-Former（Querying Transformer）：用一组可学习 query token 通过 cross-attention 从冻结视觉编码器中提取固定数量的视觉特征，使视觉-语言对齐与 LLM 解耦，大幅降低训练成本。
2. 两阶段训练策略：阶段一在冻结图像编码器上训练 Q-Former（图文对齐）；阶段二将 Q-Former 输出注入冻结 LLM，实现视觉-语言生成。
3. 以极低训练参数（只训练 Q-Former，~188M 参数）在冻结的大型视觉编码器和 LLM 之间建立桥梁，在 zero-shot VQA 等任务上达到当时 SOTA。

## 与主线的关系

- **InstructBLIP（[[M1-01_InstructBLIP]]）的直接前置**：InstructBLIP 在 BLIP-2 的 Q-Former 基础上加入指令调优，是 BLIP-2 的直系延伸
- **连接器设计分叉点**：Q-Former 是"压缩型"连接器（将可变长图像特征压缩为固定数量 query），而 LLaVA 的 MLP 是"线性投影型"。理解这个区别是理解 M1 连接器演进的基础
- MM1（[[M1-13_MM1]]）的消融实验中，Q-Former 类型连接器（C-Abstractor）与 MLP 的对比依赖于对本文的理解

## 值得继续精读吗？

**否**。BLIP-2 作为前置知识了解 Q-Former 机制即可；已在用户知识体系中（已读 BLIP），粗读补全 Q-Former 细节就够。如果后续研究多模态连接器设计，可以精读。

## 快速记录

<!-- Q-Former 的 query 数量（默认 32 个），为什么固定数量？
     阶段一的三个预训练目标（ITC/ITG/ITM）各自学习什么？
     冻结 LLM 意味着什么限制？生成能力从哪来？ -->
