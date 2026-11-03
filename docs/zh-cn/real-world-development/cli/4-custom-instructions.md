---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看存储库指令、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，应确保 Copilot 能获取相应指导。[指令文件][instruction-files]不仅说明你想要什么代码，还说明代码应该如何组织。现在你已经构建了筛选功能，接下来将查看 Copilot 使用的指令、添加文档标准，并将其应用于代码。

本课将学习如何：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体；
- 更新指令文件，确保遵循编码标准；
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 记录用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应记录 `Props` 接口，并在相关代码发生更改时同步更新注释。
- 应保留现有格式和 lint 指南。

通过使用指令文件，可以确保 Copilot 了解这些实践要求，并据此完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，帮助它更好地理解你的编码风格和要求。你可以指定偏好的编码约定、库，以及希望在代码中使用的注释类型。可以为整个存储库创建指令，也可以为特定文件类型创建指令，以提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是一个指令文件，会随该存储库的**每个**请求发送给 Copilot。此文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md` 文件可为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目中的自定义指令文件

为了帮助你开始使用，起始项目中已包含一组指令文件。在更改之前，先浏览现有文件，了解其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目的简要说明和编码指导。这些指令适用于与此存储库中的 Copilot 进行的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。你会看到适用于 Astro 文件、Drizzle 数据层、测试等的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它设置了用于确定指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可重复使用的文件，并为 Copilot 提供生成代码时可参考的示例。

## 更新指令文件，使其符合团队指导

现有文件是不错的起点，但还有不足。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 章节，通常位于第 35 行附近。
2. 在该章节末尾添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更新后的指令后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新的指导。

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 关闭对话。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中恢复保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结与后续步骤

你已了解 Copilot CLI 如何从指令文件中获取上下文，并将新的标准应用于功能。本课中，你：

- 了解了存储库指令和按路径限定的指令文件如何传递给智能体；
- 更新了指令文件，以确保遵循编码标准；
- 查看了指令文件的影响。

接下来，[自定义并运行可重复使用的 quality-checks 技能][next-lesson]，确保始终一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件合集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
