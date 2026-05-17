---
title: "MMMU: A Massive Multi-discipline Multimodal Understanding and Reasoning Benchmark for Expert AGI"
authors: "Xiang Yue, Yuansheng Ni, Kai Zhang, Tianyu Zheng, Ruoqi Liu, Ge Zhang, Samuel Stevens, Dongfu Jiang, Weiming Ren, Yuxuan Sun, Cong Wei, Botao Yu, Ruibin Yuan, Renliang Sun, Ming Yin, Boyuan Zheng, Zhenzhu Yang, Yibo Liu, Wenhao Huang, Huan Sun, Yu Su, Wenhu Chen"
venue: "CVPR 2024"
year: 2024
arxiv_id: "2311.16502"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/评测/综合基准"
prerequisites:
  - "[[M1-02]]"
---

## 核心贡献（3 句话）

1. **做了什么**：构建 MMMU，一个覆盖 6 大学科领域、11,500+ 道大学程度多学科多模态理解与推理题的评测基准。
2. **怎么做的**：从大学考试、课件、教材中精心收集需要专业知识和深度推理的多模态题目（文、理、工、医、艺、社科），每题需同时理解图像和文本才能回答。
3. **效果如何**：GPT-4V 仅达 56% 准确率（人类专家 ~88%），成为衡量 MLLM 推理能力的标准基准之一，被几乎所有后续工作引用。

## 与主线的关系

> 理解 MMMU 是理解多模态后训练工作实验章节的前提——绝大多数论文（RLHF-V、mDPO、LLaVA-CoT 等）都在 MMMU 上报告结果。粗读了解题目类型、难度分布和评分方式即可。

- 对照：MMBench（更综合日常能力评测），MathVista（专项数学推理）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：benchmark 论文，粗读了解评测设计和数字含义即可

## 快速笔记

- 6 大学科：Art & Design, Business, Science, Health & Medicine, Humanities, Tech & Engineering
- 难度：需要大学级别知识 + 多步推理，不能靠单纯视觉识别或检索
- 当前 SOTA（2025年）：多数模型已超过 70%，InternVL 2.5 是第一个开源模型突破 70%
- 常见误区：高 MMMU 分 ≠ 整体能力强，因题目偏知识密集型
