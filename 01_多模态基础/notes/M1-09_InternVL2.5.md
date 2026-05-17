---
title: "Expanding Performance Boundaries of Open-Source Multimodal Models with Model, Data, and Test-Time Scaling"
authors: "Zhe Chen, Weiyun Wang, Yue Cao, Yangzhou Liu, Zhangwei Gao, Erfei Cui, Jinguo Zhu, Shenglong Ye, Hao Tian, Zhaoyang Liu, Lixin Gu, Xuehui Wang, Qingyun Li, Yimin Ren, Zixuan Chen, Jiapeng Luo, Jiahao Wang, Tan Jiang, Bo Wang, Conghui He, Botian Shi, Xingcheng Zhang, Han Lv, Yi Wang, Wenqi Shao, Pei Chu, Zhongying Tu, Tong He, Zhiyong Wu, Huipeng Deng, Jiaye Ge, Kai Chen, Min Dou, Lewei Lu, Xizhou Zhu, Tong Lu, Dahua Lin, Yu Qiao, Jifeng Dai, Wenhai Wang"
venue: "arXiv 2024"
year: 2024
arxiv_id: "2412.05271"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/架构/架构演进"
  - "#多模态/推理/测试时计算"
prerequisites:
  - "[[M1-04]]"
---

## 核心贡献（3 句话）

1. **做了什么**：发布 InternVL 2.5 系列（1B-78B），在 InternVL 2.0 基础上全面升级模型架构、训练数据和测试时扩展策略，第一个开源 MLLM 在 MMMU 上突破 70%。
2. **怎么做的**：三方面升级：(1) 使用 InternViT-6B 大视觉编码器 + InternLM 系列 LLM；(2) 数据质量飞升（高质量 OCR、数学、科学题）；(3) 引入测试时缩放（BoN + 思维链）提升推理性能。
3. **效果如何**：多项 benchmark 上与 GPT-4o、Claude-3.5 持平，是当前最强开源多模态模型之一。

## 与主线的关系

> InternVL 2.5 是目前最具竞争力的开源 MLLM 基座之一，很多后训练工作会以 InternVL 系列作为基座（如 InternLM-XComposer2.5-Reward）。了解其能力边界有助于理解后训练能改进什么。

- 依赖于：[[M1-04]] InternVL 1.0
- 相关：[[M2-07]] InternLM-XComposer2.5-Reward（在此基础上的奖励模型）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：主要是工程扩展，架构思路在 [[M1-04]] 已建立基础

## 快速笔记

- 第一个开源 MLLM 在 MMMU 上突破 70%
- 测试时缩放：BoN（Best-of-N）+ CoT，证明测试时计算对多模态推理同样有效
- 数据质量是关键：专注于 OCR、数学、科学推理数据的高质量构建
