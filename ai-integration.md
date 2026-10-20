# AI-INTEGRATION: 跨 AI 接入配置

> 本文件说明如何让不同 AI 平台读取并维护本仓库。**它本身就是给 AI 看的操作手册。**

## 一、读取方式（三选一）

### 1. 仓库 URL（最通用）
把仓库地址直接给 AI：
```
https://github.com/{{github用户名}}/personal-ai-archive
```
多数支持联网的 AI 会自动抓取 README 和目录结构。

### 2. 原始文件直链（推荐，最稳定）
```
https://raw.githubusercontent.com/{{github用户名}}/personal-ai-archive/main/identity/01-基本信息.md
```
一次给全：
```
https://raw.githubusercontent.com/{{github用户名}}/personal-ai-archive/main/identity/01-基本信息.md
https://raw.githubusercontent.com/{{github用户名}}/personal-ai-archive/main/workflow/03-沟通风格.md
https://raw.githubusercontent.com/{{github用户名}}/personal-ai-archive/main/workflow/04-任务优先级规则.md
```

### 3. GitHub API（Agent 自动同步用）
```bash
# 列出目录
curl -s https://api.github.com/repos/{{github用户名}}/personal-ai-archive/contents/identity

# 读取单文件（返回 base64 content）
curl -s https://api.github.com/repos/{{github用户名}}/personal-ai-archive/contents/identity/01-基本信息.md
```
私有仓库需带 token：`-H "Authorization: Bearer $GITHUB_TOKEN"`

## 二、各平台接入

### ChatGPT
- **Projects / 自定义 GPT**：把 `identity/` 与 `workflow/` 的核心文件作为"项目知识"上传。
- **每次新对话开场白**：
  > 先读 https://raw.githubusercontent.com/{{github用户名}}/personal-ai-archive/main/identity/01-基本信息.md 和 workflow/03-沟通风格.md，再回答我。

### Claude
- **Projects**：新建 Project，把 5 个目录的核心 md 放进 Project Knowledge。
- **系统提示（Project instructions）**：
  > 我的个人档案在 https://github.com/{{github用户名}}/personal-ai-archive 。回答前先按 identity → workflow → memory 的顺序检索；需要更新时按 ai-integration.md 第四节执行。

### Gemini
- **Gems**：新建 Gem，知识栏粘贴 raw 直链内容。
- 支持联网时可直接给仓库 URL 要求读取。

### 其他 Agent（Meta Muse / OpenAI Dots 等）
- 在 Agent 的"记忆/知识源"中配置 GitHub 仓库地址（或 raw 目录列表）。
- 若 Agent 支持定时同步：配置为**每日拉取一次** `identity/` + `workflow/`。
- 若支持写入：按第四节规则向 `memory/` 追加，禁止改写 `identity/`。

## 三、AI 读取优先级（喂给 AI 的规则）

```
1. identity/01-基本信息.md      ← 我是谁
2. workflow/03-沟通风格.md      ← 怎么跟我说话
3. workflow/04-任务优先级规则.md ← 什么先做
4. workflow/01-工作习惯.md      ← 怎么配合
5. memory/03-AI对话沉淀偏好.md  ← 我讨厌什么、喜欢什么
6. 其余按需检索
```

## 四、自动更新规则（AI 写回档案）

**允许AI写入的目录：`memory/`、`skills/`、`archive/`**
**禁止AI擅自写入的目录：`identity/`、`workflow/`**（需用户显式同意）

写入规范：
1. **只追加（append-only）**：在文件末尾的 `<!-- AI-APPEND-ZONE -->` 之后追加，不改动已有内容。
2. **必须带时间戳**：`### YYYY-MM-DD · 来源AI名称`
3. **一条一条记**：一次只记一个经验/决策，不合并成段落。
4. **写清触发场景**：什么事、我当时怎么选的、结果如何——便于以后检索。
5. **涉及隐私的内容不写**（真实姓名以外的身份信息、联系方式、客户数据）。

追加示例：
```markdown
### 2026-10-19 · Claude
- 场景：写活动方案时我要求"先给结论再给数据"
- 做法：开头三句话给判断，后面才铺数据
- 结果：一次通过，没再改
- 沉淀：所有对外材料都按这个顺序
```

## 五、版本与回退

```bash
# 查看历史
git log --oneline

# 回退单个文件到某次提交
git checkout <commit-hash> -- memory/01-历史经验.md

# 全局回退（谨慎）
git reset --hard <commit-hash>
```

建议在 GitHub 仓库设置里开启 **Branch protection**（main 分支：Require pull request / 禁止 force push），防止 AI 误覆盖。

## 六、更新频率建议

| 目录 | 更新时机 |
|---|---|
| identity/ | 换工作、换城市、目标变化时（低频，用户手动） |
| workflow/ | 工作方式变化时（低频，用户手动） |
| memory/ | **每次 AI 协作后**（高频，AI 可写） |
| skills/ | 每季度复盘一次 |
| archive/ | 项目结束后归档 |

---

**给 AI 的一句话总结**：读 `identity/` 和 `workflow/` 了解我；把新经验追加进 `memory/`；不要动 `identity/` 和 `workflow/` 里的原文。
