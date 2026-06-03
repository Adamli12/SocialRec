**Social Rec**

# 相关工作

## General Social Interactions
- Predicting positive and negative links in online social networks (2010 WWW)
- Type of social link can be predicted by user edges
- Who says what to whom on twitter (2011 WWW)
- Concentration of attention, homophily within categories of users

## Survey
- 早期
  - Social recommendation: a review (2013)
  - A survey of collaborative filtering based social recommender systems (2014)
- GNN方法
  - A survey of graph neural network based recommendation in social networks (2023, latest paper 2023)
  - Social recommender systems: Techniques, domains, metrics, datasets and future scope (2020)
  - A Survey of Graph Neural Networks for Social Recommender Systems (2024, latest paper 2022)

## Traditional Social Recommendation
- CF方法加social regularization，或者co-factorization: TrustMF, SoRec, SocialMF等
- **TrustMF** — A matrix factorization technique with trust propagation for recommendation in social networks (2010 Recsys)
- **SocialMF** — A matrix factorization technique with trust propagation for recommendation in social networks (2010 Recsys)
- **SoRec** — Sorec: social recommendation using probabilistic matrix factorization (2008 CIKM)
- **Social Regularization** — Recommender systems with social regularization (2011 WSDM)

## GNN-based
- GNN方法建模user-user edge：GraphRec, DiffNet, DiffNet++等，包括Hypergraph（一条边是多个点的set），最近也有用diffusion model的

### GraphRec
- Graph neural networks for social recommendation (2019 WWW)
- Dataset

![Drawing 0](assets/image1.png)

### DiffNet
- A neural influence diffusion model for social recommendation (2019 SIGIR)
- Dataset
  - Rating >= 4, interaction >= 2, social link >= 2, random split 811
  - Yelp: https://www.yelp.com/dataset
  - Yelp 17237 38342; Flicker 8358 82120

### DiffNet++
- Diffnet++: A neural influence and interest diffusion network for social recommendation (2020 TKDE)
- Dataset
  - Rating >= 4, interaction >= 2, social link >= 2, random split 811
  - Yelp: https://www.yelp.com/dataset
  - Yelp 17237 38342; Flicker 8358 82120; Epinions 18202 47449; Dianping 59426 10224

### SocialLGN
- SocialLGN: Light graph convolution network for social recommendation (2022 Information Sciences)

### DHCF
- Dual Channel Hypergraph Collaborative Filtering (2020 KDD)
- Dataset

![Drawing 1](assets/image2.png)

### MHCN
- Self-Supervised Multi-Channel Hypergraph Convolutional Network for Social Recommendation (2021 WWW)
- Dataset
  - Rating >= 4, 5 fold cross-validation
  - Yelp: https://github.com/Coder-Yu/QRec/tree/master/dataset/yelp2018
  - Yelp 19539 21266; Douban 2848 39586; LastFM 1892 17632

### HGSR
- Hyperbolic Graph Learning for Social Recommendation (2023 TKDE)
- Dataset
  - Rating >= 3 (写错了应该是4), random split 82

![Drawing 2](assets/image3.png)

### GBSR
- Graph Bottlenecked Social Recommendation (2024 KDD)
- Dataset
  - Rating >= 4, random split 82
  - Yelp 19593 (写错了应该是19539) 21266; Douban-Book 13024 22347; Epinions 18202 47449

### HHGSA
- Heterogeneous Hypergraph Neural Network for Social Recommendation Using Attention Network （2025 TORS）

### SGSR
- Score-based generative diffusion models for social recommendations (2025 TKDE)
- Dataset
  - Rating >= 4, interaction >= 3, split 811 (donot know how to split)
  - Ciao & Epinions: https://www.cse.msu.edu/~tangjili/trust.html
  - Dianping: https://lihui.info/data/dianping/
  - Ciao 7317 104975; Epinions 18088 261649; Dianping 147918 11123

### ARD-SR
- Model-Agnostic Social Network Refinement with Diffusion Models for Robust Social Recommendation (2025 WWW)
- Dataset
  - Rating >= 4, interaction >= 3, chronologically split 811
  - Ciao & Epinions: https://www.cse.msu.edu/~tangjili/trust.html
  - Ciao 7291 17876; Epinions 22112 45464; Douban 2668 15940

## Cross-domain Recommendation
- **Embedding & Mapping paradigm**
  - CUT (SIGIR 24)
    - Aiming at the target: Filter collaborative information for cross-domain recommendation
    - User transformation under target domain similarity regularization
