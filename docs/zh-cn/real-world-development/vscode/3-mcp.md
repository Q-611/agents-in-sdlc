---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求之前，应先在浏览器中确认功能确实正常。你不必亲自操作应用程序，可以连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器并测试功能；然后发布分支并创建 PR。

本练习将学习如何：

- 了解 Model Context Protocol (MCP) 及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到工作区；
- 让 Copilot 使用 Playwright MCP 服务器在浏览器中手动测试筛选功能；
- 发布分支，并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式能够访问编辑器之外的资源时会更加强大。Copilot 使用 Model Context Protocol (MCP) 与外部工具和服务通信，这是智能体与外部工具和服务通信的标准方式。

![智能体模式的工作原理示意图，显示它如何与上下文、LLM 和工具交互，其中包括由 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 让 AI 智能体能够与外部工具和服务通信。使用 MCP 后，AI 智能体可以实时获取最新信息（通过资源）并代表你执行操作（通过工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理智能体与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。每个 MCP 服务器都代表 AI 智能体可访问的一组工具和资源。

以下是两个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API。AI 智能体可以使用这些 API 创建新存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能。AI 智能体可以使用它访问网页、填写表单和选择按钮。

还有许多其他 MCP 服务器，可以提供不同的工具和资源。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，方便用户发现和参与此生态系统。

> [!CAUTION]
> 从安全角度来看，请像对待项目中的其他依赖项一样谨慎对待 MCP 服务器。使用前应仔细审查其源代码、验证发布者，并考虑相关安全影响。只使用信任的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你应该会看到已经配置好的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中包含此 `github` 条目，因此 Copilot 才能在上一练习中读取议题待办事项。此配置使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装任何内容。VS Code 会通过 HTTP 连接到服务器，Copilot 首次使用其中一个工具时，你需要通过 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二台服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的浏览器，正好适合用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列。文件应如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动服务器。`--headless` 标志会让 Playwright 在后台运行浏览器，不显示可视窗口。Codespace 中没有桌面环境，因此必须启用此标志。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 运行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要使用服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择此操作，立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选步骤。如果跳过，VS Code 会在下一步中 Copilot 首次需要使用相关工具时启动服务器（并提示你信任它）。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供真实浏览器。不必亲自操作应用程序检查工作，智能体可以打开页面、导航、应用筛选并读取结果，然后向你总结观察结果。这是无需离开对话即可快速确认功能是否符合预期的方法。

Playwright MCP 服务器在底层使用网页的[无障碍树][playwright-mcp-server]，而不是截图。因此，智能体会像辅助技术一样根据带标签的结构化元素（按钮、链接、列表项）进行判断。这样，快速功能检查也能顺便验证一些基本的无障碍要求。

连接服务器并启动应用程序后，让 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，执行每一步，然后报告观察结果。根据议题中的验收标准审阅摘要。如果发现问题，可以询问后续问题，或让 Copilot 在创建拉取请求前修复代码。

> [!NOTE]
> 此测试要求应用程序在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示词前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查了。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标，暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![Source Control 面板中显示已更改内容的屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过多种方式创建拉取请求，例如使用 github.com 或 GitHub 命令行界面 (CLI)。不过，你正在使用 GitHub Copilot，因此可以让它代为创建 PR。它可以找到相关议题并创建与议题关联的 PR。

1. 转到 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认智能体下拉列表中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行获取信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示你通过 GitHub 身份验证；按照提示完成身份验证。
5. 留意 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结与后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 及 MCP 服务器如何为 Copilot 扩展工具；
- 将 Playwright MCP 服务器添加到工作区；
- 使用该服务器在浏览器中手动测试筛选功能，然后再交付；
- 发布分支并为筛选功能创建了拉取请求。

你已使用 MCP 服务器**测试**功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
