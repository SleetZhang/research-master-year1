---
title: "HII-DPO: Eliminate Hallucination via Accurate Hallucination-Inducing Counterfactual Images"
authors: "anonymous / et al."
venue: arXiv
year: 2026
arxiv_id: "2602.10425"
reading_depth: 粗读
status: 待读
tags:
  - 多模态/后训练/DPO
  - 多模态/幻觉/解码时干预
  - 多模态/幻觉/语言偏差
prerequisites:
  - "[[M0-03_DPO]]"
  - "[[M2-04_mDPO]]"
  - "[[M2-15_Hallucination-Survey]]"
estimated_hours: 2.5
---

# HII-DPO 粗读笔记

## 核心贡献（3 句话）

1. 揭示"场景条件幻觉"模式：VLMs 倾向于输出与场景高度相关的典型对象，即使该对象在图像中已被遮蔽（Masked-Object-Hallucination），说明幻觉的根源是语言偏差而非视觉感知失败。
2. 构建 HII（Hallucination-Inducing Images）合成流水线：系统性地生成能诱发幻觉的反事实图像（遮蔽场景中典型对象），并基于此构建高质量 DPO 偏好对，教模型优先利用视觉证据而非语言先验。
3. 提出 MOH（Masked-Object-Hallucination）基准评测幻觉对齐框架的有效性，HII-DPO 在多个幻觉基准上达到 SOTA，2026 年最新前沿。

## 与主线的关系

- **DPO + 幻觉缓解的 2026 年前沿**，与 mDPO（[[M2-04_mDPO]]）的关系：
  - mDPO：解决多模态 DPO 的 unconditional preference 问题（模型忽视图像）
  - HII-DPO：通过反事实图像专门针对语言偏差类幻觉，有针对性的 DPO 数据构建
- 与 VCD/OPERA（[[M2-20_VCD]]、[[M2-21_OPERA]]）对比：解码时干预 vs 训练时 DPO，HII-DPO 代表训练时方法的最新进展
- 反事实图像构建思路（场景中遮蔽典型对象）与 VisualPRM（[[M2-13_VisualPRM]]）的过程监督思路有互补之处

## 值得继续精读吗？

**推荐**（若研究幻觉缓解+DPO方向）。HII-DPO 是 2026 年最新工作，反事实数据构建方法新颖，且系统性揭示了场景条件幻觉这一新现象。

## 快速记录

<!-- 反事实图像生成流水线：用 inpainting 遮蔽对象？还是别的方法？
     MOH 基准与 POPE/HallusionBench 的区别：专门测试被遮蔽对象的幻觉
     "场景条件幻觉"具体例子：厨房场景下幻觉出微波炉？
     HII-DPO 数据规模：多少偏好对？与 VLFeedback（82K）对比 -->
