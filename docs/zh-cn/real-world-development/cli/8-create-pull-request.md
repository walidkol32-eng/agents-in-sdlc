---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都已保存在同一分支。现在将它们一起审查并创建拉取请求。星级评分拉取请求 (PR) 是你手动合并的；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分到几个独立的 PR 中。为简化本工作坊，我们将筛选功能和质量工作流保留在同一个对话和分支中，统一提交到此 PR。

本课将介绍如何：

- 了解 Agent Merge 以及它如何自动处理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动完成剩余 PR 工作，同时由开发者控制合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会解决 PR 当前的阻碍，例如修复持续集成 (CI) 检查失败、响应审查评论，以及在需要时变基；之后，如果存储库允许，它会启用 GitHub 自动合并。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以代为处理，但启用前仍应先审查工作并确认 PR 已准备好合并。

## 使用 Agent Merge 管理 PR

代码已全部完成，接下来一起审查、创建 PR，并让 Agent Merge 处理其余流程。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建新的 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成过程。所有检查通过后，它会执行合并。
7. 确认 Copilot 显示了类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或缺失权限。如果操作受阻，请阅读报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结与后续步骤

你已自动完成开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 以及它如何自动处理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

接下来，探索更多实用的 Copilot CLI [斜杠命令][next-lesson]，管理上下文和模型、共享会话，以及选择性地委派给云智能体。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
