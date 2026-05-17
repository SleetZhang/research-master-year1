---
title: "MLLM-Tool: A Multimodal Large Language Model For Tool Agent Learning"
authors: "Chenyu Wang, Weixin Luo, Sixun Dong, Xiaohua Xuan, Zhengxin Li, Lin Ma, Shenghua Gao"
venue: "WACV 2025"
year: 2025
arxiv_id: "2401.10727"
reading_depth: 粗读
status: 未读
tags:
  - "#多模态/Agent/工具调用"
  - "#多模态/Agent/函数调用"
prerequisites:
  - "[[M1-06]]"
  - "[[M3-01]]"
---

## 核心贡献（3 句话）

1. **做了什么**：构建 ToolMMBench（932 个 ML API）和 MLLM-Tool 系统，使多模态 LLM 能根据图像+文字指令选择合适的工具 API 并执行。
2. **怎么做的**：从 HuggingFace 收集 932 个高质量 ML API，构建多模态工具调用数据集；将工具选择形式化为分类问题，在 MLLM 上进行指令微调使其理解多模态工具调用语义。
3. **效果如何**：WACV 2025，在 ToolMMBench 上显著优于纯文本 LLM 的工具调用，证明视觉信息对正确工具选择的重要性。

## 与主线的关系

> 多模态工具调用的基础框架论文。了解"多模态输入 → 工具选择 → API 执行"的基本流程，为读更高级的 Agent 工作打基础。

- 被以下工作扩展：[[M3-03]] Multi-modal Agent Tuning（更自动化的工具数据生成）

## 是否值得回头精读

- [ ] 是，原因：
- [x] 否，原因：主要贡献在数据集构建和基础框架，对组内工具调用研究的参考价值有限

## 快速笔记

- ToolMMBench：932 个 HuggingFace ML 相关 API，多模态工具调用基准
- 关键挑战：仅用文字描述无法区分功能相似的 API，必须结合图像输入
- 方法：MLLM 接收图像+文字指令，输出 API 名称和参数（格式化输出）
