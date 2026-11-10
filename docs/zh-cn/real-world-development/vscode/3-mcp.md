---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚刚使用智能体模式构建了筛选功能。创建拉取请求之前，应确认它在浏览器中能够正常运行。与其亲自逐一操作应用，不如连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器测试功能，然后发布分支并创建 PR。

本练习将完成以下任务：

- 了解 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何扩展 Copilot 的工具。
- 将 Playwright MCP 服务器添加到工作区。
- 要求 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支，并为筛选工作创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式能够访问编辑器之外的资源时会更加强大。Copilot 通过 Model Context Protocol (MCP) 实现这一点：MCP 是智能体与外部工具和服务通信的标准方式。

![显示智能体模式内部工作机制及其如何与上下文、LLM 和工具（包括 MCP 服务器和 VS Code 扩展提供的工具）交互的示意图](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供与外部工具和服务通信的方式。通过 MCP，AI 智能体可实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当 AI 智能体与外部工具和服务之间的桥梁，负责管理双方之间的通信，例如现有 API 或 NPM 包等本地工具。每个 MCP 服务器都代表 AI 智能体可访问的一组工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP 服务器](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，让 AI 智能体能够创建存储库、更新现有存储库，以及管理议题和拉取请求。
- **[Playwright MCP 服务器](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，让 AI 智能体能够浏览网页、填写表单和选择按钮。

此外还有许多提供不同工具和资源的 MCP 服务器。GitHub 提供了一个 [MCP 注册表](https://github.com/mcp)，帮助大家发现并为生态系统做出贡献。

> [!CAUTION]
> 在安全方面，对待 MCP 服务器应与对待项目中的其他依赖项一样。使用前请仔细审查源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，谨慎授予其访问敏感资源或执行敏感操作的权限。

## 检查 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置好的 `github` 服务器：

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

项目模板已包含此 `github` 配置，因此 Copilot 能在上一练习中读取待办议题。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 会通过 HTTP 连接，并在 Copilot 首次调用其工具时使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供可控的浏览器，正适合用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 配置，与 `github` 并列，文件内容如下：

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

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动服务器。`--headless` 标记会让 Playwright 在后台运行浏览器，不显示可见窗口。Codespace 中没有桌面环境，因此必须使用无头模式。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 运行端到端测试，因此通常已安装浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要调用服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 配置上方会出现 **Start** 操作。选择它立即启动服务器，确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请审查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动服务器，并询问你是否信任它。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可让 Copilot 操作真实浏览器。无需亲自操作应用来检查更改，智能体可以打开页面、导航、应用筛选条件并读取结果，然后向你总结观察到的情况。无需离开对话即可快速验证功能是否符合预期。

Playwright MCP 服务器在底层使用网页的[无障碍树][playwright-mcp-server]，而不是屏幕截图。这样智能体可以像辅助技术一样，根据结构化且带标签的元素（按钮、链接和列表项）进行推理，因此快速功能检查也能兼顾基本的无障碍检查。

连接服务器并启动应用后，要求 Copilot 测试刚实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐步完成检查，并报告结果。请根据议题中的验收标准审阅摘要。如果发现问题，可以继续询问，或要求 Copilot 修复代码后再创建拉取请求。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 选择 **Sparkle** 按钮生成提交消息。

   ![显示更改内容的 Source Control 面板屏幕截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 等方式创建拉取请求。不过，既然已经在使用 GitHub Copilot，不妨让它代为创建 PR！Copilot 可以查找相关议题并创建与议题关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 在智能体下拉菜单中确认选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 要求 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，系统可能会要求你进行 GitHub 身份验证，请按照提示操作。
5. 留意 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**暂时不要合并**。

## 总结与后续步骤

恭喜！本练习中，你：

- 了解了 Model Context Protocol (MCP) 是什么，以及 MCP 服务器如何扩展 Copilot 的工具。
- 将 Playwright MCP 服务器添加到了工作区。
- 使用该服务器在发布前通过浏览器手动测试筛选功能。
- 发布了分支，并为筛选工作创建了拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专用任务。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
