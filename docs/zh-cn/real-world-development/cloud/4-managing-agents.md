---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你让 GitHub Copilot 云智能体处理了三项改善用户体验和添加功能的任务。云智能体可以异步自主工作，但监控这些任务仍然很重要。

GitHub.com 提供多种工具来管理分配给云智能体的任务，包括[智能体页面][agents-page]。在这个控制中心中，可以查看所有包含未关闭拉取请求 (PR) 的智能体任务，查看它执行的操作，并在会话进行中提供指导。

本课将介绍如何：

- 使用智能体页面监控云智能体任务。
- 在会话进行时请求添加额外功能。

## 场景

将任务分配给智能体以实现高对比度模式后，团队意识到也可以顺便添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此将浅色模式纳入同一项工作很合适。你将指导智能体，同时实现浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 为代码库补充文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 当前的工作状态和执行方式而异。

4. 同时留意右侧的拉取请求 (PR) 面板。在这里可以查看 PR 和已更改的文件，以便进一步监控。

## 指导云智能体

现在已经了解当前运行的任务，可以在 Copilot 实现高对比度模式时要求它也添加浅色模式。

1. 选择与添加高对比度模式相关的会话。会话标题可能因 Copilot 使用的名称和当前状态而异。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，直到 Copilot 完成设置并开始工作。此时会看到类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot 智能体页面中的任务，以及突出显示的 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认收到提示词，并将它纳入工作流程。

## 等待 Copilot 完成工作

与之前一样，Copilot 会继续处理更新后的任务！完成当前步骤后，它会将新要求纳入工作流程。

这可能需要几分钟。等待期间，可以回顾目前学到和探索过的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，这是监控和指导 GitHub Copilot 云智能体任务的中心位置。可以跟踪所有进行中和已完成的任务、查看工作内容，并为正在运行的会话提供指导或调整范围。

本课涵盖了以下内容：

- 使用控制中心和智能体页面监控云智能体任务。
- 调整进行中的会话，要求添加额外功能。

Copilot 正在完成无障碍功能，接下来将[迭代改进它创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
