# 仅对硬依赖显式指向 `/setup-matt-pocock-skills`

Engineering skill 依赖于由 `/setup-matt-pocock-skills` 初始化的按仓库配置（issue tracker、分流标签词汇表、领域文档布局）。某些 skill 没有这些配置就无法正常工作——它们必须发布到特定的 issue tracker 或应用特定的标签字符串。另一些仅用它来优化输出（词汇、ADR 感知），没有它也能优雅降级。

我们将这些分为**硬依赖**和**软依赖** skill：

- **硬依赖**（`to-issues`、`to-prd`、`triage`）— 包含一行显式提示：_"…应该已经提供给你了——如果没有，运行 `/setup-matt-pocock-skills`。"_ 没有映射，输出是错误的，而不仅仅是模糊的。
- **软依赖**（`diagnose`、`tdd`、`improve-codebase-architecture`、`zoom-out`）— 仅用模糊的措辞引用"项目的领域词汇表"和"你正在触及区域的 ADR"。如果文档不存在，skill 仍然可以工作；只是输出不够精准。

这种划分使软依赖 skill 保持轻量 token，避免在不关键的地方盲目添加设置指针。
