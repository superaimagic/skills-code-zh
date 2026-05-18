Skills 按 `skills/` 下的分桶文件夹组织：

- `engineering/` — 日常编码工作
- `productivity/` — 日常非编码工作流工具
- `misc/` — 留着但很少用
- `personal/` — 与个人设置相关，不对外推广
- `in-progress/` — 尚未完善的草稿
- `deprecated/` — 已弃用

`engineering/`、`productivity/` 和 `misc/` 中的每个 skill 必须在顶层 `README.md` 中有引用，并在 `.claude-plugin/plugin.json` 中有条目。`personal/`、`in-progress/` 和 `deprecated/` 中的 skill 不得出现在这两处。

顶层 `README.md` 中的每个 skill 条目必须将 skill 名称链接到其 `SKILL.md`。

每个分桶文件夹都有一个 `README.md`，列出该桶中的所有 skill 及其一行描述，skill 名称链接到其 `SKILL.md`。
