# 模块 3：多模态 Agent（次要方向）

## 模块目标

建立对多模态 Agent 核心机制（工具调用、记忆、RAG）的基础认知，了解与组内研究方向（多模态后训练）的结合点。**不深入 GUI Agent 和具身 Agent**。

---

## 子主题与论文清单

| 编号 | 标题（缩写）| 深度 | 时间 | 核心贡献 |
|------|-----------|------|------|---------|
| M3-01 | Large Multimodal Agents Survey | 粗读 | 2.5h | Agent 全景地图，感知/规划/记忆/工具/执行框架 |
| M3-02 | MLLM-Tool | 粗读 | 2.5h | 多模态工具调用基础框架，ToolMMBench |
| M3-03 | Multi-modal Agent Tuning | 粗读 | 2.5h | 自动生成工具调用数据 + SFT 路线 |
| M3-04 | Visual-ARFT | 粗读 | 2.5h | GRPO 赋能 Agent，工具调用的 RL 路线 |
| M3-05 | MemVerse | 粗读 | 2.5h | 多模态双层记忆，层次知识图谱 |
| M3-06 | VisRAG | 粗读 | 2.5h | 视觉 RAG，文档图像直接检索生成，ICLR 2025 |
| M3-07 | Multimodal RAG Survey | 粗读 | 2.5h | 多模态 RAG 全景综述 |
| M3-08 | AvaTaR | 粗读 | 2.5h | 对比推理优化工具调用，成功/失败轨迹自我更新，NeurIPS 2024 |
| M3-09 | HippoRAG | 粗读 | 2.5h | 海马体启发的长期记忆，知识图谱+PageRank 检索，NeurIPS 2024 |
| M3-10 | ColPali | 粗读 | 2.5h | ColBERT 风格视觉文档检索，patch 多向量，ICLR 2025 |
| M3-11 | MRAG-Bench | 粗读 | 2.5h | 视觉中心 RAG 评测，揭示"检索对了但没用好"瓶颈，ICLR 2025 |

**全部粗读，共 11 篇 × 2.5h = 约 27.5h**（含笔记和复习）

---

## 子主题依赖关系

```
Module 1 + 2（前置）
    ↓
M3-01 Survey（整体框架，先读）
    ├── 工具调用分支
    │   M3-02 MLLM-Tool（基础框架）
    │       ↓
    │   M3-03 MAT（自动数据生成+SFT）
    │       ↓
    │   M3-04 Visual-ARFT（RL路线，参考[[M2-12]]）
    │       ↓
    │   M3-08 AvaTaR（对比推理优化，无梯度，NeurIPS 2024）
    │
    ├── 记忆分支
    │   M3-09 HippoRAG（海马体记忆架构，基础）
    │       ↓
    │   M3-05 MemVerse（多模态双层记忆，扩展）
    │
    └── RAG 分支
        M3-06 VisRAG（VLM端到端检索生成）
        M3-10 ColPali（高效patch多向量检索）
            ↓（两者都可以用 MRAG-Bench 评测）
        M3-11 MRAG-Bench（视觉RAG评测基准）
            ↓
        M3-07 Multimodal RAG Survey（全景）
```

---

## 阅读顺序

**Week 8 Day 31（上午）**：M3-01（框架）→ M3-02（工具基础）→ M3-03（SFT路线）→ M3-09（记忆基础，HippoRAG）
**Week 8 Day 31（下午）**：M3-04（RL路线）→ M3-08（对比推理优化）→ M3-05（多模态记忆）
**Week 8 Day 32（上午）**：M3-06（VisRAG）→ M3-10（ColPali）→ M3-11（MRAG评测）→ M3-07（RAG全景）

---

## 与主线的结合点

| Agent 子方向 | 与后训练的结合点 |
|-------------|----------------|
| 工具调用（Tool Use）| 工具调用数据可通过 RL（[[M2-12]] Visual-RFT 范式）训练，而非纯 SFT |
| 记忆（Memory）| 长期对话中记忆的质量评估可用奖励模型（[[M2-07]]）打分 |
| RAG | RAG 检索到的视觉文档可通过多模态 LLM 理解，后训练质量直接影响 RAG 效果 |

---

## 读完本模块应掌握的核心问题

1. 多模态 Agent 的工具调用和纯文本 Agent 的工具调用有什么本质区别？
2. **VisRAG（VLM端到端）和 ColPali（patch多向量检索）代表了视觉文档检索的哪两种路线？各自的优劣是什么？**
3. 工具调用的三条路线（SFT/RL/对比推理优化）各适合什么场景？AvaTaR 的无梯度优化有什么实用价值？
4. HippoRAG 的海马体记忆架构如何启发了 MemVerse 的多模态双层记忆设计？
5. MRAG-Bench 揭示的"检索对了但没用好"说明多模态 RAG 的真正瓶颈在哪里？如何通过后训练来提升？
