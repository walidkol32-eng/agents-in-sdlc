---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发者需要找到所有相关文件、进行修改、运行测试、调试、查找遗漏文件，再继续修改。这项工作可能不断重复。

Copilot 智能体模式正适合此类任务。它可以在 IDE 中自主探索项目结构、修改相关文件、运行测试并修复发现的问题。现在使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但适用于不同场景。智能体模式在 IDE 中执行任务，适合快速反馈和交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 让 Copilot 智能体模式探索项目、查找相关文件并协调多处更改。
- 使用 Copilot 智能体模式跨 UI 和数据层实现功能。
- 在合并到代码库前审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏目录不断扩大，需要允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数；现在使用 Copilot 智能体模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

开始修改前，先了解 Tailspin Toys 网站当前的功能。

该网站是一个以开发者为主题的桌游众筹平台，可浏览游戏列表和详情。它是一个 Astro 应用，在构建时将页面生成为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中；交互功能由使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供 `npm run dev` 命令，用于启动单个 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回上一练习中使用的 Codespace。当前分支应为 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否要在浏览器窗口中打开 `http://localhost:4321`，选择关闭按钮 **x**。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端的 localhost 链接时，会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，网站服务器正在其中运行。

### 浏览网站

网站运行后，可查看其现有功能：

- **主页：**显示桌游列表，包括标题、图片和说明。
- **游戏详情页：**选择游戏后会显示其详细信息，包括标题、说明、发行商和类别。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用的模式和模型可能不同，相关步骤会在需要时说明。

网站的初始实现已可正常使用，但还需要添加更多功能。先查看待办事项。通过模板创建项目时，系统已自动创建议题。让 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。该智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 工作坊作者并非推荐某个模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将它列入操作步骤，希望代码建议尽可能一致。但由于大型语言模型具有概率性，生成结果可能不同于示例，这是正常情况。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示词，要求 Copilot 查看存储库待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

留意 Copilot 如何根据对用户的帮助程度为议题排序。

## 查看指令文件

让智能体生成代码前，先查看用于提供上下文的指令文件。项目中的[用户界面 (UI) 指令][ui-instructions]说明了添加网站功能时应采用的方法。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中的总体指导，包括：
   - 应用架构概述。
   - 组件设计、可测试性和无障碍原则。
   - Astro、Tailwind CSS 和 Drizzle 等文件类型的专用指令链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用内容、拆分复杂指令，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

筛选功能至少需要三处更改：

- 在数据层 (`src/lib/`) 添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前需要确保测试全部通过。Copilot 智能体模式可以处理这些工作。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 **ui** 指令文件。

   ![选择 UI 指令文件的截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选中 **Agent**。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示词，要求 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并查找相关的数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 正在探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中编辑器可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会提供 **Keep** 或 **Undo** 选项；工作尚未结束时不必选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 面板中要求用户确认运行测试的对话框](../../../_images/ex3-agent-mode-run-tests.png)

8. Copilot 在生成代码和运行测试之间迭代时，可能会暂停或遇到测试失败。这是正常的；它会继续迭代，直至任务完成且未发现错误。

   ![完成的 Copilot 智能体模式聊天会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站，在浏览器中试用新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了功能。你了解到如何：

- 使用 Copilot 智能体模式跨 UI 和数据层实现功能。
- 让 Copilot 智能体模式探索项目、查找相关文件并协调多处更改。
- 在合并到代码库前审查智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 加分探索：实现分页

随着游戏列表不断变长，网站将需要分页。可以运用本练习中学到的技能，提示 Copilot 为网站添加分页。制定提示词和审查实现时，可考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 决定分页方式，例如是否允许用户选择页面大小，还是使用固定页面大小。
- 在提示词中提供足够信息，引导 Copilot 按需求实现。
- 根据需要与 GitHub Copilot 迭代，提供更改请求和上下文。这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能和使用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
