---
title: "HallusionBench: An Advanced Diagnostic Suite for Entangled Language Hallucination and Visual Illusion in Large Vision-Language Models"
authors: "Tianrui Guan, Fuxiao Liu, Xiyang Wu, Ruiqi Xian, Zongxia Li, Xiaoyu Liu, Xijun Wang, Lichang Chen, Furong Huang, Yaser Yacoob, Gedas Bertasius, Mohit Bansal, Ying Nian Wu, Jiebo Luo"
venue: "CVPR 2024"
year: 2024
arxiv_id: "2310.14566"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/幻觉/评测"
  - "#多模态/评测/综合基准"
prerequisites:
  - "[[M2-15]]"
---

## 核心贡献（3 句话）

1. **做了什么**：构建 HallusionBench，首个专门诊断"语言幻觉"（language hallucination）与"视觉错觉"（visual illusion）交织问题的多模态基准测试。
2. **怎么做的**：346 张图 × 1129 道人工精心设计的问题，每道题要求模型区分图像的真实内容（排除语言先验的干扰）与语言预期的内容；问题设计刻意制造"语言和视觉矛盾"。
3. **效果如何**：GPT-4V 在 HallusionBench 上仅达 31.42% 问题对准确率，暴露了当时最强商业模型的视觉-语言混淆问题。

## 与主线的关系

> 是 CVPR 2024 的重要幻觉评测基准，被 RLHF-V、RLAIF-V 等对齐工作广泛引用。了解它的评测设计能帮助解读后训练论文中的"幻觉减少"数字。

- 依赖于：[[M2-15]] 幻觉综述（整体幻觉背景）
- 对照：POPE（更简单的对象存在性），MMHal-Bench（更综合的能力评测）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：benchmark 论文，核心价值在于了解评测维度和数字含义，不需要深入方法细节

## 快速笔记

- 核心设计：用真实图像 + 误导性问题，测试模型能否抵制语言先验的"拉力"
- 问题类型：视觉幻觉（图像本身不符合预期）vs 语言幻觉（回答不符合图像）
- 关键发现：多数模型在"视觉常识矛盾"类问题上失败率高（如被问到图像中明显不存在的物体，模型仍然"看到"了它）
