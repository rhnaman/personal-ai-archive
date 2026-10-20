---
title: Hermes学习资源
aliases: [Hermes Agent学习资源清单, Hermes资源索引]
tags: [personal/AI, personal/学习, type/索引, attr/参考]
status: 进行中
created: 2026-05-09
updated: 2026-05-09
author: 司马懿
related:
  - "[[奔向未来/AI学习/Hermes学习路线]]"
  - "[[奔向未来/AI学习/Hermes核心概念]]"
  - "[[奔向未来/AI学习/AI技术主题索引]]"
  - "[[奔向未来/AI学习/Agent World 指南]]"
---

# Hermes 学习资源索引

> 司马懿整合，2026年5月。涵盖 Hermes Agent（Nous Research）的官方资源、中文社区、视频教程、论文与深度分析。
> 配合 [[奔向未来/AI学习/Hermes学习路线]] 四个阶段，按"入门 → 进阶 → 实战"分级。

---

## 一、官方资源

### GitHub 仓库

| 资源 | 链接 | 说明 |
|------|------|------|
| **hermes-agent** 主仓库 | https://github.com/NousResearch/hermes-agent | 核心 Agent 框架，MIT 开源，85,000+ Stars |
| **hermes-agent-self-evolution** | https://github.com/NousResearch/hermes-agent-self-evolution | GEPA 自进化算法引擎（ICLR 2026 Oral） |
| **Hermes 3 模型** | https://huggingface.co/NousResearch | Hermes 3 8B / 70B / 405B 模型权重 |
| **官方文档站** | https://hermes-agent.nousresearch.com/docs | 安装指南、CLI 命令、配置参考、API 文档 |
| **Release Notes** | https://github.com/NousResearch/hermes-agent/releases | 版本更新日志（最新 v0.12.0，2026.4.30） |

### 官方频道

| 资源 | 链接/入口 | 说明 |
|------|-----------|------|
| **Nous Research 官网** | https://nousresearch.com | 公司动态、技术博客、Hermes 3 论文 |
| **Discord 社区** | discord.gg/NousResearch | 官方 Discord，2500+ 日活用户 |
| **Nous Portal** | https://portal.nousresearch.com | 400+ 模型 API 接入（$3.99-$5/月） |
| **X / Twitter** | @NousResearch | 项目动态、版本发布 |

---

## 二、中文教程与文章（入门）

> 按推荐阅读顺序排列

### 入门必读（30分钟建立全局认知）

| 序号  | 标题                                | 链接                                                              | 推荐理由                            |
| --- | --------------------------------- | --------------------------------------------------------------- | ------------------------------- |
| 1   | **Hermes Agent 安装配置完整指南**         | https://news.qiniu.com/archives/1776047829591                   | 一行命令部署，新手友好，覆盖 Linux/macOS/WSL2 |
| 2   | **Hermes Agent 橙皮书（17章）**         | https://github.com/alchaincyf/hermes-agent-orange-book          | 从入门到精通，最系统的中文教程，持续更新            |
| 3   | **爱马仕agent是什么？极速上手指南**            | https://www.ai-indeed.com/encyclopedia/19166.html               | 五分钟建立全局认知，适合零基础                 |
| 4   | **我用了一周 Hermes Agent，整理出这十件必做的事** | https://blog.csdn.net/weixin_66401877/article/details/160151754 | 实战经验总结，避坑干货                     |
|     |                                   |                                                                 |                                 |

### 避坑与配置

| 标题 | 链接 | 说明 |
|------|------|------|
| **避坑完全指南（16个高频坑）** | https://www.cnblogs.com/qiniushanghai/p/19898838 | API Key 配置、Ollama 上下文、WSL/macOS 问题 |
| **DeepSeek V4 接入完整指南** | https://www.cnblogs.com/qiniushanghai/p/19936464 | config.yaml 配置 + Thinking 模式 + 国内直连 |
| **Hermes Agent 部署全指南** | https://post.smzdm.com/p/a4qn9067/ | 5分钟拥有自己的AI助手 |
| **CLI 命令速查表** | https://dev.to/rosgluk/hermes-agent-cli-cheat-sheet-commands-flags-and-slash-shortcuts-3pcb | 常用命令、flag、slash 快捷方式 |

### 深度技术分析

