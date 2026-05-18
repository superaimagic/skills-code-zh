---
name: handoff
description: 将当前对话压缩为交接文档，供另一个 agent 继续工作。
argument-hint: "下一次会话将用于什么？"
---

写一份交接文档，总结当前对话，使新的 agent 能继续工作。保存到 `mktemp -t handoff-XXXXXX.md` 生成的路径（写入前先读取文件）。

建议下一次会话应使用的 skill（如果有）。

不要重复已捕获在其他产出物中的内容（PRD、计划、ADR、issue、commit、diff）。改为通过路径或 URL 引用它们。

如果用户传了参数，将其视为下一次会话将聚焦的内容描述，并相应调整文档。
