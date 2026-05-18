---
name: setup-matt-pocock-skills
description: 在 AGENTS.md/CLAUDE.md 中设置 `## Agent skills` 块和 `docs/agents/`，使 engineering skill 知道此仓库的 issue tracker（GitHub 或本地 markdown）、分流标签词汇表和领域文档布局。在首次使用 `to-issues`、`to-prd`、`triage`、`diagnose`、`tdd`、`improve-codebase-architecture` 或 `zoom-out` 之前运行——或者如果这些 skill 似乎缺少关于 issue tracker、分流标签或领域文档的上下文。
disable-model-invocation: true
---

# Setup Matt Pocock's Skills

搭建 engineering skill 所假设的按仓库配置：

- **Issue tracker** — issue 存放在哪里（默认 GitHub；也支持开箱即用的本地 markdown）
- **分流标签** — 五个规范分流角色使用的字符串
- **领域文档** — `CONTEXT.md` 和 ADR 存放在哪里，以及读取它们的消费规则

这是一个 prompt 驱动的 skill，不是确定性脚本。探索，展示你发现了什么，与用户确认，然后写。

## 流程

### 1. 探索

查看当前仓库了解其起始状态。读取任何存在的东西；不要假设：

- `git remote -v` 和 `.git/config` — 这是 GitHub 仓库吗？哪个？
- 仓库根目录的 `AGENTS.md` 和 `CLAUDE.md` — 哪个存在吗？其中一个是否已有 `## Agent skills` 节？
- 仓库根目录的 `CONTEXT.md` 和 `CONTEXT-MAP.md`
- `docs/adr/` 和任何 `src/*/docs/adr/` 目录
- `docs/agents/` — 此 skill 的先前输出是否已存在？
- `.scratch/` — 表示本地 markdown issue tracker 约定已在使用

### 2. 展示发现并询问

总结什么存在什么缺失。然后**一次一个**引导用户通过三个决策——展示一个部分，获得用户的答案，然后移到下一个。不要一次倾倒三个。

假设用户不知道这些术语是什么意思。每个部分以一个简短解释开头（它是什么，为什么这些 skill 需要它，如果选择不同会发生什么变化）。然后展示选项和默认值。

**部分 A — Issue tracker。**

> 解释：issue tracker 是此仓库 issue 存放的地方。`to-issues`、`triage`、`to-prd` 和 `qa` 等 skill 从中读取和写入——它们需要知道是调用 `gh issue create`、在 `.scratch/` 下写 markdown 文件，还是遵循你描述的其他工作流。选一个你实际为此仓库追踪工作的地方。

默认立场：这些 skill 为 GitHub 设计。如果 `git remote` 指向 GitHub，提议那个。如果 `git remote` 指向 GitLab（`gitlab.com` 或自托管主机），提议 GitLab。否则（或如果用户偏好），提供：

- **GitHub** — issue 存在于仓库的 GitHub Issues（使用 `gh` CLI）
- **GitLab** — issue 存在于仓库的 GitLab Issues（使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI）
- **本地 markdown** — issue 作为此仓库中 `.scratch/<feature>/` 下的文件存在（适合独立项目或没有远程的仓库）
- **其他**（Jira、Linear 等）— 要求用户用一段话描述工作流；skill 会将其记录为自由格式文本

**部分 B — 分流标签词汇表。**

> 解释：当 `triage` skill 处理传入 issue 时，它通过状态机移动它——需要评估、等待报告者、可供 AFK agent 领取、需要人工、或不会修复。为此，它需要应用与你*实际配置*的字符串匹配的标签（或你的 issue tracker 中的等效物）。如果你的仓库已使用不同的标签名（例如 `bug:triage` 而不是 `needs-triage`），在此映射它们，这样 skill 会应用正确的标签，而不是创建重复。

五个规范角色：

- `needs-triage` — 需要维护者评估
- `needs-info` — 等待报告者
- `ready-for-agent` — 已完全明确，AFK 就绪（agent 可以在无人文上下文的情况下领取）
- `ready-for-human` — 需要人工实现
- `wontfix` — 不会处理

默认：每个角色的字符串等于其名称。问用户是否要覆盖任何一个。如果他们的 issue tracker 没有已有标签，默认就行。

**部分 C — 领域文档。**

> 解释：一些 skill（`improve-codebase-architecture`、`diagnose`、`tdd`）读取 `CONTEXT.md` 文件以学习项目的领域语言，读取 `docs/adr/` 了解过去的架构决策。它们需要知道仓库是有一个全局上下文还是多个（例如带有独立 frontend/backend 上下文的 monorepo），以便在正确的地方查找。

确认布局：

- **单上下文** — 仓库根目录一个 `CONTEXT.md` + `docs/adr/`。大多数仓库是这样。
- **多上下文** — 根目录的 `CONTEXT-MAP.md` 指向每个上下文的 `CONTEXT.md` 文件（通常是 monorepo）。

### 3. 确认并编辑

向用户展示以下内容草稿：

- 要添加到 `CLAUDE.md` / `AGENTS.md` 中（见步骤 4 选择规则）的 `## Agent skills` 块
- `docs/agents/issue-tracker.md`、`docs/agents/triage-labels.md`、`docs/agents/domain.md` 的内容

让他们在写入前编辑。

### 4. 写入

**选择要编辑的文件：**

- 如果 `CLAUDE.md` 存在，编辑它。
- 否则如果 `AGENTS.md` 存在，编辑它。
- 如果都不存在，问用户创建哪个——不要替他们选。

当 `CLAUDE.md` 已存在时永远不要创建 `AGENTS.md`（反之亦然）——始终编辑已存在的那个。

如果所选文件中已存在 `## Agent skills` 块，就地更新其内容而不是追加重复。不要覆盖用户对周围部分的编辑。

块：

```markdown
## Agent skills

### Issue tracker

[一行总结 issue 在哪里追踪]。见 `docs/agents/issue-tracker.md`。

### Triage labels

[一行总结标签词汇表]。见 `docs/agents/triage-labels.md`。

### Domain docs

[一行总结布局——"单上下文"或"多上下文"]。见 `docs/agents/domain.md`。
```

然后使用此 skill 文件夹中的种子模板作为起点写入三个文档文件：

- [issue-tracker-github.md](./issue-tracker-github.md) — GitHub issue tracker
- [issue-tracker-gitlab.md](./issue-tracker-gitlab.md) — GitLab issue tracker
- [issue-tracker-local.md](./issue-tracker-local.md) — 本地 markdown issue tracker
- [triage-labels.md](./triage-labels.md) — 标签映射
- [domain.md](./domain.md) — 领域文档消费规则 + 布局

对于"其他"issue tracker，使用用户的描述从头写 `docs/agents/issue-tracker.md`。

### 5. 完成

告诉用户设置已完成，哪些 engineering skill 现在将读取这些文件。提到他们以后可以直接编辑 `docs/agents/*.md`——只有在想切换 issue tracker 或从头开始时才需要重新运行此 skill。
