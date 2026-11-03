---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能的实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一个分支中。现在需要一起审查这些更改并创建拉取请求。你已经自行合并了星级评分拉取请求 (PR)，这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常，功能、指令更新、技能更新和 QA 智能体会分别放在几个 PR 中。为了简化研讨会流程，筛选和质量工作流都保留在同一个对话和分支中，并通过此 PR 一并交付。

本课将学习如何：

- 了解 Agent Merge 及其如何自动化 PR 合并生命周期；
- 检查完整的功能更改和验证证据；
- 创建筛选功能 PR；
- 审查后再启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动处理剩余的 PR 工作，同时让开发人员掌握合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题，例如修复持续集成 (CI) 检查失败、响应审查评论，以及在需要时进行变基；之后，如果存储库允许，它会启用 GitHub 自动合并。

之前都是由你选择 **Merge pull request**。Agent Merge 可以代为处理这一步。启用 Agent Merge 前，请先审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码已全部创建完成，现在一起审查代码、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它将合并 PR。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限要求。如果遇到阻碍，请阅读报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结与后续步骤

你已自动化开发流程的多个部分，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动化合并生命周期；
- 检查了完整的功能更改和验证证据；
- 创建了筛选功能 PR；
- 只在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多有用的 Copilot CLI 斜杠命令][next-lesson]，以管理上下文、模型、共享和可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