| 标题 | 链接 | 说明 |
|------|------|------|
| **爱马仕agent成长机制深度揭秘** | https://www.ai-indeed.com/encyclopedia/19165.html | 自进化学习循环全解析 |
| **智能代理Hermes：三层记忆与辩证推理** | https://developer.baidu.com/article/detail.html?id=6944950 | 百度开发者，记忆系统深度 |
| **深入源码：Hermes Agent 如何实现 Self-Improving** | https://developer.aliyun.com/article/1730226 | 阿里云，源码级分析 |
| **从零构建自适应AI智能体：技术全解析** | https://developer.baidu.com/article/detail.html?id=6729282 | 技术架构全景 |
| **从"工具箱"到"数字生命"：核心技术解析及对比** | https://www.langchain.cn/t/topic/900 | LangChain 中文社区，含 OpenClaw/Claude Code 对比 |

---

## 三、视频教程

### B站（中文）

| 标题 | 链接 | 推荐理由 |
|------|------|----------|
| **【2026全网最全】B站唯一讲透 Hermes Agent 的教程** | https://www.bilibili.com/opus/1198768712832778244 | 从零安装到进阶，附面试题资料 |
| **【2026】4月最新出炉！保姆级教程（14节）** | https://www.bilibili.com/video/BV1tkd1B2EBQ/ | 含零成本安装、Harness 设计范式、生产级实战 |
| **Hermes Agent高级玩法：微信+LLM Wiki+Obsidian** | https://www.aivi.fyi/llms/hermes-wiki | Andrej Karpathy 工作流复刻 |

### 文字+视频混合教程

| 来源 | 标题 | 链接 |
|------|------|------|
| 什么值得买 | 保姆级全流程详解：从零搭建会学习的AI智能体 | https://post.smzdm.com/p/a95mm8n0/ |
| 太平洋科技 | "龙虾"OUT了！能自我进化的爱马仕智能体来了 | https://www.pconline.com.cn/zhizao/video/1570704.html |
| 阿里云 | 低成本搭建AIP自动化写作系统 | https://developer.aliyun.com/article/1725707 |
| Felo Blog | 构建你的第一个 AI 研究工作流 | https://felo.ai/zh-Hans/blog/hermes-agent-tutorial-research-workflow/ |

---

## 四、进阶资源

### 技术文档与对比分析

| 资源 | 链接 | 要点 |
|------|------|------|
| **Hermes Agent 与 OpenClaw 核心差异全解析** | https://developer.aliyun.com/article/1727526 | 部署教程 + 架构差异 + 选型建议 |
| **OpenClaw、Claude Code、Hermes Agent 记忆系统比较** | https://bbs.huaweicloud.com/blogs/477076 | 三款 Agent 记忆机制横向对比 |
| **单 Agent vs 多 Agent 架构对比** | https://blog.csdn.net/zhangmeijia5/article/details/160850144 | OpenClaw/Claude Code/Hermes 架构差异 |
| **AI Agent 年度总览（2026春）** | https://m.toutiao.com/article/7637327572271694355/ | 框架图鉴：谁在堆功能，谁在教AI学习 |
| **换掉小龙虾，Hermes全网爆火狂揽四万星** | https://m.36kr.com/p/3760886185214465 | 36氪深度报道，产业视角 |

### 记忆系统专项

| 资源 | 链接 | 说明 |
|------|------|------|
| **Milvus 博文：用 Hybrid Search 修复学习循环** | https://milvus.io/blog/hermes-agent-learning-loop-milvus-hybrid-search.md | 混合搜索优化记忆检索 |
| **OpenClaw vs Hermes Agent: Memory Compared** | https://vectorize.io/articles/openclaw-vs-hermes-agent-memory | 向量化记忆对比 |
| **Hermes Agent Memory Management 研究** | https://github.com/ForceInjection/AI-fundermentals/blob/main/08_agentic_system/memory/research/systems/hermes-agent-memory-management.md | 源码级记忆系统分析 |

### 自进化系统专项

| 资源 | 链接 | 说明 |
|------|------|------|
| **Hermes橙皮书：三层记忆+Skill系统+40+工具** | http://www.chuangze.cn/xinxi/18004.html | Skill 系统详解 |
| **AI 自我进化时代来临（电脑王阿达）** | https://www.koc.com.tw/archives/639404 | GEPA 算法详解，台媒深度报道 |
| **Hermes Agent 爆火：腾讯云/本地一键部署** | https://cloud.tencent.cn/developer/article/2659061 | 腾讯云官方技术博客 |

---

## 五、社区资源

