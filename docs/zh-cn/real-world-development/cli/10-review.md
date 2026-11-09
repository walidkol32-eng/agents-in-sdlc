---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可复用的自定义项和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在 Tailspin Toys 的连续工作流程中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace、安装 Copilot CLI、探索项目并找到模板创建的筛选功能议题。
- 添加了星级评分，在转发浏览器中审查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选功能议题开始，在 Plan 模式中制定方案，在 Autopilot 模式中构建，并在 Interactive 模式中审查。
- 使用自定义指令引导智能体，自定义现有的 `quality-checks` 技能，并使用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议 (MCP) 服务器，并在真实浏览器中检查筛选功能。
- 创建并选择质量保证 (QA) 自定义智能体，评估需求、覆盖范围、技能结果和浏览器验证证据。
- 审查完整的筛选功能更改，并授权 Agent Merge 处理筛选功能 PR。
- 探索用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 完成交付的内容

本工作坊包含两个 PR 里程碑，每个都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选功能和质量工作流：**实现筛选功能、更新指令并将其应用于功能代码、自定义 `quality-checks` 报告、创建 QA 配置文件，并包含相关测试。

从规划筛选功能到创建 PR，你始终使用同一对话和分支。为简化本工作坊，我们将这些更改放在一个 PR 中。

## 不同类型的验证

你通过多种方式检查代码：自动化测试、亲自在浏览器中检查，以及通过 MCP 让 Copilot 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 智能体会结合这些结果、需求审查和测试覆盖范围生成报告。

新增测试应解决实际覆盖缺口；QA 运行后确认无需添加测试也可能是正确结果。授权合并前，请审查代码和证据。更改代码后，应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了指令、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在多个对话中复用这些自定义项，并根据团队需求调整。指令设定标准，技能描述可重复执行的任务，自定义智能体定义专业角色，MCP 服务器连接外部工具。请审查实际更改和工具结果，而不只是智能体的总结。

根据任务选择**模式和模型**。使用 **Plan** 先思考方案，使用 **Interactive** 对小范围更改保持掌控，使用 **Autopilot** 处理范围明确的任务。例行编辑可以选择响应更快的模型，复杂任务则可选择能力更强的模型。

上下文与基础设施同样重要。清晰说明要构建*什么*、*为什么*构建以及*如何*构建，会显著影响输出结果。

## 继续探索

你已完成核心工作流程。还可以探索一些 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种观点。
- 使用 `/fleet` 并行编排独立子任务。
- 使用 `/worktree` 隔离其他任务。
- 使用 `/delegate` 将任务交给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方法就是继续练习！可以用它处理生产代码、个人项目，或一直想构建却没时间动手的小应用。与团队分享经验，也向团队成员学习。别忘了继续查看相关文档。

如果想通过终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他工作环境，可探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

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
