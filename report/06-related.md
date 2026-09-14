# 6 相关工作（Related Work）

> 原文：<https://eit-hai.github.io/thea/related.html> ｜ 中文编译导读，精确表述以原文为准
> 上一章：[5 实验](05-experiments.md) ｜ 下一章：[7 讨论](07-discussion.md)

---

## 6.1 编程智能体

现代编程智能体已能端到端、大规模地管理一个环境[1,2,43]。其共同设计已被系统化地总结为 **harness**[7]：

- 从最新观测状态重新决策的**反应式循环**[44]；
- 模型在运行时从中选择的**工具注册表**[45]；
- 读取失败轨迹并重试的**恢复机制**[46]；
- **受管理的上下文**[47]；
- 跨会话持久的**记忆**[48]。

本文把这些模式移植到物理世界，并在过程中逐个改造（第 3 章）。软件环境有两个性质在物理世界中没有对应：代码仓库天然**可读**，其测试天然**可验证**。物理世界两者都不提供，因此作者自己构建了这两者（第 4 章）。

## 6.2 具身基础模型

具身基础模型沿两条互补路线为物理世界提供原子能力：

- **视觉-语言-动作模型（VLA）**：把观测和指令端到端映射为动作，从 RT 系列[49]，到开放的通用策略[50]，再到基于 flow matching 的架构[10,31]。
- **世界动作模型（WAM）**：源自世界模型（预测世界如何演化），把动作输出融入预测网络。近期代表 DreamZero 用单个图像到视频的扩散模型闭环驱动机器人[51]。

**这两条路线都是 harness 所调用的工具，而不是它要取代的那一层。** 近期一项跨控制接口的评测发现：前沿模型直接驱动关节时大多失败，但在监督预训练策略时表现出色——接口的重要性不亚于模型本身[12]。

当今最好的策略单步成功率 p 约为 0.8–0.9[10,50]，n 步任务的成功率则只有 pⁿ（第 2 章），即使 n 不大也会迅速崩塌；在 harness 层面做恢复和评估，可以不必等待单步完美就弥合这一差距。这也是 Thea 明确把**原子能力**与**编排**分离的原因。

## 6.3 具身智能体的编排

基于语言模型的机器人技能编排始于**开环**：

- **SayCan** 逐步选择基于可供性（affordance）的技能，但没有东西评判刚执行的技能[41]；
- **Code as Policies** 一次性生成程序[52]；
- **Inner Monologue** 最接近闭环，把环境反馈送回语言模型，但反馈通道是手工挑选的，也没有通用机制判断某一步是否真正成功[53]。

后续工作补上了回路中的个别部件：供规划器读取的场景图[23]，以及对结果做裁决的失败判别器[54]。**双系统 VLA** 走了另一条路：把慢速的视觉-语言推理器与快速的底层控制器配对[55,56,57,58]。

最近，一批同期系统把编程智能体引入机器人领域：

- **RoboClaw** 通过可自我复位的动作对实现数据采集自动化[59]；
- **CaP-X** 在操作任务上评测编程智能体，并通过扩展测试时交互来提升它们[32]；
- **Guava** 在操作任务上搜索 harness 设计空间，并把结果蒸馏为便于部署的紧凑模型[60]；
- **ENPIRE** 让编程智能体通过自动复位、执行与验证，在真实机器人上自我改进策略[61]；
- **ASPIRE** 通过编写和修复控制代码发现可复用的技能[62]。

**本文则把编程智能体的范式作为一个整体迁移过来**：harness 本身就是部署的系统，在物理世界中闭合从指令到完成的整个回路。

---

## 参考文献

1. Anthropic (2026), *Claude Code by Anthropic: An Agentic Coding System*.
2. OpenAI (2026), *Codex: AI Coding Partner from OpenAI*.
7. Weng (2026), *Harness Engineering for Self-Improvement*.
10. Black et al. (2025), *π0.5: A Vision-Language-Action Model with Open-World Generalization*.
12. Berman et al. (2026), *Claude Plays Robotics*.
23. Rana et al. (2023), *SayPlan: Grounding large language models using 3D scene graphs for scalable robot task planning*.
31. Black et al. (2024), *π0: A Vision-Language-Action Flow Model for General Robot Control*.
32. Fu et al. (2026), *CaP-X: A framework for benchmarking and improving coding agents for robot manipulation*.
41. Ahn et al. (2022), *Do As I Can, Not As I Say: Grounding Language in Robotic Affordances*.
43. Wang et al. (2025), *OpenHands: An Open Platform for AI Software Developers as Generalist Agents*.
44. Yao et al. (2023), *ReAct: Synergizing Reasoning and Acting in Language Models*.
45. Schick et al. (2023), *Toolformer: Language Models Can Teach Themselves to Use Tools*.
46. Shinn et al. (2023), *Reflexion: Language Agents with Verbal Reinforcement Learning*.
47. Packer et al. (2023), *MemGPT: Towards LLMs as Operating Systems*.
48. Wang et al. (2024), *Voyager: An Open-Ended Embodied Agent with Large Language Models*.
49. Zitkovich et al. (2023), *RT-2: Vision-language-action models transfer web knowledge to robotic control*.
50. Kim et al. (2024), *OpenVLA: An open-source vision-language-action model*.
51. Ye et al. (2026), *World action models are zero-shot policies*.
52. Liang et al. (2023), *Code as policies: Language model programs for embodied control*.
53. Huang et al. (2022), *Inner monologue: Embodied reasoning through planning with language models*.
54. Duan et al. (2024), *AHA: A vision-language-model for detecting and reasoning over failures in robotic manipulation*.
55. Bjorck et al. (2025), *GR00T N1: An open foundation model for generalist humanoid robots*.
56. Figure AI (2025), *Helix: A Vision-Language-Action Model for Generalist Humanoid Control*.
57. Shi et al. (2025), *Hi Robot: Open-Ended Instruction Following with Hierarchical Vision-Language-Action Models*.
58. Gemini Robotics Team (2025), *Gemini Robotics: Bringing AI into the Physical World*.
59. Li et al. (2026), *RoboClaw: An Agentic Framework for Scalable Long-Horizon Robotic Tasks*.
60. Liu et al. (2026), *Guava: An Effective and Universal Harness for Embodied Manipulation*.
61. Xiao et al. (2026), *ENPIRE: Agentic Robot Policy Self-Improvement in the Real World*.
62. Lu et al. (2026), *ASPIRE: Agentic Skills Discovery for Robotics*.
