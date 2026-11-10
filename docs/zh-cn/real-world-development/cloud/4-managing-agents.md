---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你要求 Copilot 云智能体处理了三项改进用户体验和添加功能的任务。云智能体虽能异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心中，可以查看所有拥有未关闭拉取请求 (PR) 的智能体任务、查看它们执行的操作，还可以引导正在进行的会话。

本课将完成以下任务：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以添加浅色模式。既然智能体正在更新网站样式并添加切换功能，一并实现浅色模式也很合适。现在引导智能体同时添加浅色和高对比度模式。

## 查看 Copilot 云智能体任务

先查看分配给 Copilot 云智能体的任务状态。

1. 前往 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。列表中应能看到你分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各项步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 的工作状态和执行方案而异。

4. 同时留意右侧显示的拉取请求 (PR) 面板，可通过它查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

了解当前进行中的任务后，让 Copilot 在实现高对比度模式时也添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。出现类似下方的消息时，表示智能体已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务屏幕截图，其中高亮显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！它会在完成收到消息时正在执行的步骤后，将新请求加入工作流程。

这同样需要几分钟。可以利用这段时间回顾目前学到和探索的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心。你可以跟踪所有进行中和已完成的任务、审查智能体执行的工作，也可以在会话进行中重定向任务以调整范围或提供更多指导。

本课介绍了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 即将完成无障碍功能。接下来[迭代审查 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
