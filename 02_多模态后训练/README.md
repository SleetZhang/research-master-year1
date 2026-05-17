# 模块 2：多模态后训练（主攻方向）

## 模块目标

系统掌握多模态 LLM 在 SFT 之后的全套后训练方法论：偏好对齐（RLHF/DPO）、奖励模型构建、推理增强（CoT/RL/PRM）、幻觉缓解。这是你的核心科研方向，每篇精读论文都应该写详细笔记并提出延伸思考。

---

## 子主题与论文清单

### 子方向 A：偏好对齐（RLHF / DPO / RLAIF）

| 编号 | 标题（缩写）| 深度 | 时间 | 核心贡献 |
|------|-----------|------|------|---------|
| M2-01 | LLaVA-RLHF | 精读 | 9h | 多模态 RLHF 开山，Factually Augmented 奖励模型 |
| M2-02 | RLHF-V | 精读 | 9h | Dense human feedback + DPO，1.4K 数据减少 34.8% 幻觉 |
| M2-03 | Silkie/VLFeedback | 精读 | 9h | AI feedback 规模化，GPT-4V 评分构建 82K 偏好数据集 |
| M2-04 | mDPO | 精读 | 9h | 发现并解决多模态 DPO 的 unconditional preference 问题 |
| M2-05 | RLAIF-V | 精读 | 9h | 分治法 AI 自我对齐，超越 GPT-4V 可信度 |
| M2-06 | Calibrated Self-Rewarding | 精读 | 9h | 视觉校准的自我奖励迭代，NeurIPS 2024 |

### 子方向 B：奖励模型

| 编号 | 标题（缩写）| 深度 | 时间 | 核心贡献 |
|------|-----------|------|------|---------|
| M2-07 | XComposer2.5-Reward | 粗读 | 2.5h | 多模态奖励模型工程实现，BoN 测试时计算 |
| M2-18 | Multimodal RewardBench | 粗读 | 2.5h | 奖励模型质量评测，揭示当前上限 |

### 子方向 C：推理增强（CoT / o1-style / RL / PRM）

| 编号 | 标题（缩写）| 深度 | 时间 | 核心贡献 |
|------|-----------|------|------|---------|
| M2-08 | Multimodal CoT | 粗读 | 2.5h | 多模态 CoT 奠基，两阶段推理框架 |
| M2-09 | LLaVA-CoT | 精读 | 9h | 四阶段结构化推理 + SWIRES 搜索，ICCV 2025 |
| M2-10 | AtomThink | 精读 | 9h | 原子步骤推理 + PRM，多模态数学推理突破 |
| M2-11 | Vision-R1 | 精读 | 9h | GRPO + 冷启动，首个多模态 R1 风格 RL |
| M2-12 | Visual-RFT | 精读 | 9h | GRPO + 视觉可验证奖励，数据高效 RL 微调 |
| M2-13 | VisualPRM | 精读 | 9h | 400K 多模态过程监督数据 + BoN 测试时计算 |
| M2-14 | R1-OneVision | 粗读 | 2.5h | 跨模态形式化推理泛化，ICCV 2025 |

### 子方向 D：幻觉与可信性

#### D1：幻觉评测
| 编号 | 标题（缩写）| 深度 | 时间 | 核心贡献 |
|------|-----------|------|------|---------|
| M2-15 | Hallucination Survey | 精读 | 9h | 幻觉分类、成因、评测、缓解的全景综述 |
| M2-16 | HallusionBench | 粗读 | 2.5h | 视觉幻觉+语言幻觉交织的评测基准，CVPR 2024 |
| M2-19 | POPE | 粗读 | 2.5h | 对象幻觉评测基准的祖师爷，Polling-based 二分类评测，EMNLP 2023 |

#### D2：解码时干预（训练无关）
| 编号 | 标题（缩写）| 深度 | 时间 | 核心贡献 |
|------|-----------|------|------|---------|
| M2-20 | VCD | 粗读 | 2.5h | 对比扰动图像 logit 放大视觉信号，无需训练，CVPR 2024 |
| M2-21 | OPERA | 粗读 | 2.5h | 惩罚 summary token 过度信任，注意力层面解码干预，CVPR 2024 Highlight |

#### D3：训练时对齐（DPO/对比学习）
| 编号 | 标题（缩写）| 深度 | 时间 | 核心贡献 |
|------|-----------|------|------|---------|
| M2-17 | HACL | 粗读 | 2.5h | 对比学习缓解幻觉，表示空间视角，CVPR 2024 |
| M2-23 | HII-DPO | 粗读 | 2.5h | 反事实图像+DPO消除场景条件幻觉，2026年最新前沿 |

