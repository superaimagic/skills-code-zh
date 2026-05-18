<p>
  <a href="https://www.aihero.dev/s/skills-newsletter">
    <picture>
      <source media="(prefers-color-scheme: dark)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skills-repo-dark_2x.png">
      <source media="(prefers-color-scheme: light)" srcset="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png">
      <img alt="Skills" src="https://res.cloudinary.com/total-typescript/image/upload/v1777382277/skill-repo-light_2x.png" width="369">
    </picture>
  </a>
</p>

# 给真正工程师的 Skills

[![skills.sh](https://skills.sh/b/mattpocock/skills)](https://skills.sh/mattpocock/skills)

我每天用来做真正工程的 agent 技能包——不是玩票编码。

开发真正的应用很难。GSD、BMAD 和 Spec-Kit 等方案试图通过接管流程来帮忙。但这样做的同时，它们也拿走了你的控制权，让流程中的 bug 变得难以解决。

这些 skill 被设计得小巧、易适配、可组合。它们适用于任何模型，基于数十年的工程经验。随意折腾，变成你自己的。享受它。

如果你想跟踪这些 skill 的变更以及我创建的新 skill，可以加入约 60,000 名开发者的邮件列表：

[订阅邮件列表](https://www.aihero.dev/s/skills-newsletter)

## 快速开始（30秒设置）

1. 运行 skills.sh 安装器：

```bash
npx skills@latest add superaimagic/skills-code-zh
```

2. 选择你想要的 skill，以及要安装到哪些编码 agent 上。**确保选择 `/setup-matt-pocock-skills`**。

3. 在你的 agent 中运行 `/setup-matt-pocock-skills`，它会：
   - 询问你要使用哪个 issue tracker（GitHub、Linear 或本地文件）
   - 询问你在分流（triage）时给 issue 打什么标签（`/triage` 使用标签）
   - 询问你想把创建的文档保存在哪里

4. 搞定——你可以开始了。

## 为什么要有这些 Skill

我创建这些 skill 是为了修复我在 Claude Code、Codex 和其他编码 agent 中看到的常见失败模式。

### #1：Agent 没做我想要的

> "没有人确切知道自己想要什么"
>
> David Thomas & Andrew Hunt，[The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题**。软件开发中最常见的失败模式是对齐偏差。你以为开发知道你想要什么，然后你看到他们做出来的东西——你意识到它根本没有理解你。

在 AI 时代也是一样。你和 agent 之间存在沟通鸿沟。解决之道是**烤问（grilling）会话**——让 agent 就你要构建的东西提出详细问题。

**解决方案**是使用：

- [`/grill-me`](./skills/productivity/grill-me/SKILL.md) — 用于非编码场景
- [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) — 与 [`/grill-me`](./skills/productivity/grill-me/SKILL.md) 相同，但增加了更多功能（见下文）

这些是我最受欢迎的 skill。它们帮助你在开始之前与 agent 对齐，深入思考你要做的变更。每次要做变更时都使用它们。

### #2：Agent 太啰嗦了

> 有了统一语言（ubiquitous language），开发者之间的对话和代码表达都源自同一个领域模型。
>
> Eric Evans，[Domain-Driven-Design](https://www.amazon.co.uk/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215)

**问题**：项目初期，开发者和他们为之构建软件的人（领域专家）通常说着不同的语言。

我在 agent 身上感受到了同样的张力。Agent 通常被扔进一个项目，被要求边干边学行话。所以它们用 20 个词来表达 1 个词就能说清的事情。

**解决方案**是共享语言。这是一份帮助 agent 解码项目中行话的文档。

<details>
<summary>
示例
</summary>

这是我的 `course-video-manager` 仓库中的一个 [`CONTEXT.md`](https://github.com/mattpocock/course-video-manager/blob/076a5a7a182db0fe1e62971dd7a68bcadf010f1c/CONTEXT.md) 示例。哪个更容易读？

- **之前**："当课程中某个小节里的课时被'实体化'（即在文件系统中获得一个位置）时出了问题"
- **之后**："materialization cascade 出了问题"

这种简洁性在每次会话中都会产生复利。

</details>

这内置于 [`/grill-with-docs`](./skills/engineering/grill-with-docs/SKILL.md) 中。它是一个烤问会话，但帮助你与 AI 建立共享语言，并在 ADR 中记录难以解释的决策。

很难解释这有多强大。它可能是这个仓库里最酷的技术。试试看。

> [!TIP]
> 共享语言除了减少啰嗦之外还有很多好处：
>
> - **变量、函数和文件命名一致**，使用共享语言
> - 因此，**代码库更易于 agent 导航**
> - Agent 也**在思考上花费更少的 token**，因为它可以使用更简洁的语言

### #3：代码不工作

> "始终迈出小而刻意的步伐。反馈速率就是你的速度上限。永远不要承担太大的任务。"
>
> David Thomas & Andrew Hunt，[The Pragmatic Programmer](https://www.amazon.co.uk/Pragmatic-Programmer-Anniversary-Journey-Mastery/dp/B0833F1T3V)

**问题**：假设你和 agent 在构建什么上已经对齐了，当 agent _仍然_ 产出垃圾时怎么办？

这时候需要审视你的反馈循环。没有关于代码实际运行情况的反馈，agent 就是在盲飞。

**解决方案**：你需要常规的反馈循环组合：静态类型、浏览器访问和自动化测试。

对于自动化测试，red-green-refactor（红-绿-重构）循环至关重要。Agent 先写一个失败的测试，然后修复它。这为 agent 提供了一致的反馈水平，从而产出更好的代码。

我构建了一个 **[`/tdd`](./skills/engineering/tdd/SKILL.md) skill**，可以插入任何项目。它鼓励 red-green-refactor，并为 agent 提供大量关于什么构成好测试和坏测试的指导。

对于调试，我还构建了一个 **[`/diagnose`](./skills/engineering/diagnose/SKILL.md)** skill，将最佳调试实践包装成一个简单的循环。

### #4：我们建了一个泥球

> "每天都要投资于系统的设计。"
>
> Kent Beck，[Extreme Programming Explained](https://www.amazon.co.uk/Extreme-Programming-Explained-Embrace-Change/dp/0321278658)

> "最好的模块是深的。它们允许通过简单的接口访问大量功能。"
>
> John Ousterhout，[A Philosophy Of Software Design](https://www.amazon.co.uk/Philosophy-Software-Design-2nd/dp/173210221X)

**问题**：大多数用 agent 构建的应用都复杂且难以修改。因为 agent 可以极大地加速编码，它们也加速了软件熵。代码库以前所未有的速度变得更复杂。

**解决方案**是一种 AI 驱动开发的新方法：关注代码的设计。

这已内置于这些 skill 的每一层：

- [`/to-prd`](./skills/engineering/to-prd/SKILL.md) 在创建 PRD 之前会追问你要触及哪些模块
- [`/zoom-out`](./skills/engineering/zoom-out/SKILL.md) 让 agent 在整个系统的上下文中解释代码

关键是，[`/improve-codebase-architecture`](./skills/engineering/improve-codebase-architecture/SKILL.md) 帮助你拯救已变成泥球的代码库。我建议每隔几天在你的代码库上运行一次。

### 总结

软件工程基础比以往任何时候都重要。这些 skill 是我将这些基础浓缩为可重复实践的最佳努力，帮助你交付职业生涯中最好的应用。享受它。

## 参考

### Engineering

我日常编码工作使用的 skill。

- **[diagnose](./skills/engineering/diagnose/SKILL.md)** — 针对疑难 bug 和性能回归的严谨诊断循环：复现 → 最小化 → 假设 → 插桩 → 修复 → 回归测试。
- **[grill-with-docs](./skills/engineering/grill-with-docs/SKILL.md)** — 烤问会话，针对现有领域模型挑战你的计划，锐化术语，并内联更新 `CONTEXT.md` 和 ADR。
- **[triage](./skills/engineering/triage/SKILL.md)** — 通过分流角色的状态机对 issue 进行分流。
- **[improve-codebase-architecture](./skills/engineering/improve-codebase-architecture/SKILL.md)** — 在代码库中寻找深化机会，参考 `CONTEXT.md` 中的领域语言和 `docs/adr/` 中的决策。
- **[setup-matt-pocock-skills](./skills/engineering/setup-matt-pocock-skills/SKILL.md)** — 搭建按仓库配置（issue tracker、分流标签词汇表、领域文档布局），其他 engineering skill 会消费这些配置。在使用 `to-issues`、`to-prd`、`triage`、`diagnose`、`tdd`、`improve-codebase-architecture` 或 `zoom-out` 之前，每个仓库运行一次。
- **[tdd](./skills/engineering/tdd/SKILL.md)** — 使用 red-green-refactor 循环的测试驱动开发。每次一个垂直切片地构建功能或修复 bug。
- **[to-issues](./skills/engineering/to-issues/SKILL.md)** — 使用垂直切片将任何计划、规格或 PRD 拆分为可独立领取的 GitHub issue。
- **[to-prd](./skills/engineering/to-prd/SKILL.md)** — 将当前对话上下文转化为 PRD 并作为 GitHub issue 提交。无需面谈——直接综合你已经讨论过的内容。
- **[zoom-out](./skills/engineering/zoom-out/SKILL.md)** — 让 agent 拉远视角，为不熟悉的代码段提供更广阔的上下文或更高层次的视角。
- **[prototype](./skills/engineering/prototype/SKILL.md)** — 构建一次性原型来验证设计方案——要么是一个可运行的终端应用（用于状态/业务逻辑问题），要么是几个可从同一路由切换的截然不同的 UI 变体。

### Productivity

通用工作流工具，非编码专用。

- **[caveman](./skills/productivity/caveman/SKILL.md)** — 超压缩通信模式。砍掉填充词，保留完整技术准确性，token 用量减少约 75%。
- **[grill-me](./skills/productivity/grill-me/SKILL.md)** — 就某个计划或设计接受无情的追问，直到决策树的每个分支都被解决。
- **[handoff](./skills/productivity/handoff/SKILL.md)** — 将当前对话压缩为交接文档，以便另一个 agent 继续工作。
- **[write-a-skill](./skills/productivity/write-a-skill/SKILL.md)** — 创建具有正确结构、渐进式展示和捆绑资源的新 skill。

### Misc

留着但很少用的工具。

- **[git-guardrails-claude-code](./skills/misc/git-guardrails-claude-code/SKILL.md)** — 设置 Claude Code 钩子，在执行前阻止危险的 git 命令（push、reset --hard、clean 等）。
- **[migrate-to-shoehorn](./skills/misc/migrate-to-shoehorn/SKILL.md)** — 将测试文件从 `as` 类型断言迁移到 @total-typescript/shoehorn。
- **[scaffold-exercises](./skills/misc/scaffold-exercises/SKILL.md)** — 创建包含章节、题目、解答和解释的练习目录结构。
- **[setup-pre-commit](./skills/misc/setup-pre-commit/SKILL.md)** — 设置 Husky pre-commit 钩子，配合 lint-staged、Prettier、类型检查和测试。
