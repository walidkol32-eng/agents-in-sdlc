---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，结合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到一个遵循明确 QA 流程的自定义智能体中。

本课将介绍如何：

- 了解自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，使用统一流程审查需求、代码质量、自动化检查、测试覆盖范围和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专业角色。配置文件说明智能体的用途、指令和可用工具。本工作坊会在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择该智能体。

前面使用的自定义功能各有不同用途：存储库指令描述团队规范；`quality-checks` 技能打包可重复检查；Playwright MCP 提供浏览器工具；QA 配置文件则指导 Copilot 如何使用这些能力评估需求并报告结果。它们不互相替代，也不需要新建对话。

## 创建 QA 配置文件

创建功能 PR 前，先让 Copilot 创建可复用的 QA 配置文件，说明需要执行哪些检查以及必须遵守的边界。

1. 返回 Codespace，确认筛选功能对话处于 Interactive 模式。
2. 要求默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先检查 Copilot 生成的配置文件，确认它记录了预期的 QA 流程和边界。这样可避免智能体在你只想验证功能时修改实现代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必需字段，`name` 为可选字段；添加名称可让智能体具有清晰的显示名称。
3. 阅读配置文件指令，确认 QA 会从需求开始、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，并且不会提交更改或创建拉取请求。
5. 如果缺少任何职责或边界，要求默认智能体先修改配置文件再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置文件后，恢复同一筛选功能对话，再选择 QA，以便它使用已有的议题和规划决定。

1. 输入以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会显示在可用智能体列表中。它仍然可用，上面的命令会启用它。

2. 使用以下提示词让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 工作完成后，阅读它生成的报告。

## 总结与后续步骤

你已在工作流中添加可复用的专业角色，并审查了它的结果。本课中，你：

- 了解了自定义智能体如何与指令、技能和 MCP 工具协作。
- 创建并检查了可复用的质量保证配置文件。
- 选择 QA 智能体，根据筛选议题审查其发现。

现在，已准备好实现功能、技能更新、QA 配置文件、测试和验证报告。接下来将它们[整合到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
