---
name: triage
description: 通过分流角色驱动的状态机对 issue 进行分流。当用户想要创建 issue、对 issue 进行分流、审查收到的 bug 或功能请求、为 AFK agent 准备 issue、或管理工作流时使用。
---

# Triage（分流）

将项目 issue tracker 上的 issue 通过一个小型分流角色状态机进行流转。

分流期间发布到 issue tracker 的每条评论或 issue **必须**以此免责声明开头：

```
> *此内容由 AI 在分流过程中生成。*
```

## 参考文档

- [AGENT-BRIEF.md](AGENT-BRIEF.md) — 如何编写持久的 agent 简报
- [OUT-OF-SCOPE.md](OUT-OF-SCOPE.md) — `.out-of-scope/` 知识库如何工作

## 角色

两个**类别**角色：

- `bug` — 有东西坏了
- `enhancement` — 新功能或改进

五个**状态**角色：

- `needs-triage` — 需要维护者评估
- `needs-info` — 等待报告者提供更多信息
- `ready-for-agent` — 已完全明确，可供 AFK agent 使用
- `ready-for-human` — 需要人工实现
- `wontfix` — 不会处理

每个被分流的 issue 应恰好携带一个类别角色和一个状态角色。如果状态角色冲突，标记它并在做任何事之前问维护者。

这些是规范角色名——issue tracker 中使用的实际标签字符串可能不同。映射应该已经提供给你了——如果没有，运行 `/setup-matt-pocock-skills`。

状态转换：未标签的 issue 通常先进入 `needs-triage`；从那里移到 `needs-info`、`ready-for-agent`、`ready-for-human` 或 `wontfix`。`needs-info` 在报告者回复后回到 `needs-triage`。维护者可以随时覆盖——标记看起来不寻常的转换，先问再继续。

## 调用

维护者调用 `/triage` 并用自然语言描述他们想要什么。解释请求并执行。示例：

- "给我看需要我关注的"
- "看看 #42"
- "把 #42 移到 ready-for-agent"
- "有什么 agent 可以领取的？"

## 展示需要关注的内容

查询 issue tracker 并展示三个桶，按最旧的排前：

1. **未标签** — 从未分流过。
2. **`needs-triage`** — 评估进行中。
3. **`needs-info` 且报告者在上次分流记录后有新活动** — 需要重新评估。

展示数量和每个 issue 的一行摘要。让维护者选择。

## 分流特定 issue

1. **收集上下文。** 读取完整 issue（正文、评论、标签、报告者、日期）。解析之前的分流记录，这样你不会重新问已解决的问题。使用项目的领域词汇表探索代码库，尊重触及区域的 ADR。读取 `.out-of-scope/*.md`，展示与该 issue 相似的先前拒绝。

2. **推荐。** 告诉维护者你的类别和状态推荐及理由，加上与 issue 相关的简短代码库摘要。等待指示。

3. **复现（仅 bug）。** 在任何烤问之前，尝试复现：读取报告者的步骤，追踪相关代码，运行测试或命令。报告结果——成功复现及代码路径、复现失败、或信息不足（强 `needs-info` 信号）。确认的复现能产生更强的 agent 简报。

4. **烤问（如果需要）。** 如果 issue 需要充实，运行 `/grill-with-docs` 会话。

5. **应用结果：**
   - `ready-for-agent` — 发布 agent 简报评论（[AGENT-BRIEF.md](AGENT-BRIEF.md)）。
   - `ready-for-human` — 与 agent 简报相同的结构，但注明为什么不能委托（判断力、外部访问、设计决策、手动测试）。
   - `needs-info` — 发布分流记录（下面模板）。
   - `wontfix`（bug）— 礼貌解释，然后关闭。
   - `wontfix`（enhancement）— 写入 `.out-of-scope/`，从评论中链接，然后关闭（[OUT-OF-SCOPE.md](OUT-OF-SCOPE.md)）。
   - `needs-triage` — 应用角色。如有部分进展可选加评论。

## 快速状态覆盖

如果维护者说"把 #42 移到 ready-for-agent"，信任他们并直接应用角色。确认你将做什么（角色变更、评论、关闭），然后执行。跳过烤问。如果移到 `ready-for-agent` 但没有烤问会话，问是否要写 agent 简报。

## Needs-info 模板

```markdown
## 分流记录

**目前已确认的：**

- 要点 1
- 要点 2

**仍需你提供的（@reporter）：**

- 问题 1
- 问题 2
```

把烤问期间解决的所有内容捕获到"目前已确认的"中，这样工作不会丢失。问题必须具体且可操作，不是"请提供更多信息"。

## 恢复之前的会话

如果 issue 上有之前的分流记录，读取它们，检查报告者是否回答了任何未解决的问题，在继续之前展示更新后的情况。不要重新问已解决的问题。
