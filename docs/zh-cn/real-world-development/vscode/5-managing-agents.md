---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主工作，包括探索代码库、提出更改建议、编辑文件和运行命令。由于 Copilot 会在你的计算机上实时工作，VS Code 中的 Copilot Chat 会显示每项工具调用和文件编辑过程。你可以在 Chat 中查看每项差异、接受或拒绝单个更改，并通过后续提示词引导对话来完善或扩展工作，全程无需离开编辑器。

本练习将学习如何：

- 实时监控无障碍智能体的工作；
- 审查并接受建议的文件编辑；
- 在会话进行中引导智能体，除了高对比度模式之外再添加浅色模式切换开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队又提出了一项需求：他们也希望为觉得默认主题太暗的用户添加浅色模式切换开关。不必从头开始新对话，可以引导现有会话扩展工作。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每一步，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以内联差异的形式显示在 Chat 中，并提供 **Keep** 和 **Undo** 控制选项。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，滚动 Chat 查看每项工具调用。你会看到文件读取、文件编辑，以及它请求运行的终端命令。
3. 智能体提出文件编辑时，差异会以内联方式显示：
    - 选择 **Keep** 接受更改；
    - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开 Chat 顶部的 **Files changed** 指示器，查看此会话中智能体修改的所有文件。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式的实现。完成后应会显示总结所做更改的消息。

## 引导智能体添加浅色模式

高对比度模式已就绪，现在扩展同一对话，添加浅色模式切换开关。由于继续使用同一 Chat 会话，智能体会保留先前工作中的所有上下文，包括修改的组件、使用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示词：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续之前的工作，探索已做的更改并提出浅色模式的新增内容。
3. 新编辑出现时，像处理高对比度模式一样进行审查。对每项差异选择 **Keep** 或 **Undo**。
4. 如果智能体的实现方向不符合预期，例如尝试以你不喜欢的方式重构切换 UI，可以选择 Chat 输入框旁边的 **Stop** 按钮停止任务，然后发送说明后续方向的提示词。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以使用后续提示词要求调整（“简化 CSS，改用 CSS 变量，避免重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式集成到现有设置面板中”），或扩展范围（“为两个切换开关都添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看已更改文件的完整列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开部分已更改文件并查看差异。确认其中体现了自定义智能体要求的无障碍模式，包括 ARIA 属性、键盘导航、语义 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结与后续步骤

你已使用 Copilot Chat 智能体模式监控并引导正在进行的会话：查看每项文件编辑、接受或拒绝更改，并通过后续提示词扩展工作，同时保留上下文。

本课介绍了以下概念：

- 实时监控智能体的工作；
- 接受或拒绝单个文件编辑；
- 使用后续提示词引导正在进行的会话。

在[下一课][next-lesson]中，你将通过拉取请求发布无障碍功能，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
