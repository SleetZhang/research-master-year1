---
title: "Qwen2-VL: Enhancing Vision-Language Model's Perception of the World at Any Resolution"
authors: "Peng Wang, Shuai Bai, Sinan Tan, Shijie Wang, Zhihao Fan, Jinze Bai, Keqin Chen, Xuejing Liu, Jialin Wang, Wenbin Ge, Yang Fan, Kai Dang, Mengfan Dong, Xuancheng Ren, Junjie Wang, Bowen Yu, Dayiheng Liu, Fei Huang, Jingren Zhou, Junyang Lin"
venue: "arXiv 2024"
year: 2024
arxiv_id: "2409.12191"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/架构/架构演进"
  - "#多模态/架构/高分辨率"
prerequisites:
  - "[[M1-03]]"
  - "[[M1-06]]"
---

## 核心贡献（3 句话）

1. **做了什么**：提出 Naive Dynamic Resolution（朴素动态分辨率）机制，让模型能自适应地将任意分辨率图像编码为数量可变的视觉 token，从而更高效地处理不同分辨率的图像。
2. **怎么做的**：视觉编码器不再使用固定数量的视觉 token，而是根据图像实际内容动态生成 token 数量；同时在 LLM 中引入窗口注意力（Window Attention）提升训练效率；并对视觉编码器中的位置编码（RoPE）进行 2D 扩展（M-RoPE）以支持空间感知。
3. **效果如何**：Qwen2-VL-72B 在多个 benchmark 上与 GPT-4o、Claude-3.5-Sonnet 持平，成为当时最强开源 MLLM 之一。

## 与主线的关系

> 动态分辨率是 2024 年多模态架构的重要趋势；理解这个机制有助于理解为何后续模型在细粒度视觉任务（如文档理解、精细感知）上大幅提升，也是后训练工作（如推理增强）的重要基础。

- 依赖于：[[M1-03]] LLaVA-1.5（AnyRes 固定分辨率切片的前身思路）
- 同期对比：[[M1-06]] LLaVA-OneVision（AnyRes vs 动态分辨率）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：动态分辨率的核心思想粗读已能掌握；对后训练研究而言，这只是基座能力的工程升级

## 快速笔记

- Naive Dynamic Resolution：根据图像实际分辨率动态决定视觉 token 数量，不再 pad 到固定大小
- M-RoPE：2D 旋转位置编码，让模型感知图像的空间坐标（而非仅线性序列位置）
- 窗口注意力：视觉编码器中使用局部窗口自注意力，大幅降低高分辨率图像的计算量
