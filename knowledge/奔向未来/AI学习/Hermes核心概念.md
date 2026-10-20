---
title: Hermes核心概念
aliases: [Hermes技术概念速查, Hermes架构核心]
tags: [personal/AI, personal/学习, type/指南, attr/参考]
status: 进行中
created: 2026-05-09
updated: 2026-05-09
author: 司马懿
difficulty: 高等
related:
  - "[[奔向未来/AI学习/Hermes学习路线]]"
  - "[[奔向未来/AI学习/Hermes学习资源]]"
  - "[[奔向未来/AI学习/Agent World 指南]]"
  - "[[奔向未来/AI学习/Agent团队/贾维斯-Jarvis]]"
---

# Hermes 核心概念速查

> 司马懿整理，2026年5月。记录 Hermes Agent 的核心技术概念，为主公后续深入学习提供"知识地图"。

---

## 一、Hermes Agent 是什么

**Hermes Agent** 是 Nous Research 于 2026 年 2 月开源的自主 AI Agent 框架，口号 **"The Agent That Grows With You"（与你一同成长的智能体）**。MIT 开源协议，GitHub 85,000+ Stars，最新版本 v0.12.0（2026.4.30）。

**核心定位**：不是另一个"让 LLM 调工具的框架"，而是一个**会从经验中自动学习、记忆持久化、越用越懂你**的 AI 助手。

> [!quote] 一句话总结
> Hermes = LLM 路由器 + 闭环学习循环 + 分层记忆 + 跨平台网关

---

## 二、五环节闭环学习循环（核心架构）

这是 Hermes 区别于所有其他 Agent 框架的**最根本差异**。

```
┌─────────────────────────────────────────────────┐
│               Hermes 闭环学习循环                  │
│                                                   │
│  ① 自主精选记忆 ──→ ② 自主生成Skill               │
│       ↑                    ↓                      │
│       │              ③ Skill自我改进               │
│       │                    ↓                      │
│  ⑤ Honcho用户建模 ←── ④ FTS5跨会话检索           │
│                                                   │
│  每一个环节都在下一个会话中增强 Agent 的能力        │
└─────────────────────────────────────────────────┘
```

| 环节 | 触发条件 | 功能 | 产物 |
|------|----------|------|------|
| **① 自主精选记忆** | 每 10 个用户回合 | 后台 fork 独立 Agent，静默审查对话，提取有价值的用户偏好/环境事实 | `MEMORY.md` / `USER.md` 原子增量更新 |
| **② 自主生成 Skill** | 复杂任务完成（≥5 次工具调用） | 将成功解决方案提炼为可复用的 Markdown 操作文件 | `~/.hermes/skills/*.md` |
| **③ Skill 自我改进** | Skill 被再次调用且遇到问题 | 用 `fuzzy_find_and_replace` 精准 patch 过时/错误步骤，不重写全量 | Skill 文件增量更新 |
| **④ FTS5 跨会话检索** | 每次新会话启动 | SQLite FTS5 全文索引按需检索历史对话片段，注入当前上下文 | 上下文窗口注入 |
| **⑤ Honcho 用户建模** | 持续进行（可选插件） | 辩证分析用户行为，推断偏好与身份特征 | 用户画像数据 |

---

## 三、四层记忆系统

> Hermes 记忆设计参照认知科学：情景记忆 + 语义记忆 + 程序性记忆

```
Layer 4: 外部记忆 Provider（可选）
  ├── Honcho / Mem0 / Hindsight / Holographic / RetainDB（7 种可选）

Layer 3: 上下文窗口管理
  └── ContextCompressor（工具输出裁剪 → LLM 摘要 → 迭代更新）

Layer 2: 会话归档（全文检索）
  └── SessionDB（SQLite + WAL + FTS5 全文搜索）

Layer 1: 内建文件记忆（始终启用）
  ├── MEMORY.md（2200 字符硬上限）-- Agent 的"笔记"
  └── USER.md（1375 字符硬上限）-- Agent 对用户的"认知"
```

### 记忆分离设计

| 文件 | 内容 | 硬上限 | 分隔符 |
|------|------|--------|--------|
| `MEMORY.md` | 环境事实、项目约定、工具特性 | 2,200 字符（~800 tokens） | `§` |
| `USER.md` | 用户偏好、沟通风格、工作习惯 | 1,375 字符（~500 tokens） | `§` |

