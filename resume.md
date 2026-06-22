# 何宸锐

联系电话：176-1150-9150 | 邮箱：hechenrui123@gmail.com | 出生年月：1994-10-23 | 性别：男

---

## 教育背景

**2017.9～2020.6　北京大学　通信与信息系统　硕士研究生**
主修课程：信息论与编码理论、软件工程、网络大数据管理理论和应用

**2013.9～2017.6　北京大学　电子系（通信方向）　本科**
主修课程：数据结构与算法、通信原理、操作系统、计算机网络、嵌入式 Linux 操作系统

---

## 工作经历

### 2025.4～至今　北京智源人工智能研究院　大模型推理框架开发　[AI框架研发组]

AI框架研发组负责院内自研训推框架 FlagScale 的开发，以及具体业务的支持。个人的负责方向为具身智能场景（VLM 大脑模型 + 端到端 VLA 模型的训推优化，以及 RoboOS 端云协同系统）。

- **PyTorch 自定义硬件后端插件（PrivateUse1）。** 基于 PyTorch PrivateUse1 扩展机制，实现通用硬件加速器的 PyTorch 后端插件，支持将 PyTorch 工作负载透明调度到非原生设备上运行。设计并实现 Caching Device Allocator（power-of-2 bin block pooling），相比 PyTorch 原生 CUDACachingAllocator 减少 ~17% 分配开销；构建 Op Dispatch 框架（DeviceBoxingGuard），通过 metadata swap 零拷贝地将 op 调度到 CUDA kernel，消除全部 CPU fallback 的 D2H/H2D 数据搬运；实现可配置的多后端 dispatcher（CUDA/FlagGems/CPU），支持 per-op 粒度的后端切换。在 Qwen3-0.6B / H800 上达到 46.93 tok/s，超越原生 PyTorch CUDA 推理性能（+1.1%）。
- **推理部署自动调参。** 接入新推理后端 llama.cpp，并支持跨后端（vLLM、SGLang）的自动调优。搜索空间包括：实例数、TP、PP、显存分页大小、PrefixCache 的 ChunkSize 等。输出指标包括：输入/输出 token 吞吐、首 token 延时、token 间延时。
- **Ernie 45T 模型接入 vLLM。** 从 Fastdeploy/Paddle 迁移到 vLLM/Torch，精度对齐。支持 FlagScale 和百度官方同时发布。
- **华为 OmniInfer 框架作为后端接入 FlagScale。** 跑通 Qwen3 和 Qwen2.5VL 的推理部署流程。
- **具身 VLM/VLA 模型训练优化。** 负责 RoboBrain2（7B/32B）从 DeepSpeed 迁移至 Megatron-LM（FlagScale-Train），适配 DP/TP/PP 并行策略；设计非均匀流水线并行（减少 ViT 所在 PP-Stage 的 LLM 层数）解决 ViT 显存瓶颈；落地 Energon + WebDataset 数据管线，数据加载预处理延时降低 90%；通过内存分配预处理解决显存碎片化问题，ViT 开启 Re-Computation 降低显存峰值。端到端训练加速比 154.81%（对比 Llama-Factory/DeepSpeed）。针对 VLA（Robotics 3.3B）后训练，完成 Lerobot 数据集到 Energon 的适配，排查冻结 VLM 后吞吐不及理论预期（实际 50% vs 理论 66.7%）的原因。
- **具身模型推理部署与量化。** 对 RoboBrain2 和 VLA 模型采用 INT8 Weight-Only 量化（ViT 全精度 + LLM W8A16），端到端具身任务推理延时降低约 30%；在 H100 上 Prefill 阶段延时 -37.5%，在 4090 上 Decode 阶段延时 -24%。支持松灵双机械臂真机部署，打通 FlagEval 评测框架的具身 VLA 真机评测流程。
- **RoboOS 端云协同优化。** 基于 Redis 订阅/发布 + Socket 中断替换原生共享内存轮询方案，支持跨机通信与 Slave 并发执行，平均通信延迟降低至 <3ms；实现基于 SentenceTransformers 的技能语义检索，总 Token 使用量降低 29.8%；机器人技能注册标准化，典型场景输入 Token 减少 65%。
- **FLM-Audio 流式语音对话模型推理优化。** 对端到端流式语音模型（Mimi 音频编解码器 + LM Backbone + Depformer 自回归解码）进行 Profiling 与推理加速。通过 torch.compile 端到端推理延时降低 48%（95ms→49ms/帧）。设计多客户端并发架构：模型权重全局共享，per-client 隔离 KV Cache / 流式状态 / 环形缓冲区，通过线程池 + 推理队列实现多会话并发推理。

