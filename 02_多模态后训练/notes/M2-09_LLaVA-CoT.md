---
title: "LLaVA-CoT: Let Vision Language Models Reason Step-by-Step"
authors: "Guowei Xu, Peng Jin, Hao Li, Yibing Song, Lichao Sun, Li Yuan"
venue: "ICCV 2025"
year: 2025
arxiv_id: "2411.10440"
reading_depth: 精读
status: 未读
tags:
  - "#多模态/推理/CoT"
  - "#多模态/推理/o1风格"
  - "#多模态/推理/测试时计算"
prerequisites:
  - "[[M1-03]]"
  - "[[M2-08]]"
---

## 核心问题

> 如何让视觉语言模型实现自发的、多阶段的系统性推理（类 o1 风格），而非单步响应？

## 动机

- 现有 VLM 倾向于直接回答，缺乏显式推理过程
- 文本 o1 的成功证明"先推理再回答"范式的有效性，但多模态场景面临新挑战：图像信息何时引入推理链？
- 推理链的每个阶段（视觉感知、逻辑推理、总结）是否应该有明确结构？

## 方法

### 总体框架

构建 LLaVA-CoT-100K 结构化推理数据集，训练模型按四个固定阶段推理：(1) 摘要（Summary）→ (2) 描述（Caption）→ (3) 推理（Reasoning）→ (4) 结论（Conclusion）。同时提出 SWIRES（Stage-Wise Retracing Search）测试时搜索算法。

### 关键模块

1. **四阶段推理结构**（对应论文 Sec.3.1）：
   - **\<SUMMARY\>**：总结问题和视觉信息
   - **\<CAPTION\>**：详细描述图像（纯视觉感知阶段）
   - **\<REASONING\>**：基于摘要和描述的多步逻辑推理
   - **\<CONCLUSION\>**：最终答案

2. **LLaVA-CoT-100K 数据集构建**：
   - 从多个 VQA 数据集（MMStar、AI2D、ScienceQA 等）收集问答对
   - 用 GPT-4o 生成四阶段结构化推理链
   - 100K 带推理链的训练样本

3. **SWIRES 测试时搜索**：
   - 在每个推理阶段结束时采样多个候选延续
   - 用奖励模型对各候选打分，选择最优路径继续
   - 阶段级搜索（而非 token 级搜索），效率更高

### 关键公式 / 算法

SWIRES 搜索（对应论文 Algorithm 1）：

在阶段 $s_i$ 生成 $K$ 个候选：$\{y_i^1, ..., y_i^K\}$，用奖励模型打分选最优：

$$y_i^* = \arg\max_{k} r_\phi(x, y_{<i}, y_i^k)$$

其中 $r_\phi$ 为训练好的阶段级奖励模型。

## 关键实验

| 实验 | 数据集 / 设置 | 主要结论 |
|------|--------------|---------|
| 主实验（直接生成）| 8 个推理 benchmark | 比基础模型平均提升 9.4% |
| 与大模型对比 | MMStar, MathVista | 超越 Gemini-1.5-Pro，GPT-4o-mini，Llama-3.2-90B |
| SWIRES 效果 | MathVista, MMStar | SWIRES 在基础生成上再提升 ~4% |
| 阶段消融 | 4阶段 vs 减少某阶段 | Caption 阶段对视觉推理最关键 |

**最重要的一个数字结果**：仅用 100K 训练数据，LLaVA-CoT 在 8 个推理基准平均提升 9.4%，且超越参数量大 10× 的开源模型。

## 局限

- 四阶段固定结构限制了灵活性（不适合所有任务类型）
- 推理链训练数据由 GPT-4o 生成，引入了 GPT-4o 的偏见
- SWIRES 需要训练额外的阶段级奖励模型

## 个人思考

**方法的核心 insight 是**：将视觉感知（Caption 阶段）和逻辑推理（Reasoning 阶段）显式分离是关键——让模型先"看清楚"再"想明白"，而不是交织在一起。这对多模态 CoT 的设计有重要启示。

**与我研究方向的关联**：LLaVA-CoT 是理解多模态"慢思考"推理的入门论文。它将 o1 风格推理引入多模态，为后续 Vision-R1（[[M2-11]]）等 RL 推理增强工作铺垫了"结构化推理链"的概念。

**延伸问题 / 可能的改进方向**：
- SWIRES 奖励模型如何训练？→ 参见 [[M2-13]] VisualPRM
- 能否用 RL 端到端训练替代 SFT 的四阶段结构？→ 参见 [[M2-11]] Vision-R1

## 与其他论文的联系

- 继承自：[[M2-08]] Multimodal CoT（两阶段框架），o1/o1-style 推理（结构化思维链）
- 被以下工作改进/参照：[[M2-11]] Vision-R1（RL 替代 SFT），[[M2-13]] VisualPRM（奖励模型训练）
- 同期对比：[[M2-10]] AtomThink（原子步骤 vs 四阶段结构）
