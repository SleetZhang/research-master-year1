---
title: "Multimodal Chain-of-Thought Reasoning in Language Models"
authors: "Zhuosheng Zhang, Aston Zhang, Mu Li, Hai Zhao, George Karypis, Alex Smola"
venue: "TMLR 2023"
year: 2023
arxiv_id: "2302.00923"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/推理/CoT"
  - "#多模态/推理/链式思维"
prerequisites:
  - "[[M1-02]]"
---

## 核心贡献（3 句话）

1. **做了什么**：首次系统地将 CoT（Chain-of-Thought）推理扩展到多模态场景，在 ScienceQA 数据集上实现两阶段多模态 CoT 框架。
2. **怎么做的**：两阶段框架：首先生成图文融合的推理链（rationale），再用推理链辅助生成最终答案。使用视觉特征 + 语言推理链的多模态 fusion 机制。
3. **效果如何**：在 ScienceQA 上超越 GPT-3.5（Chain-of-Thought）约 16%，是多模态推理的早期里程碑。

## 与主线的关系

> 这是多模态 CoT 的奠基论文，后续 LLaVA-CoT（[[M2-09]]）、AtomThink（[[M2-10]]）等工作都建立在"先推理再回答"的两阶段框架之上。粗读了解范式起点即可。

- 被继承：[[M2-09]] LLaVA-CoT，[[M2-10]] AtomThink

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：方法相对基础，后续工作（[[M2-09]] 等）更成熟；只需了解历史起点

## 快速笔记

- ScienceQA：科学题推理数据集（小学-高中），含图像+文字+选项
- 两阶段：rationale 生成 → 答案生成（用 rationale 作为额外输入）
- 关键发现：多模态 CoT 比纯文本 CoT 更有效（图像特征对推理有正向贡献）
