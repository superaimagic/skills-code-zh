# Issue tracker：GitLab

此仓库的 issue 和 PRD 存放在 GitLab issue 中。所有操作使用 [`glab`](https://gitlab.com/gitlab-org/cli) CLI。

## 约定

- **创建 issue**：`glab issue create --title "..." --description "..."`。多行描述使用 heredoc。传 `--description -` 打开编辑器。
- **读取 issue**：`glab issue view <number> --comments`。使用 `-F json` 获取机器可读输出。
- **列出 issue**：`glab issue list -F json`，配合适当的 `--label` 过滤。
- **评论 issue**：`glab issue note <number> --message "..."`。GitLab 将评论称为"note"。
- **添加/移除标签**：`glab issue update <number> --label "..."` / `--unlabel "..."`。多个标签可逗号分隔或重复标志。
- **关闭**：`glab issue close <number>`。`glab issue close` 不接受关闭评论，所以先用 `glab issue note <number> --message "..."` 发解释，然后关闭。
- **合并请求**：GitLab 将 PR 称为"合并请求"。使用 `glab mr create`、`glab mr view`、`glab mr note` 等——与 `gh pr ...` 形状相同，只是用 `mr` 替代 `pr`，`note`/`--message` 替代 `comment`/`--body`。

从 `git remote -v` 推断仓库——在 clone 内运行时 `glab` 自动完成。

## 当 skill 说"发布到 issue tracker"

创建 GitLab issue。

## 当 skill 说"获取相关工单"

运行 `glab issue view <number> --comments`。
