# 模块 0：RL / RLHF / DPO / GRPO 基础补漏

## 模块目标

在进入模块 2（多模态后训练）之前，补齐强化学习基础。模块 2 中的 LLaVA-RLHF（PPO 路线）、mDPO/RLAIF-V（DPO 路线）、Vision-R1/Visual-RFT（GRPO 路线）等核心论文，都以本模块的概念为前置。

**不读本模块直接读模块 2，会反复卡在"为什么这么设计"上。**

---

## 阅读顺序（严格遵循）

```
PPO 教程（1 天，不下载论文）
    ↓
M0-02 InstructGPT（精读 12h）—— RLHF + PPO 完整落地
    ↓
M0-03 DPO（精读 12h）—— 从 RLHF 目标推导 DPO，绕开 PPO
    ↓
M0-04 DeepSeek-R1（粗读 3h）—— GRPO：去 critic、group-relative advantage
```

**演进逻辑**：PPO 是根 → DPO 绕开 PPO（闭式解替代强化学习迭代）→ GRPO 简化 PPO（去掉 critic 网络）。必须先理解 PPO，再读 DPO 和 GRPO。

---

## 论文清单

| 编号 | 标题 | arXiv ID | 深度 | 预计时间 |
|------|------|----------|------|---------|
| M0-01 | PPO 教程（非论文） | — | 教程 | 1 天（8h） |
| M0-02 | InstructGPT | 2203.02155 | 精读 | 12h |
| M0-03 | DPO | 2305.18290 | 精读 | 12h |
| M0-04 | DeepSeek-R1 | 2501.12948 | 粗读 | 3h |

**模块合计**：约 35h（含笔记整理）

---

## M0-01：PPO 基础教程（不下载论文，1 天）

从以下资源任选 1-2 份学习，推荐顺序按列出顺序：

1. **Costa Huang "The 37 Implementation Details of PPO"**
   - 逐行讲透 PPO 实现细节，是目前最接地气的 PPO 教程
   - 搜索关键词：`Costa Huang 37 implementation details PPO`

2. **Lilian Weng "Policy Gradient Algorithms" 博客**
   - 数学推导清晰，覆盖 REINFORCE → Actor-Critic → PPO 的完整演进
   - 搜索关键词：`Lilian Weng policy gradient algorithms blog`

3. **Hugging Face TRL 文档 RLHF 章节**
   - 从工程角度串联 reward model 训练 → PPO 训练的完整流程
   - 搜索关键词：`huggingface TRL RLHF tutorial`

**学完 PPO 教程，需要能回答**：
- PPO 的目标函数长什么样？clip 参数 ε 的作用是什么？
- importance sampling ratio $r_t(\theta) = \pi_\theta(a_t|s_t) / \pi_{\theta_{old}}(a_t|s_t)$ 为什么要 clip？
- KL penalty 和 clip 的关系是什么？哪种更常用？
- critic 网络的作用是什么（估计 value function / baseline）？
- 为什么需要 reference model？KL 散度约束防止的是什么？

---

## M0-02：InstructGPT 精读要点

**核心问题**：RLHF + PPO 的完整落地，是模块 2 所有方法的语言模态前身。

重点理解四个组件的职责：
- **SFT model**：从人类示范数据微调，作为策略初始化
- **Reward model (RM)**：Bradley-Terry 模型，从偏好对（chosen/rejected）学习打分
- **PPO policy（= actor）**：被优化的模型，初始化自 SFT model
- **Reference model**：固定不动的 SFT model 副本，用于计算 KL penalty

KL 惩罚项：$r(x,y) = r_\phi(x,y) - \beta \cdot \text{KL}(\pi_\theta \| \pi_\text{ref})$

**为什么 KL penalty 必不可少**：防止 policy 通过 reward hacking 脱离合理分布（例如输出无意义但 RM 打分高的文本）。

---

## M0-03：DPO 精读要点

**核心问题**：从 RLHF 优化目标用数学推导闭式最优解，彻底绕开 PPO 迭代。

**必须手推的推导**：

1. RLHF 目标：$\max_\pi \mathbb{E}[r(x,y)] - \beta \text{KL}(\pi \| \pi_\text{ref})$
2. 闭式最优解：$\pi^*(y|x) \propto \pi_\text{ref}(y|x) \cdot \exp(r(x,y)/\beta)$
3. 反解奖励函数：$r(x,y) = \beta \log \frac{\pi^*(y|x)}{\pi_\text{ref}(y|x)} + \beta \log Z(x)$
4. 代入 Bradley-Terry 模型，$Z(x)$ 消去，得到 DPO loss：

$$\mathcal{L}_\text{DPO} = -\mathbb{E}\left[\log \sigma\left(\beta \log \frac{\pi_\theta(y_w|x)}{\pi_\text{ref}(y_w|x)} - \beta \log \frac{\pi_\theta(y_l|x)}{\pi_\text{ref}(y_l|x)}\right)\right]$$

**DPO 的优雅之处**：不需要训练独立的 reward model，不需要 PPO 迭代，只需要偏好对数据 + 一次监督训练。但代价是 off-policy：偏好数据是用 $\pi_\text{ref}$ 而非当前 $\pi_\theta$ 生成的。

---

## M0-04：DeepSeek-R1 粗读要点

**只需关注 GRPO 部分**（论文第 2-3 节），不需要读完整篇。

**GRPO 与 PPO 的核心差异**：

| 组件 | PPO | GRPO |
|------|-----|------|
| Critic 网络 | 需要（额外训练） | 不需要 |
| Advantage 估计 | GAE（Generalized Advantage Estimation）| group-relative：同一 prompt 多次采样，归一化组内 reward |
| 适用场景 | 通用 RL | 有明确 reward 信号的推理任务（数学、代码、格式验证）|

**GRPO advantage 估计**：对同一 prompt $x$ 采样 $G$ 个输出 $\{y_i\}$，advantage = $(r_i - \text{mean}(r)) / \text{std}(r)$，不需要 value function。

---

## 读完本模块应掌握的核心问题

1. PPO 中 clip 的作用是什么？为什么不能无约束地最大化 reward？
2. DPO loss 是怎么从 RLHF 目标一步步推导出来的？每一步的关键假设是什么？
3. GRPO 为什么能去掉 critic？group-relative advantage 的计算方式是什么？
4. DPO 和 GRPO 分别解决了 PPO 的哪个痛点？
5. 为什么 InstructGPT 需要 reference model 而 DPO 不需要单独的 RM 网络？
