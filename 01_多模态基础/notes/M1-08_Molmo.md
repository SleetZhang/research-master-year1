---
title: "Molmo and PixMo: Open Weights and Open Data for State-of-the-Art Multimodal Models"
authors: "Matt Deitke, Christopher Clark, Sangho Lee, Rohun Tripathi, Yue Yang, Jae Sung Park, Mohammadreza Salehi, Niklas Muennighoff, Kyle Lo, Luca Soldaini, Jiasen Lu, Taira Anderson, Erin Bransom, Kiana Ehsani, Huong Ngo, YenSung Chen, Ajay Patel, Mark Yatskar, Chris Callison-Burch, Andrew Head, Rose Hendrix, Favyen Bastian, Eli VanderBilt, Nathan Lambert, Yvonne Chou, Arnavi Chheda, Jenna Sparks, Sam Skjonsberg, Michael Schmitz, Aaron Sarnat, Byron Bischoff, Pete Walsh, Chris Newell, Piper Wolters, Tanmay Gupta, Kuo-Hao Zeng, Jon Borchardt, Dirk Groeneveld, Crystal Nam, Sophie Lebrecht, Caitlin Wittlif, Carissa Schoenick, Oscar Michel, Ranjay Krishna, Luca Weihs, Noah A. Smith, Hannaneh Hajishirzi, Ross Girshick, Ali Farhadi, Aniruddha Kembhavi"
venue: "arXiv 2024 / NeurIPS 2024 workshop invited talk"
year: 2024
arxiv_id: "2409.17146"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/架构/架构演进"
  - "#多模态/数据/数据驱动"
prerequisites:
  - "[[M1-03]]"
---

## 核心贡献（3 句话）

1. **做了什么**：Allen AI 推出完全开放权重+数据的多模态模型 Molmo，核心创新在于 PixMo 数据集——完全由人工语音标注的高质量细粒度图像描述数据。
2. **怎么做的**：通过人工口述的方式收集高密度、细粒度的图像描述（而非文字填写），生成空前详细的 caption 数据；模型架构采用标准 ViT + LLM + connector 结构；独特地支持 2D Pointing 功能（指点图像中的具体位置）。
3. **效果如何**：NeurIPS 2024 Keynote 级别工作，在多个 benchmark 上超越 GPT-4V，证明纯数据质量可以弥补架构复杂度。

## 与主线的关系

> Molmo 是"数据质量 > 模型复杂度"论点的最佳证明——高质量人工语音描述数据显著优于 GPT-4 合成数据。这对后训练的数据构建策略（如奖励数据收集方式）有重要启示。

- 依赖于：[[M1-02]] LLaVA（基础架构），[[M1-10]] ShareGPT4V（数据质量主题）
- 对照：[[M1-10]] ShareGPT4V（GPT-4V 合成 vs 人工语音标注）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：主要贡献在数据收集方法，架构无突破；对后训练主线影响有限

## 快速笔记

- PixMo 数据：人工通过语音口述图像细节（而非打字），获得更自然、更密集的描述
- Pointing 功能：模型能输出图像中具体像素坐标，实现精确的视觉定位
- 关键结论：即使用标准 ViT + MLP + LLM 架构，高质量数据也能超越复杂架构
