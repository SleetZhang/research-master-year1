---
title: "ShareGPT4V: Improving Large Multi-Modal Models with Better Captions"
authors: "Lin Chen, Jinsong Li, Xiaoyi Dong, Pan Zhang, Conghui He, Jiaqi Wang, Feng Zhao, Dahua Lin"
venue: "ECCV 2024"
year: 2024
arxiv_id: "2311.12793"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/数据/合成数据"
  - "#多模态/SFT/指令微调"
prerequisites:
  - "[[M1-02]]"
  - "[[M1-03]]"
---

## 核心贡献（3 句话）

1. **做了什么**：构建了含 100K GPT-4V 精标 + 1.2M 高质量扩展 caption 的 ShareGPT4V 数据集，并系统验证"高质量描述数据"对多模态模型的提升效果。
2. **怎么做的**：用 100K 精标数据训练高质量 caption 模型 Share-Captioner，再用 Share-Captioner 生成 1.2M 多样化高质量 caption；将 ShareGPT4V 数据替换 LLaVA-1.5 的预训练数据。
3. **效果如何**：用 ShareGPT4V 数据替换原数据后，模型在几乎所有 benchmark 上均有提升（平均 2-4%），证明"caption 质量"是被低估的重要因素。

## 与主线的关系

> 揭示了"数据质量"在多模态 SFT 中的核心地位——不仅是后训练偏好数据质量重要，预训练/SFT 数据的描述质量同样关键。这对理解多模态数据工程有直接价值。

- 依赖于：[[M1-02]] LLaVA（原始数据 pipeline）
- 对照：[[M1-08]] Molmo（人工标注 vs GPT-4V 合成标注的对比）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：核心结论简单清晰（高质量 caption → 更好模型），粗读足以理解

## 快速笔记

- 关键发现：GPT-4V 生成的 caption 比原有图题详细 10x，世界知识、空间关系、属性描述更丰富
- Share-Captioner：100K 精标数据训练的专用描述模型，可规模化生成高质量 caption
- 与 Molmo 对比：GPT-4V 合成 vs 人工语音；两者都强调描述质量
