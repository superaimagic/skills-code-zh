---
name: review
description: 沿两个轴审查自固定点（commit、分支、tag 或 merge-base）以来的变更——标准（代码是否遵循仓库的文档化编码标准？）和规格（代码是否匹配原始 issue/PRD 的要求？）。两个审查作为并行子 agent 运行并并排报告。当用户想要审查分支、PR、进行中的变更、或说"review since X"时使用。
---

# 审查

对 `HEAD` 和用户提供的固定点之间 diff 的双轴审查：

- **标准** — 代码是否符合仓库文档化的编码标准？
- **规格** — 代码是否忠实地实现了原始 issue / PRD / 规格？

两个轴作为**并行子 agent** 运行，互不污染上下文，然后此 skill 聚合它们的发现。

应该已提供 issue tracker——如果 `docs/agents/issue-tracker.md` 缺失，运行 `/setup-matt-pocock-skills`。

## 流程

### 1. 确定固定点

用户说的就是固定点——commit SHA、分支名、tag、`main`、`HEAD~5` 等。不要自作主张；直接传递。如果没指定，问："审查对照什么——分支、commit 还是 `main`？"获得答案前不要继续。

一次性确定 diff 命令：`git diff <fixed-point>...HEAD`（三点号，比较基于 merge-base）。同时通过 `git log <fixed-point>..HEAD --oneline` 记录 commit 列表。

### 2. 识别规格来源

按此顺序查找原始规格：

1. Commit 消息中的 issue 引用（`#123`、`Closes #45`、GitLab `!67` 等）——通过 `docs/agents/issue-tracker.md` 中的工作流获取。
2. 用户作为参数传入的路径。
3. `docs/`、`specs/` 或 `.scratch/` 下匹配分支名或功能的 PRD/规格文件。
4. 如果什么都没找到，问用户规格在哪里。如果说没有，**规格**子 agent 将跳过并报告"无可用规格"。

### 3. 识别标准来源

仓库中任何记录代码应如何编写的文档。常见位置：

- `CLAUDE.md`、`AGENTS.md`
- `CONTRIBUTING.md`
- `CONTEXT.md`、`CONTEXT-MAP.md`、每个上下文的 `CONTEXT.md` 文件
- `docs/adr/`（架构决策即标准）
- `.editorconfig`、`eslint.config.*`、`biome.json`、`prettier.config.*`、`tsconfig.json`（机器强制标准——记下但不重新检查工具已检查的内容）
- 仓库根目录或 `docs/` 下的 `STYLE.md`、`STANDARDS.md`、`STYLEGUIDE.md` 或类似文件

收集文件列表。**标准**子 agent 将读取它们。

### 4. 并行启动两个子 agent

发送一条消息，包含两个 `Agent` 工具调用。两个都用 `general-purpose` 子 agent。

**标准子 agent 提示**——包含：

- 完整 diff 命令和 commit 列表。
- 第 3 步找到的标准来源文件列表。
- 任务："阅读标准文档。然后阅读 diff。报告——按文件/hunk 相关处——diff 违反文档化标准的每一处。引用标准（文件 + 规则）。区分硬性违规和判断性取舍。跳过工具已强制执行的。400 字以内。"

**规格子 agent 提示**——包含：

- diff 命令和 commit 列表。
- 规格路径或获取的内容。
- 任务："阅读规格。然后阅读 diff。报告：(a) 规格要求但缺失或不完整的需求；(b) diff 中未被要求的行为（范围蔓延）；(c) 看起来已实现但实现有误的需求。每个发现引用规格原文。400 字以内。"

如果缺少规格，跳过规格子 agent 并在最终报告中注明。

### 5. 聚合

在 `## 标准` 和 `## 规格` 标题下呈现两份报告，原文或轻度清理。**不要**合并或重排发现——两个轴刻意分开，让用户独立查看。

以一行总结结束：每个轴的发现总数，以及标记的最严重单个问题（如果有）。

## 为什么两个轴

一个变更可以通过一个轴而未通过另一个：

- 代码遵循每个标准但实现了错误的东西 → **标准通过，规格失败。**
- 代码完全按 issue 要求但违反了项目约定 → **规格通过，标准失败。**

分开报告防止一个轴掩盖另一个。
