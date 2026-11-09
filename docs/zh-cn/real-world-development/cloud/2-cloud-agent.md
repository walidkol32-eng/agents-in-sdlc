---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因时间不足而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体，加快开发流程，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围清晰的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初创建网站的承包商没有为项目编写文档。团队希望首先为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们想在每款游戏的详情页上显示相关游戏（即同一类别中的其他游戏），供用户继续浏览。目前不要求实现完整体验，只需提供可用于验收测试的基本实现。这项工作虽然被搁置，但当前还有优先级更高的议题。

这些都是容易被延后的任务，也适合交给云智能体异步处理。开发者可以同时处理其他工作，之后再审查结果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体][copilot-agents]可以像开发者一样在后台执行任务。可以通过多种方式向它分配工作，例如[将 GitHub 议题分配给 Copilot][assign-issue]。分配后，Copilot 会创建草稿拉取请求以跟踪进度、准备环境并开始处理任务。你可以在会话进行中或完成后查看详情。建议方案准备好供审查时，Copilot 会在拉取请求中通知你。

## 编写范围明确的指令

GitHub Copilot 并非魔法，不能替代思考。看似简单的操作也可能包含复杂环节，因此要谨慎界定云智能体的任务范围，并参阅[云智能体最佳实践][cloud-agent-best-practices]。将它视为 AI 结对编程助手：分阶段工作、学习、试验并根据结果调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已完成自定义指令练习，就会看到 `.github/copilot-instructions.md` 的实际效果；若尚未查看，现在可以先快速浏览。分配任务前，以只读方式查看存储库中的指令文件，稍后便能观察它们的影响。

在 GitHub Web 界面中，或在已启动的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，留意它如何遵循这些指令中的 TSDoc、TypeScript 规范和文件注释头。后续练习会再次审查这些 PR。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要准备特定环境并运行设置脚本。将任务分配给 Copilot 时也一样，它会像软件工程师一样完成工作。

云智能体使用 [GitHub Actions][github-actions] 环境。可通过 `.github/workflows/copilot-setup-steps.yml` 中的[专用设置工作流][setup-workflow]配置预先运行的步骤，为它安装所需工具和依赖项。本实验已提前配置该工作流：安装 Node.js 和项目依赖项、Playwright 浏览器，以及为 Astro 应用迁移并填充 SQLite 数据库。

```yaml
name: "Copilot Setup Steps"

# 用于从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，步骤完成后 Copilot 会自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此工作流与其他 GitHub 工作流类似，但有几点需要留意：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- 其中的 `workflow_dispatch` 触发器可让你从存储库的 **Actions** 选项卡手动运行工作流。这有助于验证工作流，而不必等待 Copilot 启动它。

## 添加文档

大多数项目都存在文档过时或缺失的问题。这类技术债务容易被忽略，会降低工作效率，也让维护代码和引入新开发者更加困难。Copilot 擅长生成文档，因此很适合将这类议题分配给云智能体。它会在后台创建所需文档，稍后可以审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未显示新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的方案。

13. 查看 Copilot 即将完成的任务列表。
14. 向下浏览 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session**。

    ![查看 Copilot 会话](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态。

16. 浏览实时会话，观察 Copilot 如何探索代码、了解当前状态、暂停思考并制定方案，然后创建代码。

这可能需要几分钟。云智能体的一个主要优点是可以异步执行任务，让你同时处理其他工作。接下来会再分配一项任务，然后转而编写应用功能。

## 在游戏详情页添加相关游戏

GitHub Copilot 云智能体的一大优点是可以拆分工作：你处理一组任务时，它可以处理另一组。为设计团队添加相关游戏区域可能不需要很长时间，但也可以把这段时间用于其他工作。现在将任务分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后会看到议题中的首条评论出现 👀，表示 Copilot 已开始处理。

Copilot 正在处理第二项请求。云智能体会像软件工程师一样工作，因此无需一直监控，完成后再审查即可。接下来将创建并使用自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给 Copilot，让它异步工作。云智能体可用于处理技术债务、创建新功能，或协助将代码迁移到其他框架。

本课涵盖了以下内容：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围清晰的议题。
- 将议题分配给 Copilot。

云智能体正在后台工作，接下来可以创建并使用自定义智能体。[云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用之前练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