- **Unified Distribution paradigm**
  - social 关系与隐式反馈的区别是interaction从ui变为uu，而不存在其他数据分布的实体，所以可能更考虑这种范式
  - CDCDR (SIGIR 25)
    - CD-CDR: Conditional Diffusion-based Item Generation for Cross-Domain Recommendation
    - Domain-shared item representation by unified diffusion model
  - 还有很多和GNN结合的方法，与social rec中对GNN的利用较为相似

## Research Gap: Social × Semantic ID

- **调研**: Social Rec × Semantic ID 相关工作较少
  - SID 社区（TIGER, LC-Rec, LETTER, LLM-BS, SETRec, RPG等）22年开始，与 Social Rec 社区（GraphRec, DiffNet, GBSR）任务早期就有，GNN从19年开始
  - 4 篇 survey/handbook 均未提及另一方: GRID handbook (2025), VQ4Rec (2024), GNN Social Rec (2024), Gen. Rec. (2025)
  - 接近方向:
    - MMQ (WSDM 2026) 等方法多模态 SID (text+image)
    - ULMRec, UserLLM 等方法试图建模User Profile到SID
- SID 中已有 user encoding 方法（user prompt, user token 对齐），但关注 per-user 个性化，不涉及 user 间结构化关系
- 多模态 SID (text+image→codebook) 已有成熟方案（MMQ, MQL），但无工作将 social 作为"模态"

---

# 数据集：Rating matrix + user edges

![Drawing 3](assets/image4.png)

- **Douban** — https://github.com/Coder-Yu/QRec/tree/master?tab=readme-ov-file#related-datasets
- **DoubanBook** — https://github.com/librahu/HIN-Datasets-for-Recommendation-and-Network-Embedding/tree/master/Douban%20Book
- **Yelp** — https://github.com/Coder-Yu/QRec/tree/master/dataset/yelp2018
  - https://github.com/Coder-Yu/QRec/tree/master?tab=readme-ov-file#related-datasets
- **Epinions** — https://www.cse.msu.edu/~tangjili/trust.html
  - https://github.com/Coder-Yu/QRec/tree/master?tab=readme-ov-file#related-datasets
- **Ciao** — https://www.cse.msu.edu/~tangjili/trust.html
  - https://github.com/Coder-Yu/QRec/tree/master?tab=readme-ov-file#related-datasets

**Rating >= 4, interaction >= 3, chronologically leave-one-out**

![Drawing 4](assets/image5.png)
![Drawing 5](assets/image6.png)
![Drawing 6](assets/image7.png)
![Drawing 7](assets/image8.png)
![Drawing 8](assets/image9.png)

|  | User | Item | Inters | Trusts |
| --- | --- | --- | --- | --- |
| Douban | 2664 | 15938 | 535198 | 32705 |
| DoubanBook | 11383 | 21731 | 595393 | 792062 |
| Yelp | 19535 | 15045 | 440526 | 143765 |
| Ciao | 7634 | 18663 | 145207 | 57144 |
| Epinions | 42326 | 53552 | 576286 | 355813 |

---

# 初步实验

|  | **BPR** |  | **SASRec (BPR)** |  | **LC-Rec** |  | **TIGER** |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  | Hit@10 | NDCG@10 | Hit@10 | NDCG@10 | Hit@10 | NDCG@10 | Hit@10 | NDCG@10 |
| Douban | 0.0068 | 0.0042 | 0.1592 | 0.0986 |  |  |  |  |
| DoubanBook | 0.0922 | 0.0534 | 0.0521 | 0.0273 |  |  |  |  |
| **Yelp** | 0.0365 | 0.0176 | 0.0554 | 0.0301 |  |  |  |  |
| **Ciao** | 0.0283 | 0.0147 | **0.0333** | **0.0175** | 0.0251 | 0.0136 | 0.0302 | 0.0158 |
| **Epinions** | 0.0420 | 0.0228 | 0.0441 | 0.0248 |  |  |  |  |

TIGER (Ciao) 使用和GRID框架产生的语义ID，（也基于SASRec embedding）

目前的观察，**Yelp，Ciao，Epinion**这几个数据集序列性会强一些，另外的两个数据集DoubanBook里CF模型表现较好，Douban的效果有些问题

---

# Backbone实现

## LCRec

