---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供具体上下文，其中一种重要方式是使用[指令文件][instruction-files]，指导 Copilot 如何生成代码。

本练习将完成以下任务：

- 探索项目专属上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 使用当前指令生成筛选功能的第一个数据层切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何遵循新标准。
- 将指令更新和筛选功能切片提交到 `main`，供 Copilot 在下一练习中使用更新后的指南。

> [!CAUTION]
> 生成的代码可能不符合部分标准。Copilot 的输出具有不确定性。本练习旨在观察更新指令后行为的整体变化趋势，而不是要求输出逐字相同。

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

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的单个指令文件。内容应是大多数聊天或 CLI 请求都相关的项目级上下文，例如技术栈、项目概述、最佳实践和全局指南。
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

花些时间阅读此存储库提供的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组面向不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在 **Code standards** 中，留意嵌套的 **GitHub Actions Workflows** 指南。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹，查看其中关于 Astro 文件、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；其中的 glob 相对于存储库根目录，决定指令适用于哪些文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用此指令。
5. 留意此项目中有关创建单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。查看指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小且可复用的文件，并提供可供 Copilot 参考的示例。（其中的路径相对于指令文件，而非存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求为代码添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

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
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始干净的新对话。不同练习所用模式和模型可能不同；相关步骤会在需要时说明。

先使用当前指令文件发送提示，观察自定义指令的效果。之后更新指令，再次发送相同提示以比较结果。

1. 关闭之前练习中打开的编辑器选项卡，避免 Copilot 获取不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更改，通常会同时修改辅助函数文件和测试。
6. 留意建议的辅助函数是一个带类型的函数：第一个参数为 `db` 客户端，返回值为带类型的发行商数组。这些做法来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有不确定性，因此即使未明确要求，它也可能添加文档注释。若出现这种情况也没关系，本练习的重点仍是观察添加指令后行为是否更加一致。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，改进 Copilot 的代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约位于第 27 行。留意其中记录的项目编码标准；目前没有代码文档规则，因此 Copilot 生成的辅助函数不含文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件可以放在存储库级 `.github/copilot-instructions.md` 中提供全局指南，也可以放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。你刚添加的注释规则属于项目级标准，因此存储库级文件是合适的位置。

## 再次运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除对话内容。
2. 再次选中 `src/lib/publishers.ts`，让 Copilot 聚焦于正确的文件。
3. 发送与之前**完全相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意建议的文件现在以类似以下的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供用于从数据库检索发行商信息的函数。
    */
   ```

5. 留意建议的函数现在包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * 返回所有发行商的 ID 和名称列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用于真实代码。下一练习会继续使用这段代码。

## 提交并推送指令分支

指令文件与存储库中的其他资源一样，也需要通过源代码管理。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他辅助更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并将其关联到待办事项中的文档标准议题。也可以手动操作，不过 Copilot 可以使用已连接到项目的 GitHub 工具代为完成。让 Copilot 查找议题、创建并关闭该议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一个由 Copilot 代表你在 GitHub 上操作的练习。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用这些工具时，VS Code 可能会提示你登录 GitHub。请按照提示操作。之后的练习会介绍这种连接的工作方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在智能体选择器中选中 **Agent**。
3. 要求 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 如果系统提示允许 Copilot 代表你执行 GitHub 操作，请审查命令，并酌情选择 **Allow**。
6. PR 创建后，要求 Copilot 合并 PR 并将本地分支切换回 main：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统提示允许 Copilot 执行 GitHub 操作和 shell 命令，请审查命令，并酌情选择 **Allow**。

你已借助 GitHub Copilot 创建并合并拉取请求！

## 总结与后续步骤

你探索了 Copilot 如何从项目指令文件获取上下文，并在 VS Code 中使用 Copilot Chat 完成以下任务：

- 发送代码生成提示，观察 Copilot 如何根据**现有**指令生成代码。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察建议的代码如何遵循新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习继续使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，让 Copilot 在整个代码库中添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
