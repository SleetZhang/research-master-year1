---
title: "InternVL: Scaling up Vision Foundation Models and Aligning for Generic Visual-Linguistic Tasks"
authors: "Zhe Chen, Jiannan Wu, Wenhai Wang, Weijie Su, Guo Chen, Sen Xing, Muyan Zhong, Qinglong Zhang, Xizhou Zhu, Lewei Lu, Bin Li, Ping Luo, Tong Lu, Yu Qiao, Jifeng Dai"
venue: "CVPR 2024 Oral"
year: 2024
arxiv_id: "2312.14238"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/架构/架构演进"
  - "#多模态/架构/视觉编码器"
prerequisites:
  - "[[CLIP]]"
  - "[[M1-02]]"
---

## 核心贡献（3 句话）

1. **做了什么**：将视觉基础模型从 ViT-L（307M）扩展至 6B 参数，并通过渐进式对齐策略与 LLM 结合，构建 InternVL 系列。
2. **怎么做的**：三阶段训练：(1) 对比预训练（图文配对）；(2) 生成式监督微调（使用 8B 参数 LLM Middleware）；(3) 全参数指令微调。视觉编码器从 ViT 架构扩展至 InternViT-6B。
3. **效果如何**：CVPR 2024 Oral，成为当时最强开源视觉语言基础模型，为后续 InternVL 2.x 系列奠定基础。

## 与主线的关系

> InternVL 是理解"为何大视觉编码器有效"的关键案例，也是后续 InternVL 2.5（[[M1-09]]）的直接前身。了解此文可以建立对视觉编码器规模化的直觉。

- 依赖于：[[CLIP]]（对比学习范式），[[M1-02]]（指令微调框架）
- 被继承：[[M1-09]] InternVL 2.5

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：主要贡献是规模化视觉编码器，与后训练主线关系较弱；读 InternVL 2.5 即可了解最新状态

## 快速笔记

- InternViT-6B：在 ViT 基础上扩展到 6B 参数，使用渐进式对齐训练
- LLM Middleware (8B)：作为"胶水层"将视觉特征重组为语言特征，后期版本简化了这一设计
- 渐进式对齐：先对比预训练→再生成式微调，避免大规模视觉编码器训练不稳定
