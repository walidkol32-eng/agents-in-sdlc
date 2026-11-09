---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解一些背景信息，就要确保它能访问这些信息。[指令文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解要做什么，也了解如何完成。

本练习将介绍如何：

- 通过存储库自定义指令和按路径限定的指令文件，了解项目上下文、编码指南和文档标准如何传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加一项新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流程不同，本工作流程不执行提示词的前后对比。Copilot 云智能体会在 GitHub 议题上异步工作，因此较难实时并排展示指令的影响。之后审查云智能体创建的拉取请求时，你会看到这些指令的作用。

## 指令文件

### 场景

Tailspin Toys 团队有一系列开发实践规范和要求：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代感。
- 代码中应通过 TSDoc 文档注释提供文档。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 了解这些实践要求并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求，从而更好地生成建议和代码片段。可以指定编码规范、库以及注释风格。指令既可以适用于整个存储库，也可以针对特定文件类型或任务。

指令文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送此指令文件。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对具体文件类型（例如 TypeScript）或任务（例如创建 UI 组件或单元测试）提供指令。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。示例项目展示了一种有代表性的做法：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目内容、结构和全局编码规范。
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
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses

## 浏览本项目中的自定义指令文件

花点时间阅读存储库中的指令文件：其中有一个核心 `copilot-instructions.md`，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。留意 Astro、Drizzle 数据层、测试等指令文件。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段：它通过相对于存储库根目录的 glob 指定适用文件。此处会匹配 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 留意此文件中有关创建单元测试的说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并查看文件末尾。留意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成可复用的小文件，并指向生成代码时可参考的示例。（这些路径相对于指令文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分描述了项目编码规范，但尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 添加存储库级标准

接下来要做的是在 `.github/copilot-instructions.md` 中添加一项项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。云智能体稍后处理本工作流程中的议题时会遵循这项规则。

编辑文件前，先创建并切换到新分支（云智能体会从议题目标分支读取指令，但仍应妥善提交自己的更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码规范有助于提升代码建议质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应位于第 27 行附近）。留意它描述了项目编码规范，但尚未要求在代码中添加文档注释。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前一课所述，指令文件既可以放在存储库级别（`.github/copilot-instructions.md`），也可以放在针对特定语言、文件类型或任务的 `*.instructions.md` 文件中。项目级标准（例如文档注释规则）应放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将把议题分配给 Copilot，届时它会从 `main` 分支创建分支。因此，指令更改必须先合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并合并该请求。下一练习中，云智能体就会读取这些指令。

> [!TIP]
> 如果希望直接在 `main` 上完成本研讨会，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了演示实际项目中的工作方式。

## 总结与后续步骤

你已了解 Copilot 如何读取本项目的指令文件，并向 `.github/copilot-instructions.md` 添加了存储库级标准。后续练习中，云智能体创建的拉取请求会体现该规则的作用。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
