---
title: "第 5 课 - 自定义并使用质量检查技能"
description: "探索现有的 quality-checks 技能，自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码并不是开发工作的全部。我们已经手动验证代码能够运行，也使用指令文件确保代码符合规范。接下来还需要考虑测试、Lint，以及持续集成 (CI) 中的其他环节。

**智能体技能**正适合处理这类任务！技能可以帮助 Copilot 正确运行这些操作。

本课将完成以下任务：

- 探索现有的 `quality-checks` 技能。
- 自定义结果的显示格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、Lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 指令、脚本和资源

智能体技能将可复用的任务指令、可执行脚本和辅助资源打包在一起，由智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。该文件通过 frontmatter 中的名称和描述定义技能，概述其用途，并说明何时调用。文件夹还可以包含供技能调用时使用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外的文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目中已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

接下来查看 Tailspin Toys 团队用于运行单元测试、Lint 和类型检查的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读文件顶部的 `name` 和 `description`。描述会帮助 Copilot 判断何时调用技能。
3. 阅读指令，留意它们如何引导 Copilot 完成测试和 Lint 流程。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 更改前运行技能

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言提出请求。先让 Copilot 运行这三项检查。

1. 返回 Interactive 模式下的筛选对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

我们希望报告能说明运行了什么、是否成功，以及工具实际报告了什么。现在更新技能，让它生成这样的报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

修改完成后，看看技能的实际效果！Copilot CLI 可以在不重启对话的情况下重新加载编辑后的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告，并与第一次的报告进行比较。

## 总结与后续步骤

你已自定义并使用现有的智能体技能。本课中，你：

- 探索了用于单元测试、Lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果的显示格式。
- 重新加载并运行了技能。

这项更改将与筛选功能一起纳入功能 PR。接下来，[使用 Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，并非额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和捆绑的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
