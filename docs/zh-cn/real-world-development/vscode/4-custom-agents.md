---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可重复使用的智能体，例如执行 [WCAG][wcag] 合规要求的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在 `~/.copilot/agents` 中进行全局定义。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description` 字段，后面是用于定义智能体行为、专长和指令的 Markdown 提示词。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，并告知 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是执行工作的角色，**技能**是工具。

自定义智能体拥有自己的上下文窗口，其设计目标是协调技能（甚至其他智能体）来完成工作。在本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；执行任务时，它可以调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一的“正确”方式。与 AI 的其他用法一样，需要通过测试和迭代，找到适合当前环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，部分用户觉得网站当前的暗色主题难以阅读，因为文本与背景颜色的对比度不足。为解决这一无障碍问题，设计团队要求实现可由用户启用或关闭的高对比度模式。

无障碍访问至关重要，因此希望尽快实现此功能。接下来将使用自定义智能体生成相关功能。

本练习将学习如何：

- 查看现有的无障碍自定义智能体；
- 在 Copilot Chat 中使用无障碍智能体实现高对比度模式。

## 查看无障碍自定义智能体

系统已为你创建了一个无障碍自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 Codespace，打开终端，并从 `main` 分支创建新的分支来实现无障碍功能（将练习 3 中的筛选功能 PR 单独保留）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 接着，浏览后面的章节，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责；
   - 无障碍最佳实践；
   - HTML、CSS 和 JavaScript 代码示例；
   - 常见问题和错误列表。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉列表中显示 `.github/agents` 中定义的所有自定义智能体。你可以选择自定义智能体，让 Chat 会话遵循该智能体的指令和工具配置。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习所用的模式和模型可能不同，相关步骤会在需要时说明。

1. 如果 Chat 视图的智能体下拉列表中尚未选中 **Agent**，请选择它。

   ![Chat 视图中的智能体选择器示例](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的智能体下拉列表（其中显示当前智能体，默认为 **default**）。
3. 从可用智能体列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示词：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改建议并将编辑应用到项目文件。每项编辑都会以内联方式显示在 Chat 中，同时包含文件路径和可供审查的差异。
6. 随着编辑内容应用到项目中，**Files changed** 指示器会更新，让你查看智能体修改的工作集。

> [!NOTE]
> 这可能需要几分钟。Copilot 会实际修改存储库，包括更新 Astro 组件、CSS 和相关测试等现有文件。

下一练习将审查并引导这项正在进行的工作。

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，它们是针对特定任务和领域定制的 AI 助手。使用自定义智能体可以将团队的专业知识和标准整理成可重复使用的智能体，更有效地指导 Copilot 完成工作。

本课介绍了以下概念：

- 如何定义自定义智能体；
- 如何在 Copilot Chat 智能体模式中使用自定义智能体。

接下来，[监控并引导智能体的工作][next-lesson]，在更改发生时进行审查，并在同一个会话中添加浅色模式切换开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [awesome-copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