> [!important] 字符硬上限的设计哲学
> 有界记忆 > 无限记忆。上限迫使 Agent 做信息压缩和优先级排序，避免"记忆膨胀"导致的检索噪音。这是与 RAG 系 Agent（无上限向量库）的本质差异。

### 冻结快照机制（缓存优化关键）

```
会话启动 → 加载冻结快照 → 整个会话系统提示词不变 → 保护前缀缓存
                                                      ↓ 节省 ~75% token 成本
工具写入 → 修改活跃态 memory_entries → 立即持久化磁盘
                                       → 当前会话不触发系统提示词重建
                                       → 下次会话自动加载新记忆
```

这一设计让 Hermes 在**当前会话**享受前缀缓存，同时在**下次会话**自动加载新记忆，兼顾成本与能力。

---

## 四、Skill 系统：会自我进化的程序性记忆

### Skill 是什么

`~/.hermes/skills/` 下的一系列 Markdown 文件，记录"如何完成某类任务"的操作步骤。本质是**从经验中自动提炼的程序性知识**。

### 创建门槛

| 条件 | 原因 |
|------|------|
| 工具调用 ≥ 5 次 | 简单任务不值得固化为 Skill |
| 踩过坑再修复 | 平滑路径没有学习价值 |
| 用户纠正过做法 | 用户反馈是最宝贵的训练信号 |

### Skill 自我修补（Patch 机制）

```
发现步骤有遗漏 → fuzzy_find_and_replace 精确局部 patch
         → 自动安全扫描（12 种正则 + 10 种 Unicode 检测）
         → 不通过则自动回滚
         → 通过则保留 → 下次复用已修补版本
```

### 渐进式加载

系统提示词中只放**轻量索引**（Skill 名称 + 一句话描述），Agent 判断相关时才通过 `skill_view` 按需加载完整内容 —— "先看目录再翻全文"，防止上下文膨胀。

### Skill 生态

- 遵循 **agentskills.io** 开放标准
- 已有 **131 个社区 Skill**，支持跨 Agent 框架迁移
- 官方市场：https://agentskills.io

---

## 五、自进化引擎：GEPA

### 定位

**GEPA**（Genetic-Pareto Prompt Evolution）是 Hermes 自进化能力的算法核心，子项目 `hermes-agent-self-evolution`。获 **ICLR 2026 Oral**（最高荣誉论文）。

### 核心原理

```
读取当前 Skill/Prompt → 生成评估数据集
         ↓
GEPA 优化器（遗传搜索 + Pareto 多目标优化）
         ↓
候选变体 → 评估测试 → 约束门控（pytest/大小/语义）
         ↓
最佳变体 → PR 形式提交人工审查
```

### GEPA vs 传统强化学习（GRPO）

| 维度 | GEPA | GRPO（传统 RL） |
|------|------|----------------|
| 训练数据量 | 极少（GRPO 的 1/35） | 海量 |
| 单次优化成本 | ~$2-10 | 高昂（GPU 集群） |
| 性能提升 | 6 项任务平均高 6% | 基准 |
| 数学测试 AIME-2025 | 比最强工具 MIPROv2 高 12% | 基准 |
| 是否需要 GPU | 否 | 是 |
| 论文等级 | ICLR 2026 Oral | -- |

### 五阶段路线图

| 阶段 | 内容 | 状态 |
|------|------|------|
| Phase 1 | `SKILL.md` 自动优化 | ✅ 已实现 |
| Phase 2 | 工具描述（tool descriptions）优化 | 计划中 |
| Phase 3 | 系统提示词各区段优化 | 计划中 |
| Phase 4 | 工具实现代码本身优化（Darwinian Evolver） | 计划中 |
| Phase 5 | 全流程持续优化自动化管道 | 计划中 |

---

## 六、Nudge Engine：学习触发器

两个计数器驱动 Agent 的自我反思，后台静默执行，用户无感知：

| 触发器 | 间隔 | 关注点 |
|--------|------|--------|
| **Memory Review** | 每 10 个用户回合 | 用户偏好、个人信息、环境事实 |
| **Skill Review** | 每 10 次工具迭代 | 非平凡的解题过程、踩坑修复经验 |