### GitHub 社区项目

| 项目 | 链接 | 说明 |
|------|------|------|
| **hermes-agent-orange-book** | https://github.com/alchaincyf/hermes-agent-orange-book | 橙皮书，17章系统教程 |
| **awesome-HermesAgent-tutorial** | https://github.com/xianyu110/awesome-HermesAgent-tutorial | Awesome 列表，入门指南集合 |
| **hermes-agent-docs（社区文档）** | https://github.com/mudrii/hermes-agent-docs | v0.2.0 社区维护文档 |

### 一键部署平台

| 平台 | 链接 | 说明 |
|------|------|------|
| **百度云 LS** | https://cloud.baidu.com/product/LS/hermes.html | 国内一键部署 |
| **腾讯云** | 见腾讯云技术博客 | 支持微信接入 |
| **VPS 24/7** | 官方文档有 $5/月 方案 | 全天候运行 |

---

## 六、论文与学术资源

| 资源 | 说明 |
|------|------|
| **GEPA: Genetic-Pareto Prompt Evolution** (ICLR 2026 Oral) | Hermes 自进化引擎的理论基础，效率远超 GRPO（强化学习），训练数据仅需 1/35 |
| **Hermes 3 Technical Report** | Nous Research 官方技术报告，Hermes 3 模型架构与能力评估 |
| **DSPy 框架论文** | GEPA 基于 DSPy 的编程模型，声明式 Prompt 优化 |
| **EvoMap vs Hermes 争议报告** | EvoMap 出具的 10步对齐/12组术语替换技术对比（非学术论文，但含架构对比价值） |

---

## 七、与现有知识体系的关联

### 本 Vault 内关联

| 关联笔记 | 关系 |
|----------|------|
| [[奔向未来/AI学习/Hermes学习路线]] | 本资源清单为其"参考资源"章节的详细展开 |
| [[奔向未来/AI学习/Hermes核心概念]] | 技术概念速查，配合资源清单交叉阅读 |
| [[奔向未来/AI学习/Agent World 指南]] | 现有 Agent 知识体系的入口 |
| [[奔向未来/AI学习/Agent团队/贾维斯-Jarvis]] | 当前使用的 Agent 系统（Claudian），对比学习 |
| [[知识管理/团队协作章程]] | 贾维斯团队运作规则 |
| [[奔向未来/AI学习/AI技术主题索引]] | Vault AI 主题总索引 |

### 外部知识体系对接

| 外部体系 | 对接点 |
|----------|--------|
| **OpenAI Agents SDK** | Hermes 的 Function Calling 格式兼容 OpenAI 标准 |
| **LangChain / AutoGPT** | 传统 Agent 框架对比参照，Hermes 更侧重长期记忆与自我进化 |
| **Claude Code (Claudian)** | 主公当前主用的 Agent，对比学习两者记忆机制差异 |
| **MCP 协议** | Hermes 支持 6000+ MCP 工具接入，与 Anthropic MCP 生态互通 |

---

## 八、快速安装命令

```bash
# 一行命令安装（Linux / macOS / WSL2 / Termux）
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
source ~/.bashrc && hermes

# 初始化配置
hermes setup

# 查看版本
hermes --version
```

> ⚠️ **关键避坑**：API Key 必须写入 `~/.hermes/.env`，不能写 `config.yaml` 或用 `export`；Ollama 用户需设置 `OLLAMA_CONTEXT_LENGTH=65536`。

---

## 九、资源分类索引

| 学习阶段 | 对应章节 | 核心资源 |
|----------|----------|----------|
| **第一阶段：认知入门** | 第二章（入门必读）+ 第一章（官方） | 橙皮书第1-3章、极速上手指南 |
| **第二阶段：核心机制** | 第二章（深度技术）+ 第四章（进阶） | 深入源码、记忆系统分析、对比文章 |
| **第三阶段：架构技术** | 第四章（进阶）+ 第六章（论文） | 架构对比、GEPA 论文、向量化记忆 |
| **第四阶段：实践集成** | 第三章（视频）+ 第五章（部署平台） | B站14节教程、百度云/腾讯云部署 |

---

> **创建时间**：2026-05-09
> **维护者**：司马懿（资源整合专家）
> **关联索引**：[[奔向未来/AI学习/Hermes学习路线]] | [[奔向未来/AI学习/Hermes核心概念]] | [[奔向未来/AI学习/AI技术主题索引]]