在Yelp，Ciao，Epinion上跑LC-Rec的结果：
因Ciao的数据集没有文本信息，目前看到有两篇文章用SASRec的embedding初始化RQVAE做tokenization （LLaDA-Rec，ETEGRec）在amazon几个数据集的表现都是不输于文本初始化的RQVAE，我们可以也这么做，然后Epinion&Yelp正常用文本初始化

**RQVAE tokenizer训练**
- Yelp: SASRec_best_20260113_061228.pth
- Ciao: SASRec_best_20260109_010834.pth
- Epinions: SASRec_best_20260116_181427.pth

|  | SASRecCollision_rate | title&rating embCollision_rate |
| --- | --- | --- |
| **Yelp** | 0.0024 | N/A |
| **Ciao** | 0.0498 | N/A |
| **Epinions** | 0.1302 |  |

Ciao的index/generate_indices.py最后一轮迭代：

![Drawing 9](assets/image10.png)

这是用了Sinkhorn算法降低collision了，这个rate等于38/18664

LC-Rec尝试了初步调参，在Ciao上表现目前不够好（目前表现类似BPR比不过SASRec），可能因为他的6个任务不能全部跑，因为没有user文本信息不能跑其中2个。

## TIGER（基于GRID/LETTER框架）

### Ciao数据集
无文本，用SASRec初始化SID，TIGER效果不如SASRec

### Yelp数据集
有许多版本都是从yelp.com/dataset下载的，但他会周期性更新，为了能复现结果，考察了Genrec用的最多的LETTER & P5版本，来源于S^3Rec，在P5 repo中找到rawdata（经验证确实和当前yelp官网直接下是不同的）。
|  | **BPR** |  | **SASRec (BPR)** |  | **TIGER (GRID LETTERSID)** |  | **TIGER (GRID RKMEANSSID)** |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  | Hit@10 | NDCG@10 | Hit@10 | NDCG@10 | Hit@10 | NDCG@10 | Hit@10 | NDCG@10 |
| **Yelp** |  |  | 0.0313 | 0.0157 | 0.0284 | 0.0149 |  | 0.0127(在训练中) |

|  | **BPR** |  | **SASRec (BPR)** |  | **LETTER-TIGER (LETTER LETTERSID)** |  | **TIGER (LETTER RKMEANSSID)** |  | **TIGER (LETTER VANILLA RQVAE)** |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  | Hit@10 | NDCG@10 | Hit@10 | NDCG@10 | Hit@10 | NDCG@10 | Hit@10 | NDCG@10 | Hit@10 | NDCG@10 |
| **Yelp** |  |  | 0.0333 | 0.0166 | 0.0427 | 0.0225 | 0.0368 | 0.0199 | 0.0387 | 0.0202 |

目前SASRec指标比LETTER原文稍好，TIGER指标比原论文差，原论文0.0407，0.0213

---

# 初步思路

## Insight: Social 的独特性 — 推理逻辑

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

## Candidate 方向与 Method

### #1: Social as Structured Collaborative Prior（比较可操作）

Social graph 提供协同信号，编码 cluster/edge。本质区别: 多模态 SID 各模态是 per-item 的，social 是 per-graph-structure 的。

Plugin 兼容性好：只需在 user encoder 侧加入 social 模块，配合 social 信号监督的辅助任务，不改变 tokenization 或 generation，可接入任何 SID backbone。

| Method | 改进对象 | Core Idea | 状态 |
|--------|---------|-----------|------|
| Social Codebook | Codebook (RQ-VAE) | GNN 预训练 social embedding → 融入 codebook 初始化/学习 | 计划中 |
| Social Auxiliary Task | Model (Seq2Seq) | In-batch 对比学习 + social link prediction 辅助任务 | **进行中** |

### #2: Zero-shot Generation via Social（好讲故事）

Social connections 让 SID 模型获得冷启动能力 — 新 user 有 social link → neighbor 行为作 behavioral anchor → 跳过 behavior sequence 直接生成 token。解决 user-side 冷启动（text/image 解决 item-side，social 解决 user-side）。

| Method | 改进对象 | Core Idea | 状态 |
|--------|---------|-----------|------|
| Social Beam Search | Inference (Beam Search) | 推理时用 social neighbor 行为调整 token 概率 | 计划中 |

### #3: Inter-user Generation Dependency（难度较高）

