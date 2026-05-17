# 模块 1：多模态基础

## 模块目标

补齐 BLIP 之后（2023-2025）多模态大模型架构演进的全景认知，理解从 BLIP-2 / LLaVA 到 InternVL3 / Qwen2.5-VL 的发展脉络，重点掌握：
1. 模态对齐与连接器设计（Q-Former → MLP → Resampler → Native Multimodal）
2. 主流 MLLM 系列的架构差异（LLaVA / Qwen-VL / InternVL / Cambrian）
3. 多阶段训练范式（预训练对齐 → SFT → RLHF）

## 子主题与依赖关系

```
CLIP / ViT（已读）────── BLIP（已读）
        │                    │
        └────────┬───────────┘
                 │
          ┌──────┴───────┐
          │    BLIP-2    │  ← Q-Former 连接器
          │  InstructBLIP│  ← Instruction-aware Q-Former
          └──────┬───────┘
                 │
    ┌────────────┼──────────────┐
    │            │              │
  LLaVA       Qwen-VL      InternVL
  (MLP投影)   (Resampler)   (大规模ViT)
    │            │              │
  LLaVA-1.5   Qwen2-VL     InternVL 1.5
    │         (动态分辨率)   (动态高分辨率)
    │            │              │
    └────────────┼──────────────┘
                 │
          ┌──────┴───────┐
          │   Cambrian-1  │  ← 视觉中心的 MLLM 设计
          │   MM1         │  ← 缩放规律研究
          │   CogVLM      │  ← 视觉专家深度注入
          │   VILA        │  ← 交错数据预训练
          │   NVLM        │  ← 原生多模态
          │   Molmo       │  ← 像素级理解
          └──────────────┘
```

## 论文清单与阅读顺序

| 编号 | 论文 | 会议/年份 | arXiv ID | 阅读深度 | 依赖 |
|------|------|-----------|----------|----------|------|
| M1-01 | BLIP-2 | ICML 2023 | 2301.12597 | 精读 | BLIP |
| M1-02 | LLaVA | NeurIPS 2023 Oral | 2304.08485 | 精读 | CLIP, Vicuna |
| M1-03 | InstructBLIP | NeurIPS 2023 | 2305.06500 | 粗读 | BLIP-2 |
| M1-04 | LLaVA-1.5 | CVPR 2024 Highlight | 2310.03744 | 精读 | LLaVA |
| M1-05 | Qwen-VL | arXiv 2023 | 2308.12966 | 精读 | - |
| M1-06 | InternVL | CVPR 2024 | 2312.14238 | 精读 | - |
| M1-07 | CogVLM | arXiv 2023 | 2311.03079 | 粗读 | - |
| M1-08 | VILA | CVPR 2024 | 2312.07533 | 粗读 | LLaVA |
| M1-09 | Cambrian-1 | NeurIPS 2024 Oral | 2406.16860 | 精读 | LLaVA-1.5 |
| M1-10 | MM1 | ICML 2024 | 2403.09611 | 精读 | LLaVA-1.5 |
| M1-11 | InternVL 1.5 | arXiv 2024 | 2404.16821 | 粗读 | InternVL |
| M1-12 | Qwen2-VL | arXiv 2024 | 2409.12191 | 精读 | Qwen-VL |
| M1-13 | NVLM | arXiv 2024 | 2409.11402 | 粗读 | InternVL 2 |

### 推荐阅读顺序

BLIP-2 → LLaVA → InstructBLIP → LLaVA-1.5 → Qwen-VL → InternVL → CogVLM → VILA → MM1 → Cambrian-1 → InternVL 1.5 → Qwen2-VL → NVLM

详细论文信息见 `../学习计划.md` 的完整论文清单表格。
