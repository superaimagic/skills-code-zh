# 翻译指令

将本仓库中所有 `.md` 文件从英文翻译为中文。

## 翻译规则

1. **frontmatter**（`---` 包围的YAML）中：
   - `description` 字段翻译成中文
   - `name` 字段保留英文不翻译
   
2. **正文**全部翻译成中文

3. **保持不变**：
   - markdown 格式结构
   - 链接路径（如 `./SKILL.md`、`./CONTEXT-FORMAT.md`）
   - 代码块内容
   - HTML 标签

4. **技术术语保留英文**（首次出现可加中文注释）：
   - TDD、ADR、SOLID、red-green-refactor
   - PRD、CI/CD、PR、Issue
   - 其他通用技术术语

5. **专有名词不翻译**：
   - 文件名：`CONTEXT.md`、`SKILL.md`、`CLAUDE.md` 等
   - 命令名：`/grill-me`、`/setup-matt-pocock-skills` 等
   - 项目名：Claude Code、Codex 等

6. **人名书名保留英文**

7. **翻译风格**：
   - 自然流畅，避免机翻味
   - 技术文档风格，简洁准确
   - 保留原文的语气和强调方式

8. **README.md 特殊处理**：
   - 安装命令中的仓库名改为 `superaimagic/skills-code-zh`
   - 即 `npx skills@latest add superaimagic/skills-code-zh`

9. **CONTEXT.md 特殊处理**：
   - 这是共享语言/术语表，翻译时保持术语定义的精确性

10. **LICENSE 文件不翻译**

## 执行方式

找到所有 `.md` 文件（排除 LICENSE），逐个读取、翻译、覆盖写回。使用 UTF-8 编码写入。

## 文件列表（60个）

根目录：
- README.md
- CLAUDE.md
- CONTEXT.md

docs/：
- docs/adr/0001-explicit-setup-pointer-only-for-hard-dependencies.md

.out-of-scope/：
- .out-of-scope/mainstream-issue-trackers-only.md
- .out-of-scope/question-limits.md
- .out-of-scope/setup-skill-verify-mode.md

skills/engineering/：
- skills/engineering/README.md
- skills/engineering/diagnose/SKILL.md
- skills/engineering/grill-with-docs/SKILL.md
- skills/engineering/grill-with-docs/ADR-FORMAT.md
- skills/engineering/grill-with-docs/CONTEXT-FORMAT.md
- skills/engineering/improve-codebase-architecture/SKILL.md
- skills/engineering/improve-codebase-architecture/DEEPENING.md
- skills/engineering/improve-codebase-architecture/INTERFACE-DESIGN.md
- skills/engineering/improve-codebase-architecture/LANGUAGE.md
- skills/engineering/prototype/SKILL.md
- skills/engineering/prototype/LOGIC.md
- skills/engineering/prototype/UI.md
- skills/engineering/setup-matt-pocock-skills/SKILL.md
- skills/engineering/setup-matt-pocock-skills/domain.md
- skills/engineering/setup-matt-pocock-skills/issue-tracker-github.md
- skills/engineering/setup-matt-pocock-skills/issue-tracker-gitlab.md
- skills/engineering/setup-matt-pocock-skills/issue-tracker-local.md
- skills/engineering/setup-matt-pocock-skills/triage-labels.md
- skills/engineering/tdd/SKILL.md
- skills/engineering/tdd/deep-modules.md
- skills/engineering/tdd/interface-design.md
- skills/engineering/tdd/mocking.md
- skills/engineering/tdd/refactoring.md
- skills/engineering/tdd/tests.md
- skills/engineering/to-issues/SKILL.md
- skills/engineering/to-prd/SKILL.md
- skills/engineering/triage/SKILL.md
- skills/engineering/triage/AGENT-BRIEF.md
- skills/engineering/triage/OUT-OF-SCOPE.md
- skills/engineering/zoom-out/SKILL.md

skills/productivity/：
- skills/productivity/README.md
- skills/productivity/caveman/SKILL.md
- skills/productivity/grill-me/SKILL.md
- skills/productivity/handoff/SKILL.md
- skills/productivity/write-a-skill/SKILL.md

skills/misc/：
- skills/misc/README.md
- skills/misc/git-guardrails-claude-code/SKILL.md
- skills/misc/migrate-to-shoehorn/SKILL.md
- skills/misc/scaffold-exercises/SKILL.md
- skills/misc/setup-pre-commit/SKILL.md

skills/personal/：
- skills/personal/README.md
- skills/personal/edit-article/SKILL.md
- skills/personal/obsidian-vault/SKILL.md

skills/in-progress/：
- skills/in-progress/README.md
- skills/in-progress/review/SKILL.md
- skills/in-progress/writing-beats/SKILL.md
- skills/in-progress/writing-fragments/SKILL.md
- skills/in-progress/writing-shape/SKILL.md

skills/deprecated/：
- skills/deprecated/README.md
- skills/deprecated/design-an-interface/SKILL.md
- skills/deprecated/qa/SKILL.md
- skills/deprecated/request-refactor-plan/SKILL.md
- skills/deprecated/ubiquitous-language/SKILL.md
