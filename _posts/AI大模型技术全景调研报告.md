# AI大模型技术全景调研报告（修订版）

> **报告日期**：2026年7月8日
> **修订日期**：2026年7月8日（P0/P1/P2复核修订）
> **数据时效**：截至2026年7月8日，含7月最新发布信息
> **报告范围**：国内外主流大模型模态格局、IDE模型路由机制、MoE混合专家架构、AI基础设施新趋势（MCP协议与CPU价值重估）、推理优化技术栈、后训练对齐、Scaling Law、RAG、Agent框架

---

## 摘要

本报告系统梳理了2026年7月AI大模型领域的技术全景。报告首先对比国内外主流模型的模态分类（纯文本vs多模态），分析差异背后的五重结构性因素；随后以Trae为例剖析IDE工具的模型路由机制；深入讲解MoE混合专家架构在训练与推理中的作用；接着探讨推理优化技术栈与后训练对齐技术；最后探讨正在重塑AI基础设施的趋势——MCP协议对"纯文本模型"概念的消解、Agentic AI时代CPU价值的重估、国产AI芯片生态、RAG与Agent框架。报告附有实操选型框架、评测体系与趋势展望。

---

## 目录

- [第一部分：国内外大模型的模态格局](#第一部分国内外大模型的模态格局)
- [第二部分：IDE工具的模型路由机制](#第二部分ide工具的模型路由机制)
- [第三部分：MoE混合专家架构详解](#第三部分moe混合专家架构详解)
- [第四部分：AI基础设施的新趋势](#第四部分ai基础设施的新趋势)
- [第五部分：实操选型框架与趋势展望](#第五部分实操选型框架与趋势展望)

---

# 第一部分：国内外大模型的模态格局

## 1.1 国内主流模型分类（截至2026年7月）

### 纯文本/文本为主模型

这类模型聚焦文字输入输出，在逻辑推理、代码、长文本处理上更强，但不支持图像/音视频的理解与生成。

| 模型 | 厂商 | 特点 |
|------|------|------|
| DeepSeek（V4-Pro/R1） | 深度求索 | 数学、代码、硬核推理国产第一梯队，全系开源，主模型线（V3/V4/R1）为纯文本，多模态能力几乎为零（注：另有独立的Janus多模态分支，见1.3节） |
| Kimi（K2.6） | 月之暗面 | 200万token超长上下文（约150万字），专注长文本精读（注：K2.5为多模态分支） |
| GLM-5.2 | 智谱AI | 开源最佳（BenchLM 83分），文本+代码为主，另有独立的GLM-5V多模态分支 |

### 多模态模型

| 模型 | 厂商 | 多模态能力 |
|------|------|------------|
| 豆包（Seed-2.1 Pro） | 字节跳动 | 图文/语音/视频全多模态，6月23日发布，编程评测进入第一梯队 |
| 通义千问（Qwen3.7 Max） | 阿里巴巴 | 全模态覆盖，依托通义万相+通义听悟，BenchLM 84分 |
| 文心一言（ERNIE 5.0） | 百度 | 全模态内容生成，适配政企合规场景 |
| 混元（Hy3） | 腾讯 | 强化智能体能力，高性价比+开源，MoE架构（389B总参/52B激活） |
| 讯飞星火（Spark V4.0） | 科大讯飞 | 语音+文本双融合能力突出，语音/方言是独家壁垒 |
| MiniMax（M3） | 稀宇科技 | 情感语音、虚拟数字人、AI视频生成赛道头部 |
| 天工（Skywork 3.5） | 昆仑万维 | AI音乐、短剧、多模态内容创作突出 |

## 1.2 海外及国际排行榜主流模型分类（截至2026年7月）

根据BenchLM 2026年7月7日验证的排行榜：

### 多模态模型（主流趋势）

| 排名 | 模型 | 综合分 | 厂商 | 多模态能力 |
|------|------|--------|------|-----------|
| 1 | Claude Fable 5 | 92 | Anthropic | 基础图文 |
| 2 | 豆包 Seed-2.1 Pro | 91 | 字节跳动 | 图文/语音/视频全多模态 |
| 3 | Gemini 3.1 Pro | 89 | Google | 原生多模态（标杆） |
| 4 | Gemini 3 Pro Deep Think | 88 | Google | 原生多模态 |
| 5 | GPT-5.4 | 87 | OpenAI | 原生多模态 |
| 6 | Claude Opus 4.8 | 85 | Anthropic | 基础图文理解（以代码/推理见长） |
| 7 | Qwen3.7 Max | 84 | 阿里巴巴 | 多模态（国产模型，因进入国际排行榜故一并列出） |

### 纯文本/文本为主模型

| 模型 | 厂商 | 多模态情况 | 定位 |
|------|------|-----------|------|
| Claude Opus 4.8 | Anthropic | 基础图文理解，无视频、无原生图像生成 | 深度推理+低幻觉，代码83.58分全球第一 |
| Mistral Large 3 | Mistral AI | 纯文本（MoE架构，41B激活/675B总参） | 大型推理模型，Apache 2.0开源 |
| Devstral 2 | Mistral AI | 纯文本（代码专用） | 代理编程专用，123B密集模型 |

### 7月即将发布的变动

- **GPT-5.6 Sol**：7月正式发布，TerminalBench 2.1达91.9%
- **Grok 4.5**：7月9日向公众开放，基于1.5万亿参数V9基础模型
- **Gemini 3.5 Pro**：7月17日发布，前端/视觉代码生成跨越式跃升
- **DeepSeek V4正式版**：7月中旬上线，引入峰谷定价策略

## 1.3 原生多模态 vs 拼接多模态——2026年最重要的技术分水岭

"多模态"这个标签本身不够，需区分三种架构路线：

### 三种架构的本质差异

**1. 拼接式（Late-Fusion）**
- 架构："视觉编码器+投影层+语言模型"三明治结构
- 代表：Qwen-VL、早期LLaVA、早期GPT-4V
- 信息损耗高：视觉特征经压缩、转译后，空间结构与细节易丢失

**2. 伪原生（带外挂的改良缝合）**
- 架构：共用语言主干，但保留外置视觉编码器+外置VAE图像分词器
- 代表：DeepSeek Janus、部分厂商的"图文统一模型"
- 短板：图像信息存在双层压缩损耗

**3. 真正的原生统一多模态（Early/Unified Fusion）**
- 架构：单一Transformer处理所有模态，从token级别开始跨模态交叉注意力
- 代表：GPT-5.5/6、Gemini 3.1、商汤SenseNova U1、Qwen3.5-Omni
- 信息损耗低：原始像素/波形直接token化

### 性能差距

| 维度 | 原生统一架构 | 拼接式架构 |
|------|------------|-----------|
| 跨模态理解准确率 | 高34%-65% | 信息损耗达20%以上 |
| 响应速度 | 端到端232-320毫秒 | 多模块串行2秒以上 |
| 模糊图像容错 | 像素与文本互相校准 | 视觉编码器定稿后不可逆 |
| 指代消解 | 像素和token直接建注意力 | 空间信息被压平 |
| 多图联合推理 | 像素级对比 | 各自总结后机械拼接 |

### 多模态生成模型

上述讨论聚焦于多模态**理解**（输入图像/视频→输出文本/代码）。多模态**生成**（输入文本→输出图像/视频/3D）是另一条独立技术线：

| 生成类型 | 代表模型 | 核心架构 | 参考链接 |
|---------|---------|---------|---------|
| 图像生成 | FLUX、Stable Diffusion 3、DALL-E 3、通义万相 | DiT（Diffusion Transformer） | [DiT论文](https://arxiv.org/abs/2212.09748) |
| 视频生成 | Sora、Veo 3、可灵、MiniMax Video | 时空DiT + 物理模拟 | [Sora技术报告](https://openai.com/research/video-generation-models-as-world-simulators) |
| 3D生成 | 3D Gaussian Splatting、TRIPO | NeRF / 3D-GS / 扩散3D | [3D-GS论文](https://arxiv.org/abs/2308.04079) |

2026年的趋势是理解与生成的架构统一：GPT-5.5+、Gemini 3.1已开始用同一模型同时支持理解与生成，不再依赖独立的扩散模型外挂。

## 1.4 国内外差异的五重因素分析

国内外大模型在多模态/纯文本格局上产生差异，是技术、算力、数据、商业、监管五重因素叠加的结果。

### 因素一：算力约束

- **海外**：英伟达H100/H200/Blackwell芯片供给充足，OpenAI拥有超50万张H100专用集群
- **国内**：高端GPU受出口管制，国产昇腾910C性能接近H100但训练效率低30-50%（详见4.3节国产芯片生态）
- 多模态是算力富余者的游戏，算力紧张时先把文本做扎实更划算

### 因素二：技术路线

- **海外主导底层范式**：Transformer优化、RLHF、Agent架构、多模态端到端框架均源自美国
- **国内走工程优化+场景适配**：擅长MoE、动态稀疏、量化压缩、国产化适配

### 因素三：数据质量

- **海外**：拥有开放、多语种、高纯度的学术、代码、书籍数据，高质量英文数据占全球近60%
- **国内**：中文互联网低质、重复、营销内容多；但在中文垂类、行业知识库、方言数据上占绝对优势

### 因素四：商业模式

- **海外（订阅制+API直接付费）**：ChatGPT约7亿周活，多模态能力能直接转化为订阅溢价
- **国内（基础免费+场景变现+项目制）**：C端用户有"免费"预期，专精模型按场景定价更灵活

### 因素五：监管与合规

- **海外**：模型闭源为主，多模态能力可直接通过API售卖
- **国内**：《数据安全法》等要求使金融、政务、医疗偏好私有化部署，倒逼开源轻量化路线
- 多模态的合规成本（深度伪造、图片版权、内容审核）远高于纯文本，形成反向激励
- **AI内容治理**：2026年AI水印与版权追踪技术已进入规范化阶段，多模态内容需嵌入隐形水印并接入审核平台

### 趋势：差距正在缩小

高盛研报指出，2026下半年将是关键拐点，中美通用模型差距有望压缩至3-6个月。但"美国主导基础创新，中国主导应用落地"的长期共存格局大概率不会变。

### 开源vs闭源生态竞争

| 维度 | 闭源阵营 | 开源阵营 |
|------|---------|---------|
| 代表 | GPT、Claude、Gemini | Llama、Mistral、DeepSeek、Qwen |
| 优势 | 能力天花板高、迭代快 | 可私有化部署、可微调、成本低 |
| 劣势 | 数据隐私风险、供应商锁定 | 能力通常落后闭源6-12个月 |
| 趋势 | 通过API+MCP构建生态壁垒 | 通过社区贡献快速追赶，权重格式标准化（GGUF/Safetensors） |

2026年开源模型（DeepSeek V4、Qwen3.7、Llama 4）已在多个评测中逼近甚至超越闭源模型，开源生态对闭源的追赶周期从18个月缩短至6-9个月。

## 1.5 长上下文技术原理

Kimi的200万token、DeepSeek V4的1M上下文等长上下文能力背后，涉及多项关键技术：

| 技术类别 | 具体技术 | 原理 | 参考链接 |
|---------|---------|------|---------|
| 位置编码扩展 | RoPE（旋转位置编码） | 通过旋转矩阵编码相对位置，支持外推 | [RoPE论文](https://arxiv.org/abs/2104.09864) |
| 位置编码扩展 | YaRN | RoPE的频率插值改进，支持超长外推 | [YaRN论文](https://arxiv.org/abs/2309.00071) |
| 位置编码扩展 | NTK-aware插值 | 非均匀频率缩放，保留短距离精度 | [NTK讨论](https://www.reddit.com/r/LocalLLaMA/comments/14lz7j5/) |
| 注意力优化 | Ring Attention | 跨节点分布式注意力，突破单卡上下文上限 | [Ring Attention](https://arxiv.org/abs/2310.01889) |
| KV缓存压缩 | StreamingLLM（attention sink） | 保留头部"锚点"token，丢弃中间冗余 | [StreamingLLM](https://arxiv.org/abs/2309.17453) |
| KV缓存压缩 | H2O（Heavy-Hitter Oracle） | 动态保留重要token的KV缓存 | [H2O论文](https://arxiv.org/abs/2306.14048) |

长上下文的真正挑战不在于"能放多少token"，而在于"长上下文中的信息检索准确率"（Needle-in-a-Haystack评测）。多数模型在32K以内准确率>95%，但超过128K后显著下降。

---

# 第二部分：IDE工具的模型路由机制

## 2.1 为什么需要模型路由

传统"编辑器+AI插件"式工具的根本问题是**模型与任务静态绑定**，导致两个典型后果：

- 高射炮打蚊子：简单代码补全也调用大模型，算力浪费
- 小马拉大车：跨服务重构这种需要全局推理的任务却分给轻量模型

Trae的解法是把"选哪个模型"这件事从用户手里拿走，交给一个智能调度中枢自动决策。

## 2.2 Trae的四层架构与路由位置

Trae采用交互层-智能层-协议层-生态层四层解耦架构，模型路由位于**智能层**：

```
[交互层] 多模态输入 → 标准化封装请求
           ↓
[智能层] ┌─ ① 任务动态识别引擎（分类+打分）
         ├─ ② 多模型动态路由（选模型）    ← 路由核心
         ├─ ③ 项目上下文管理（加载全局上下文）
         └─ ④ 智能体执行（多步任务编排）
           ↓
[协议层] MCP协议统一调用模型/工具
           ↓
[生态层] 插件/智能体扩展
```

## 2.3 任务动态识别

路由的第一步是用轻量语义分类模型给任务打标签：

| 任务类型 | 路由偏好 | 典型场景 |
|---------|---------|---------|
| 中文业务代码生成 | 优先豆包、DeepSeek | 写业务逻辑、中文注释 |
| 复杂架构、跨服务重构 | 优先Claude、GLM-5 | 微服务拆分、遗留系统改造 |
| 算法、数学逻辑推导 | 优先DeepSeek-R1 | 排序算法、数值计算 |
| 前端/UI/视觉转代码 | 优先视觉模型+代码模型组合 | 截图转React组件 |

## 2.4 多模型动态路由

Trae内置多款模型，支持Auto模式智能选择，路由逻辑有三层：

**1. 静态模型池 + 路由策略表**（用户可配置model_routing.yaml）

**2. 三层调度机制**：静态池→动态匹配→故障迁移

**3. Auto模式的智能选择**：
- 非核心任务（代码格式检查）→ 轻量模型，200ms内反馈
- 核心任务（业务逻辑生成）→ 大模型，保证质量
- 跨语言混合开发 → 自动匹配跨语言优势模型

## 2.5 冷热任务分离

| 任务热度 | 特征 | 调度策略 | 响应要求 |
|---------|------|---------|---------|
| 热任务 | 代码补全、语法检查 | 轻量模型实时处理 | <200ms |
| 冷任务 | 架构设计、复杂逻辑生成 | 大模型异步处理 | 秒级可接受 |

## 2.6 免费版与付费版的多模态差异

| 工具 | 免费版图片解析 | 付费版图片解析 | 限制本质 |
|------|--------------|--------------|---------|
| Trae | 不支持 | Pro版支持 | 多模态能力入口置灰，仅付费开放 |
| Qoder | 不支持 | 付费版支持 | 图片解析入口关闭，付费解锁 |
| CodeBuddy | 受限 | 付费版支持 | 免费版仅基础混元模型，无多模型切换 |

### 限制的本质

路由机制本身不收费，但**路由能调用的模型池**是收费分界线。免费版的模型池里没有视觉模型，路由系统再聪明也无处可调。

### 免费版用户的可行策略

- 需要图片功能又不想付费 → 选支持BYOK的工具（如Qoder社区版），自己配多模态模型API
- 只做代码和文本 → 免费版的文本模型能力已足够

## 2.7 "路由"一词的多义辨析

"路由"至少在四个不同语境下被使用，含义完全不同：

| 路由类型 | 发生位置 | 路由对象 | 决策依据 |
|---------|---------|---------|---------|
| IDE内部模型调度 | IDE应用层 | 不同模型API | 任务类型、关键词 |
| MoE专家路由 | 模型内部Transformer层 | 专家子网络 | token隐式特征 |
| 大模型网关路由 | 模型供应商与应用之间 | 不同供应商 | 成本、延迟、可用性 |
| 网络层路由 | 网络传输层 | 网络数据包 | 网络协议 |

三层模型相关路由的层次关系：

```
用户请求
  ↓
[IDE内部路由] → 选GLM还是DeepSeek（语义路由）
  ↓
[网关路由] → 走OpenAI官方还是OpenRouter（成本/可用性路由）
  ↓
[模型内部MoE路由] → 激活哪些专家（token级路由）
  ↓
输出结果
```

## 2.8 图文混合场景的路由实例

以"选了GLM-5.2还能分析图片"为例的完整路由流程：

```
你输入：[截图] + "把这个UI转成React组件"
     ↓
① 交互层：Tab-Cue引擎接收截图
   - 先用CLIP-ViT/OCR提取视觉语义
   - 转成结构化描述："含1个提交按钮、3个输入框、栅格布局"
     ↓
② 智能层：任务识别引擎判定
   - 任务类型 = "前端/UI转代码"
   - 需要能力 = 视觉理解(已完成) + 代码生成
     ↓
③ 多模型动态路由：
   - 视觉理解步骤 → 已由视觉模型(如GLM-5V-Turbo)完成
   - 代码生成步骤 → 路由到你选的GLM-5.2(主推理模型)
     ↓
④ 智能体执行：
   - 把视觉描述作为文本上下文喂给GLM-5.2
   - GLM-5.2生成React组件代码
     ↓
⑤ 输出：含className、useState的jsx文件
```

关键点：你在UI上选的"GLM-5.2"指的是主推理/代码生成模型，但图片解析那一步早已被路由系统分发给了视觉模型。

---

# 第三部分：MoE混合专家架构详解

## 3.1 MoE核心原理

MoE（Mixture of Experts，混合专家模型）是一种神经网络架构设计，核心思想是"不激活全部参数，只调用相关专家"。

### 核心组成

- **专家网络（Experts）**：通常由FFN层构成，每个专家是独立神经网络
- **路由网络（Router/Gating Network）**：轻量级线性层+Softmax（注：DeepSeek-V3改用Sigmoid函数计算token-to-expert亲和度，对选中分数归一化产生门控值，与传统Softmax方案不同）
- **聚合机制（Aggregation）**：加权聚合被选中专家的输出

### 工作机制

每个token只激活Top-K个专家（如DeepSeek-V3总671B参数，每次只激活37B）。

### 标准Transformer vs MoE Transformer

```
标准Transformer每层：
  Input → Attention → FFN(单一) → Output
  （所有参数对每个token都激活）

MoE Transformer每层：
  Input → Attention → Router → [Expert1 | Expert2 | ... | ExpertN]
                          ↓
                    选Top-K个专家 → 加权求和 → Output
  （只有被选中的K个专家参与计算）
```

### 主流MoE模型参数对比

| 模型 | 总参数 | 激活参数 | 专家数 | 每token激活专家 | 激活率 |
|------|--------|---------|--------|----------------|--------|
| DeepSeek-V3 | 671B | 37B | 256路由+1共享 | 8 | ~5.5% |
| DeepSeek V4-Pro | 1.6T | 49B | — | 6 | ~3.1% |
| GLM-5.1 | 754B | 40B | 256 | 8 | ~5.3% |
| Mixtral 8x7B | 47B | 13B | 8 | 2 | ~27.7% |
| 腾讯混元Large | 389B | 52B | 16+1共享 | — | ~13.4% |

> **数据来源**：DeepSeek-V3技术报告 [arXiv:2412.19437](https://arxiv.org/abs/2412.19437)，每层256个路由专家+1个共享专家，Top-8激活。

## 3.2 MoE在训练中的作用

### 解决的核心问题：大模型的"不可能三角"

- 模型大 → 能力强（想要）
- 模型大 → 推理慢（害怕）
- 模型大 → 训练贵（付不起）

MoE的突破：让总参数量很大（知识广），但每次计算量很小（速度快）。DeepSeek-V3训练成本仅$5.5M，同规模稠密模型需$100M+。

### 训练过程中的五个核心机制

**1. 路由器训练**：与模型其他参数一同预训练，通过反向传播优化

**2. 专家特化**：不同专家自然分化出专门化能力（如名词专家、介词专家、代码专家、数学专家）

**3. 负载均衡**：防止"专家坍塌"（少数专家被反复使用，其他闲置）

传统MoE（Switch Transformer等）使用辅助损失函数强制均衡：

```
L_aux = α × N × Σ(f_i × p_i)

其中：
- f_i = 专家i实际接收的token比例
- p_i = 路由器分配给专家i的平均概率
- N = 专家总数
- α = 平衡系数（通常0.01）

L_total = L_task + L_aux
```

> **注意**：此公式为传统MoE方案。DeepSeek-V3已通过无辅助损失方案绕过此机制（见3.4节）。

**4. 专家容量与Token丢弃**（传统MoE机制）：

```
Expert_Capacity = (total_tokens / num_experts) × capacity_factor
```

传统MoE（GShard/Switch Transformer）中，超出容量的token直接通过残差连接传递，对性能影响极小（约0.1%）。**但DeepSeek-V3已实现"不丢弃任何token"**（No Token-Dropping），通过无辅助损失负载均衡+互补序列级辅助损失（α=0.0001）避免了这一问题。

**5. 专家并行**：All-to-All通信，使用DeepSpeed-MoE、FairScale、Megablocks等库

### 2026年训练阶段最新进展

- **超级专家现象**：极少数"超级专家"对性能至关重要，剪枝会导致性能急剧下降
- **专家-路由器耦合损失（ERC Loss）**：字节跳动提出，让路由器准确理解专家能力
- **STAR路由**：结构感知的子空间学习，提高路由稳定性
- **SonicMoE**：Mamba作者团队提出，解决高粒度MoE训练通信瓶颈

## 3.3 MoE在推理中的作用

### 正面作用

**1. 计算量大幅降低**：推理计算量与激活参数成正比，实测豆包吞吐量是同参数稠密模型的3.2倍

**2. 首 token 延迟低**：适合实时交互场景

**3. 高并发场景成本优势**：同样GPU跑更多请求，成本降60%+

**4. 长上下文场景显存优势**：小batch下稀疏激活减少内存访问量

### 硬伤（2026年仍存在）

**1. 显存骗局**：省的是FLOPs，不是显存。所有专家都需常驻显存

**2. 专家负载不均**：部分专家被频繁调用形成热点，GPU利用率失衡

**3. 跨节点通信开销**：128卡以上规模通信开销可达25%

**4. 小Batch推理效率低**：动态路由无法充分利用并行性，反而不如稠密模型

**5. 多步推理稳定性问题**：

| 场景 | MoE问题 | 稠密模型优势 |
|------|---------|------------|
| 长上下文(>64K) | 门控决策准确率下降15% | 全参数参与，上下文一致性好 |
| 多智能体协作编程 | 路由切换导致工具调用链断裂 | 多步任务完成率高21% |
| 仓库级代码修复 | 文件间引用追踪不稳定 | SWE-bench高约4分 |
| 长链路自动化 | 多工具串行调用易出错 | 工具调用能力强1.68倍 |

### 2026年推理优化最新进展

**1. 动态专家调度**（戴尔研究院，2026年7月）：吞吐量提升2.3倍，延迟降低50%，GPU利用率从58%提升至86%

**2. CPU-GPU混合推理**（清华OSDI 2026）：
- 注意力放GPU，路由专家放CPU内存
- INT4 DeepSeek-V3达28 tokens/s
- 双RTX 5090可支持45K prompt在30秒内完成

**3. REAP专家剪枝**（Cerebras，2026年5月）：
- 一次性剪枝50%专家，保留97.6%代码生成能力
- 对生成任务，剪枝优于合并（合并破坏路由器动态调制能力）

## 3.4 DeepSeek的关键架构创新

### 1. 细粒度专家分割

解决经典MoE的**知识混杂问题**：
- 保持总参数不变，把大专家拆成多个小专家
- 同时激活更多专家（如从Top-2变为Top-8）
- 专家组合可能性呈指数级增长

> **示意性说明**：假设传统MoE有8个专家、Top-2激活，组合数为C(8,2)=28种；DeepSeekMoE将每个专家拆分为更细粒度的小专家后，专家数和激活数同步增加，组合数激增。以DeepSeek-V3的实际配置为例：256个路由专家、Top-8激活，理论组合数C(256,8)≈4.7×10¹⁴，远超传统方案。

### 2. 共享专家隔离

解决**知识冗余问题**：
- 划分固定激活的共享专家，专门学习通用知识
- 其他路由专家专注特定领域，无需重复存储常识
- DeepSeek-V3：1个共享专家 + 256个路由专家

### 3. 无辅助损失负载均衡

- 传统方案引入辅助损失强制均衡，但干扰路由精度
- DeepSeek通过**动态偏置项**直接在路由机制中融入负载均衡逻辑
- 配合序列级负载均衡，解决同一序列内token集中分配
- 实现不丢弃任何token（No Token-Dropping）

> **技术细节**：偏置项b_i的更新速度γ前14.3T tokens设为0.001，后500B tokens设为0.0；互补序列级辅助损失的α仅设为0.0001。详见 [DeepSeek-V3技术报告](https://arxiv.org/abs/2412.19437)。

### 4. 节点限制路由（Node-Limited Routing）

DeepSeek-V3引入的路由优化：每个token最多被发送到4个节点（M=4），在分布式训练中减少跨节点All-to-All通信量。

## 3.5 路由策略演进谱系

| 路由策略 | 提出者 | 核心思路 | 优劣 |
|---------|--------|---------|------|
| Top-K路由 | Switch Transformer (Google, 2021) | 选得分最高的K个专家 | 计算高效，易专家坍塌 |
| Noisy Top-K | Shazeer等 (Google, 2017) | 打分时加入高斯噪声 | 提升负载均衡，噪声干扰精度 |
| Soft MoE | Google (2023) | 加权融合所有专家输出 | 无硬选择，计算量大 |
| Expert Choice | **Google (Zhou et al., ICML 2022)** | 专家反向选token | 负载固定均衡，训练复杂度高 |
| Ordinal-Sampling | 2026新研究 | 信任"序位"而非"绝对值"，双层序贯估计 | 边缘专家可"复活"，尚在研究阶段 |

> **更正说明**：Expert Choice路由由Google于2022年提出（论文：*"Mixture-of-Experts with Expert Choice Routing"*，[arXiv:2202.09368](https://arxiv.org/abs/2202.09368)），非DeepSeek原创。DeepSeek的贡献在于细粒度专家分割+共享专家隔离+无辅助损失负载均衡。

## 3.6 DeepSeek-V4最新突破（2026年7月）

### 注意力机制变体谱系

在深入MLA之前，需了解注意力机制的完整演进：

| 变体 | 提出者/时间 | 核心优化 | KV缓存压缩 | 代表模型 |
|------|-----------|---------|-----------|---------|
| MHA（标准多头注意力） | Vaswani 2017 | 基线方案 | 1× | 原始Transformer |
| MQA（多查询注意力） | Shazeer 2019 | 所有头共享K/V | N倍压缩 | PaLM, Falcon |
| GQA（分组查询注意力） | Ainslie 2023 | MQA与MHA的折中 | G倍压缩 | Llama 2/3, Mistral |
| MLA（多头潜在注意力） | DeepSeek 2024(V2) | KV低秩联合压缩 | 最优 | DeepSeek-V2/V3/V4 |
| MLA+RoPE解耦 | DeepSeek 2024 | 解决RoPE与低秩压缩冲突 | — | DeepSeek-V2+ |

> 参考链接：[GQA论文](https://arxiv.org/abs/2305.13245) | [MQA论文](https://arxiv.org/abs/1911.02150) | [MLA论文(DeepSeek-V2)](https://arxiv.org/abs/2405.04434)

### 多头潜在注意力（MLA）

MLA（Multi-head Latent Attention）是**DeepSeek-V2引入**、V3/V4延续的关键架构创新，与DeepSeekMoE并列：

- **核心原理**：对注意力的键（Key）和值（Value）进行低秩联合压缩，将KV缓存压缩到原维度的很小一部分
- **推理优化**：显著减少推理时的KV缓存内存占用，提升推理效率
- **训练无影响**：MLA主要优化推理效率，对训练过程无直接帮助
- **与MoE协同**：MLA解决注意力层的内存瓶颈，MoE解决FFN层的计算瓶颈，两者组合实现整体效率最大化

### 混合注意力机制（CSA + HCA）

- **CSA（压缩稀疏注意力）**：KV条目压缩为1/4，负责局部重点信息
- **HCA（重度压缩注意力）**：极小计算量补充全局结构感知
- 两者交替使用：CSA负责细节，HCA负责全局
- 效果：相比V3.2，V4-Pro单token推理仅需27%的FLOPs、10%的KV缓存内存

### 流形约束超连接（mHC）

- 传统残差：输出=输入+层处理，比例固定1:1
- mHC：可学习的线性变换，模型自主控制新旧信息保留比例

### V4的MoE配置

| 参数 | V3 | V4-Pro | V4-Flash |
|------|-----|--------|----------|
| 总参数 | 671B | 1.6T | 121B |
| 激活参数 | 37B | 49B | 21B |
| 激活率 | ~5.5% | ~3.1% | ~17.4% |
| 上下文 | 128K | 1M | 1M |

## 3.7 "2%激活"的真相

流传甚广的"GPT-4使用1.8万亿参数中的2%"需要澄清：

1. **仅指MoE层中的专家FFN参数**：注意力权重、词表嵌入等共享层是全程激活的
2. **MoE层可能只占全模型参数的30%左右**：实际激活的FFN参数占比可能低至0.47%
3. **激活比例是动态的**：短提示下低至1.7%，长对话中升至3.1%
4. **"参数驻留但计算稀疏"**：未激活的专家权重仍在显存里——省的是FLOPs，不是显存

## 3.8 MoE发展历史脉络

| 时间 | 里程碑 | 贡献 |
|------|--------|------|
| 1991 | Jacobs, Jordan, Nowlan & Hinton | 首次提出MoE概念（*Adaptive Mixtures of Local Experts*） |
| 2017 | Shazeer等 | Sparsely-Gated MoE，首次用于LSTM（[论文](https://arxiv.org/abs/1701.06538)） |
| 2020 | Google GShard | MoE嵌入Transformer，引入专家容量约束 |
| 2021 | Switch Transformer (Google) | 简化为Top-1路由，bf16训练，7倍预训练加速 |
| 2022 | Expert Choice Routing (Google) | 专家反向选token，负载均衡（[论文](https://arxiv.org/abs/2202.09368)） |
| 2024 | Mixtral 8x7B (Mistral AI) | 首个高质量开源MoE |
| 2024 | DeepSeekMoE | 细粒度分割+共享专家+无辅助损失均衡（[论文](https://arxiv.org/abs/2401.06066)） |
| 2024 | DeepSeek-V2 | 引入MLA多头潜在注意力（[论文](https://arxiv.org/abs/2405.04434)） |
| 2024 | DeepSeek-V3 | 671B总参/37B激活，训练成本仅$5.5M（[报告](https://arxiv.org/abs/2412.19437)） |
| 2026 | DeepSeek-V4 | CSA/HCA混合注意力+mHC，1.6T总参/49B激活 |
| 2026 | REAP剪枝 | 一次性剪枝50%专家，保留97.6%能力 |

## 3.9 MoE的微调与量化

### MoE微调的特殊挑战

MoE模型微调比稠密模型更复杂，需要针对性策略：

| 挑战 | 解决方案 |
|------|---------|
| 全参数微调成本过高 | LoRA适配器注入专家层 |
| 门控网络需保持稳定 | 冻结路由器，仅微调专家权重 |
| 专家特化易被破坏 | 仅更新Top-K路由索引映射表 |
| 稀疏模型适配性 | 小batch+大学习率更适合MoE微调 |

### 量化技术完整谱系

| 方法 | 精度 | 特点 | 参考链接 |
|------|------|------|---------|
| GPTQ | INT4/INT8 | 训练后量化，基于二阶信息 | [论文](https://arxiv.org/abs/2210.17323) |
| AWQ | INT4 | 激活感知量化，保持关键权重 | [论文](https://arxiv.org/abs/2306.00978) |
| GGUF | INT2-INT8 | llama.cpp格式，端侧部署主流 | [GitHub](https://github.com/ggerganov/llama.cpp) |
| FP8 | 8位浮点 | 训练+推理通用，NVIDIA H100原生支持 | [NVIDIA文档](https://docs.nvidia.com/deeplearning/transformer-engine/user-guide/examples/fp8_primer.html) |
| SmoothQuant | INT8 | 平滑激活异常值 | [论文](https://arxiv.org/abs/2211.10438) |
| 分组量化 | INT4/INT8 | 按专家分组量化，减少精度损失 | — |
| MoE-to-Dense蒸馏 | — | 将稀疏激活行为转化为稠密层参数 | — |

- **INT8量化**：豆包Lite版用INT8将模型体积压缩60%，推理速度提升2.3倍
- **门控网络蒸馏**：用小型稠密网络拟合原始门控逻辑，保留路由决策能力

### 知识蒸馏（Knowledge Distillation）

知识蒸馏是小模型获取大模型能力的核心技术：

| 蒸馏类型 | 原理 | 应用 |
|---------|------|------|
| 输出蒸馏 | 学生模仿教师的softmax输出 | 标准KD |
| 特征蒸馏 | 学生模仿教师的中间层特征 | TinyBERT |
| MoE-to-Dense蒸馏 | 将MoE稀疏行为转化为稠密层 | 生成兼容性强的稠密变体 |
| 数据蒸馏 | 用教师生成训练数据训练学生 | Phi系列、Alpaca |

> 参考链接：[知识蒸馏综述](https://arxiv.org/abs/2006.05525)

## 3.10 MoE的Scaling Law研究

### 大模型整体Scaling Law背景

| 定律 | 提出者/时间 | 核心发现 | 参考链接 |
|------|-----------|---------|---------|
| Kaplan Scaling Law | OpenAI 2020 | 模型性能与参数量、数据量、算力的幂律关系 | [论文](https://arxiv.org/abs/2001.08361) |
| Chinchilla定律 | DeepMind 2022 | 数据量与参数量应等比增长（约20:1） | [论文](https://arxiv.org/abs/2203.15556) |
| 训练后Scaling Law | 2025-2026 | 推理算力投入开始超过训练算力的拐点 | — |

### MoE专属Scaling Law

"Slicing and Dicing"研究对超过2000次预训练实验（模型规模高达6.6B总参数）的系统分析发现：

1. **性能随总MoE参数单调提升**：即使在极端激活比例（如128个专家）下也是如此
2. **最优专家尺寸几乎与总参数数量无关**：仅取决于活跃参数预算
3. **设计简化**：这一发现简化了MoE架构设计——先确定活跃参数预算，再据此决定专家尺寸，最后通过增加专家数量来扩大总参数

这为"增加专家数量而非增大单个专家"的设计哲学提供了实证支撑。

## 3.11 超级专家现象

2026年研究发现在MoE大语言模型中存在**极少数"超级专家"（Super Experts）**：

- 数量极少，但对模型性能至关重要
- 表现出罕见但极端的激活异常值，在decoder层间的隐藏状态中产生巨大激活
- 剪枝它们会导致模型性能急剧下降
- **分布特征**：模型特定的、与数据无关的、不受后训练过程影响
- 是Transformer中系统性异常值机制的主要来源，压缩它们会严重破坏注意力沉没（attention sinks）机制

这一发现对MoE的压缩和剪枝策略有重要指导意义：超级专家不可剪枝，其他冗余专家可以。

## 3.12 非Transformer架构：Mamba/SSM

2023-2026年，非Transformer架构作为替代路线受到关注：

| 架构 | 提出者 | 核心思想 | 优势 | 劣势 | 参考链接 |
|------|--------|---------|------|------|---------|
| Mamba (S6) | Gu & Dao 2023 | 选择性状态空间模型，线性时间复杂度 | 长序列推理快、显存恒定 | 短序列不如Transformer，生态不成熟 | [论文](https://arxiv.org/abs/2312.00752) |
| Mamba-2 | Gu & Dao 2024 | 改进SSD（状态空间对偶） | 更快训练，与注意力统一 | 仍需验证大规模能力 | [论文](https://arxiv.org/abs/2405.21060) |
| Jamba | AI21 2024 | Mamba+Transformer混合 | 兼顾长序列与短序列性能 | 架构复杂，落地少 | [论文](https://arxiv.org/abs/2403.19887) |
| RWKV | RWKV社区 | 线性注意力RNN | 推理效率高，可并行训练 | 能力上限待验证 | [GitHub](https://github.com/BlinkDL/RWKV-LM) |

2026年趋势：纯Mamba在大规模语言模型上尚未取代Transformer，但**混合架构**（Mamba层+注意力层交替）正在成为长上下文优化的新选择。稀疏注意力（Sparse Attention）也在持续演进，与MoE形成互补。

## 3.13 MoE完整知识框架

```
MoE混合专家模型
├── 核心思想：解耦参数量与计算量（稀疏激活）
│
├── 架构组件
│   ├── 路由器/门控网络（轻量，决定token去哪个专家）
│   ├── 专家网络（FFN子网络，各有所长）
│   └── 共享专家（固定激活，处理通用知识）[DeepSeek创新]
│
├── 路由策略演进
│   ├── Top-K（经典，Switch Transformer）
│   ├── Noisy Top-K（加噪声均衡，Shazeer 2017）
│   ├── Soft MoE（加权融合，Google 2023）
│   ├── Expert Choice（专家反向选token，Google 2022）
│   └── Ordinal-Sampling（序位信任，2026新研究）
│
├── 训练阶段
│   ├── 负载均衡：辅助损失 / 无辅助损失（DeepSeek动态偏置）
│   ├── 专家特化：自然分化出数学/代码/语言等专精
│   ├── 专家并行：All-to-All通信
│   └── 超级专家现象：极少数专家至关重要
│
├── 推理阶段
│   ├── 优势：计算量低、吞吐量高、首token延迟低
│   ├── 硬伤：显存不省、负载不均、通信开销、小batch效率低
│   └── 优化：动态专家调度、CPU-GPU混合、REAP剪枝
│
├── 关键创新（DeepSeek体系）
│   ├── 细粒度专家分割（解决知识混杂）
│   ├── 共享专家隔离（解决知识冗余）
│   ├── 无辅助损失负载均衡（不丢弃token）
│   └── V4：CSA/HCA混合注意力 + mHC残差连接
│
├── 微调与量化
│   ├── LoRA注入专家层
│   ├── 冻结路由器仅调专家
│   ├── GPTQ/AWQ/GGUF/FP8/SmoothQuant量化
│   └── MoE-to-Dense蒸馏
│
├── 适用场景
│   ├── 优势：云端高并发、实时对话、简单代码补全
│   └── 劣势：多步推理、长链自动化、小batch本地部署
│
└── 与稠密模型的关系
    └── 双轨制共存：MoE主打云端降本，Dense坚守端侧与高可靠
```

## 3.14 MoE与稠密模型的双轨制共存

2026年的趋势不是"MoE取代稠密"，而是**双轨制共存**：

| 维度 | MoE | 稠密模型 |
|------|-----|---------|
| 适用场景 | 云端高并发、实时对话、简单代码 | 多步推理、长链自动化、小batch本地部署 |
| 优势 | 计算量低、吞吐量高 | 上下文一致性强、工具调用稳定 |
| 劣势 | 显存不省、多步推理易断链 | 计算成本高、推理慢 |

阿里在MoE霸榜的2026年5月仍开源Qwen3.6-27B纯稠密模型，并在数学和代码评测里超越更大的MoE模型——这是双轨制最直观的体现。

## 3.15 后训练对齐技术

模型预训练完成后，需要通过后训练对齐（Post-training Alignment）使其安全、有用、诚实。这是大模型技术栈不可或缺的环节：

| 技术 | 核心思想 | 提出者/时间 | 参考链接 |
|------|---------|-----------|---------|
| SFT（监督微调） | 用高质量指令数据微调基座模型 | 2022 | — |
| RLHF（人类反馈强化学习） | 训练奖励模型+PPO优化策略 | OpenAI 2022 (InstructGPT) | [论文](https://arxiv.org/abs/2203.02155) |
| Constitutional AI | AI自我批评+改进，减少人工标注 | Anthropic 2022 | [论文](https://arxiv.org/abs/2212.08073) |
| RLAIF | AI反馈替代人类反馈 | Anthropic 2023 | [论文](https://arxiv.org/abs/2309.00267) |
| DPO（直接偏好优化） | 免奖励模型，直接从偏好数据优化 | Stanford 2023 | [论文](https://arxiv.org/abs/2305.18290) |
| GRPO（群体相对策略优化） | 用组内相对奖励估计基线，免Critic模型 | DeepSeek 2024 (DeepSeekMath) | [论文](https://arxiv.org/abs/2402.03300) |

### DeepSeek-R1的对齐创新

DeepSeek-R1的训练流程是2026年后训练对齐的标杆案例：

1. **R1-Zero**：纯RL（GRPO）训练，无SFT，验证了RL可以独立激发推理能力
2. **R1**：冷启动SFT → 推理RL → 拒绝采样 → 全场景SFT → 第二轮RL
3. **关键发现**：RL训练中模型自发出现"aha moment"（自我反思行为），无需显式编程

> 参考链接：[DeepSeek-R1技术报告](https://arxiv.org/abs/2501.12948)

---

# 第四部分：AI基础设施的新趋势

## 4.1 推理优化技术栈

2024-2026年，围绕大模型推理部署形成了完整的技术栈。这些技术与MoE架构正交，可叠加使用：

### 核心推理优化技术

| 技术 | 作用 | 代表实现 | 参考链接 |
|------|------|---------|---------|
| PagedAttention | KV缓存分页管理，消除内存碎片 | vLLM | [论文](https://arxiv.org/abs/2309.06180) |
| Continuous Batching | 动态拼batch，提升GPU利用率 | vLLM / TGI | [vLLM文档](https://docs.vllm.ai/) |
| Flash Attention 2/3 | IO感知注意力，降显存+加速 | Tri Dao | [论文](https://arxiv.org/abs/2307.08691) |
| Speculative Decoding | 小模型草拟+大模型校验，降延迟 | Medusa / EAGLE | [Medusa论文](https://arxiv.org/abs/2401.10774) |
| Tensor Parallelism | 跨GPU切分矩阵运算 | Megatron-LM | [论文](https://arxiv.org/abs/1909.08053) |
| Pipeline Parallelism | 跨GPU切分层 | DeepSpeed | [GitHub](https://github.com/microsoft/DeepSpeed) |
| Chunked Prefill | 将长prompt分块处理，避免prefill阻塞decode | vLLM / SGLang | [SGLang](https://github.com/sgl-project/sglang) |

### 主流推理框架对比

| 框架 | 定位 | 优势 | 劣势 |
|------|------|------|------|
| vLLM | 通用高吞吐推理 | PagedAttention，生态最完善 | 对非NVIDIA GPU支持弱 |
| TensorRT-LLM | NVIDIA官方优化 | 极致NVIDIA性能 | 仅NVIDIA，闭源 |
| SGLang | 结构化生成 | RadixAttention缓存复用 | 生态较新 |
| TGI（HuggingFace） | 易部署 | HF生态集成 | 性能略逊vLLM |
| llama.cpp | 端侧/CPU推理 | GGUF量化，跨平台 | 吞吐量低 |
| vLLM-Ascend | 昇腾适配 | 支持华为910B/C | 功能滞后vLLM主分支 |

> 参考链接：[vLLM](https://github.com/vllm-project/vllm) | [TensorRT-LLM](https://github.com/NVIDIA/TensorRT-LLM) | [vLLM-Ascend](https://github.com/vllm-project/vllm-ascend)

## 4.2 MCP协议：消解"纯文本模型"概念

### 4.2.1 什么是MCP

MCP（Model Context Protocol，模型上下文协议）是Anthropic于2024年11月开源的标准化通信协议，2026年已移交Linux基金会Agentic AI基金会托管，成为AI智能体领域的通用工业标准。

一句话定位：**MCP是AI的"USB-C接口"**——让大模型能标准化、安全地调用外部工具和数据源。

> 参考链接：[MCP官方规范](https://modelcontextprotocol.io/specification) | [Anthropic公告](https://www.anthropic.com/news/model-context-protocol)

### 4.2.2 解决的核心痛点：M×N集成灾难

MCP出现前，大模型工具调用完全碎片化：M个AI应用 × N个外部工具，需要写M×N套定制适配代码。MCP将复杂度从乘法降为加法：任意支持MCP客户端的AI应用，可直接对接任意MCP服务端，一次开发、全生态复用。

### 4.2.3 三层架构

**1. Host（主机）——AI应用调度中心**
- 典型实例：Claude Desktop、Cursor、Trae、企业Agent平台
- 职责：加载管理Client连接、承载LLM推理、任务规划、动态工具编排、安全授权

**2. Client（客户端）——通信代理**
- 一对一绑定一个MCP Server，隔离会话
- 建立有状态长连接（本地stdio / 远程SSE/HTTP）
- 握手协商能力、转发请求、维护独立会话上下文

**3. Server（服务端）——能力提供方**
- 本地Server：文件读写、Git、本地数据库
- 远程Server：联网搜索、企业API、CRM、向量库

### 4.2.4 三大核心基元

| 原语 | 控制权 | 作用 | 典型场景 |
|------|--------|------|---------|
| Tools（工具） | 模型主动调用 | 执行操作（有副作用） | 数据库查询、代码运行、API请求 |
| Resources（资源） | 应用/服务器 | 只读数据访问 | 本地文档、数据库表、网页内容 |
| Prompts（提示模板） | 用户复用 | 标准化工作流 | 代码评审模板、数据分析模板 |

### 4.2.5 对"纯文本模型"概念的颠覆性影响

MCP让能力获取方式从"模型是否原生支持"变成了"能否挂载对应工具"：

| 场景 | 前MCP时代 | MCP时代 |
|------|----------|----------|
| 文本模型要看图 | 必须模型内部集成视觉编码器 | 挂一个视觉MCP Server即可 |
| 文本模型要查实时数据 | 必须训练时注入 | 挂搜索MCP Server |
| 文本模型要操作文件 | 不可能 | 挂文件系统MCP Server |

一个纯文本模型 + 视觉MCP Server + 搜索MCP Server的组合，在功能层面可以逼近原生多模态模型。**"纯文本"这个标签正在失去技术意义**。

### 4.2.6 双向通信与传输机制

MCP不只是模型单向调用工具，支持双向通信：

**Client → Server（下行）**：动态枚举工具/资源/模板、按需读取外部上下文、串行/并行调用工具、传递跨工具上下文、发送进度通知/任务取消/错误回滚

**Server → Client（上行，Sampling采样）**：Server可反向发起LLM推理请求，实现：
- 嵌套子智能体、递归工具编排
- 工具执行后需要模型二次解析结果
- 子任务自动拆分、嵌套调用子Agent
- 多工具输出冲突时，调用模型做信息融合校验

**传输层机制**：底层基于JSON-RPC 2.0双向有状态通信，支持两种传输：
- **stdio（标准输入输出）**：本地进程间通信，低延迟，适合本地Server
- **Streamable HTTP（流式HTTP）**：远程通信，支持SSE流式传输（2025-03-26版本起替代原HTTP+SSE方案）

> 参考链接：[MCP传输规范](https://modelcontextprotocol.io/specification/2025-11-25/basic/transports)

### 4.2.7 安全机制

MCP原生定义分层安全模型：
- OAuth 2.1企业级认证
- 内置权限控制、沙箱与认证
- 避免模型越权访问敏感数据
- 完整会话状态、标准化安全审计

### 4.2.8 MCP的边界

MCP挂载的外部能力有两个根本限制：

1. **无法做跨模态联合推理**：视觉MCP Server返回的是"图片描述文本"，而非像素级特征，不能像原生多模态那样让像素和token在同一注意力空间交互
2. **延迟和成本叠加**：每个MCP调用都是一次额外请求

因此，MCP消解的是"功能层面"的纯文本，而非"能力质量层面"的纯文本。简单图文问答场景，文本模型+视觉MCP已够用；复杂视觉推理场景，原生多模态仍有代际优势。

### 4.2.9 生态现状（2026年7月）

- 发起方：Anthropic
- 支持方：OpenAI、Google、微软、百度、阿里、腾讯、字节跳动等主流厂商均已接入
- 协议完全开源，已有大量MCP Server实现
- 火山方舟体验中心已支持MCP能力

## 4.3 RAG（检索增强生成）

RAG是2024-2026年LLM应用的核心模式，与MCP有交叉但独立发展。MCP解决"如何调用工具"，RAG解决"如何接入私有知识"。

### RAG技术栈

| 组件 | 作用 | 代表方案 | 参考链接 |
|------|------|---------|---------|
| 向量数据库 | 存储文档嵌入向量 | Milvus / Qdrant / Pinecone | [Milvus](https://github.com/milvus-io/milvus) |
| 嵌入模型 | 将文本转为向量 | BGE / GTE / Cohere | [BGE模型](https://huggingface.co/BAAI/bge-large-en) |
| 检索策略 | 从向量库召回相关文档 | 稠密检索 / 混合检索 / 重排序 | — |
| 生成 | 将检索结果作为上下文喂给LLM | 任意LLM | — |
| 框架 | 端到端RAG pipeline | LangChain / LlamaIndex | [LangChain](https://github.com/langchain-ai/langchain) |

### RAG vs 长上下文

2026年随着百万token上下文模型普及，"RAG是否过时"成为讨论热点：

| 维度 | RAG | 长上下文（直接塞入） |
|------|-----|-------------------|
| 知识量 | 无上限（向量库可扩展） | 受上下文窗口限制 |
| 检索精度 | 依赖嵌入质量，可能召回不准 | 模型直接处理，精度高 |
| 成本 | 低（只检索相关片段） | 高（处理全部token） |
| 延迟 | 低（检索快） | 高（长prompt处理慢） |
| 适用场景 | 大规模知识库 | 中等规模文档分析 |

2026年共识：两者互补而非替代——长上下文做精读，RAG做大规模检索。

## 4.4 Agent框架

Agent是大模型从"对话工具"走向"自主执行"的关键架构层：

### 主流Agent框架对比

| 框架 | 定位 | 核心特性 | 参考链接 |
|------|------|---------|---------|
| LangChain / LangGraph | 通用LLM应用框架 | 链式调用、图状态机 | [GitHub](https://github.com/langchain-ai/langchain) |
| AutoGen | 微软多智能体框架 | 多Agent对话协作 | [GitHub](https://github.com/microsoft/autogen) |
| CrewAI | 角色化多智能体 | 角色分工+任务委派 | [GitHub](https://github.com/crewAIInc/crewAI) |
| OpenAI Swarm | 轻量多Agent | Handoff机制 | [GitHub](https://github.com/openai/swarm) |
| Dify | 低代码AI应用平台 | 可视化工作流+RAG+Agent | [GitHub](https://github.com/langgenius/dify) |

### Agent设计模式

| 模式 | 描述 | 典型场景 |
|------|------|---------|
| ReAct | 推理+行动交替（Thought→Action→Observation） | 通用任务 |
| Plan-and-Execute | 先规划再执行 | 复杂多步任务 |
| Reflection | 执行后自我反思修正 | 代码生成、写作 |
| Multi-Agent | 多个Agent分工协作 | 软件开发、研究分析 |
| Tool-Use | 工具调用（MCP/Function Call） | 数据查询、代码执行 |

> 参考链接：[ReAct论文](https://arxiv.org/abs/2210.03629) | [Reflection论文](https://arxiv.org/abs/2303.11366)

---

## 4.5 Agentic AI时代CPU的价值重估

### 4.5.1 核心论点

Agentic AI时代，CPU的角色正从"GPU的配角"变成"与GPU并重"。CPU:GPU配比从2024-25年的1:8，将在2030年回归到约2:1，推理集群中接近1:1。

### 4.5.2 最新核心研究进展（2026年5-7月）

**1. 英特尔+佐治亚理工联合论文**

论文《A CPU-Centric Perspective on Agentic AI》对五大代表性Agent工作负载实测：

| 指标 | 传统LLM推理 | 完整Agent链路 |
|------|------------|--------------|
| CPU耗时占比 | 10%-20% | **43.8%-90.6%** |
| GPU耗时占比 | 80%-90% | 10%-50% |
| CPU能耗占比 | 较低 | **~44%（与GPU接近）** |

用户一次看似简单的请求，真正需要GPU做矩阵生成的环节可能仅占总耗时的10%-30%，剩下70%以上的时间都消耗在CPU负责的逻辑处理与系统调度上。

**2. NVIDIA发布Vera：首款"为Agent而生"的CPU**（2026年5月31日）

- 比x86处理器快1.8倍完成任务
- 88个Olympus自研核心，空间多线程，LPDDR5X带宽1.2TB/s
- 采用方：Anthropic、OpenAI、ByteDance、Oracle Cloud Infrastructure
- 黄仁勋："AI agents will be the largest users of computing"

**3. 分离式推理架构（Disaggregated Inference）**

把推理的prefill和decode阶段物理分离到不同硬件池：

| 阶段 | 计算特征 | 适配硬件 |
|------|---------|---------|
| Prefill | 计算密集（FLOPS） | GPU |
| Decode | 内存带宽密集 | 大内存GPU或CPU |
| Router/调度 | 逻辑调度 | CPU |

NVIDIA Dynamo框架已实现这套架构。2026年6月arXiv论文首次用博弈论分析分离式推理，发现自适应路由可使系统效率提升3.1倍。

### 4.5.3 CPU:GPU配比的三阶段演进

| 阶段 | CPU:GPU配比 | 驱动因素 | 时间 |
|------|------------|---------|------|
| 训练主导期 | 1:8 | 矩阵并行，CPU仅做数据加载 | 2020-2024 |
| 基础推理期 | 1:4 | 数据预处理、任务分发需求提升 | 2025-2026初 |
| 智能体时代 | **1:1甚至CPU>GPU** | Agent多环节逻辑调度 | 2026起 |

各方预测数据：
- 英特尔CEO陈立武：客户正从1:8调整至1:1，极端场景可能4:1
- ARM：传统AI数据中心每GW需~3000万CPU核心，Agent时代飙升至1.2亿核心（4倍增长）
- 德勤：推理占AI总算力从2023年1/3提升至2026年2/3

### 4.5.4 CPU为什么在Agent时代变得关键

一个典型Agent任务包含十几个环节：

```
意图识别 → 任务拆解 → 工具调用 → 代码执行 → 结果校验 → 多轮反思 → token生成
   ↑           ↑          ↑          ↑          ↑          ↑         ↑
  CPU         CPU        CPU        CPU        CPU        CPU       GPU
```

只有最后的"token生成"是GPU的主场，其余环节都是CPU负责的逻辑处理与系统调度。

GPU利用率瓶颈实证：头部企业推理集群GPU平均利用率不足40%，中小企业低于15%——瓶颈不在GPU算力，而在CPU的任务排队、KV缓存管理跟不上。高并发下（Batch Size=128），CPU端到端延迟从2.9秒跃升至6.3秒——**系统瓶颈彻底从GPU转向CPU**。

### 4.5.5 CPU架构的三大进化方向

1. **指令集的AI原生升级**：英特尔与AMD联合推出AI计算扩展指令集，单周期矩阵运算提升一个数量级
2. **内存带宽与互联升级**：多通道DDR5 + CXL2.0扩展内存
3. **多线程与并发优化**：海光SMT4（512线程）、NVIDIA Vera空间多线程

### 4.5.6 产业验证

CPU需求爆发已反映在供应链：
- 价格上涨10%-15%
- 交货期从1-2周延长至8-12周
- AMD FY26Q1数据中心收入同比增长57%，2030年CPU TAM上调至1200亿美元
- Arm AGIC CPU发布6周需求翻倍，突破20亿美元

### 4.5.7 需要校正的认知

1. **"CPU并重"≠"CPU替代GPU"**：是分工重新平衡，不是取代
2. **"1:1配比"是集群级**：非单机级1颗CPU配1块GPU
3. **GPU绝对需求仍在增长**：只是CPU增长速度更快

### 4.5.8 与MoE讨论的交叉点

MoE架构与CPU复兴是相互强化的：MoE的稀疏激活特性天然适合CPU大内存+GPU算力的异构分工。清华OSDI 2026论文已实现把MoE路由专家放CPU、注意力放GPU的混合推理系统。

## 4.6 国产AI芯片与软件生态

### 国产AI加速器全景

| 厂商 | 芯片 | 算力对标 | 软件栈 | 参考链接 |
|------|------|---------|--------|---------|
| 华为 | 昇腾910B/910C | 接近H100（训练效率低30-50%） | CANN + MindSpore + vLLM-Ascend | [昇腾官网](https://www.hiascend.com/) |
| 寒武纪 | 思元590 | 接近A100 | Neuware SDK | [寒武纪官网](https://www.cambricon.com/) |
| 海光 | DCU | 接近A100 | ROCm兼容 | [海光官网](https://www.hygon.com/) |
| 摩尔线程 | MTT S4000 | 中端GPU | MUSA | [摩尔线程](https://www.mthreads.com/) |
| 壁仞 | BR104 | 中高端 | BIRENSUPA | [壁仞科技](https://www.birentech.com/) |

### 华为昇腾生态详解

昇腾是国产AI芯片中最成熟的生态：

| 层级 | 组件 | 说明 |
|------|------|------|
| 硬件 | 昇腾910B/910C | NPU架构，达芬奇算子 |
| 驱动 | CANN（Compute Architecture for Neural Networks） | 类似CUDA的算子库+运行时 |
| 框架 | MindSpore / PyTorch适配 | MindSpore原生，PyTorch通过插件支持 |
| 推理 | vLLM-Ascend | vLLM的昇腾适配分支 |
| 模型 | AscendModel支持 | DeepSeek/Qwen等主流模型已适配 |

### 国产化部署DeepSeek的实践要点

- **CANN版本**：需与模型框架版本严格匹配
- **vLLM-Ascend**：功能可能滞后vLLM主分支1-2个版本
- **性能调优**：昇腾的算子覆盖不如CUDA完善，部分自定义算子需手工适配
- **多卡通信**：HCCL（Huawei Collective Communication Library）替代NCCL

### CPU复兴对国内的结构性利好

- 国产CPU（海光C86-5G、华为鲲鹏）在通用计算上的差距比GPU小
- Agent场景对"峰值算力"要求低于训练，对"逻辑调度+内存带宽"要求高
- 信创政策下CPU国产化比GPU国产化更成熟

这可能成为缩小国内外AI差距的另一个结构性变量。

---

# 第五部分：实操选型框架与趋势展望

## 5.1 按任务类型选模型

| 任务特征 | 推荐路线 | 避免 |
|---------|---------|------|
| 复杂逻辑推理、数学、长文档 | 纯文本旗舰（DeepSeek-R1、Claude Opus 4.8、Kimi） | 多模态模型在这几项上往往更弱 |
| 代码生成、重构 | 代码专精模型（DeepSeek、Claude、Devstral 2） | 通用多模态模型 |
| 简单图文问答、OCR、图表提取 | 文本模型+视觉MCP，或拼接式多模态 | 不必追求原生多模态 |
| UI截图转代码 | 原生多模态（Gemini、GPT-5.5）或视觉模型+代码模型组合 | 纯文本模型单独不够 |
| 复杂视觉推理（多图对比、指代消解） | 原生多模态（Gemini、GPT-5.5） | 拼接式会出错 |
| 视频理解、实时音视频交互 | 原生多模态（Gemini 3.1） | 其他都弱 |
| 图像生成 | 专用生成模型（DALL-E、Midjourney、通义万相、FLUX） | 不要用理解型多模态模型生成 |
| 私有知识问答 | RAG（向量库+LLM） | 依赖模型参数记忆 |
| 多步自动化任务 | Agent框架（Dify/LangChain）+工具调用 | 纯对话式调用 |

## 5.2 在IDE工具里判断是否需要路由到视觉模型

- 输入包含截图/设计稿/图片 → 视觉模型必须介入
- 纯代码+文本输入 → 文本模型足够
- 任务是"分析报错截图" → 需要视觉模型先做OCR
- 任务是"根据描述重构代码" → 纯文本模型反而更强

## 5.3 模态数量与模型质量的关系

需要校正一个隐含假设：我们默认了"多模态比纯文本更先进"。但实际上：

- DeepSeek专注文本推理和代码，在编程和数学上反而是国产第一梯队
- Claude多模态弱但代码和长文档能力顶尖
- 做好多模态的模型不一定推理能力强

**"模态数量"和"模型质量"不是正相关**。国内厂商专注文本/代码未必是"落后"，可能是更务实的资源分配。

## 5.4 评测体系与局限性

### 主流评测基准

| 评测基准 | 考察能力 | 参考链接 |
|---------|---------|---------|
| MMLU | 多任务语言理解（57个学科） | [论文](https://arxiv.org/abs/2009.03300) |
| HumanEval | 代码生成（函数级） | [论文](https://arxiv.org/abs/2107.03374) |
| GSM8K | 小学数学推理 | [论文](https://arxiv.org/abs/2110.14168) |
| MATH | 高中/竞赛数学 | [论文](https://arxiv.org/abs/2103.03874) |
| SWE-bench | 仓库级代码修复（真实GitHub Issue） | [论文](https://arxiv.org/abs/2310.06770) |
| TerminalBench | 终端任务执行 | — |
| Needle-in-a-Haystack | 长上下文信息检索 | — |

### 评测的局限性

| 问题 | 说明 |
|------|------|
| 数据污染 | 评测集泄漏到训练数据，分数虚高 |
| 基准覆盖 | MMLU偏知识记忆，不代表推理能力 |
| 采样偏差 | 单次评测随机性大，需多轮取平均 |
| 自评问题 | 厂商自报分数可能选择性披露 |
| 生态适配 | 评测高分不等于实际部署效果好 |

**建议**：选型时不要仅看排行榜分数，应结合自身场景做POC实测。

## 5.5 趋势展望

### 短期（2026下半年）
- GPT-5.6 Sol、Grok 4.5、Gemini 3.5 Pro、DeepSeek V4正式版7月密集发布
- 中美通用模型差距有望压缩至3-6个月
- MCP协议生态进一步成熟，跨厂商多模型编排成为标配

### 中期（2027-2028）
- 原生多模态统一架构成为新模型默认选择
- 分离式推理架构（Disaggregated Inference）规模化落地
- CPU:GPU配比在推理集群中普遍达到1:2至1:1

### 长期（2029-2030）
- 端侧模型强化"文本优先、多模态按需调用"格局
- "纯文本模型"概念可能被MCP完全消解
- CPU在AI算力中的价值占比达到50%
- 国产CPU在Agent场景的竞争力可能缩小国内外AI差距

### 端侧模型：一个被低估的结构性变量

端侧大模型正在改变多模态的部署形态，可能强化而非弱化"文本优先"格局：

- **算力约束**：端侧算力有限（高通骁龙8 Gen4、华为昇腾310B的INT4推理算力约20TOPS/W）
- **7B边缘模型**：推理延迟已低于100ms，准确率可达175B云侧模型的92%
- **端侧模型技术路线**：知识蒸馏（大→小）+ INT4量化（GGUF格式）+ 模型架构优化
- **代表模型**：Phi-4、Gemma 3、Qwen-1.5B/3B、MiniCPM
- **天然倾向"文本优先"**：本地常驻的只能是轻量文本模型，视觉、语音等多模态能力按需调用云端或专用NPU
- **与国内路线同构**：端侧"文本为主+多模态按需"的分工，与国内"轻量化+私有化部署"的技术路线高度契合

这意味着端侧模型的普及可能让国内外多模态格局差异**结构固化**而非缩小：
- 海外：云端原生多模态旗舰 + 端侧轻量多模态（Apple Intelligence）
- 国内：云端纯文本旗舰 + 端侧轻量文本 + 按需调用视觉

## 5.6 值得继续关注的方向

- MoE在多模态模型中的应用（如Llama 4的多模态MoE）
- MoE的Scaling Law（参数规模、专家数量、激活比例的最优配比）
- MoE与Agent的结合（多智能体如何利用专家特化）
- CPU-native AI框架的出现
- CXL内存池化对CPU-GPU内存边界的影响
- 国产CPU在Agent工作负载下的实测表现
- 非Transformer架构（Mamba/SSM）在大规模语言模型上的突破
- 后训练Scaling Law（推理算力vs训练算力的拐点）
- 端侧模型与云端的协同分工

---

## 附录A：完整讨论逻辑链

本报告源自一次连续的技术对话，讨论逻辑链如下：

1. **起点**：国内主流模型中哪些是纯文本、哪些是多模态
2. **延伸**：海外主流模型的同类分类
3. **深入**：国内外差异的原因分析（五重因素）
4. **转折**：IDE工具中选纯文本模型却能分析图片的技术原理
5. **展开**：Trae的模型路由机制详解
6. **验证**：免费版与付费版的多模态差异
7. **反思**：整体逻辑的盲点与补充维度
8. **深化一**：MCP协议详解
9. **深化二**：MoE在训练场景的含义与作用
10. **深化三**：MoE在推理场景的优劣
11. **深化四**：MoE的完整知识体系补充（含微调量化、Scaling Law、超级专家、知识框架图）
12. **深化五**：Agentic AI时代CPU的价值重估
13. **深化六**：DeepSeek MLA多头潜在注意力、端侧模型结构性变量
14. **复核补充**：MCP双向通信/传输层/安全机制、MoE微调与量化、Scaling Law、超级专家现象、端侧模型
15. **修订补充**：Expert Choice归属修正、MLA版本修正、推理优化技术栈、后训练对齐、RAG、Agent框架、国产芯片生态、Mamba/SSM、评测体系、多模态生成、量化技术谱系、注意力变体谱系、长上下文技术原理

---

## 附录B：参考资源来源

> 以下资源按报告章节顺序分类整理。访问日期：2026年7月8日。

### B.1 国内大模型模态格局

1. 大模型技术体系、核心能力与落地应用调研报告. https://juejin.cn/post/7654149755891056691
2. 国产前五通用大模型快速对比（2026年6月）. https://parcadia.blog.csdn.net/article/details/162439045
3. 国产大模型混战2026：谁在卷参数，谁在拼场景？. https://post.m.smzdm.com/p/a95xwzwe/
4. 2026 主流大模型深度测评与横向对比. https://blog.51cto.com/u_17705031/14667089
5. 2026上半年国内通用大模型综合实力TOP10. https://caifuhao.eastmoney.com/news/20260618105758680229790
6. 国内主流大模型全面对比. https://k.sina.cn/article_7857201856_1d45362c00190786t6.html
7. 2026 国产 AI 大模型横评. https://jishuzhan.net/article/2065289201393954817
8. 国产AI助手哪个好用？深度横评2026. https://www.aitozen.cn/2026/04/18/
9. 豆包,元宝,千问、deepseek、文心一言哪个好？2026全方面测评. https://www.51ima.com/news/12668.html

### B.2 海外大模型模态格局

10. Claude Opus 4.7 vs GPT-5.5 vs Gemini 3.1 Pro: April 2026 Flagship Head-to-Head. https://www.swfte.com/blog/claude-opus-4-7-vs-gpt-5-5-vs-gemini-3-1-pro
11. 2026 主流 AI 模型实测横评. https://g.pconline.com.cn/ai/article/1553639.html
12. Llama 4: The Complete Developer Guide (2026). https://codersera.com/blog/llama-4-complete-guide-2026/
13. AI应用大模型三巨头深度对比分析. http://m.toutiao.com/group/7649999735663542838/
14. Grok 4发布:xAI推出新一代旗舰模型. https://cj.sina.cn/articles/view/7857141524/1d452771401903r6g6
15. Meta Llama 4 Review 2026: Scout, Maverick & Behemoth Tested. https://ai-tools-hub.tech/blog/meta-llama-4-review-2026/
16. Mistral Medium 3.5: The 128B Open-Source Flagship for 2026. https://chats-llm.com/en/blog/mistral-medium-3-5-release

### B.3 国内外差异分析

17. 中美AI产业短期差距与2026下半年拐点分析. https://xueqiu.com/9216592857/395046074
18. 国内外大模型的区别与差距. https://jishuzhan.net/article/2044374061103513602
19. 2026国内外主流大模型全景对比. https://jishuzhan.net/article/2017027396340858882
20. 为什么中美头部AI模型的"模型能力差距"已经明显小于"算力供给差距"？. https://xueqiu.com/4954019641/393265158

### B.4 原生多模态 vs 拼接多模态

21. 从"多模型拼接"到"端到端原生多模态"：VITA 3.0上线. https://cloud.tencent.cn/developer/article/2694806
22. 原生多模态：让AI真正"看懂"世界的关键跃迁. https://post.m.smzdm.com/p/az8mvpx0/
23. Grok原生多模态 vs 拼接式多模态. https://segmentfault.com/a/1190000047883184

### B.5 多模态生成模型（新增）

24. Scalable Diffusion Models with Transformers (DiT). https://arxiv.org/abs/2212.09748
25. 3D Gaussian Splatting for Real-Time Radiance Field Rendering. https://arxiv.org/abs/2308.04079
26. Sora技术报告. https://openai.com/research/video-generation-models-as-world-simulators

### B.6 长上下文技术（新增）

27. RoFormer: Enhanced Transformer with Rotary Position Embedding. https://arxiv.org/abs/2104.09864
28. YaRN: Efficient Context Window Extension of Large Language Models. https://arxiv.org/abs/2309.00071
29. Efficient Streaming Language Models with Attention Sinks. https://arxiv.org/abs/2309.17453
30. H2O: Heavy-Hitter Oracle for Efficient Generative Inference. https://arxiv.org/abs/2306.14048
31. Ring Attention with Blockwise Parallelism. https://arxiv.org/abs/2310.01889

### B.7 IDE工具模型路由机制

32. 第二篇：深度拆解 TRAE IDE 四层架构. https://blog.csdn.net/jiangfuofu555/article/details/162514266
33. Trae 官网 - 多模型动态适配. https://trae.aigc.cn/
34. 第四篇：深度解析 TRAE IDE 算力调度架构. https://blog.csdn.net/jiangfuofu555/article/details/162542372

### B.8 IDE工具免费版与付费版差异

35. Trae免费版和付费版有什么区别？. https://m.php.cn/faq/2522573.html
36. 国产AI编程IDE横向实测:Trae、Qoder与腾讯方案对比. https://post.smzdm.com/p/arlnr8dw
37. CodeBuddy免费版有哪些限制？. https://m.php.cn/faq/2499999.html
38. Qoder 社区版正式发布. https://qoder.com/zh/blog/qoder-community

### B.9 MoE混合专家架构（核心论文）

39. Jacobs, Jordan, Nowlan & Hinton (1991). Adaptive Mixtures of Local Experts. https://www.cs.toronto.edu/~hinton/absps/jjnh91.pdf
40. Shazeer et al. (2017). Outrageously Large Neural Networks: The Sparsely-Gated MoE Layer. https://arxiv.org/abs/1701.06538
41. Switch Transformer (Google, 2021). https://arxiv.org/abs/2101.03961
42. Expert Choice Routing (Google, ICML 2022). https://arxiv.org/abs/2202.09368
43. DeepSeekMoE (2024). https://arxiv.org/abs/2401.06066
44. DeepSeek-V2 (引入MLA, 2024). https://arxiv.org/abs/2405.04434
45. DeepSeek-V3 Technical Report (2024). https://arxiv.org/abs/2412.19437
46. DeepSeek-R1 (2025). https://arxiv.org/abs/2501.12948
47. Mixtral 8x7B. https://arxiv.org/abs/2401.04088

### B.10 MoE架构解析（技术博客）

48. 大模型原理--混合专家模型. https://bbs.huaweicloud.com/blogs/477106
49. 大模型｜MoE混合专家系统介绍. https://cloud.tencent.cn/developer/article/2616938
50. DeepSeek-V3 高效训练关键技术分析. https://cloud.tencent.com/developer/article/2675228
51. 混合专家模型 MoE 架构深度解析. https://juejin.cn/post/7629603625098674222
52. REAP: One-Shot Pruning for Trillion-Parameter MoE Models. https://www.cerebras.ai/blog/reap
53. 戴尔中国研究院实现MoE推理加速. http://mobile.qudong.com/tag=lznb-TA68pt.html

### B.11 注意力机制变体（新增）

54. Grouped-Query Attention (GQA). https://arxiv.org/abs/2305.13245
55. Fast Transformer Decoding with Multi-Query Attention (MQA). https://arxiv.org/abs/1911.02150
56. Flash Attention 2. https://arxiv.org/abs/2307.08691

### B.12 推理优化技术栈（新增）

57. Efficient Memory Management for LLMs with PagedAttention (vLLM). https://arxiv.org/abs/2309.06180
58. vLLM项目. https://github.com/vllm-project/vllm
59. TensorRT-LLM. https://github.com/NVIDIA/TensorRT-LLM
60. vLLM-Ascend（昇腾适配）. https://github.com/vllm-project/vllm-ascend
61. SGLang. https://github.com/sgl-project/sglang
62. llama.cpp. https://github.com/ggerganov/llama.cpp
63. Megatron-LM. https://arxiv.org/abs/1909.08053
64. DeepSpeed. https://github.com/microsoft/DeepSpeed
65. Medusa: Simple LLM Inference Acceleration. https://arxiv.org/abs/2401.10774

### B.13 量化技术（新增）

66. GPTQ: Accurate Post-Training Quantization. https://arxiv.org/abs/2210.17323
67. AWQ: Activation-aware Weight Quantization. https://arxiv.org/abs/2306.00978
68. SmoothQuant: Accurate and Efficient Post-Training Quantization. https://arxiv.org/abs/2211.10438
69. 知识蒸馏综述. https://arxiv.org/abs/2006.05525

### B.14 后训练对齐技术（新增）

70. InstructGPT (RLHF). https://arxiv.org/abs/2203.02155
71. Constitutional AI. https://arxiv.org/abs/2212.08073
72. RLAIF. https://arxiv.org/abs/2309.00267
73. DPO: Direct Preference Optimization. https://arxiv.org/abs/2305.18290
74. GRPO (DeepSeekMath). https://arxiv.org/abs/2402.03300

### B.15 Scaling Law（新增）

75. Scaling Laws for Neural Language Models (Kaplan). https://arxiv.org/abs/2001.08361
76. Training Compute-Optimal LLMs (Chinchilla). https://arxiv.org/abs/2203.15556

### B.16 非Transformer架构（新增）

77. Mamba: Linear-Time Sequence Modeling with Selective State Spaces. https://arxiv.org/abs/2312.00752
78. Mamba-2: Transformers are SSMs. https://arxiv.org/abs/2405.21060
79. Jamba: A Hybrid Transformer-Mamba Language Model. https://arxiv.org/abs/2403.19887
80. RWKV. https://github.com/BlinkDL/RWKV-LM

### B.17 MCP协议

81. MCP官方规范. https://modelcontextprotocol.io/specification
82. MCP传输规范. https://modelcontextprotocol.io/specification/2025-11-25/basic/transports
83. Anthropic MCP公告. https://www.anthropic.com/news/model-context-protocol
84. MCP深度解析. https://blog.csdn.net/tongbowen_123/article/details/162612504
85. 火山方舟MCP. https://docs.volcengine.com/docs/82379/1539085

### B.18 RAG（新增）

86. RAG原始论文. https://arxiv.org/abs/2005.11401
87. Milvus向量数据库. https://github.com/milvus-io/milvus
88. LangChain. https://github.com/langchain-ai/langchain
89. LlamaIndex. https://github.com/run-llama/llama_index

### B.19 Agent框架（新增）

90. ReAct: Synergizing Reasoning and Acting. https://arxiv.org/abs/2210.03629
91. Reflection: Language Agents with Verbal Reinforcement Learning. https://arxiv.org/abs/2303.11366
92. AutoGen (Microsoft). https://github.com/microsoft/autogen
93. CrewAI. https://github.com/crewAIInc/crewAI
94. OpenAI Swarm. https://github.com/openai/swarm
95. Dify. https://github.com/langgenius/dify

### B.20 CPU价值重估与Agentic AI

96. 推理时代的CPU反攻. https://cj.sina.cn/article/norm_detail
97. NVIDIA Vera: The Max Single-Threaded CPU at Scale. https://www.nvidia.com/en-us/
98. Agentic AI Requires More CPUs - Intel. https://www.intel.cn/content/www/us/en/content-details/916705/
99. NVIDIA Unveils Vera. https://nvidianews.nvidia.com/news/nvidia-unveils-vera-the-cpu-for-agents
100. Achieving Cloud-Grade SLOs for Local MoE Inference (OSDI 2026) - 清华大学. https://craft.cs.tsinghua.edu.cn/zh/publication/achieving-cloud-grade-slos-for-local-mixture-of-experts-inference-through-cpugpu-hybrid-design/
101. The Price of Anarchy in Disaggregated Inference (arXiv 2026). https://arxiv.org/abs/2606.17081v1
102. Disaggregated Inference with NVIDIA Dynamo (GTC 2026). https://www.nvidia.cn/gtc/session-catalog/sessions/gtc26-p81109/

### B.21 国产AI芯片生态（新增）

103. 华为昇腾官网. https://www.hiascend.com/
104. vLLM-Ascend. https://github.com/vllm-project/vllm-ascend
105. 寒武纪官网. https://www.cambricon.com/
106. 海光官网. https://www.hygon.com/
107. 大模型千亿参数让GPU显存告急，英特尔居然让你试试CPU. https://www.intel.cn/content/www/cn/zh/artificial-intelligence/billion-parameter-large-models.html

### B.22 评测体系（新增）

108. MMLU: Measuring Massive Multitask Language Understanding. https://arxiv.org/abs/2009.03300
109. HumanEval. https://arxiv.org/abs/2107.03374
110. GSM8K. https://arxiv.org/abs/2110.14168
111. SWE-bench. https://arxiv.org/abs/2310.06770

### B.23 最新模型动态与排行榜（2026年7月）

112. LLM Leaderboard & AI Model Benchmarks — July 2026. https://www.benchlm.ai/
113. 7月AI大乱斗：GPT-5.6、Grok4.5、Gemini 3 Pro、DeepSeek V4登场. https://c.m.163.com/news/a/L1AUJBVP05198NMR.html
114. Claude Opus 4.8问世：三项全球第一. https://soft.china.com/article/1923162.html
115. Introducing Claude Opus 4.8 - Anthropic. https://www.anthropic.com/news/claude-opus-4-8
116. 2026年发布的Kimi K2.5：月之暗面开源多模态旗舰模型. https://blog.51cto.com/u_13539/14632989

### B.24 多模态合规与内容治理

117. AI内容治理规范化再升级. https://szzg.gov.cn/2026/szzg/xyzx/202607/t20260703_5342349.htm
118. 2026年AI审核与版权追踪技术协同应用. https://m.book118.com/html/2026/0611/6231110002012145.shtm

### B.25 大模型网关路由

119. 大模型网关怎么选？Fenno、OpenRouter、LiteLLM、One API全景对比. https://segmentfault.com/a/1190000047944223
120. LiteLLM是连接所有LLM最简单的方式吗？. https://sider.ai/zh-CN/blog/ai-tools/is-litellm-the-easiest-way-to-talk-to-every-llm-a-practical-review

---

## 附录C：修订日志

### v1.1（2026-07-08 复核修订）

**P0修正（5处事实错误）：**
1. Expert Choice路由归属：从"DeepSeek创新"修正为"Google 2022年提出"（3.4/3.5节）
2. MLA引入版本：从"V3引入"修正为"V2引入、V3/V4延续"（3.6节）
3. DeepSeek-V3专家数：3.1节表格从"64"修正为"256路由+1共享"，消除与3.4节的内部矛盾
4. Token丢弃描述：标注为"传统MoE做法"，补充DeepSeek-V3"No Token-Dropping"
5. DeepSeek多模态定位：1.1节补充"另有独立的Janus多模态分支"，消除与1.3节的矛盾

**P1修正（7处逻辑/描述问题）：**
1. 排行榜排名补全（1.2节，补充第2/6名）
2. Qwen分类标题修正为"海外及国际排行榜"（1.2节）
3. 细粒度分割组合数举例改为示意性说明+实际配置数据（3.4节）
4. 辅助损失公式标注为"传统MoE方案"（3.2节）
5. 新增注意力机制变体谱系表（3.6节）
6. 新增推理优化技术栈小节（4.1节）
7. 新增长上下文技术原理小节（1.5节）

**P2补充（15项遗漏知识点+参考链接）：**
1. 后训练对齐技术（SFT/RLHF/DPO/GRPO/Constitutional AI）→ 3.15节
2. Scaling Law（Kaplan/Chinchilla/训练后Scaling Law）→ 3.10节
3. RAG检索增强生成 → 4.3节
4. 量化技术完整谱系（GPTQ/AWQ/GGUF/FP8/SmoothQuant）→ 3.9节
5. 国产AI芯片与软件生态（昇腾/寒武纪/海光/CANN/vLLM-Ascend）→ 4.6节
6. 端侧模型技术路线（蒸馏+量化+架构优化）→ 5.5节
7. 多模态生成模型（DiT/Sora/3D-GS）→ 1.3节
8. 评测体系与局限性（MMLU/HumanEval/SWE-bench+数据污染问题）→ 5.4节
9. 知识蒸馏（KD）→ 3.9节
10. 非Transformer架构（Mamba/SSM/Jamba/RWKV）→ 3.12节
11. Agent框架（LangChain/AutoGen/CrewAI/Dify）→ 4.4节
12. 开源vs闭源生态竞争 → 1.4节
13. 模型水印与版权（AI内容治理）→ 1.4节
14. DeepSeek路由函数Sigmoid补充 → 3.1节
15. DeepSeek节点限制路由补充 → 3.4节

---

*报告完*

> 本报告基于2026年7月8日的公开信息整理，AI大模型领域变化迅速，部分信息可能已有更新。报告中涉及的模型能力、性能数据均来自公开来源，不代表官方确认。投资与技术选型决策请以最新信息为准。
