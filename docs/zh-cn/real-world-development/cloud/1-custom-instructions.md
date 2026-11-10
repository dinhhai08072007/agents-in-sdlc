---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。[指令文件][instruction-files]用于向 Copilot 提供指导，让它了解你想让它做什么以及如何完成。

本练习将完成以下任务：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，这里不会运行更新指令前后的提示进行比较。Copilot 云智能体会在 GitHub 上异步处理议题，因此难以实时并排展示指令的影响。稍后审查云智能体生成的拉取请求时，可以看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应使用深色模式并具有现代风格。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加注释块，说明文件的用途。

通过使用指令文件，可以确保 Copilot 了解并遵循这些实践。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。你可以指定编码规范、库，甚至希望添加的注释类型。指令可以适用于整个存储库，也可以针对特定文件类型提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。内容应是大多数 Copilot 聊天或 CLI 请求都相关的项目级上下文，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可针对具体任务或文件类型创建。例如，为 TypeScript 或 Astro 等语言提供指南，或说明如何创建 UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本研讨会不深入讲解创建指令文件的全过程，但示例项目展示了一种具有代表性的做法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指南，例如项目概述、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体指令。
- 使用自然语言，保持指南清晰，并提供代码应有和不应有的示例。

创建指令文件没有唯一方法，使用 AI 也是如此。通过试验找出最适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一套完善的指令文件。查看本项目中的指令文件时，你会发现其中包括[用户界面更新][ui-instructions]和 [Astro][astro-instructions] 等多种任务。
>
> Copilot 也可以帮助生成指令文件。不同界面的操作方式各有不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适用时介绍具体方式。
>
> 需要模板或起点？请查看 [awesome-copilot][awesome-copilot]，其中汇集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索此项目中的自定义指令文件

花些时间阅读此存储库提供的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组面向不同任务的 `*.instructions.md` 文件。可在 GitHub Web 界面或已运行的 Codespace 中打开这些文件。

- 打开 `.github/copilot-instructions.md`，查看 **Code standards** 部分，特别留意 TypeScript 约定和 TSDoc 文档注释要求。
- 打开 `.github/instructions/unit-tests.instructions.md`，留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件中。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意生成的拉取请求是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述指令文件。之后的练习审查 PR 时还会再次说明。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求为代码添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库标准

接下来只需编辑一处：添加项目级规则，要求通过 TSDoc 文档注释和文件级注释头将文档写入代码。之后本学习路径中云智能体处理议题时会使用这些规则。

先创建用于此项工作的分支（云智能体会读取议题目标分支中的指令，但仍要以规范方式提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约位于第 27 行。留意其中记录的项目编码标准；目前没有代码文档规则，因此生成的辅助函数不含文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件可以放在存储库级 `.github/copilot-instructions.md` 中提供全局指南，也可以放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。你刚添加的注释规则属于项目级标准，因此存储库级文件是合适的位置。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支，因此指令更改必须先合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。下一练习中，云智能体处理议题时就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，也可以跳过分支步骤，直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结与后续步骤

你探索了 Copilot 如何从项目指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。接下来整个学习路径中，都可以在云智能体生成的拉取请求中看到该标准的效果。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
