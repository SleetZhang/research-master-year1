---
title: "Aligning Large Multimodal Models with Factually Augmented RLHF"
authors: "Zhiqing Sun, Sheng Shen, Shengcao Cao, Haotian Liu, Chunyuan Li, Yikang Shen, Chuang Gan, Liang-Yan Gui, Yu-Xiong Wang, Yiming Yang, Kurt Keutzer, Trevor Darrell"
venue: "ICLR 2024"
year: 2024
arxiv_id: "2309.14525"
reading_depth: 精读
status: 未读
tags:
  - "#多模态/后训练/RLHF"
  - "#多模态/后训练/奖励模型"
  - "#多模态/幻觉/缓解"
prerequisites:
  - "[[M1-03]]"
---

## 核心问题

> 如何将 RLHF 从纯文本域扩展到多模态域，并解决视觉幻觉带来的奖励 hacking 问题？

## 动机

- 多模态 LLM 存在严重幻觉问题（生成与图像不符的描述），但当时没有专门的 RLHF 对齐方法
- 直接将文本 RLHF 搬到多模态会面临"奖励 hacking"：奖励模型可能被语言流畅度欺骗而非真正奖励视觉忠实性
- 人工偏好标注成本高、标注者难以判断视觉幻觉

## 方法

### 总体框架

两阶段方法：(1) 构建 LLaVA-SFT+ 基座（用 VQA 数据增强 SFT）；(2) 构建 Factually Augmented 奖励模型（用 caption + ground truth 信息辅助）；(3) 用 PPO 优化策略。

### 关键模块

1. **LLaVA-SFT+**：在 LLaVA 基础上添加 VQAv2（83K）、A-OKVQA（16K）多轮对话数据，增强模型的事实性
2. **人工偏好数据收集**：标注者比较两个回答，标记幻觉回答为 rejected（10K 对）
3. **Factually Augmented Reward Model（FA-RM）**：
   - 在奖励模型输入中加入图像 caption 和 ground-truth（作为"事实锚点"）
   - 防止奖励模型只看文本流畅度而忽略视觉忠实性
   - 关键公式：$r = r_\text{LM}(x, y) + \alpha \cdot r_\text{VQA}(x, y, c)$（对应论文 Eq.2-3）
4. **PPO 优化**：使用 FA-RM 作为 reward 信号，PPO 策略优化语言模型

### 关键公式 / 算法

Factually Augmented Reward：在标准 LM reward 之上，用 VQA 答案正确率额外惩罚幻觉：

$$r_\text{total}(x, y) = r_\text{RM}(x, y) + \alpha \cdot \mathbb{1}[\text{VQA-correct}(x, y)]$$

## 关键实验

| 实验 | 数据集 / 设置 | 主要结论 |
|------|--------------|---------|
| 主实验 | LLaVA-Bench | LLaVA-RLHF 达到 GPT-4 的 94%（基线 87%）|
| 幻觉评测 | MMHAL-Bench | 较基线提升 60%，显著降低幻觉 |
| 消融：FA vs 普通 RM | LLaVA-Bench | FA reward 避免奖励 hacking，普通 RM 出现 reward hacking |
| 数据量 | 10K vs more | 10K 人工标注已足够 |

**最重要的一个数字结果**：在 MMHAL-Bench 上，LLaVA-RLHF 较 LLaVA-SFT+ 基线提升 60% 幻觉率降低。

## 局限

- PPO 训练复杂，需要4个模型（policy, reference, reward, value）同时运行，显存需求高
- 人工偏好数据仍需 10K 标注，成本不低
- FA-RM 中的 VQA 分量只能处理封闭域答案，难以泛化

## 个人思考

**方法的核心 insight 是**：多模态 RLHF 的关键挑战不是 RL 算法，而是奖励信号质量——用事实锚点（caption/GT）增强奖励模型是防止幻觉奖励 hacking 的关键。

**与我研究方向的关联**：这是多模态后训练领域的开山之作，建立了"多模态 RLHF = 偏好数据 + 奖励模型 + RL 优化"的基本范式。后续 RLHF-V、Silkie、mDPO 都是对这个范式的简化（去掉 PPO）或增强。

**延伸问题 / 可能的改进方向**：
- 能否去掉 PPO，用 DPO 直接优化？→ 参见 [[M2-02]] RLHF-V，[[M2-03]] Silkie
- 如何获得更大规模的偏好数据？→ 参见 [[M2-03]] Silkie（AI feedback），[[M2-05]] RLAIF-V

## 与其他论文的联系

- 继承自：[[M1-03]] LLaVA-1.5（基座），InstructGPT（RLHF 范式）
- 被以下工作改进：[[M2-02]] RLHF-V（dense feedback+DPO），[[M2-03]] Silkie（AI feedback scale up）
- 同期对比：[[M2-02]] RLHF-V（同一年，不同对齐策略）
