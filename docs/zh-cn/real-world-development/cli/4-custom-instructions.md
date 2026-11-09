---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看存储库指令、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能够访问相关指导。[指令文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。筛选功能已经完成，现在查看 Copilot 使用的指令、添加文档标准，并将其应用于代码。

本课将介绍如何：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 团队有一系列开发实践规范和要求：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口契约；相关代码更改时应同步更新注释。
- 保留现有格式和 lint 指南。

通过指令文件，可以让 Copilot 遵循这些实践要求。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求，从而生成更相关的建议和代码片段。可以指定编码规范、库以及注释风格。指令既可以适用于整个存储库，也可以针对特定文件类型或任务。

指令文件分为两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送此文件，适合存放与大多数请求相关的项目级指导。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖特定格式前，请先查看[自定义指令支持参考][custom-instructions-support]。

## 浏览本项目中的自定义指令文件

启动项目时已包含一组指令文件。先查看已有内容，再进行更改，以便了解更改带来的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此文件适用于你在此存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中内容。留意 Astro、Drizzle 数据层、测试等指令文件。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用 glob 指定指令的适用文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成可复用的小文件，并提供可参考的示例。

## 更新指令以符合团队规范

现有文件是不错的起点，但仍缺少一项指导。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（应位于第 35 行附近）。
2. 将以下内容添加为此部分的最后一项：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

更改会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存指令后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选功能对话，并确认当前检出的分支为 `game-filters-cli`。

3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结与后续步骤

你已了解 Copilot CLI 如何读取指令文件，并将新标准应用于功能代码。本课中，你：

- 了解存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

接下来，将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
