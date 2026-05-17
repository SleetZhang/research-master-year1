# 模块 3：多模态 Agent（工具调用 / Memory / RAG）

## 模块目标

建立多模态 Agent 在工具调用、Memory 机制、多模态 RAG 三个方向的基本认知。注意：**不包括 GUI Agent 和具身 Agent**。

## 子主题与依赖关系

```
多模态 Agent
├── 工具调用（Tool Use）
│   ├── MLLM-Tool ───── 多模态工具选择学习
│   ├── HAMMR ────────── 层级化 ReAct 多 Agent 工具组合
│   ├── AvaTaR ───────── 对比推理优化工具使用
│   └── TRACER ───────── 工具使用溯源验证
│
├── Memory 机制
│   ├── HippoRAG ─────── 海马体启发的长期记忆（NeurIPS 2024）
│   ├── MemEye ───────── 视觉中心记忆评测
│   └── MemLens ──────── 长上下文 vs 记忆Agent 对比评测
│
└── 多模态 RAG
    ├── ColPali ───────── VLM 文档检索（ICLR 2025）
    ├── MRAG-Bench ────── 视觉知识 vs 文本知识评测
    ├── mR^2AG ────────── 检索-反思增强生成
    └── MuRAG ─────────── 多模态知识密集型 RAG（ICLR 2024）
```

## 论文清单与阅读顺序

| 编号 | 论文 | 会议/年份 | arXiv ID | 阅读深度 | 依赖 |
|------|------|-----------|----------|----------|------|
| **工具调用** | | | | | |
| M3-01 | MLLM-Tool | WACV 2025 | 2401.10727 | 粗读 | - |
| M3-02 | HAMMR | arXiv 2024 | 2404.05465 | 粗读 | MLLM-Tool |
| M3-03 | AvaTaR | NeurIPS 2024 | 2406.11200 | 精读 | - |
| **Memory 机制** | | | | | |
| M3-04 | HippoRAG | NeurIPS 2024 | 2405.14831 | 精读 | - |
| M3-05 | MemEye | arXiv 2026 | 2605.15128 | 粗读 | HippoRAG |
| M3-06 | MemLens | arXiv 2026 | 2605.14906 | 粗读 | MemEye |
| **多模态 RAG** | | | | | |
| M3-07 | ColPali | ICLR 2025 | 2407.01449 | 精读 | - |
| M3-08 | MRAG-Bench | ICLR 2025 | 2410.08182 | 精读 | - |
| M3-09 | mR^2AG | arXiv 2024 | 2411.15041 | 粗读 | MRAG-Bench |
| M3-10 | TRACER | arXiv 2026 | 2605.09934 | 粗读 | MLLM-Tool |

### 推荐阅读顺序

**工具调用**：MLLM-Tool → HAMMR → AvaTaR
**Memory**：HippoRAG → MemEye → MemLens
**多模态 RAG**：ColPali → MRAG-Bench → mR^2AG
**溯源**：TRACER

三个子方向可以交叉进行，没有严格的先后依赖。

详细论文信息见 `../学习计划.md` 的完整论文清单表格。
