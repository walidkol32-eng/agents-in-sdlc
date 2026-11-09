---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "查看或配置 Copilot CLI 中的 Playwright MCP，并在浏览器中检查筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面所说，编写代码只是开发工作的一部分。还需要处理数据和外部服务，并为 Copilot 提供更多自动化能力。MCP 服务器可以扩展 Copilot CLI 内置的能力，为它提供更多工具和服务。

本课将介绍如何：

- 了解模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 在尚未配置时添加 Playwright MCP 服务器。
- 让智能体操作浏览器并检查筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新还需要实际与 UI 交互。现在将网站交给 Copilot，让它像用户一样操作，以进一步自动化更改验证并提升信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]让 AI 智能体能够与外部工具和服务通信。使用 MCP 后，智能体可以实时获取最新信息，并代表你执行操作。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当智能体与外部工具和服务之间的桥梁，每个服务器都提供一组可供智能体访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP 服务器][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP 服务器][playwright-mcp-server]**：提供 Playwright 浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供 [MCP 注册表][mcp-registry]，帮助发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Copilot CLI 尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 可以像用户一样操作网站。

> [!NOTE]
> 如果已在 Copilot CLI 中配置 Playwright MCP，请跳过这些设置步骤，直接继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中输入：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 使用 Playwright 让 Copilot 检查功能

之前你已手动确认功能正常。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的检查。

1. 使用以下提示词，让 Copilot 使用 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需明确要求 Copilot 使用 MCP 服务器，它一般会自行判断。但指出预期使用的服务器有助于保持结果一致，也能减少一些令牌消耗。

2. 观察 Copilot 在浏览器中逐步检查功能。
3. 阅读报告，确认所有功能均符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器，然后提供报告。

## 总结与后续步骤

恭喜，你已通过 Copilot CLI 中的 Playwright MCP 服务器，在真实浏览器中检查功能。本课中，你：

- 了解了模型上下文协议 (MCP) 以及 Copilot CLI 如何使用它。
- 在尚未配置时添加了 Playwright MCP 服务器。
- 让智能体操作浏览器并检查筛选功能。

接下来，将创建一个[结合技能和浏览器工具的 QA 自定义智能体][next-lesson]。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
