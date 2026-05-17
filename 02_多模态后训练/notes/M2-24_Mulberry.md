---
title: "Mulberry: Empowering MLLM with o1-like Reasoning and Reflection via Collective Monte Carlo Tree Search"
authors: "Yao et al."
venue: NeurIPS
year: 2024
arxiv_id: "2412.18319"
reading_depth: 粗读
status: 待读
tags:
  - 多模态/推理/o1风格
  - 多模态/推理/MCTS
  - 多模态/推理/树搜索
prerequisites:
  - "[[M2-09_LLaVA-CoT]]"
  - "[[M2-10_AtomThink]]"
estimated_hours: 2.5
---

# Mulberry 粗读笔记

## 核心贡献（3 句话）

1. 提出 Collective Monte Carlo Tree Search（CoMCTS）：让多个模型协作（集体）进行推理路径搜索，通过扩展→模拟/错误定位→反向传播→选择的迭代操作，高效找到正确推理路径，搜索成功率达 80.2%（vs 单模型 58.2%）。
2. 基于 CoMCTS 构建 Mulberry-260K 数据集：每个问题对应一棵含丰富中间步骤的推理树，步骤数量随问题复杂度自适应（数学题平均 8.9 步，图表题 6.8 步）；训练得到具备 o1 风格推理和反思能力的 MLLM。
3. NeurIPS 2025 Spotlight，Mulberry 系列模型在多模态数学推理（MathVista 等）和反思推理（要求模型纠正错误）任务上显著超越 LLaVA-CoT 等基线。

## 与主线的关系

- **推理增强方向的 MCTS 路线**，与已有方法的对比：
  - LLaVA-CoT（[[M2-09_LLaVA-CoT]]）：四阶段固定推理结构
  - AtomThink（[[M2-10_AtomThink]]）：原子步骤 + PRM 过程监督
  - Vision-R1/Visual-RFT（[[M2-11_Vision-R1]]、[[M2-12_Visual-RFT]]）：GRPO 在线强化学习
  - **Mulberry**：离线 MCTS 树搜索构建数据 → SFT 训练，强调数据质量和推理树结构
- 与 VisualPRM（[[M2-13_VisualPRM]]）的对比：都关注过程级推理质量，但 Mulberry 用 MCTS 生成树，VisualPRM 用 Monte Carlo 估计步骤得分
- NeurIPS 2025 Spotlight 说明该方向的影响力，是当前推理增强方向的重要补充

## 值得继续精读吗？

**视情况**。MCTS 用于推理路径搜索的思路新颖，且有大规模数据集开源。若研究推理数据生成或 o1 风格推理，值得精读。

## 快速记录

<!-- "集体"MCTS 的"集体"体现在哪里？哪些模型参与协作？
     错误定位（Error Positioning）步骤如何判断某步推理是否有错？
     Mulberry-260K 与 VisualPRM400K 在数据结构上的差异（树 vs 线性）
     推理 + 反思：反思能力是如何训练的？有专门的反思数据吗？ -->