### 2022.3～2025.4　抖音　内容安全架构开发　[Data-Arch-IES]

安全架构位于安全算法（80人）和安全服务端（300人）团队之间，负责业务流程、偏业务的算法基础设施搭建维护。

- **大模型推理加速。** 各类模型（Qwen、InternLM、GPT2、Yi-34b 等）接入&适配推理加速框架（主要是 vLLM 和 TensorRT-LLM，其次是 LMDeploy）；并行、并发度调优；SOTA PTQ 量化（AWQ、Smooth Quant、GPTQ、LLM.int8 等）性能测试以及上线；多模态模型在加速框架上的适配；多模态大模型共用 backbone + 多任务 head 部署，通过 PrefixCache 获得吞吐收益。半年时间在约 3 万卡的资源中优化约 20 个模型服务获得资源收益约 2000 张。
- **模型特征平台项目。** 开发维护重构模型在线预估系统，负责迁移内容安全最大场景抖音视频先审（模型 300+、特征 100K+、配置 3MB+、QPS 2K+），资源收益 1w core+。建立 AB 实验流程。独立开发模型自动迭代系统，接入场景 8 个。
- **其他垂类场景支持。** 共同开发基于频谱 landmark 的音频消重聚类模型，独立完成计算和 IO 优化（10s→2s）。独立开发维护抖音书籍锚点&底 Bar 的算法侧在线预估系统。独立开发风险发现业务的触发系统，接入场景 6 个。

### 2020.7～2022.3　腾讯云　私有化交付平台开发　[私有化交付研发]

本系统是整个腾讯云私有化交付的平台，流程覆盖云产品研发、售前、售中和售后。两年内累计交付项目 20+。

- 独立开发云产品规划子系统，负责定制化场景下的交付资源和报价项的计算。
- 协助开发交付规划子系统，负责由资源出发规划云产品的部署方式。

---

## 实习经历

### 2019.5～2019.7　谷歌　Wiki 数据抽取 Pipeline 的优化　[Search-DataZ]

- 完成 MapReduce 到 Flume 的代码迁移；对 WikiTemplate 进行 dedup，把运行时间从 20 天减少到约 20 小时。分析 dedup 策略对 Label 的影响，评估 Label 被下游 client 采用后对 KnowledgeGraph 和搜索引擎的影响。

### 2018.9～2018.11　香侬科技　Kubernetes 容器化平台实践　[基础架构组]

- 独立完成 Kubernetes 高可用方式的搭建；完成多个项目的容器化构建；为 Kubernetes 1.11 版本的官方指南提交 PR 修复 etcd 集群和 apiserver 顺序问题。

### 2018.6～2018.8　VLC　Qt/QML 播放界面的重新设计　[谷歌 GSoC 开源项目]

- 对 VLC 的原生 Qt/C++ 播放界面进行重新设计，使用 QML 实现并嵌入；把模型分离在 C++ 端，使模型与视图进一步分离。

---

## 论文发表

1. RoboBrain 2.0 Technical Report. BAAI RoboBrain Team (including **Chenrui He**). arXiv, 2025.
2. RoboOS: A Hierarchical Embodied Framework for Cross-Embodiment and Multi-Agent Collaboration. Huajie Tan, ..., **Chenrui He**, et al. arXiv, 2025.
3. RoboOS-NeXT: A Unified Memory-based Framework for Lifelong, Scalable, and Robust Multi-Robot Collaboration. Huajie Tan, ..., **Chenrui He**, et al. arXiv, 2025.
4. TrimTokenator: Towards Adaptive Visual Token Pruning for Large Multimodal Models. Hao Zhang, Mengsi Lyu, **Chenrui He**, Yulong Ao, Yonghua Lin. arXiv, 2025.

---

## 获奖情况

- **学术类：** 国家奖学金（1人/班/学年）、五四奖学金（二等奖）、颐天民生奖学金、北京大学 ACM 三等奖（2次）、北京大学三好学生（2次）、北京大学优秀毕业生
- **实践类：** 北京大学优秀军训工作者、北京高校棒球经典赛亚军（担任右外野手）

---

## 技术栈

PyTorch Internals (Dispatcher, TensorImpl, DeviceGuard, CUDACachingAllocator), C++/CUDA, vLLM, TensorRT-LLM, Python, Kubernetes, Docker, Qt, Linux
