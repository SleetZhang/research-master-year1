# 模块 2：多模态后训练（主攻方向）

## 模块目标

系统掌握多模态大模型后训练技术的完整图谱，包括：
1. **指令微调与 SFT 数据**：数据合成方法、指令多样性、高质量视觉指令数据的构建
2. **偏好对齐（RLHF/DPO/RLAIF）**：从 RLHF-V 到 Silkie、MM-RLHF 的对齐技术演进
3. **推理增强**：多模态 CoT、o1-style 推理、过程奖励模型的探索
4. **幻觉评测与缓解**：从评测基准到缓解方法的完整脉络

## 子主题与依赖关系

```
多模态 SFT 数据
├── LLaVA (已列入 M1)
├── ShareGPT4V ────── 高质量合成 Caption
├── ALLaVA ────────── GPT-4V 合成指令数据
└── LLaVA-NeXT ───── 改进的数据混合策略

        │
        ▼
多模态偏好对齐（RLHF / DPO）
├── LLaVA-RLHF ────── 首个多模态 RLHF（事实增强）
├── RLHF-V ────────── 细粒度人类修正 + 密集 DPO
├── Silkie ────────── 大规模 AI 反馈 + DPO（无需人类标注）
├── MM-RLHF ──────── 120K 人类偏好 + 基于 Critique 的奖励模型
├── M3PO ──────────── 强模型引导的偏好优化
├── Re-Align ──────── 检索增强 DPO
└── Continual SFT ──── 连续 SFT 挑战 RLHF 必要性

        │
        ▼
多模态推理增强
├── LLaVA-CoT ─────── 4 阶段结构化推理 + test-time scaling
├── Mulberry ──────── 集体 MCTS 推理树搜索
├── Insight-V ─────── 多 Agent + 迭代 DPO 生成长推理链
└── Insight-V++ ───── 进一步扩展长链推理

        │
        ▼
多模态幻觉
├── 评测
│   ├── POPE ───────── 首个系统性对象幻觉评测
│   ├── HallusionBench ─ 语言幻觉+视觉错觉诊断
│   └── Hallucination Survey ─ 综述
│
└── 缓解
    ├── VCD ─────────── 对比解码（训练无关）
    ├── OPERA ──────── 过度信任惩罚+回退（训练无关）
    ├── HII-DPO ─────── 反事实图像 DPO
    ├── DA-DPO ──────── 难度感知 DPO
    └── MHSA ────────── 注意力引导（训练无关）
```

## 论文清单与阅读顺序

| 编号 | 论文 | 会议/年份 | arXiv ID | 阅读深度 | 依赖 |
|------|------|-----------|----------|----------|------|
| **SFT 与指令数据** | | | | | |
| M2-01 | ShareGPT4V | arXiv 2023 | 2311.12793 | 精读 | LLaVA |
| M2-02 | LLaVA-NeXT | arXiv 2024 | 2401.xxxxx | 粗读 | LLaVA-1.5 |
| **偏好对齐** | | | | | |
| M2-03 | LLaVA-RLHF | arXiv 2023 | 2309.14525 | 精读 | LLaVA, RLHF |
| M2-04 | RLHF-V | CVPR 2024 | 2312.00849 | 精读 | LLaVA, DPO |
| M2-05 | Silkie / VLFeedback | EMNLP 2024 | 2410.09421 | 精读 | LLaVA-1.5, DPO |
| M2-06 | MM-RLHF | arXiv 2025 | 2502.10391 | 精读 | RLHF-V, Silkie |
| M2-07 | M3PO | arXiv 2025 | 2508.12458 | 粗读 | Silkie |
| M2-08 | Re-Align | EMNLP 2025 | 2502.13146 | 粗读 | DPO |
| M2-09 | Continual SFT vs RLHF | arXiv 2024 | 2411.14797 | 精读 | RLHF, SFT |
| M2-10 | RPO (Reflective PO) | arXiv 2025 | 2512.13240 | 粗读 | DPO |
| **推理增强** | | | | | |
| M2-11 | LLaVA-CoT | ICCV 2025 | 2411.10440 | 精读 | LLaVA-NeXT |
| M2-12 | Mulberry | Tech Report | 2412.18319 | 精读 | LLaVA-CoT |
| M2-13 | Insight-V | arXiv 2024 | 2411.14432 | 精读 | LLaVA-NeXT |
| M2-14 | Insight-V++ | arXiv 2025 | 2603.18118 | 粗读 | Insight-V |
| **幻觉评测** | | | | | |
| M2-15 | POPE | EMNLP 2023 | 2305.10355 | 精读 | - |
| M2-16 | HallusionBench | CVPR 2024 | 2310.14566 | 精读 | POPE |
| M2-17 | Hallucination Survey | arXiv 2024 | 2404.18930 | 精读 | - |
| **幻觉缓解** | | | | | |
| M2-18 | VCD | NeurIPS 2024 | 2311.xxxxx | 精读 | - |
| M2-19 | OPERA | CVPR 2024 | 2311.17911 | 精读 | VCD |
| M2-20 | HII-DPO | arXiv 2025 | 2602.10425 | 粗读 | DPO |
| M2-21 | DA-DPO | arXiv 2025 | 2601.00623 | 粗读 | DPO |
| M2-22 | MHSA | arXiv 2025 | 2605.14966 | 粗读 | OPERA |

### 推荐阅读顺序

**第一阶段：SFT 与对齐基础**
ShareGPT4V → LLaVA-RLHF → RLHF-V → Silkie

**第二阶段：高级对齐与推理**
MM-RLHF → Continual SFT vs RLHF → LLaVA-CoT → Mulberry

**第三阶段：幻觉全栈**
POPE → HallusionBench → Hallucination Survey → VCD → OPERA

**第四阶段：前沿跟进**
Insight-V → M3PO → Re-Align → HII-DPO/DA-DPO → MHSA

详细论文信息见 `../学习计划.md` 的完整论文清单表格。
