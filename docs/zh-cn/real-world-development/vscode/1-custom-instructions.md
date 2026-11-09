---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能够访问相关指导。[指令文件][instruction-files]可以提供有关代码生成方式的说明。

本练习将介绍如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 使用当前指令生成筛选功能的首个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 再次运行相同提示词，观察生成的代码如何采用新规范。
- 提交指令更新和筛选数据切片到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不符合所有规范。Copilot 的行为具有非确定性。本练习的目的是观察更新指令前后行为的趋势，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 团队有一系列开发实践规范和要求：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代感。
- 代码中应通过 TSDoc 文档注释提供文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 了解这些实践要求并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求，从而生成更相关的建议和代码片段。可以指定编码规范、库以及注释风格。指令既可以适用于整个存储库，也可以针对特定文件类型或任务。

指令文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送此文件，适合存放与大多数请求相关的项目级指导。
- `.github/instructions/*.instructions.md`：可针对特定语言（例如 TypeScript）或任务（例如创建 UI 组件或单元测试）提供指令。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围。示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目介绍、结构和全局编码规范。
- 使用 `*.instructions.md` 为文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，确保指导清晰，并提供代码示例说明推荐或不推荐的写法。

创建指令文件没有唯一正确的方法，使用 AI 也是如此。通过试验，逐渐找到适合项目和场景的方式。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应准备一套完善的指令文件。浏览本项目中的文件时，会看到它们覆盖了多种任务，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以协助生成指令文件。不同产品界面的操作有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适用时介绍具体操作。
>
> 如需模板或入门示例，请查看包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository

## 浏览本项目中的自定义指令文件

项目模板已包含一组指令文件。在开始修改前，先查看已有内容并了解其作用。

1. 返回 Codespace。
2. 在 `.github/copilot-instructions.md` 文件中，查看项目简介及相关指导。这些指令适用于你在此存储库中与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。留意 Astro、Drizzle 数据层、测试等指令文件。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用 glob 设定指令的适用文件。
5. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成可复用的小文件，并提供可参考的示例。

## 创建更改分支

按照最佳实践，为本次更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。不同练习所用的模式和模型可能不同，相关步骤会在需要时说明。

先使用当前指令文件发送提示词，再更新指令并重新发送相同提示词，即可观察自定义指令的影响。

1. 关闭之前练习留下的编辑器标签页，避免 Copilot 获取不必要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，让 Copilot 可以修改文件。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数和测试文件。
6. 留意生成的辅助函数是一个类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md` 数据层规范。
7. 留意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的行为具有非确定性，因此即使没有收到指令，它也可能添加文档注释。若出现这种情况也没关系，本练习仍可通过比较指令更新前后的行为来观察规范的影响。

## 添加存储库级标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码规范有助于提升代码建议质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分。留意其中还没有关于代码文档的规则，这也是之前生成的辅助函数缺少文档注释的原因。
3. 在现有规范下方添加以下内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前一课所述，指令文件既可以放在存储库级别（`.github/copilot-instructions.md`），也可以放在针对特定语言、文件类型或任务的 `*.instructions.md` 文件中。项目级标准（例如文档注释规则）应放在存储库级文件中。

## 重新运行提示词并观察差异

1. 返回 Copilot Chat 并选择 **New Chat**，清除之前的对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 关注正确的文件。
3. 发送与之前相同的提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意生成的文件现在会以类似以下内容的注释块开头：

   ```typescript
   /**
    * Tailspin Toys 众筹平台的发行商数据访问辅助函数。
    * 提供从数据库检索发行商信息的函数。
    */
   ```

5. 留意生成的函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * 返回包含所有发行商 ID 和名称的列表。
    *
    * @param db - Drizzle 数据库客户端。
    * @returns 解析为发行商对象数组的 Promise。
    */
   ```

你已引导 Copilot 遵循新的项目规范，并将其应用到下一练习将继续使用的代码中。

## 提交指令并推送分支

指令文件和其他资源一样，也需要通过源代码管理来维护。现在将指令更新和辅助函数提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开新的终端窗口。
2. 在终端中确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为基础辅助函数修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更改（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

推送分支后，需要创建拉取请求并将它关联到待办事项中的文档标准议题。可以手动操作，也可以让 Copilot 使用已连接的 GitHub 工具查找议题、创建 PR 并关闭对应议题。

> [!NOTE]
> 这是第一个让 Copilot 代表你操作 GitHub 的练习。项目模板已连接 GitHub 工具，无需另行配置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示完成登录。后续练习会介绍这种连接所用的模型上下文协议 (MCP)。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 要求 Copilot 找到与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更改和新的发行商辅助函数：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 如果系统提示允许 Copilot 代表你执行 GitHub 操作，请审查操作并酌情选择 **Allow**。
6. 创建 PR 后，要求 Copilot 合并 PR 并将分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果系统提示允许 Copilot 执行 GitHub 操作和 Shell 命令，请审查命令并酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并拉取请求。

## 总结与后续步骤

你已了解 Copilot 如何读取指令文件，并通过 VS Code 中的 Copilot Chat：

- 使用提示词生成代码，观察 Copilot 如何遵循现有指令。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示词，观察代码如何采用新标准。
- 将指令更新和基础辅助函数提交到 `main`，让下一练习可以继续构建。

接下来，让 Copilot 在[智能体模式][next-lesson]中运用这些指令，跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
