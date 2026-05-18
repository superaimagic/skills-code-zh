# `setup-matt-pocock-skills` 的验证/检查模式

本项目不会为 `setup-matt-pocock-skills` 添加专用的验证/检查模式（或单独的验证 skill）。

## 为什么不在范围内

第二个 skill——或 `--verify` 标志——用于检查 `docs/agents/*.md` 产物是否仍然匹配种子模板 schema，会重复现有设置 skill 在对话中已经处理的工作。

预期的工作流是：**运行 `/setup-matt-pocock-skills` 并告诉它验证你当前的设置。** 该 skill 是提示驱动的，所以维护者可以将范围限定为验证通过（"不要重写任何东西，只需对照当前种子模板检查我的现有文件并报告偏差"），而不需要单独的代码路径。添加标志或兄弟 skill 会分割一个已可通过自然语言入口表达的功能的表面积。

将配置管理保持在单一 skill 也避免了种子模板演进时两个 skill 互相漂移的维护成本。

## 先前请求

- #106 — Feature request: verify/check mode for setup-matt-pocock-skills