触发后 fork 独立 Agent 实例（输出重定向 /dev/null），拿主对话快照做静默审查。如无可保存内容，输出 "Nothing to save." 并停止，防止无意义写入。

---

## 七、Function Calling 机制

### 单工具多动作接口

```python
# Hermes 的记忆工具采用 "单工具 + 动作参数" 模式
MEMORY_SCHEMA = {
    "name": "memory",
    "parameters": {
        "action": {"enum": ["add", "replace", "remove"]},
        "target": {"enum": ["memory", "user"]},
        "content": {...},
        "old_text": {...}
    }
}
```

### 与 OpenAI Function Calling 的关系

- Hermes 的 Function Calling 格式**兼容 OpenAI 标准**（JSON Schema 定义工具）
- 但 `memory` 工具不走标准 `registry.dispatch()` 路径，在 `run_agent.py` 的 Agent 循环中**被直接拦截**，以获得对 `_memory_store` 的访问权
- 支持 40+ 内置工具 + 6000+ MCP 外部工具

---

## 八、核心对比：Hermes vs 传统 Agent vs OpenAI Agents vs Claudian

| 维度 | **Hermes Agent** | **传统 Agent** (LangChain/AutoGPT) | **OpenAI Agents (Codex)** | **Claudian (Claude Code)** |
|------|:---:|:---:|:---:|:---:|
| **开发方** | Nous Research | 社区/开源 | OpenAI | Anthropic |
| **开源** | MIT 全开源 | 部分开源 | 闭源 | 部分开源 |
| **模型绑定** | **模型无关**（200+ 模型） | 多模型 | 仅 OpenAI | 仅 Claude |
| **记忆系统** | **四层分层记忆** | 向量库 RAG | 新增偏好记忆（简单） | CLAUDE.md + auto memory |
| **自学习闭环** | **✅ 核心能力** | ❌ | ❌ | ❌ |
| **Skill 系统** | **✅ 自动生成+自我修补** | ❌ | ❌ | ❌ |
| **记忆持久化** | 跨会话自动 | 需手动设计 | 有限 | 有限 |
| **多平台通讯** | **15+ 平台** | CLI/Web | macOS 原生 | CLI / IDE |
| **子 Agent 并发** | delegate_task（3个） | 多 Agent 链 | ✅ 多 Agent 并发 | Agent tool |
| **沙箱隔离** | 6 种后端 | Docker | 沙箱 | 本地/Docker |
| **定时任务** | ✅ 内置 Cron | ❌ | ❌ | 需外部 |
| **MCP 协议** | 6000+ 工具 | 有限 | 90+ 插件 | 支持 |
| **安装方式** | 一行命令 | pip/源码 | 桌面 App | npm/brew |
| **适用场景** | 通用助手、记忆沉淀 | 工作流自动化 | 云端编程、系统操作 | 编程/代码重构 |
| **学习曲线** | 中等 | 高 | 低 | 中等 |
| **运行成本** | ~$32/月（混合模型） | 视模型而定 | ~$80/月 | ~$120/月 |

### 性能基准对比（2026）

| 基准测试 | Hermes | Claude Code | ChatGPT Agents |
|----------|:------:|:-----------:|:--------------:|
| WebArena（网页任务） | **87.3%** | 74% | 71% |
| SWE-bench（代码修复） | 82.1% | **89%** | 68% |
| ToolBench（API 使用） | **91.2%** | 76% | 73% |
| AgentBench（多领域） | **88.7%** | 72% | 69% |

### 核心差异一句话

| Agent | 核心卖点 | 本质 |
|-------|----------|------|
| **Hermes** | 越用越强，180天"肌肉记忆"搬不走 | 卖的是**知识沉淀** |
| **Claude Code** | 嵌进程序员工作流 | 卖的是**程序员时间** |
| **OpenAI Codex** | 系统级多 Agent 并发 | 卖的是**平台生态** |
| **传统 Agent** | 灵活搭建工作流 | 卖的是**工具链** |

---

## 九、当前限制与争议

### 项目阶段

- 版本：v0.12.0（仍属实验性产品）
- 纯编程深度不如 Claude Code（SWE-bench 82% vs 89%）
- 自部署有门槛（需 Linux/macOS/WSL2，Windows 原生不支持）
- 某些功能仅付费订阅可用（Tool Gateway: 搜索、生图、TTS）

