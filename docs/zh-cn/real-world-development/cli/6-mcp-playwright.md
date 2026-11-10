---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码只是开发工作的一部分。我们还需要处理数据、外部服务，并为 Copilot 提供更多自动化能力。这时就需要 MCP 服务器。MCP 服务器可以扩展 Copilot CLI 的内置功能，为它提供其他工具和服务。

本课将完成以下任务：

- 了解 Model Context Protocol (MCP) 是什么，以及 Copilot CLI 如何使用它。
- 如果尚未添加 Playwright MCP 服务器，则进行添加。
- 要求智能体操作浏览器并探索筛选功能。

## 场景

单元测试和端到端测试固然重要，但验证界面更新还需要实际与界面交互。你希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化更改流程，并增强对更新按预期运行的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体提供了与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁。每个 MCP 服务器都代表智能体可访问的一组工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP 服务器][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求所需的 API。
- **[Playwright MCP 服务器][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

此外还有许多其他 MCP 服务器。GitHub 提供了一个 [MCP 注册表][mcp-registry]，帮助大家发现并为生态系统做出贡献。

> [!CAUTION]
> 对待 MCP 服务器应与对待项目中的其他依赖项一样。使用前请审查其源代码、验证发布者，并考虑相关安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请将它添加到 Copilot，使其能够像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 要求 Copilot 通过 Playwright 探索功能

之前你已手动确认功能运行正常。现在使用 Copilot 中配置好的 Playwright MCP 服务器重复验证。

1. 使用以下提示，要求 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，因为它会自行判断。但既然你知道应使用哪个服务器，明确提示也无妨！这样可以提高结果的一致性，还能节省一些 token。

2. 观察 Copilot 如何列出它在浏览器中执行的步骤，以确认功能是否正常。
3. 阅读报告，确保一切符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器，然后向你提供报告。

## 总结与后续步骤

恭喜！你已使用 Playwright MCP 服务器，通过 Copilot CLI 在真实浏览器中探索功能。本课中，你：

- 了解了 Model Context Protocol (MCP) 是什么，以及 Copilot CLI 如何使用它。
- 如果尚未添加 Playwright MCP 服务器，则完成了添加。
- 要求智能体操作浏览器并探索筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
