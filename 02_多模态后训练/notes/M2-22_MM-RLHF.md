---
title: "MM-RLHF: The Next Step Forward in Multimodal LLM Alignment"
authors: "Zhang et al. (Kuaishou)"
venue: arXiv
year: 2025
arxiv_id: "2502.10391"
reading_depth: 精读
status: 待读
tags:
  - 多模态/后训练/RLHF
  - 多模态/后训练/偏好数据
  - 多模态/后训练/奖励模型
prerequisites:
  - "[[M2-01_LLaVA-RLHF]]"
  - "[[M0-02_InstructGPT]]"
estimated_hours: 9
---

# MM-RLHF 粗读笔记

## 核心贡献（3 句话）

1. 构建 MM-RLHF 数据集：120K 细粒度人工标注偏好对，覆盖 10 个维度（幻觉/安全/指令遵循/推理等），规模和维度均优于现有多模态偏好数据集，是迄今最系统的多模态 RLHF 资源。
2. 提出 Critique-Based Reward Model（CBRM）：奖励模型在打分前先生成对输出的批判性分析（critique），提供更具解释性的奖励信号，并引入 Dynamic Reward Scaling 根据奖励信号调整每个样本的损失权重。
3. 在 27 个基准上验证，LLaVA-OV-7B 经 MM-RLHF 后，对话能力提升 19.5%，安全性提升 60%；数据集和模型均开源。

## 与主线的关系

- **2025 年最系统的多模态 RLHF 更新**，与 LLaVA-RLHF（[[M2-01_LLaVA-RLHF]]）相比：
  - M2-01：单一幻觉缓解视角的 RLHF，8K 数据
  - MM-RLHF：全维度多模态对齐，120K 数据，CBRM 更可解释
- 数据集规模填补了多模态偏好数据匮乏的问题，对 mDPO（[[M2-04_mDPO]]）和 RLAIF-V（[[M2-05_RLAIF-V]]）等工作有数据层面的补充意义
- CBRM 的"先批评后打分"思路与 Calibrated Self-Rewarding（[[M2-06_Calibrated-Self-Rewarding]]）的自我迭代理念相通

## 值得继续精读吗？

**视情况**。数据集论文为主，若研究多模态偏好数据构建，需精读；当前阶段了解数据维度设计和 CBRM 架构即可。

## 快速记录

<!-- 10 个标注维度具体是什么？标注员如何培训？标注一致性？
     CBRM 的 critique 是怎么生成的？用的什么 backbone？
     Dynamic Reward Scaling 的具体公式：根据 reward 值如何调整损失权重？
     与 Silkie/VLFeedback（[[M2-03_Silkie]]）的对比：人工标注 vs AI 反馈的质量差异 -->
