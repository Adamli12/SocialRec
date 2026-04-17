---
theme: seriph
layout: cover
title: Social Recommendation
---

# Social Recommendation: Related Work & Future Directions

2026/03/30

---

# Background: Social Recommendation

- Social interactions 可以预测正/负链接 (2010 WWW)
- 核心思路: 利用 social relations ($u$-$u$ edges) 提升推荐质量
- 与 CF 的区别: 除 $u$-$i$ 边外 加入 $u$-$u$ edge interaction
- 传统方法: CF + social regularization (TrustMF, SocialMF, SoRec, 2008–2011)

---

# Related Work: GNN-based Methods

- GNN 建模 user-user edge: GraphRec (2019), DiffNet/DiffNet++ (2019–2020)
- Hypergraph: DHCF (2020), MHCN (2021)
- 最新进展:
  - GBSR (2024 KDD): Graph Bottlenecked Social Recommendation
  - SGSR (2025 TKDE): Score-based generative diffusion
  - ARD-SR (2025 WWW): Diffusion model for robust social rec
- 趋势: social regularization → GNN → Hypergraph → Diffusion models

---

# Research Gap: Social × Semantic ID

- **调研**: Social Rec × Semantic ID 相关工作较少
  - SID 社区（TIGER, LC-Rec, LETTER, LLM-BS, SETRec, RPG等）22年开始，与 Social Rec 社区（GraphRec, DiffNet, GBSR）任务早期就有，GNN从19年开始
  - 4 篇 survey/handbook 均未提及另一方: GRID handbook (2025), VQ4Rec (2024), GNN Social Rec (2024), Gen. Rec. (2025)
  - 接近方向: 
    - MMQ (WSDM 2026) 等方法多模态 SID (text+image)
    - ULMRec, UserLLM 等方法试图建模User Profile到SID
- SID 中已有 user encoding 方法（user prompt, user token 对齐），但关注 per-user 个性化，不涉及 user 间结构化关系
- 多模态 SID (text+image→codebook) 已有成熟方案（MMQ, MQL），但无工作将 social 作为"模态"


---

# Insight: Social 的独特性 — 推理逻辑

- **核心问题**: Social 和其他 user feature 有什么本质不同？
- **Step 1**: Social 是 user-level feature，不是 item-level feature
  - Social 信息不能编入 history item 交互序列的 tokens（它是 user-user 关系）
  - SID 已有很多 user encoding 方法，social 理论上也可以用
- **Step 2**: Social signal 不是属于单个 user 的 feature
  - 普通 user feature（年龄、性别）独立于其他 users
  - Social 是一个**图结构**，编码 users 之间的**相互依赖关系**
- **Step 3**: Social signal 属于协同信号
  - Social 本身是协同信号（user-user interaction），和 CF 类似，可被预测作为辅助任务

> **定位**: 以 framework/plugin 形式为 SID 增加 social module —— 但不能简单套用多模态 SID（social 不是 per-item 的）以及user-centric SID（social 不是每个user独立的）

---

# 3 个 candidate 方向

- **#1: Social as Structured Collaborative Prior** （比较可操作）
  - Social graph 提供协同信号，编码 cluster/edge
  - 本质区别: 多模态 SID 各模态是 per-item 的，social 是 per-graph-structure 的
  - **Plugin 兼容性好**: 只需在 user encoder 侧加入 social 模块，配合social信号监督的辅助任务，不改变 tokenization 或 generation，可接入任何 SID backbone
- **#2: Zero-shot Generation via Social** （好讲故事）
  - Social connections 让 SID 模型获得冷启动能力 — 新 user 有 social link → neighbor 行为作 behavioral anchor → 跳过behavior sequence直接生成 token
  - 解决 user-side 冷启动 — text/image 解决 item-side，social 解决 user-side，也可以引入userrelated item信息
  - 可能更多在推理时引入
- **#3: Inter-user Generation Dependency** （难度较高）
  - 去掉"每个 user 独立生成"的基本假设 — 现有 SID 中从未考虑
  - Socially connected users 的生成过程应有联合分布
  - 实现难度高，需修改生成过程本身

---

# Method Candidates — Overview

- **目标**: 设计 framework/plugin，为现有 SID 模型增加 social module
- **对比表格**:

  | Approach | Core Idea | Insight |
  |----------|-----------|---------|
  | Social Auxiliary Task | SID 训练 + link prediction 辅助任务 | #1 |
  | Social Codebook | 经典方法的 embedding 引入 codebook | #1, #3 |
  | Beam Search | 在infer/beamsearch阶段 引入 social prior | #2 |

