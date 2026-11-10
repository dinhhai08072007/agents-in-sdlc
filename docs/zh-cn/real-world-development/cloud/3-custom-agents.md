---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库 `.github/agents` 文件夹中创建 Markdown 文件来定义智能体，可以为 Copilot 提供具体指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编写成可复用的智能体，例如执行 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter（至少有 `name` 和 `description`），后面跟着定义智能体行为、专长和指令的 Markdown 提示。

### 自定义智能体与智能体技能的对比

自定义智能体与[智能体技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，都用于指导 AI 执行操作。最简单的区分方式是：**自定义智能体**是执行工作的角色，**技能**是工具。

自定义智能体拥有自己的上下文窗口，并可在工作时协调技能（甚至其他智能体）。在本实验中，无障碍自定义智能体会根据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流技能或用于运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，需要反复测试和迭代，找到适合环境和场景的配置。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

自定义智能体可用于以下任务：

- 了解自定义智能体的定义方式。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，无论用户的视觉能力或偏好如何。近期用户反馈指出，当前深色主题中的文字与背景对比度不足，导致部分用户难以阅读。为解决此无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

由于无障碍至关重要，你希望尽快实现此功能，因此将使用自定义智能体生成相关功能。

## 查看无障碍自定义智能体

项目已为无障碍工作准备了自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 Codespace，打开无障碍自定义智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含带有 `name` 和 `description` 的 frontmatter。

3. 接着浏览并审查以下部分：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误列表。

## 创建并分配议题

任务控制中心是管理环境中所有智能体的中心位置。你可以向 Copilot 云智能体分配任务、监控任务，还可以重定向正在进行的会话并提供更多指导。现在将创建高对比度模式的任务并分配给 Copilot。

1. 前往存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![云智能体分配屏幕截图，其中高亮显示自定义智能体和 accessibility](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结与后续步骤

本课介绍了 GitHub Copilot 中的[自定义智能体][custom-agents]，也就是为特定任务和领域定制的 AI 助手。使用自定义智能体可以将团队专业知识和标准编写成可复用的智能体，指导 Copilot 更有效地完成特定类型的工作。

本课介绍了以下概念：

- 自定义智能体的定义方式。
- 将任务分配给自定义智能体。

Copilot 正在后台实现高对比度模式。接下来前往任务控制中心，[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中使用自定义智能体前的准备工作][org-custom-agents]
- [在企业中使用自定义智能体前的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
