# Issue tracker 集成仅限于主流工具

`setup-matt-pocock-skills` 仅对**主流** issue tracker 提供一等支持。添加小众、新兴或单一厂商实验性 tracker 支持的请求不在范围内。

## 为什么不在范围内

每个 issue-tracker 后端都会将 CLI 形态硬编码到 skill 中（命令、标志、输出解析）。每个新后端都是永久的维护负担——它必须随着工具 CLI 的演进而持续工作，并持续接受 `/to-prd`、`/to-issues`、`/triage` 等的测试。只有当相当比例的用户真正使用某个 tracker 时，这个成本才值得支付。

"主流"是一个判断，不是数字门槛：

- GitHub、GitLab 和 Backlog.md 属于我们会认为是主流的工具——广泛知名、广泛使用、早已过了实验阶段。
- 一个只有几百个 GitHub star 的新 agent 专用工具不算，无论设计多有趣。

Star 数、年龄和下载数是有用的信号，但都不是规则。规则是：一个典型的工程师是否会认识这个工具，并有可能为团队选择了它？

非主流 tracker 的逃生通道已经存在：

- `local markdown` 用于轻量级仓库内追踪。
- `other/custom` 用于想自己接线的用户。

两者都不需要核心 skill 了解特定工具。

## 先前请求

- #99 — "Add dex as an issue tracker backend"（dex 在请求时约 3 个月大，约 300 star）
