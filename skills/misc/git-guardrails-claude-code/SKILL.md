---
name: git-guardrails-claude-code
description: 设置 Claude Code hook 来阻止危险 git 命令（push、reset --hard、clean、branch -D 等）在执行前被拦截。当用户想要防止破坏性 git 操作、添加 git 安全 hook、或在 Claude Code 中阻止 git push/reset 时使用。
---

# 设置 Git 安全护栏

设置一个 PreToolUse hook，在 Claude 执行之前拦截并阻止危险 git 命令。

## 被阻止的操作

- `git push`（所有变体，包括 `--force`）
- `git reset --hard`
- `git clean -f` / `git clean -fd`
- `git branch -D`
- `git checkout .` / `git restore .`

被阻止时，Claude 会看到一条消息，告知它无权执行这些命令。

## 步骤

### 1. 询问范围

问用户：仅为**此项目**安装（`.claude/settings.json`）还是**所有项目**（`~/.claude/settings.json`）？

### 2. 复制 hook 脚本

捆绑脚本位于：[scripts/block-dangerous-git.sh](scripts/block-dangerous-git.sh)

根据范围复制到目标位置：

- **项目**：`.claude/hooks/block-dangerous-git.sh`
- **全局**：`~/.claude/hooks/block-dangerous-git.sh`

用 `chmod +x` 使其可执行。

### 3. 将 hook 添加到设置

添加到相应的设置文件：

**项目**（`.claude/settings.json`）：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/block-dangerous-git.sh"
          }
        ]
      }
    ]
  }
}
```

**全局**（`~/.claude/settings.json`）：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "~/.claude/hooks/block-dangerous-git.sh"
          }
        ]
      }
    ]
  }
}
```

如果设置文件已存在，将 hook 合并到已有的 `hooks.PreToolUse` 数组中——不要覆盖其他设置。

### 4. 询问自定义

问用户是否想从阻止列表中添加或移除任何模式。相应编辑复制的脚本。

### 5. 验证

快速测试：

```bash
echo '{"tool_input":{"command":"git push origin main"}}' | <脚本路径>
```

应该以代码 2 退出并在 stderr 打印 BLOCKED 消息。