去掉"每个 user 独立生成"的基本假设 — 现有 SID 中从未考虑。Socially connected users 的生成过程应有联合分布。实现难度高，需修改生成过程本身。

覆盖 Codebook（共享表示）+ Model（联合训练），暂无独立 method。

## 改进计划

### Codebook: Social Codebook（编码社交信息至 codebook）

在 RQ-VAE codebook 学习阶段引入 social 信息，使社交关系相似的 user 在 codebook 空间中更接近。

- [ ] 使用 GNN（如 DiffNet/GraphRec）预训练 social embedding
- [ ] 将 social embedding 融入 codebook 初始化或 RQ-VAE 的 residual quantization 过程
- [ ] 不改变下游 Seq2Seq 训练和 Beam Search 推理流程

### Model: Social Auxiliary Task

**问题**: GenRec 框架中 Seq2Seq 模型的训练目标是 next-token prediction (NTP)，没有显式建模 user 间的社交关系。社交好友有相似的消费偏好，这种信号在 NTP 损失中未被利用。

**方法**: 在 NTP 训练中引入 social contrastive auxiliary task — 从 encoder hidden states 构造 user representation，让社交好友的表示更接近（对比学习正例），非好友的表示更远离（负例）。这是一个 plug-in 方案：不改变 codebook、不改变推理逻辑，只增加训练时的监督信号。

**具体实现**: SocialTrainer 在标准 forward 之后，从 `encoder_last_hidden_state` mean-pool + L2-norm 得到 user_repr，然后对 batch 内满足社交关系的用户对计算 contrastive loss，与 CE loss 加权求和作为 total loss。

- [x] User ID 穿透数据 pipeline（data.py `__getitem__` 返回 `user_id`，collator 提取 tensor）
- [x] `social_utils.py`: SocialGraph 类加载无向社交图
- [x] `social_loss.py`: SupCon (term=1) + Mean Sim Diff (term=2)，全矩阵运算
- [x] `finetune.py`: SocialTrainer，`remove_unused_columns=False` 防止 HF Trainer 4.47 移除 user_id
- [x] 冒烟测试通过：DDP 双 GPU 训练无 crash，social_loss 非零
- [x] DDP 性能问题定位：15x 慢的根因是 `CUDA_LAUNCH_BLOCKING=1`（debug 变量未注释），去掉后 social 仅比 vanilla 慢 ~27%（3.7→2.9 it/s @ bs=256）
- [ ] loss_term=1 实验完成并测试
- [ ] 网格搜索：λ ∈ {0.05, 0.1}, τ ∈ {0.07, 0.1}

#### 扩展方向

当前实现是最简单的版本（直接 mean-pool encoder output + in-batch 对比）。可能的改进：
- [ ] 硬负例挖掘（non-friend 但行为相似的用户）
- [ ] Graph Neural Network encoder 替代 mean-pool（如 LightGCN social encoder）
- [ ] 多任务学习：social link prediction + item generation

### Inference: Social Beam Search（推理时引入 social prior）

在 beam search 推理阶段引入 social 信息调整 token 生成概率，使 cold-start user 可借助社交好友的行为 history 进行推荐。

- [ ] 在 beam 的每一步，用 social neighbor 的历史行为调整 token 概率
- [ ] Social connections → 冷启动能力：新 user 有 social link → neighbor 行为作 behavioral anchor
- [ ] 注意：social neighbor 偏好 ≠ target user 偏好，不能直接加权

## LLM 结合方向

- **Social信息融入prompt中**（类似user profile加feature）
  - Who You Are Matters: Bridging Topics and Social Roles via LLM-Enhanced Logical Recommendation (Kuaishou, NeurIPS 2025)

  ![Drawing 10](assets/image11.png)

- **和GNN-based social信息融合**，GNN处理关系，LLM处理user/item文本（比较解耦）
  - LLM-BRec: Personalizing Session-based Social Recommendation with LLM-BERT Fusion Framework (Sony Research, Gen-IR@SIGIR2024)

  ![Drawing 11](assets/image12.png)

- **用户作为agent**
- 目前还没有**结合语义ID**的工作
- 最简单的可以借鉴多模态的工作将社交信息视为一个模态，和内容信息并列做编码
- 也可以设计一些辅助任务，例如Predicting positive and negative links这篇，引入社交领域的监督

## 对公司方法的增强

我们增强codebook和用辅助任务训user-centric social-aware codebook之后，公司里可以用codebook里增强的embedding