---

# Method Candidates — 具体设计

- **Social Auxiliary Task**
  - 在 SID 训练中加入 social supervision (link prediction)
  - 之前已有经典文献formulate task/setting:
    - 2010 WWW + 2011 WSDM social regularization, 让 user encoder 隐式学到 social-aware 表示
- **Social Codebook**
  -  DiffNet/GraphRec 预训练 social embedding → 引入 user-centric 的 codebook 初始化/学习的过程
- **Beam Search**
  - 引入策略不能太简单（social neighbor 偏好 ≠ target user 偏好）

---

# Datasets & Experimental Setup

| Dataset | Users | Items | Interactions | Trusts |
|---------|-------|-------|--------------|--------|
| Ciao | 7,634 | 18,663 | 145,207 | 57,144 |
| Epinions | 42,326 | 53,552 | 576,286 | 355,813 |
| Yelp | 19,535 | 15,045 | 440,526 | 143,765 |

- **预处理**: Rating $\geq$ 4, interaction $\geq$ 3, chronologically leave-one-out
- **评估指标**: Hit@10, NDCG@10
- **注**: 调研了 5 个数据集（含 Douban, DoubanBook）最终选取了 3 个用的较广序列性较强的

---

# Preliminary Experiments

| Dataset | BPR Hit@10 | BPR NDCG@10 | SASRec Hit@10 | SASRec NDCG@10 |
|---------|-----------|-------------|---------------|----------------|
| Ciao | 0.0283 | 0.0147 | 0.0333 | 0.0175 |
| Epinions | 0.0420 | 0.0228 | 0.0441 | 0.0248 |
| Yelp | 0.0365 | 0.0176 | 0.0554 | 0.0301 |

- 三个数据集序列性较强，SASRec 整体优于 BPR
- 这是 baseline 结果，后续在此基础上加入 social signal

---

# Backbone Status & Conclusion

- LC-Rec 在新数据集上调参，当前效果还不理想
  - Ciao/Yelp 无文本 → 用 SASRec embedding 初始化 RQVAE（参考 LLaDA-Rec, ETEGRec）
  - Epinions 用文本初始化 RQVAE
- **下一步**: 在 SID baseline 上实现 Social Plugin

---
layout: center
---

# Part II: 线上实验方向与计划

<br/>

---

# 动机: 流式模型的固有问题

<br/>

- **时效性偏差**: 流式训练更注重近期交互，过度关注 UI 信息，UU（user-user）信息被掩埋

<br/>

- **信息茧房**: 近期关注的内容被推太多，兴趣越推越窄

<br/>

- **核心矛盾**: 模型追求实时性，但牺牲了长期兴趣的多样性

<br/>

> 社交信号天然具有**长期稳定**的特性，恰好可以弥补这一短板

---

# 社交信号的价值

<br/>

- 现有算法（如抖音大盘推荐）本身已较优，但不会对社交关系有强偏好
  - 并非社交关系"不存在"，而是没有被充分利用

<br/>

- 精排模型分布上已有社交信息（或样本本身带有社交关系）
  - 但大盘样本上引入社交信息可能带来更大增益

<br/>

- **社交信号的独特价值**: 作为长期锚点，对抗时效性偏差，打破信息茧房

---

# 方法候选: 线上场景

<br/>

| Approach | Core Idea |
|----------|-----------|
| **辅助任务** | 以辅助任务形式将社交信息引入全部样本，用显式监督替代分布隐式编码（GNN 引入已验证有效） |
| **多跳与异质边 / UU CF** | 通过间接路径建立连接（用户→好友[关注]→好友常看的作者），本质是 user-side CF |
| **User-centric Codebook** | 辅助任务训练 codebook，增强 embedding，结构上做 user-centric 设计 |

---

# 线上实验设计探索

<br/>

- 如何评估社交信号在真实业务指标上的增益？

<br/>

- 与现有模型的对比实验如何设计？

<br/>

- 精排 vs 大盘样本的效果差异如何量化？

<br/>

<div class="text-center mt-8 text-sm opacity-50">

*待讨论细化*

</div>

---

<div class="text-center text-xl opacity-70">

谢谢 / Discussion Welcome

</div>
