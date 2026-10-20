# personal-ai-archive

> 跨 AI、跨设备的个人记忆系统。**一次搭建，全平台复用。**

## 这是什么

一个纯 Markdown 的个人档案仓库。任何能读 GitHub 的 AI（ChatGPT / Claude / Gemini / 各类 Agent）都可以直接读取这里的文件，了解"我是谁、我怎么工作、我做过什么决定"，不必每次重新交代背景。

## 目录

| 目录 | 放什么 | 谁会读 |
|---|---|---|
| `identity/` | 身份、职业、核心标签、人生目标 | 所有 AI，每次对话起点 |
| `workflow/` | 工作习惯、常用工具、沟通风格、任务优先级规则 | 所有 AI，决定"怎么配合我" |
| `memory/` | 历史经验、重要决策记录、AI 对话沉淀的偏好 | 所有 AI，避免重复踩坑 |
| `skills/` | 已掌握技能、学习计划、待提升清单 | 所有 AI，用于推荐方案与学习路径 |
| `knowledge/` | 个人学习笔记（读书笔记/知识管理/个人成长，已脱敏） | 需要方法论与观点时检索 |
| `archive/` | 历史项目、旧文档、已完成任务 + `vault-map.md`（本地资料库地图） | 按需检索 |

## 快速开始

1. **填**：把各文件里的 `{{占位符}}` 换成你的真实信息（不填也能用，AI 会识别为"待补充"）。
2. **接**：按 [`ai-integration.md`](ai-integration.md) 把你常用的 AI 接进来。
3. **用**：每次开新对话，让 AI 先读 `identity/` + `workflow/` 两个目录。
4. **养**：AI 完成任务后，把新经验追加到 `memory/`，不要覆盖历史。

## 约定

- 所有文件首行是 `# AI-READONLY: xxx` 标签，用于 AI 快速定位。
- **READONLY 的含义**：AI 可自由读取；**修改需用户显式授权**（防止误删篡改）。
- 只追加不覆盖：`memory/` 与 `archive/` 遵循 append-only。
- 每次修改都有 git 记录，可回退到任意历史版本。

## 安全提醒

⚠️ 本仓库若设为 **public**，其中内容对全网可见、可能被搜索引擎收录。**请勿直接填入身份证号、手机号、住址、账号密码、客户名单等敏感信息**；如需存敏感内容，请把仓库改为 private（`gh repo edit --visibility private --accept-visibility-change-consequences`）。
