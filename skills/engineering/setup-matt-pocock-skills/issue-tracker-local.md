# Issue tracker：本地 Markdown

此仓库的 issue 和 PRD 存放在 `.scratch/` 中的 markdown 文件里。

## 约定

- 每个功能一个目录：`.scratch/<feature-slug>/`
- PRD 为 `.scratch/<feature-slug>/PRD.md`
- 实现 issue 为 `.scratch/<feature-slug>/issues/<NN>-<slug>.md`，从 `01` 开始编号
- 分流状态记录在每个 issue 文件顶部附近的 `Status:` 行（见 `triage-labels.md` 获取角色字符串）
- 评论和对话历史追加到文件底部的 `## Comments` 标题下

## 当 skill 说"发布到 issue tracker"

在 `.scratch/<feature-slug>/` 下创建新文件（需要时创建目录）。

## 当 skill 说"获取相关工单"

读取引用路径的文件。用户通常会直接传递路径或 issue 编号。
