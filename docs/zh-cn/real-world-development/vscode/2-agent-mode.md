---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使对应用程序进行简单更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发人员通常需要找到所有相关文件、进行更改、运行测试、调试、发现遗漏的文件，再次更新……工作还不止这些。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可在 IDE 中更自主地工作。它的工作方式类似开发人员：先探索现有项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。接下来看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于两种不同的体验。智能体模式在 IDE 中执行任务，可快速反馈并随时交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改；
- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- 在将代码合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增加，因此希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改任何内容之前，先探索 Tailspin Toys 网站，了解其当前功能。

该网站是一个以开发者为主题的棋盘游戏众筹平台。用户可以浏览游戏列表并查看详情。网站是一个单体 Astro 应用程序，在构建时将页面呈现为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能则使用少量作用域受限的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单体 Astro 开发服务器。按照以下步骤，可以在 GitHub Codespace 中运行该任务：

1. 返回 Codespace。上一练习结束后，当前应位于 `main` 分支。
2. 在 Codespace 中按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新终端。
3. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令以启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示包含 URL 的横幅，类似以下内容：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要为 `http://localhost:4321` 打开浏览器窗口，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中，从终端选择 localhost URL 后，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的专用通道，此时 Web 服务器正在 Codespace 中运行。

### 探索网站

网站启动后，可以浏览其功能。网站主要包含以下功能：

- **主页**：显示棋盘游戏列表，包括标题、图像和描述。
- **游戏详情页**：选择游戏后，会打开详情页，显示标题、描述、发行商和类别等详细信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习所用的模式和模型可能不同，相关步骤会在需要时说明。

网站的初始实现已经可用，但我们希望添加更多功能。先查看待办事项。在从模板创建项目时，系统已自动创建议题待办事项。让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令并调用工具。

   ![Chat 视图中的智能体选择器示例](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时，我们使用了 Claude Sonnet 4.5，因此也在说明中列出了该模型。这样做是希望代码建议相对一致，从而带来更好的体验。不过，LLM 具有概率性，因此生成的建议可能与研讨会中的示例有所不同。这完全正常，也是预期情况。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，询问议题待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

请注意，Copilot 还根据对用户的帮助程度为项目排定了优先级。

## 查看指令文件

让智能体生成代码之前，先查看用于向 Copilot 提供工作上下文的指令文件。项目中的[用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)包含了如何为网站添加功能的上下文。

1. 在 Codespace 中转到 `.github/instructions/ui.instructions.md`。
2. 留意其中关于添加功能的整体指导，包括：
   - 架构概述；
   - 组件设计、可测试性和无障碍访问原则；
   - 指向多种文件类型对应指令文件的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。这些路径相对于指令文件所在位置。这样可以重复使用内容、将复杂指导拆分成更易管理的部分，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要更新应用程序的三个部分：

- 在数据层 (`src/lib/`) 中添加或完善剩余筛选逻辑；
- 添加或更新筛选行为的测试；
- 更新游戏列表页，添加筛选 UI。

此外，合并前需要运行并通过测试。Copilot 智能体模式可以代为完成这些任务，现在来添加该功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍然选中 **Agent**。

   ![Chat 视图中的智能体选择器示例](../../../_images/shared-chat-mode-selector.png)

4. 确认仍然选择 **Claude Sonnet 4.5** 作为模型。
5. 使用以下提示词，让 Copilot 根据待办事项中相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件。你会看到它查找数据层辅助函数、UI 和测试，随后开始修改文件并运行测试。

   ![Copilot 正在探索项目文件](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo** 按钮，因此无需在工作过程中选择它们。

7. 按 Copilot 的提示，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中的对话框，询问用户是否允许运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现暂停，甚至某些测试可能失败。这没有关系。Copilot 会在生成代码和运行测试之间反复迭代，直至任务完成且未发现错误。

   ![已完成的 Copilot 智能体模式 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 请记住，始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站浏览器，试用新功能。
11. 确认所有内容都能正常工作并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你已了解如何：

- 使用 GitHub Copilot 智能体模式跨 UI 和数据层实现新功能；
- 使用 Copilot 智能体模式探索项目、识别相关文件并协调更改；
- 在将代码合并到代码库之前，审查 Copilot 智能体模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]并为其创建拉取请求。

### 加分探索练习 — 实现分页

随着游戏列表不断增加，需要启用分页。使用本练习所学知识，让 Copilot 更新网站以实现分页。编写代码时可考虑：

- 遵循现有最佳实践，包括使用已有的指令文件；
- 考虑如何实现分页，例如允许用户选择每页大小，还是使用固定值；
- 编写提示词时，为 Copilot 提供所需指导，以便按你的期望实现功能；
- 可能需要与 GitHub Copilot 迭代交互，要求修改并提供上下文。这是使用 Copilot 的正常工作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
