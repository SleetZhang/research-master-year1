# 模块 1：多模态基础

## 模块目标

补齐从 BLIP → 2024 年主流 MLLM 的知识断层，建立架构演进的全景认知。理解"视觉编码器 + 连接器 + LLM"三件套的设计权衡，为后续后训练模块打好基础。

---

## 子主题与论文清单

| 编号 | 标题（缩写）| 深度 | 时间 | 核心贡献 |
|------|-----------|------|------|---------|
| M1-01 | InstructBLIP | 粗读 | 2.5h | Q-Former 指令感知化，BLIP 后首个通用多模态指令微调 |
| M1-02 | LLaVA（原始）| 精读 | 9h | 视觉指令微调开山，GPT-4 合成多模态数据范式 |
| M1-03 | LLaVA-1.5 | 精读 | 9h | MLP 投影 + 混合数据，简单改动建立强基线 |
| M1-04 | InternVL 1.0 | 粗读 | 2.5h | 大视觉编码器（6B），渐进式对齐 |
| M1-05 | Cambrian-1 | 精读 | 9h | 系统比较 15+ 视觉编码器，SVA 连接器，NeurIPS 2024 Oral |
| M1-06 | LLaVA-OneVision | 粗读 | 2.5h | LLaVA 系列集大成，AnyRes 高分辨率 |
| M1-07 | Qwen2-VL | 粗读 | 2.5h | Naive Dynamic Resolution，动态 token 数量 |
| M1-08 | Molmo | 粗读 | 2.5h | 人工语音标注数据，数据质量的极致体现 |
| M1-09 | InternVL 2.5 | 粗读 | 2.5h | 当前开源 MLLM 最强基座，突破 MMMU 70% |
| M1-10 | ShareGPT4V | 粗读 | 2.5h | 高质量 GPT-4V caption 数据对模型的系统影响 |
| M1-11 | MMMU | 粗读 | 2.5h | 多学科多模态评测标准，必须了解的基准 |
| M1-12 | InternVL3 | 粗读 | 2.5h | Native Multimodal Pretraining 范式，V2PE 动态位置编码 |
| M1-13 | MM1 | 精读 | 9h | Apple 大规模消融：连接器不重要，视觉编码器+数据才是关键，ICML 2024 |
| M1-14 | BLIP-2 | 粗读 | 2.5h | Q-Former 两阶段预训练，冻结编码器+LLM，InstructBLIP 的直接前置 |

**精读 4 篇（共 36h）+ 粗读 10 篇（共 25h）= 模块合计约 61h**（含笔记整理）

---

## 子主题依赖关系

```
BLIP（已读）
    ↓
M1-14 BLIP-2（Q-Former 预训练，前置知识）
    ↓
M1-01 InstructBLIP（Q-Former 指令感知化）

M1-02 LLaVA（MLP路线起点）← CLIP（已读）
    ↓
M1-03 LLaVA-1.5（MLP路线成熟）
    ├→ M1-05 Cambrian-1（系统比较连接器与编码器）
    │       ↑
    │   M1-13 MM1（更早的系统消融：连接器不重要论）
    ├→ M1-06 LLaVA-OneVision（LLaVA系列演进）
    └→ M1-10 ShareGPT4V（数据质量影响）

M1-04 InternVL 1.0（大视觉编码器路线）
    ├→ M1-09 InternVL 2.5（开源SOTA）
    └→ M1-12 InternVL3（Native Multimodal Pretraining）

M1-07 Qwen2-VL（动态分辨率路线）
M1-08 Molmo（纯数据驱动路线）

M1-11 MMMU（评测，随时参考）
```

---

## 阅读顺序

**按以下顺序阅读**（尊重依赖关系）：

1. M1-14 → M1-01（BLIP-2 → InstructBLIP，Q-Former 完整链条，Week 2 Day 5 上午）
2. M1-02 → M1-03（LLaVA → LLaVA-1.5，MLP 范式，Week 2 Day 5-6）
3. **M1-13 MM1**（精读，Week 2 Day 7，理解消融结论后再读 Cambrian-1）
4. M1-04 → M1-05（InternVL → Cambrian-1，Week 2 Day 7-8）
5. M1-06 → M1-07 → M1-08 → M1-09 → M1-10 → M1-11 → M1-12（Week 3 Day 9-10）

**注**：M1-13 MM1 应在 M1-05 Cambrian-1 之前读，两篇结论有争议，先读 MM1 再读 Cambrian-1 可形成对比。

---

## 读完本模块应掌握的问题

1. LLaVA 的两阶段训练和 MLP 连接器设计思路
2. Q-Former（BLIP-2/InstructBLIP）和 MLP 连接器各有什么优缺点？
3. **MM1 说"连接器不重要"，Cambrian-1 说"多编码器融合更好"——两个结论矛盾吗？如何调和？**
4. 动态分辨率（Qwen2-VL）和 AnyRes（LLaVA-OneVision）有什么不同？
5. InternVL3 的 Native Multimodal Pretraining 相比两阶段训练范式的核心优势是什么？
6. 当前（2024-2025）最强开源 MLLM 是哪些，性能瓶颈在哪里？
