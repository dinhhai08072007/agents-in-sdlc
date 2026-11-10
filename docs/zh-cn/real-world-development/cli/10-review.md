---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目，并找到预先创建的筛选议题。
- 添加了星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下构建，并在 Interactive 模式下审查。
- 使用自定义指令引导智能体，自定义现有的 `quality-checks` 技能，并用它运行单元测试、Lint 和类型检查。
- 添加了 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖范围、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的内容

本研讨会包含两个 PR 里程碑，每个里程碑都从更新后的 `main` 分支创建独立分支：

1. **星级评分：** 在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流：** 实现筛选功能、更新指令并将其应用于功能代码、自定义 `quality-checks` 报告、创建 QA 配置文件，并添加相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为简化研讨会流程，我们将这些工作合并为一个 PR。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 操作浏览器。`quality-checks` 技能运行单元测试、Lint 和类型检查，并按你新设定的格式生成报告。QA 智能体在 PR 创建前将这些结果与需求和测试覆盖范围审查结合起来。

新添加的测试应弥补真实缺口；QA 运行后不需要增加测试也可能是正确的。授权合并前，请审查代码和证据；更改后应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需要进行调整。指令设定标准，技能描述可重复任务，自定义智能体定义专用角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择**模式和模型**。使用 **Plan** 在构建前确定方案，使用 **Interactive** 跟进范围明确的更改，使用 **Autopilot** 处理范围清晰的任务。日常修改可选择速度更快的模型；复杂工作则使用能力更强的模型。

上下文与基础设施同样重要。明确描述希望构建的内容、原因和方式，会显著影响输出。

## 更多探索

你已完成核心工作流。还可以探索以下 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种观点。
- 使用 `/fleet` 并行协调彼此独立的子任务。
- 使用 `/worktree` 隔离单独的任务。
- 使用 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

提高工具使用能力的最佳方法就是持续使用它！将工具用于生产代码、个人项目，或者多年来一直想开发却还没开始的小应用。与团队分享心得，也向团队学习。一如既往，还可以继续探索相关文档。

如果想在终端中扩展 Tailspin Toys，可继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他环境，可探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
