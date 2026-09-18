# math-physics-skills

Claude Code 插件：数学/物理推导 skill ＋ 配套子代理。本仓库同时充当 **marketplace**，
可直接通过 `/plugin` 安装。

## 安装

```
/plugin marketplace add w-c-y-l/math-physics-skills
/plugin install math-physics@math-physics-skills
```

安装后 skill 的调用名带命名空间：`/math-physics:math-physics-complete`（按 description
自动触发同样有效）。

## 包含什么

### skill：`math-physics-complete`

适用于一切需要严谨公式与逐步推导的场景：数学、理论物理、工程物理、控制论、力学、电磁学、
量子/统计/连续介质物理、公式密集的文献阅读、推导、证明、量纲分析，以及产出 Overleaf
可直接编译的 LaTeX 文档。

- **完整推导**：先文字复述问题，声明坐标系、号差、单位制与假设，再逐步推导，不跳步、不省略边界/初始条件。
- **符号定义**：每个符号首次出现即定义；保留求和与积分范围、域与边界条件。
- **自动检查**：收尾做量纲、符号约定与极限情形校验，发现歧义会指出而非臆造。
- **符号表**：长推导附「符号 — 含义 — 单位」对照表，按类别分组。
- **论文公式解释**：带原式编号，逐符号定义，讲清公式在文中的角色与前后衔接。

交付方式：

- **命令行**：以可渲染 LaTeX（`\(...\)` / `\[...\]`）直接输出推导。
- **Overleaf**：生成 .tex 并写入项目（中文走 ctexart + XeLaTeX）；长载荷走 gzip + 索引分段的注入通道。
- **模型分级**：重推导核心可整体打包给最高推理档子代理（fable 槽），提问/排版/交付等胶水
  流程留在主会话，控制成本。

使用：触发 skill 后按门禁逐轮回答（内容计划 → 目的地 → 新写/改 → 模型档位），即可得到
完整、可检查的推导交付。

### agent：`deriver`

skill 的「最高档 fable」分支所用的推导执行器。它**必须是 agent 而非 skill**——因为 skill
只能在当前会话模型上执行，换不了模型；只有子代理才能被解析到 fable 槽。
只回传推导内容本身，不做排版、不碰浏览器、不选目的地。

frontmatter 为 `model: fable`，由 harness 按 `ANTHROPIC_DEFAULT_FABLE_MODEL` 解析到当前
provider 的最强推理档。**该解析在插件装载后是否仍然生效，需在首次安装后实测确认。**

## 目录

```
.claude-plugin/marketplace.json                     市场清单
plugins/math-physics/
├── .claude-plugin/plugin.json                      插件清单
├── skills/math-physics-complete/SKILL.md           skill 定义与全部规则
└── agents/deriver.md                               推导子代理定义
```

## 开发与更新

本仓库的开发工作副本即插件的来源。Claude Code 安装后克隆到自己的缓存目录，
**不从本目录直接加载**。

### 更新流程（实测）

1. 在本仓库改文件，`git commit` ＋ `git push`
2. `/plugin marketplace update math-physics-skills`
   —— 实测输出 `Updated 1 marketplace (1 plugin bumped)`
3. `/reload-plugins` 让当前会话重新加载

**不要用第 3 步代替第 2 步**：`/reload-plugins` 只从**现有缓存**重新加载，不拉取新
commit。实测：只跑 reload 时缓存仍停在旧 sha，改动不会生效。

### 缓存机制

按 **commit sha** 缓存，实测路径
`~/.claude/plugins/cache/math-physics-skills/math-physics/<sha 前 12 位>/`。
因此**不需要手工提升 `plugin.json` 的 `version` 字段**。新旧 sha 的缓存目录并存，
回退只需切回旧 sha。

### 改完记得核对调用名

插件装载会把组件命名空间化：skill 是 `math-physics:math-physics-complete`，
**agent 是 `math-physics:deriver`**（不是裸 `deriver`）。跨文件引用这类名字时，
插件化会静默改变它们——改完务必端到端跑一次，别只做单点检查。
