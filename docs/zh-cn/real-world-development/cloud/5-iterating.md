---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 一起完成了多项任务，以改进用户体验并添加功能。你让 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。接下来，我们来查看代码更改；必要时，可以向 Copilot 提供反馈以改进其工作。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循正常的 DevOps 流程。因此，在为团队启用代码审查之前，先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能生成的建议。

## 安全性与 GitHub Copilot 云智能体

由于 Copilot 云智能体异步且无需监督地执行任务，因此设有一些安全限制，以确保操作安全。这些限制包括：

- Copilot 只能读取存储库，并且**只能**写入用于处理代码的分支。
- 云智能体在 GitHub Actions 中运行，并在其中创建独立的临时环境进行工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

首先，查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。此操作会使用 GitHub.com 上的标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。你可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似于 **Add missing documentation** 的拉取请求，也可能是其他更具体的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。此时请等待 Copilot 完成任务。这可能需要几分钟，可以稍作休息或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新更新的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求的检查区域中排队。如果一切正常，单体 Astro 应用程序的项目检查就会通过。这可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作处理拉取请求并非单向沟通。你可以像对待团队的其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动新的会话来处理请求。由于结果并非确定，因此无法提供固定的请求文本。可以让 Copilot：

- 在每个代码文件开头添加简短的注释头，说明文件的用途；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建 README，介绍 Astro 应用程序的结构。

1. 通过添加评论请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以使用上面的任一示例，也可以提出其他希望 Copilot 为代码库补充文档的要求。
2. 选择 **View Session**，观察 Copilot 执行任务。留意 Copilot 如何启动新会话来进行更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会显示一项新提交。
5. 选择 **Files changed** 选项卡，审查更改。

如果还不满意，可以继续迭代。满意后，可以将 PR 从草稿转换为准备审查状态，然后合并到主分支。

![将 PR 转为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Show related games on the game details page** 的 PR，也可能是其他更具体的标题。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流提示。
7. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 你应该会看到工作流在拉取请求的检查区域中排队。如果一切正常，单体 Astro 应用程序的项目检查就会通过。这可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到此分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！和之前一样，可以迭代改进 Copilot 云智能体的工作。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加注释头和 TSDoc 文档注释（请记住，分配此任务时，你**尚未**更新自定义指令）。与之前一样，可以在 **Conversation** 选项卡中添加评论来提出更改，Copilot 会看到评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的无障碍功能。此 PR 应该同时包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似于 **Add high contrast mode to website** 的 PR，也可能是其他更具体的标题。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求标题中会包含 **[WIP]** 标记。此时请等待 Copilot 完成任务。这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换模式的 UI 组件；
   - 用于保存用户偏好的本地存储；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 你应该会看到工作流在拉取请求的检查区域中排队。如果一切正常，单体 Astro 应用程序的项目检查就会通过。这可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到此分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用程序，在浏览器中测试高对比度和浅色模式切换开关，确认功能正常，并且在重新加载页面后仍会保留设置。

留意无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍方面的问题或改进点，可以像处理之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派工作

云智能体最适合处理真实的待办事项，让你能同时专注于其他工作。若要培养这种工作方式，可以为存储库创建一些议题并分配给 Copilot。以下是一些建议：

- 在游戏详情页中创建支持者意向表单；
- 为游戏列表页添加分页；
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。在这些课程中，你：

- **检查了此存储库附带的自定义指令文件**，以便之后观察它们对云智能体输出的影响；
- **将议题分配给 Copilot 云智能体**，并观察它如何设置环境、制定计划和异步执行任务；
- **创建并使用自定义智能体**处理无障碍问题，添加高对比度和浅色模式切换开关；
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话；
- **审查并迭代改进云智能体创建的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

你已完成云智能体学习路径。如果想继续探索，其他学习路径可以补充此处学到的内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，可以尝试以下后续操作：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
