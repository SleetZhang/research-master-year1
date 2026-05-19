---
title: "MMInstruct: A High-Quality Multi-Modal Instruction Tuning Dataset with Extensive Diversity"
authors: "Yangzhou Liu, Yue Cao, Zhangwei Gao, Weiyun Wang, Zhe Chen, Wenhai Wang, Hao Tian, Lewei Lu, Xizhou Zhu, Tong Lu, Yu Qiao, Jifeng Dai"
venue: "Science China Information Sciences 2024"
year: 2024
arxiv_id: "2407.15838"
reading_depth: 粗读
status: 未读
estimated_hours: 2.5
tags:
  - "#多模态/SFT/指令微调"
  - "#多模态/数据/合成数据"
prerequisites:
  - "[[M1-02_LLaVA]]"
  - "[[M1-10_ShareGPT4V]]"
---

## 核心贡献（3 句话）

1. **做了什么**：构建 MMInstruct 数据集，覆盖 24 个领域、973K 条多模态指令，包含理解型、创意型、推理型、感知型四种指令类型，是目前多样性最强的开源多模态 SFT 数据集之一。
2. **怎么做的**：半自动生成流程：先用 GPT-4V 生成初始标注，再由 GPT-3.5 扩充变体，最后人工审核质量过滤；在减少纯人工标注成本的同时保证了指令多样性和回答准确性。
3. **效果如何**：在 MMInstruct 上微调的模型在 12 个评测基准中 10 个达到 SOTA，验证了"指令多样性 + 数据质量"对 VLM 泛化能力的直接影响。

## 与主线的关系

> 补充偏好对齐研究的 SFT 基线认知——多模态 DPO/RLHF 工作的 SFT baseline 质量直接决定后训练上限，MMInstruct 展示了高质量 SFT 数据的构建范式。

- 依赖于：[[M1-02_LLaVA]]（视觉指令微调基础框架）
- 铺垫：[[M2-01_LLaVA-RLHF]]、[[M2-04_mDPO]]（偏好对齐论文中的 SFT 基线）
- 对照：[[M1-10_ShareGPT4V]]（同样关注指令数据质量，但 ShareGPT4V 侧重 caption，MMInstruct 侧重指令多样性）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：数据构建工程论文，核心逻辑粗读已能掌握；后续若研究 SFT 数据构建流程可精读

## 快速笔记

<!-- 24 个领域具体涵盖哪些（图表/文档/场景理解等）？
     四类指令（理解/创意/推理/感知）分别占比多少？
     与 ShareGPT4V（caption 质量导向）的核心区别：MMInstruct 更关注指令多样性
     半自动生成流程：GPT-4V 生成 → GPT-3.5 扩充 → 人工过滤的质量控制机制 -->
