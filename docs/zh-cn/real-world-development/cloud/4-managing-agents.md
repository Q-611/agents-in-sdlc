---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三个任务，旨在改进用户体验和添加功能。云智能体可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。你可以在这个任务控制中心查看所有已创建开放拉取请求 (PR) 的智能体任务，查看它执行的操作，甚至可以引导正在进行的会话。

本课将学习如何：

- 浏览智能体页面，监控云智能体的任务；
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给智能体以实现高对比度模式后，团队意识到现在也是添加浅色模式的好时机。网站样式和切换功能都在更新，因此一并实现浅色模式很合理。现在需要引导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 转到 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该能看到分配给 Copilot 的任务，包括：
    - 更新代码文档；
    - 为修改游戏生成 API；
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作。这些操作可能包括：
    - 从存储库检出代码；
    - 为 Copilot 创建工作环境；
    - 设置 MCP 服务器；
    - 执行完成分配任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和处理方式而有所不同。

4. 同时留意右侧的拉取请求窗格，可在此查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

现在已经了解当前正在执行的任务，接下来请求 Copilot 在处理高对比度模式时也添加浅色模式切换开关。

1. 选择表示正在添加高对比度模式的会话。确切标题会根据 Copilot 使用的名称和当前工作状态而有所不同。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面上的云智能体任务，其中突出显示了“Steer active session while Copilot is working”对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 对提示词的确认，以及它如何将请求纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务！在收到消息后，它会先完成当前步骤，再将新请求加入工作流程。

这可能还需要几分钟。等待时可以暂停一下，回顾目前学到和探索过的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。可以在任务控制中心跟踪所有进行中和已完成的任务、审查处理过程，并重定向正在运行的任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务；
- 重定向正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
