---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已经为项目添加了一项小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流，确保以正确的方式构建正确的内容。本课是后续多个课程的第一课，你将遵循典型的智能体驱动开发流程：从议题着手生成新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续课程会继续使用同一对话和分支来完成该功能工作流。通常，不同文件类型的更改会放在不同分支或 PR 中，但为了专注于核心概念，本研讨会会采用简化流程。

本课将学习如何：

- 根据 GitHub 议题启动新的 Copilot 对话；
- 在 Plan 模式中确定需求；
- 使用 Autopilot 模式实现新功能；
- 审查代码；
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩展，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了此功能，但在开始编码前，仍需商定类别组合等细节。你将使用 Plan 模式确定这些决策，然后批准 Autopilot 按明确的范围实现功能。

## 背景

在开发流程中引入 AI 编码智能体并不会改变基本原则，事实上这些原则变得更重要了！大多数开发人员的流程大致如下：

1. 从已创建的议题开始，了解需要完成的工作。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码经过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述方式类似。

遵循这种标准方法，可以确保 AI 生成的代码满足需求，并经过与手动编写代码相同的审查流程。

## 对话模式

**对话模式**决定智能体可以自主执行多少工作。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你的输入后再继续。
- **Plan**：智能体先创建计划，且不能编辑项目文件。
- **Autopilot**：智能体自主工作，包括编写代码、运行测试并迭代，直至任务完成。

先使用 Plan 模式，审查计划，然后使用 Autopilot 实施计划。

## 从议题开始

开始筛选功能前，返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示词中，并返回 **Session** 选项卡。

请注意，此时提示词以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划可以在将功能或任务交给 Copilot 之前，确定实现方案。对于复杂任务，花些时间规划总是值得的。现在切换到 Plan 模式，让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示词：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划！它会先探索项目，然后确定最佳方案。

3. 在此过程中，Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一的正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 向上和向下滚动，审查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已经编写并审查，现在可以开始实施。使用 Autopilot 让 Copilot 完成工作。

Autopilot 会让 Copilot 持续迭代，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所用版本中标签类似的选项。
2. 确认提示词下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按已确定的计划进行迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能会立即开始，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，和其他代码一样，合并前需要审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 将结果与议题及规划时作出的决定进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决任何失败项。

## 试用新功能

代码看起来没问题，但能否正常运行？像之前一样启动应用程序，并通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用程序：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 显示端口 `4321` 可用时，选择 **Open in Browser**。
3. 试用类别筛选、发行商筛选，以及规划中商定的组合。
4. 确认重置和无结果时的行为符合议题和你的决定。
5. 返回 Codespace，并让 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你已使用不同的对话模式构建并审查功能。本课中，你：

- 根据 GitHub 议题启动了新的 Copilot 对话；
- 在 Plan 模式中确定了需求；
- 使用 Autopilot 模式实现新功能；
- 审查了代码；
- 在转发的浏览器中手动验证了功能。

接下来，[使用自定义指令][next-lesson]进一步了解如何生成代码，确保代码符合文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
