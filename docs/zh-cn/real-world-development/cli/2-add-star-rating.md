---
title: "第 2 课 - 添加星级评分：快速见效"
description: "使用 Copilot CLI 对游戏卡片做一处小改动，在转发的浏览器中查看效果，并将其作为你的第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装好 Copilot CLI 并尝试过一次对话后，现在可以对项目做第一处改动了。改动很小：游戏数据中已经有星级评分，但主页上的游戏卡片还没有显示。你将让智能体把评分显示出来，审查更改，并将其作为第一个拉取请求合并。

在本课中，你将：

- 在功能分支上开始一次专注的 Copilot 对话。
- 让智能体对项目做一处小改动。
- 使用 `/diff` 审查更改。
- 运行应用，在转发的浏览器中确认更改。
- 创建并合并你的第一个拉取请求。

## 场景

Tailspin Toys 中的每个游戏都可以有星级评分，游戏详情页上已经显示了评分。但主页上的游戏卡片只显示标题、类别、发行商和描述。作为热身，你将让智能体在每张卡片上显示现有评分。这是一个小巧而独立的改动，非常适合作为第一次会话的内容。

## 对话的构成

**对话**是你使用 Copilot CLI 完成任务的地方。与 Copilot app 不同，普通的 CLI 对话使用终端中当前检出的存储库和 Git 分支，而不会创建专用的工作树。保存的对话可让你日后回到同一场讨论，而文件和分支仍是磁盘上普通的 Git 状态。

在对话中可以看到三类内容：你的提示和智能体的回复、智能体探索和编辑文件时的工具活动，以及可以用 `/diff` 查看的更改。

## 开始对话并提出更改请求

开始一次新对话，着手实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果尚未启动 Copilot，请使用以下命令启动：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保开始一个新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到用于显示游戏详情的文件，并生成所需的代码。这样就用 Copilot CLI 添加了一项新功能！

## 审查差异

所有 AI 生成的更改在合并前都应经过审查，即使是很小的更改。直接在 Copilot CLI 中查看这些更改。

1. 输入 `/diff` 并检查每个已更改的文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖了这两种状态。
4. 查看 Copilot 运行的检查结果，并让它修复所有失败项。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 是概率性的而非确定性的，因此生成的代码可能与示例不同。请审查行为，而不要期望得到某一种固定的实现。

## 检查更改

当然，不能只读代码就假定它能正常工作。让 Copilot 启动网站，以便在 Codespaces 转发的浏览器中查看更新后的用户界面 (UI)。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了五星制评分。
4. 返回 Copilot，让它停止自己启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已经完成！现在创建拉取请求 (PR)，将新代码合并到项目中。

1. 让默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 在创建 PR 时会推送已有的提交，并显示 PR 的 URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux）并选择 Copilot CLI 显示的 URL，打开该 PR。
4. 查看已更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，使用 `/exit` 退出 Copilot CLI。
7. 更新本地的 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结与后续步骤

恭喜！你已使用 GitHub Copilot CLI 交付了第一个更改。具体来说，你：

- 在功能分支上开始了一次专注的 Copilot 对话。
- 指导智能体对游戏卡片做了一处小改动。
- 使用 `/diff` 审查了更改。
- 运行应用，在转发的浏览器中确认了星级评分。
- 创建并合并了第一个拉取请求。

接下来，将[从筛选 issue 入手，使用计划模式和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
