---
title: "Evaluating Object Hallucination in Large Vision-Language Models"
authors: "Li et al."
venue: EMNLP
year: 2023
arxiv_id: "2305.10355"
reading_depth: 粗读
status: 待读
tags:
  - 多模态/幻觉/评测
  - 多模态/幻觉/对象幻觉
prerequisites:
  - "[[M2-15_Hallucination-Survey]]"
estimated_hours: 2.5
---

# POPE 粗读笔记

## 核心贡献（3 句话）

1. 提出 POPE（Polling-based Object Probing Evaluation）：将对象幻觉评测转化为二分类问题（"图中是否存在对象X？"），通过随机/流行/对抗三种负例采样策略控制难度，使幻觉评测高效、可重复。
2. 发现 LVLMs 存在严重的对象幻觉，且倾向于对高频共现对象产生幻觉（statistical bias）；不同模型在不同采样策略下的幻觉程度差异显著。
3. POPE 成为对象幻觉评测的标准基准（被 HallusionBench、RLAIF-V 等后续论文广泛引用），揭示了对象幻觉的主要来源是语言先验偏差而非视觉感知能力不足。

## 与主线的关系

- **[[M2-15_Hallucination-Survey]] 中"评测"章节的核心文献**：幻觉综述必然提到 POPE 作为对象幻觉评测基准
- **[[M2-16_HallusionBench]] 的历史前身**：HallusionBench 在 POPE 基础上进一步区分视觉幻觉与语言幻觉的交互
- **[[M2-05_RLAIF-V]] 的验证工具**：RLAIF-V 等偏好对齐论文用 POPE 评测幻觉缓解效果
- 理解 POPE 的三种采样策略（随机/流行/对抗）有助于理解幻觉的不同成因

## 值得继续精读吗？

**否**。POPE 是经典基准论文，粗读掌握评测方法和三种难度设置即可。方法本身很简单，无需深入研究。

## 快速记录

<!-- POPE 三种采样策略的具体含义？"对抗"模式如何选择负例？
     为什么把幻觉评测转化为二分类问题有效？与开放式生成评测的区别？
     POPE 分数与实际用户感知的幻觉程度相关吗？局限是什么？ -->
