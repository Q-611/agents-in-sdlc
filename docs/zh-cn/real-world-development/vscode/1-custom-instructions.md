---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种主要方式是使用[指令文件][instruction-files]，指导 Copilot 如何生成代码。

本练习将学习如何：

- 通过存储库自定义指令和按路径限定的指令文件，让 Copilot 获取项目上下文、编码准则和文档标准；
- 使用当前指令生成筛选功能的第一个数据层切片（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加一条新的存储库级标准；
- 重新运行相同提示词，观察生成的代码如何遵循新标准；
- 将指令更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合所有已设置的标准，因为 Copilot 具有非确定性。本练习的重点是观察更新指令后行为的**整体变化趋势**，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 数据层必须始终配备单元测试。
- UI 应采用暗色模式，并具有现代感。
- 代码应通过 TSDoc 文档注释提供文档。
- 每个文件开头都应添加一段注释，说明该文件的用途。

通过使用指令文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以指定偏好的编码约定、库，以及希望在代码中使用的注释类型。可以为整个存储库创建指令，也可以为特定文件类型创建指令，以提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个指令文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型提供指导。例如，可以为 TypeScript 或 Astro 文件，或者创建 UI 组件或一组单元测试等任务编写指导。

> [!NOTE]
> 在 IDE 中使用 Copilot 时，指令文件只用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md 文件。

### 管理指令文件的最佳实践

在本研讨会中全面讨论如何创建指令文件并不现实。不过，示例项目展示了具有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目内容、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例说明什么做法合适、什么做法不合适。

创建指令文件没有唯一的正确方式，正如使用 AI 也没有唯一的正确方式。你可以通过实践，找到最适合自己项目的方法。

> [!TIP]
> 使用 GitHub Copilot 的每个项目都应具备完善的指令文件集合。浏览此项目的指令文件时，你可能会注意到其中包含多种任务的指导，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 还可以帮助你生成指令文件。不同产品界面的操作方式各有不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应产品的课程会在相关章节介绍这些功能。
>
> 如果需要模板或起点，可以浏览 [awesome-copilot][awesome-copilot]，其中收录了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读此存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组适用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览该文件，留意项目的简要说明，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等章节。在 **Code standards** 下方，留意嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。你会看到适用于 Astro 文件、Drizzle 数据层、测试等的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于确定指令适用的文件。在此示例中，符合 `**/*.test.ts` 的任何 TypeScript 测试文件都会匹配。
5. 留意专门用于在此项目中创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可重复使用的文件，并为 Copilot 提供生成代码时可参考的示例。这些路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 章节记录了项目的编码标准，但尚未要求添加代码内文档。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建新分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，请返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始一个干净的对话。不同练习所用的模式和模型可能不同，相关步骤会在需要时说明。

要查看自定义指令的影响，先在使用当前指令文件的情况下发送提示词。稍后更新指令文件后，再发送相同提示词，比较结果。

1. 关闭之前练习中打开的编辑器标签页，让 Copilot 只获取你需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，让 Copilot 应用文件更改。

   ![Chat 视图中的智能体选择器示例](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是一个带类型的函数，它将 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定，该文件适用于 `src/lib/*.ts`。
7. 留意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此即使未明确要求，也可能添加文档注释。如果发生这种情况也没关系，本练习的重点仍是观察更新指令后的**一致性**变化。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。补充存储库编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 章节，该章节通常位于第 27 行附近。留意其中记录的项目编码标准，但还没有规定代码内文档要求，这就是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中编写全局指令，也可以在 `*.instructions.md` 文件中编写适用于特定语言、文件类型或任务的指令。项目级标准（例如文档注释规则）适合放在存储库级文件中。

## 重新运行提示词并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空当前对话。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前**相同的提示词**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 留意建议的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有发行商 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会在此基础上继续。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，也需要使用源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更新（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并将其关联到待办事项中的文档标准议题。也可以手动操作，不过 Copilot 可以使用已连接到项目的 GitHub 工具代为完成。让 Copilot 查找议题、创建用于关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已经将 GitHub 工具连接到工作区，因此无需额外设置。Copilot 首次使用其中一个工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录。稍后的练习将介绍这种连接的工作原理（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，请先审查命令，再酌情选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并将当前分支切换回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和运行 shell 命令时，请先审查命令，再酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求。

## 总结与后续步骤

你已了解 Copilot 如何从项目指令文件中获取上下文，并在 VS Code 中使用 Copilot Chat 完成以下任务：

- 发送代码生成提示词，观察 Copilot 如何使用**现有**指令生成代码；
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准；
- 重新运行相同提示词，观察建议的代码如何遵循新标准；
- 将指令更新和辅助函数基础提交到 `main`，以便下一练习继续使用。

接下来，让 Copilot 在[智能体模式][next-lesson]中运用这些指令，跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件合集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