### 子方向 E：2025-2026 新进展（补充）

| 编号 | 标题（缩写）| 深度 | 时间 | 核心贡献 |
|------|-----------|------|------|---------|
| M2-22 | MM-RLHF | 粗读 | 2.5h | 120K 细粒度偏好数据 + Critique-based RM，2025年最系统的多模态 RLHF |
| M2-24 | Mulberry | 粗读 | 2.5h | 集体 MCTS 构建推理树数据，o1 风格推理+反思，NeurIPS 2025 Spotlight |

**合计：精读 12 篇（129h）+ 粗读 12 篇（30h）= 模块约 159h**（含笔记整理）

---

## 子主题依赖关系

```
Module 1（多模态基础）← 前置
    ↓
【偏好对齐主线】
M2-01 LLaVA-RLHF（RLHF 范式）
    ↓
M2-02 RLHF-V（DPO + Dense Feedback）
    ↓
M2-03 Silkie（AI Feedback 规模化）─→ M2-04 mDPO（条件问题）
    ↓                                      ↓
M2-05 RLAIF-V（自我对齐）←────────────────┘
    ↓
M2-06 Calibrated Self-Rewarding（自我奖励迭代）
    ↓
M2-07 XComposer Reward（奖励模型工程）─→ M2-18 RewardBench

【推理增强主线】
M2-08 Multimodal CoT（起点）
    ↓
M2-09 LLaVA-CoT（四阶段结构化推理）
    ↓
M2-10 AtomThink（原子步骤+PRM）
    ↓
M2-11 Vision-R1（GRPO RL 推理）
    ├→ M2-12 Visual-RFT（RL 扩展到感知任务）
    │       └→ M2-14 R1-OneVision（跨模态泛化）
    └→ M2-13 VisualPRM（PRM 规模化 + BoN）

【幻觉主线】（贯穿偏好对齐和推理增强）
M2-15 Hallucination Survey（全景，先读）
    ├→ M2-19 POPE（对象幻觉评测基准）
    ├→ M2-16 HallusionBench（视觉+语言交织评测）
    ├─【解码时干预路线】
    │   M2-20 VCD（对比 logit）
    │       ↓
    │   M2-21 OPERA（注意力模式干预）
    └─【训练时对齐路线】
        M2-17 HACL（对比学习）
            ↓
        M2-23 HII-DPO（反事实图像+DPO，2026）

【2025-2026 新进展】
M2-22 MM-RLHF（系统性多模态RLHF，读完M2-01后可读）
M2-24 Mulberry（MCTS推理树，读完M2-10/11后可读）
```

---

## 阅读顺序

按以下顺序（尊重依赖 + 难度梯度）：

**Week 2 后半**：M2-01 → M2-02（RLHF基础）
**Week 3**：M2-03 → M2-04 → M2-05 → M2-06（DPO/RLAIF 主线）
**Week 4**：M2-07（粗）→ M2-08（粗）→ M2-09 → M2-10 → M2-11（推理增强）
**Week 5**：M2-12 → M2-13（RFT/PRM）→ M2-14（粗）→ M2-24 Mulberry（粗）
**Week 6-7**：M2-15 → M2-19（粗）→ M2-20（粗）→ M2-21（粗）→ M2-16（粗）→ M2-17（粗）→ M2-23 HII-DPO（粗）→ M2-18（粗）→ M2-22 MM-RLHF（粗）

---

## 读完本模块应掌握的核心问题

1. 多模态 RLHF 和纯文本 RLHF 有哪些本质区别？幻觉奖励 hacking 怎么解决？
2. 为什么标准 DPO 在多模态场景经常失效？mDPO 的解决方案是什么？
3. RLAIF-V 的分治评估和 Silkie 的整体打分有什么本质区别？
4. LLaVA-CoT 的四阶段和 AtomThink 的原子步骤，两种推理结构化方式各适合什么场景？
5. Vision-R1 的 GRPO + 冷启动与 VisualPRM 的 PRM + BoN，这两条推理增强路线的本质区别是什么？
6. 多模态幻觉的主要类型是什么？哪类幻觉用 DPO 最有效？哪类需要 PRM/CoT？
7. **解码时干预（VCD/OPERA）和训练时对齐（DPO）缓解幻觉的本质区别是什么？各自适合什么场景？**
8. MM-RLHF 的 Critique-based RM 相比标准 RM 有什么优势？为什么说"批判先于打分"？
9. Mulberry 的 Collective MCTS 和 VisualPRM 的 Monte Carlo 步骤估计有什么区别？
