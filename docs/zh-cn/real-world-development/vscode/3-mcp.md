---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚使用智能体模式构建了筛选功能。创建拉取请求前，应先在浏览器中确认功能正常。无需亲自逐项操作应用，可以连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器测试功能，然后发布分支并创建 PR。

本练习将介绍如何：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 在浏览器中手动测试筛选功能。
- 发布分支并创建筛选功能拉取请求。

## 什么是模型上下文协议 (MCP)？

当智能体模式可以访问编辑器之外的资源时，它的能力会大幅增强。MCP 是 Copilot 与外部工具和服务通信的标准方式。

![智能体模式的工作原理示意图，展示其与上下文、LLM 和工具的交互，其中包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 让 AI 智能体能够与外部工具和服务通信。使用 MCP 后，智能体可以实时获取最新信息（通过资源）并代表你执行操作（通过工具）。

智能体通过 MCP 服务器访问这些工具和资源。MCP 服务器充当智能体与外部工具、服务之间的桥梁，例如现有 API 或 npm 本地工具。每个 MCP 服务器提供一组可供智能体访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP 服务器](https://github.com/github/github-mcp-server)**：提供管理 GitHub 存储库的 API，让智能体能够创建存储库、更新现有存储库并管理议题和拉取请求。
- **[Playwright MCP 服务器](https://github.com/microsoft/playwright-mcp)**：通过 Playwright 提供浏览器自动化能力，让智能体能够导航网页、填写表单和选择按钮。

还有许多其他 MCP 服务器可访问不同工具和资源。GitHub 提供 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前，请仔细审查源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 查看已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板已配置 `github` 项，因此 Copilot 能在上一练习中读取待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装。VS Code 会通过 HTTP 连接服务器；Copilot 首次使用其中的工具时，需要使用 GitHub 完成身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]可为 Copilot 提供可控的浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁添加 `playwright` 项，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程的 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会通过 `npx @playwright/mcp@latest` 启动它。`--headless` 参数会让 Playwright 在不显示窗口的情况下运行浏览器，这适用于没有桌面环境的 Codespace。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会在需要时启动 MCP 服务器：Copilot 首次使用服务器工具时，VS Code 会启动它并要求你确认是否信任。

1. 在 `.vscode/mcp.json` 中，`playwright` 项上方会出现 **Start** 操作。选择该操作以立即启动服务器并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便服务器启动。

也可以不手动启动。若跳过此步骤，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动它并询问是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可操作的真实浏览器。它可以打开页面、导航、应用筛选并读取结果，然后向你汇报。无需离开对话即可快速确认功能是否符合预期。

Playwright MCP 服务器会根据页面的[无障碍树][playwright-mcp-server]而不是屏幕截图进行操作。这意味着智能体会像辅助技术一样，通过带标签的结构化元素（按钮、链接和列表项）推理，因此功能检查也可以顺便验证基本无障碍情况。

连接服务器并启动应用后，要求 Copilot 测试刚完成的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并执行每个步骤，然后报告结果。根据议题中的验收标准审查报告；如发现问题，可继续提问或让 Copilot 先修复代码，再创建 PR。

> [!NOTE]
> 测试时应用必须运行在 `http://localhost:4321`。如果已停止开发服务器，请先重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。首先发布 `filtering-vscode` 分支。

1. 在 Codespace 中打开 **Source Control** 面板并审查 Copilot 所做的更改。
2. 选择 **+** 暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish** 将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建 PR。也可以让 GitHub Copilot 查找相关议题并创建关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 在智能体下拉菜单中确认已选择 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 要求 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 按需选择 **Continue**，允许 Copilot 获取信息并执行操作。首次使用 GitHub 工具时，可能会提示你登录 GitHub；按照提示完成身份验证。
5. 留意 Copilot 如何搜索议题、找到相关议题并创建 PR。
6. 选择 Copilot 生成的链接，审查拉取请求，但**暂时不要合并**。

## 总结与后续步骤

恭喜！本练习中，你：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何为 Copilot 扩展工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用它在提交筛选功能前通过浏览器手动测试。
- 发布了分支并创建筛选功能拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，创建一个[用于简化无障碍审查等专门任务的自定义智能体][next-lesson]。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [GitHub MCP 服务器][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
