---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务。这可能是尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体能够自主完成更新代码、添加功能等任务。任务完成后，它会创建草稿 PR，供开发人员审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发人员专注于更重要的工作。

本练习将学习如何使用 Copilot 云智能体：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 分配议题。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，实际上，代码完全缺少文档。首先，团队希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每个游戏详情页都显示相关游戏，即同一类别中的其他游戏，让用户可以继续浏览。目前不要求实现经过完善的功能，只需能用于 UX 验收测试即可。这项工作目前受阻，不过还有其他优先级更高的议题。

这两项工作都可能很快被搁置，因此很适合分配给 Copilot 云智能体。Copilot 云智能体可以异步处理任务，让开发人员继续其他工作，之后再回来审查 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发人员一样在后台执行任务。就像与人类开发人员协作一样，你可以通过多种方式分配任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。你可以在 Copilot 会话进行中或结束后查看会话。解决方案准备好供审查时，它会在拉取请求中提及你。

## 明确指令范围的重要性

GitHub Copilot 并非万能，不能替代思考。即使看似简单的操作，深入分析后也可能很复杂，因此[为 Copilot 云智能体设定任务范围时要谨慎][cloud-agent-best-practices]。应将它视为 AI 结对程序员：分阶段工作、学习、试验，并根据结果调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果已完成自定义指令的实践练习，就已经见过 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看一下。将工作分配给 Copilot 云智能体之前，先以只读方式查看存储库中已有的指令文件，稍后便能观察它们的效果。

在 GitHub Web 界面中打开存储库里的以下文件；如果已启动 Codespace，也可以在那里打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 章节，尤其留意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它会将这些指令限定在 `**/*.test.ts` 文件中。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求是否采用了 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自上述指令文件。稍后的练习中还会再次介绍它们。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保项目状态正常。将任务分配给 Copilot 时也一样，因为它会以类似软件工程师的方式处理任务。

云智能体使用 [GitHub Actions][github-actions] 执行任务时所需的环境。你可以通过 `.github/workflows/copilot-setup-steps.yml` 文件配置[专用设置工作流][setup-workflow]，让它在开始工作前运行，以便访问所需的开发工具和依赖项。此工作流已预先配置，可确保实验流程顺畅并帮助你学习。它会为 Copilot 安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单体 Astro 应用程序经过迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在存储库的“Actions”选项卡中手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限限制为*这些步骤*所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流文件相同，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 留意 `workflow_dispatch` 触发器，它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这有助于测试工作流是否成功运行，而不必等待 Copilot 启动它。

## 添加文档

大家都知道文档的重要性，但大多数项目要么文档过时，要么完全没有文档。这类技术债务常常得不到处理，降低了工作效率，也增加了维护代码库或让新开发人员加入团队的难度。Copilot 擅长创建文档，因此很适合让 Copilot 云智能体处理此议题。它会在后台生成所需的文档，之后的练习会审查其成果。

1. 在新的浏览器标签页中，转到 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建空白议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

    ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，其标题类似于 `[WIP]: Code lacks documentation`。如果列表中没有出现新的 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看列表和 Copilot 准备完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到状态指示器更新。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并确定合适的计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来会利用这一点：先再分配一个任务给 Copilot 云智能体，然后开始编写代码，为应用程序添加功能。

## 在游戏详情页中添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以分配不同的工作：你专注于一组任务，同时让它处理其他任务。即使为设计团队添加相关游戏部分不会花太多时间，这些时间也可以用于其他工作。现在就将此任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，议题的第一条评论中会显示 👀，表示 Copilot 已开始处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理第二个请求。Copilot 云智能体的工作方式类似软件工程师，因此你不需要持续监控，而是可以等任务完成后再审查结果。现在来了解如何创建和使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步执行任务。你可以用 Copilot 处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本课介绍了以下概念：

- 自定义代码生成环境；
- 确保操作安全；
- 编写范围明确的议题；
- 分配议题。

云智能体正在后台处理任务，接下来可以创建并使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用之前课程中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
