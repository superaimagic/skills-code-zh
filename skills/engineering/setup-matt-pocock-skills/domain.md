# 领域文档

engineering skill 在探索代码库时应如何消费此仓库的领域文档。

## 探索前，先读取这些

- 仓库根目录的 **`CONTEXT.md`**，或
- 仓库根目录的 **`CONTEXT-MAP.md`**（如果存在）——它指向每个上下文一个 `CONTEXT.md`。读取与主题相关的每一个。
- **`docs/adr/`** —— 读取触及你即将工作区域的 ADR。在多上下文仓库中，也检查 `src/<context>/docs/adr/` 获取上下文范围的决策。

如果这些文件不存在，**静默继续**。不要标记它们的缺失；不要建议预先创建。生产者 skill（`/grill-with-docs`）在术语或决策实际被解决时惰性创建它们。

## 文件结构

单上下文仓库（大多数仓库）：

```
/
├── CONTEXT.md
├── docs/adr/
│   ├── 0001-event-sourced-orders.md
│   └── 0002-postgres-for-write-model.md
└── src/
```

多上下文仓库（根目录存在 `CONTEXT-MAP.md`）：

```
/
├── CONTEXT-MAP.md
├── docs/adr/                          ← 系统级决策
└── src/
    ├── ordering/
    │   ├── CONTEXT.md
    │   └── docs/adr/                  ← 上下文特定决策
    └── billing/
        ├── CONTEXT.md
        └── docs/adr/
```

## 使用词汇表的词汇

当你的输出命名一个领域概念时（在 issue 标题、重构提案、假设、测试名中），使用 `CONTEXT.md` 中定义的术语。不要漂移到词汇表明确避免的同义词。

如果你需要的概念还不在线索中，那是一个信号——要么你在发明项目不使用的语言（重新考虑），要么存在真正的缺口（记下来给 `/grill-with-docs`）。

## 标记 ADR 冲突

如果你的输出与现有 ADR 矛盾，显式标记而不是静默覆盖：

> _与 ADR-0007（事件溯源订单）矛盾——但值得重新讨论，因为…_