### 抄袭争议（2026年4月）

- **指控方**：EvoMap（中国 AI 团队）
- **指控内容**：Hermes 的 10 步自进化主循环照抄其开源项目 Evolver（早 36 天公开）
- **证据**：10 步循环一一对齐、12 组术语系统性替换、7 份材料零归属
- **Nous Research 回应**：称仓库 2025 年 7 月已创建（彼时私有），回应 "Delete your account"
- **当前状态**：尚无定论，开源社区热议中
- **对学习者的影响**：不影响学习技术架构本身，但需了解背景

---

## 十、知识地图：深入学习路径

```
Hermes 核心知识地图
│
├── 第一层：理解"是什么"
│   ├── Hermes 是什么 → [[奔向未来/AI学习/Hermes核心概念]]（待创建）
│   ├── 为什么需要"自我进化" → 与传统 Agent 的本质差异
│   └── 一行命令安装体验 → [[Hermes学习资源#快速安装命令]]
│
├── 第二层：理解"怎么做"（核心机制）
│   ├── 闭环学习循环 → 本页第二章
│   ├── 四层记忆系统 → 本页第三章
│   ├── Skill 系统 → 本页第四章
│   ├── Nudge Engine → 本页第六章
│   └── Function Calling → 本页第七章
│
├── 第三层：理解"为什么这么设计"
│   ├── 冻结快照与缓存优化 → 本页第三章
│   ├── 有界记忆设计哲学 → [[Hermes学习资源#记忆系统专项]]
│   ├── GEPA 进化算法 → 本页第五章
│   └── 安全设计（12种正则+10种Unicode检测）
│
├── 第四层：对比与选型
│   ├── Hermes vs Claudian → 本页第八章
│   ├── Hermes vs OpenAI Codex → 本页第八章
│   └── 何时用 Hermes / 何时用 Claude Code → [[奔向未来/AI学习/Hermes核心概念]]（待创建）
│
└── 第五层：实践与集成
    ├── 本地部署 → [[Hermes学习资源#快速安装命令]]
    ├── 与 Obsidian 集成 → [[知识管理/系统/Python与Obsidian集成使用指南]]（待创建）
    ├── 与贾维斯团队联动 → [[奔向未来/AI学习/Hermes学习总结]]（待创建）
    └── 微信/飞书/钉钉接入 → [[Hermes学习资源#一键部署平台]]
```

---

## 十一、关键术语中英对照

| 英文 | 中文 | 说明 |
|------|------|------|
| Hermes Agent | 爱马仕智能体 | Nous Research 的自主 AI Agent 框架 |
| Self-Evolution / Self-Improving | 自我进化 | 从经验中自动提炼和改进技能的能力 |
| Learning Loop | 闭环学习循环 | 记忆→生成Skill→改进→检索→建模 五环 |
| GEPA | 遗传帕累托提示进化 | Genetic-Pareto Prompt Evolution，自进化算法引擎 |
| Nudge Engine | 推动引擎 | 后台静默的学习触发器（每N回合触发审查） |
| Skill | 技能 | 可复用的操作指南 Markdown 文件（程序性记忆） |
| Skill Patch | 技能修补 | 精准局部修改 Skill，不全文重写 |
| Frozen Snapshot | 冻结快照 | 会话开始时加载、整个会话不变的系统提示词（保护前缀缓存） |
| Memory Store | 记忆存储 | 内建文件记忆系统（MEMORY.md + USER.md） |
| Context Compressor | 上下文压缩器 | 工具输出裁剪→摘要→迭代更新 |
| Nous Portal | Nous 门户 | Nous Research 的模型 API 服务（400+模型） |
| Tool Gateway | 工具网关 | 付费功能：网页搜索、生图、TTS、浏览器自动化 |
| Harness | 马具 | Hermes 的 Agent 运行时层，与 Framework 区分 |
| agentskills.io | 技能市场 | 开放标准的 Skill 生态，131个技能，跨平台迁移 |

---

> **创建时间**：2026-05-09
> **维护者**：司马懿（资源整合专家）
> **关联索引**：[[奔向未来/AI学习/Hermes学习路线]] | [[奔向未来/AI学习/Hermes学习资源]] | [[奔向未来/AI学习/AI技术主题索引]] | [[奔向未来/AI学习/Agent World 指南]]
