---
name: scaffold-exercises
description: 创建带有章节、问题、解答和解释且通过 lint 检查的练习目录结构。当用户想要搭建练习、创建练习存根、或设置新课程章节时使用。
---

# 搭建练习

创建通过 `pnpm ai-hero-cli internal lint` 的练习目录结构，然后用 `git commit` 提交。

## 目录命名

- **章节**：`exercises/` 内的 `XX-section-name/`（例如 `01-retrieval-skill-building`）
- **练习**：章节内的 `XX.YY-exercise-name/`（例如 `01.03-retrieval-with-bm25`）
- 章节编号 = `XX`，练习编号 = `XX.YY`
- 名称用 dash-case（小写，连字符）

## 练习变体

每个练习至少需要以下子文件夹之一：

- `problem/` - 带 TODO 的学生工作区
- `solution/` - 参考实现
- `explainer/` - 概念材料，没有 TODO

创建存根时，默认用 `explainer/`，除非计划另有指定。

## 必需文件

每个子文件夹（`problem/`、`solution/`、`explainer/`）需要一个 `readme.md`：

- **不能为空**（必须有实际内容，哪怕一行标题也行）
- 没有坏链接

创建存根时，生成带标题和描述的最小 readme：

```md
# 练习标题

描述内容
```

如果子文件夹有代码，还需要 `main.ts`（>1 行）。但存根只需要 readme 就行。

## 工作流

1. **解析计划** - 提取章节名、练习名和变体类型
2. **创建目录** - 对每个路径 `mkdir -p`
3. **创建存根 readme** - 每个变体文件夹一个带标题的 `readme.md`
4. **运行 lint** - `pnpm ai-hero-cli internal lint` 验证
5. **修复错误** - 迭代直到 lint 通过

## Lint 规则摘要

Linter（`pnpm ai-hero-cli internal lint`）检查：

- 每个练习有子文件夹（`problem/`、`solution/`、`explainer/`）
- 至少存在 `problem/`、`explainer/` 或 `explainer.1/` 之一
- 主子文件夹中 `readme.md` 存在且非空
- 没有 `.gitkeep` 文件
- 没有 `speaker-notes.md` 文件
- readme 中没有坏链接
- readme 中没有 `pnpm run exercise` 命令
- 每个子文件夹需要 `main.ts`，除非仅含 readme

## 移动/重命名练习

重新编号或移动练习时：

1. 用 `git mv`（不是 `mv`）重命名目录——保留 git 历史
2. 更新数字前缀以维持顺序
3. 移动后重新运行 lint

示例：

```bash
git mv exercises/01-retrieval/01.03-embeddings exercises/01-retrieval/01.04-embeddings
```

## 示例：从计划创建存根

给定计划：

```
章节 05: Memory Skill Building
- 05.01 Introduction to Memory
- 05.02 Short-term Memory (explainer + problem + solution)
- 05.03 Long-term Memory
```

创建：

```bash
mkdir -p exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer
mkdir -p exercises/05-memory-skill-building/05.02-short-term-memory/{explainer,problem,solution}
mkdir -p exercises/05-memory-skill-building/05.03-long-term-memory/explainer
```

然后创建 readme 存根：

```
exercises/05-memory-skill-building/05.01-introduction-to-memory/explainer/readme.md -> "# Introduction to Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/explainer/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/problem/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.02-short-term-memory/solution/readme.md -> "# Short-term Memory"
exercises/05-memory-skill-building/05.03-long-term-memory/explainer/readme.md -> "# Long-term Memory"
```
