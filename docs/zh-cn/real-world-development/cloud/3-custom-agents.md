---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中的特定任务或领域定制的 AI 助手。可以在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，为 Copilot 提供专门的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用的智能体，例如执行 [WCAG][wcag] 无障碍标准的智能体、遵循安全编码实践的智能体，或维护统一测试模式的智能体。

自定义智能体由项目 `.github/agents` 文件夹或全局 `~/.copilot/agents` 文件夹中的 Markdown 文件定义。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description` 字段；其余 Markdown 内容用于定义智能体的行为、专业知识和指令。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行任务。可以这样区分：**自定义智能体负责工作，技能是工具**。

自定义智能体拥有自己的上下文窗口，能够编排技能甚至其他智能体来完成任务。本实验中的无障碍自定义智能体会根据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流技能，或运行和管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，需要通过测试和迭代，找到适合环境和场景的方式。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

自定义智能体将用于完成以下任务：

- 了解自定义智能体的定义方式。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户（无论视觉能力或偏好如何）都能访问其众筹平台。近期用户反馈指出，当前暗色主题中文字与背景的对比度不足，部分用户难以阅读。设计团队因此要求添加可切换的高对比度模式。

无障碍对用户至关重要，因此希望尽快完成这项功能。接下来将使用自定义智能体生成实现。

## 查看无障碍自定义智能体

我们已为你准备好无障碍自定义智能体。打开 Codespace 并查看其内容，了解它将如何引导 Copilot。

1. 打开 `.github/agents/accessibility.md`。
2. 留意包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，了解其中列出的内容：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见问题和错误。

## 创建并分配议题

智能体控制面板是管理所有智能体的中心位置。在这里可以将任务分配给 Copilot 云智能体、监控任务并提供额外指导。现在将创建高对比度模式的任务并分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义智能体列表中选择 **Accessibility agent**。

    ![在云智能体分配界面中突出显示自定义智能体和 Accessibility agent](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 将在后台开始处理任务。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 自定义智能体][custom-agents]，也就是为特定任务和领域定制的 AI 助手。自定义智能体可以将团队的专业知识和标准整理成可复用内容，引导 Copilot 更有效地完成特定类型的工作。

本课涵盖了以下内容：

- 自定义智能体的定义方式。
- 将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，前往智能体控制面板[监控并指导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做好准备][org-custom-agents]
- [为企业使用自定义智能体做好准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
