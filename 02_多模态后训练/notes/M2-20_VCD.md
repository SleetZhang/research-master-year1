---
title: "Mitigating Object Hallucinations in Large Vision-Language Models through Visual Contrastive Decoding"
authors: "Leng et al."
venue: CVPR
year: 2024
arxiv_id: "2311.16922"
reading_depth: 粗读
status: 待读
tags:
  - 多模态/幻觉/解码时干预
  - 多模态/幻觉/对比解码
prerequisites:
  - "[[M2-15_Hallucination-Survey]]"
  - "[[M2-19_POPE]]"
estimated_hours: 2.5
---

# VCD 粗读笔记

## 核心贡献（3 句话）

1. 提出 Visual Contrastive Decoding（VCD）：在解码时对比"加噪图像输入"和"原始图像输入"的输出概率分布，通过放大视觉信息的相对贡献来减少语言先验偏差导致的幻觉，**无需额外训练**。
2. VCD 的核心假设：幻觉源于模型对语言统计先验的过度依赖；用高斯噪声扰动图像后，模型输出更多依赖语言先验；对比两者的 logit 差即可放大视觉信号。
3. 在 CHAIR、POPE 等幻觉基准上显著减少对象幻觉，且与模型无关（即插即用），无需修改训练流程。

## 与主线的关系

- **"解码时干预"路线的代表作**，与 DPO 系列"训练时对齐"路线互补
  - DPO 路线（[[M2-02_RLHF-V]]、[[M2-04_mDPO]]）：修改训练目标来减少幻觉
  - VCD 路线：不改训练，在推理时干预解码过程
- **[[M2-21_OPERA]]** 是另一个解码时干预的代表，两者对比可理解"解码干预"的不同思路
- VCD 的局限（训练无关但可能有推理开销、只针对对象幻觉）与训练时方法的优劣对比是重要思考点

## 值得继续精读吗？

**否**。VCD 方法简单（扰动图像→对比logit），核心思想粗读即可掌握。若研究解码时幻觉缓解方向，可精读作为起点。

## 快速记录

<!-- VCD 的具体操作：如何给图像加噪？噪声强度怎么设置？
     "放大视觉信号"的数学形式：logit(原始) - α × logit(扰动) 还是别的形式？
     推理开销有多大？需要额外一次前向传播 → 实用性？
     与 OPERA 的关键区别：VCD 对比视觉信号，OPERA 对比注意力分配 -->
