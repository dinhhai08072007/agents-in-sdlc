---
slug: zh-cn/real-world-development/cloud
title: "GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)**可在云端异步执行工作。你在 GitHub 上分配任务后，云智能体会在后台处理，包括探索存储库、进行更改并创建拉取请求，让你可以继续处理其他工作。

本系列练习将添加云智能体会遵循的自定义指令，然后分配 GitHub 议题并让它实现功能。你将审查并使用自定义智能体来引导它的工作方式，在智能体页面监控和引导会话，最后审查它创建的拉取请求并迭代改进结果。

## 练习

| 练习 | 主题 | 说明 |
|----------|-------|-------------|
| [0. 先决条件][ex0] | 设置 | 创建存储库和 Codespace |
| [1. 自定义指令][ex1] | 上下文 | 添加云智能体会遵循的自定义指令 |
| [2. 云智能体][ex2] | 异步智能体 | 将议题分配给 Copilot 云智能体 |
| [3. 自定义智能体][ex3] | 专用智能体 | 查看并使用自定义智能体 |
| [4. 管理智能体][ex4] | 监控 | 监控并引导智能体会话 |
| [5. 迭代][ex5] | 审查 | 审查 PR、迭代 Copilot 的工作并了解后续步骤 |

## 先决条件

参加本研讨会前，请确保具备以下条件：

- [ ] 拥有 GitHub 账户，并已启用 **Copilot Student、Pro、Pro+、Business 或 Enterprise** 计划
- [ ] 账户或组织已启用 Copilot 云智能体

> [!TIP]
> 没有付费计划？通过验证的学生可以通过 [GitHub Education][callout-student-plan-education] 免费使用 GitHub Copilot。**Copilot Student** 计划包含本研讨会使用的智能体、MCP、代码审查和 Copilot CLI 功能，因此可以用它完成所有学习路径。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> 除 Copilot Free 外，**Copilot Student、Pro、Pro+、Business 和 Enterprise** 均提供云智能体。Copilot Business 或 Enterprise 账户需要由管理员为组织启用该功能。

> [!NOTE]
> 本学习路径不介绍 MCP。若想了解 Copilot 如何使用 MCP 服务器，请参阅 [CLI 学习路径](../cli/)或 [VS Code 学习路径](../vscode/)。

## 开始学习

**[从练习 0：先决条件开始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
