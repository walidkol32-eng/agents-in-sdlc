---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 小幅修改游戏卡片，在转发浏览器中审查，并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试对话后，现在可以首次修改项目了。先从小处着手：游戏数据中已有星级评分，但主页的游戏卡片尚未显示评分。你将要求智能体显示评分、审查更改，并通过第一个拉取请求合并更改。

本课将介绍如何：

- 在功能分支中开始有针对性的 Copilot 对话。
- 要求智能体小幅修改项目。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且评分已显示在游戏详情页上。但主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身，你将让智能体在每张卡片上显示现有评分。这项小型且独立的更改很适合作为首次会话任务。

## 对话剖析

**对话**是你使用 Copilot CLI 完成任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，不会创建专用工作树。保存的对话可以让你稍后返回之前的讨论，而文件和分支仍是磁盘上的普通 Git 状态。

对话中包含三类内容：你发送的提示词和智能体的回复、智能体探索和编辑文件时产生的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

现在开始一个新对话，实现此功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，执行以下命令：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new`，然后按 <kbd>Enter</kbd>，确保开始新会话。
5. 使用以下提示词请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件并创建所需代码。现在，你已使用 Copilot CLI 添加新功能。

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使更改很小。现在直接在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查所有已更改的文件。
2. 确认游戏卡片在评分存在时显示数值评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 查看 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此生成的代码可能不同。请审查实际行为，而不是期待完全一致的实现。

## 检查更改

不要只看代码就假定它能正常运行。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的用户界面 (UI)。

1. 使用以下提示词要求 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示的评分满分为五分。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已完成。接下来创建拉取请求 (PR)，将新代码合并到项目中。

1. 要求默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查已更改的文件和检查结果。
5. 准备好后，选择 **Merge pull request** 并确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

恭喜！你已使用 GitHub Copilot CLI 交付第一项更改。本课中，你：

- 在功能分支中开始有针对性的 Copilot 对话。
- 指示智能体修改游戏卡片。
- 使用 `/diff` 审查更改。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并第一个拉取请求。

接下来，你将[从筛选功能议题开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
