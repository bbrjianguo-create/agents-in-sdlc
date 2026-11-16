---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使对应用程序做最简单的更新，通常也需要修改多个文件并执行运行测试等操作。作为开发人员，你的工作流通常是：找出所有必要的文件，进行更改，运行测试，调试，找出遗漏的文件，再做一次更新……如此循环往复。

这正是 Copilot 代理模式发挥作用的地方。

Copilot 代理模式旨在于 IDE 中更自主地行动。它的行为类似开发人员：先探索现有项目结构，执行必要的更新，运行测试等任务，并自动修复发现的错误。下面来探索如何使用代理模式为网站引入新功能。

> [!NOTE]
> 虽然名称相近，但代理模式和 cloud agent 面向两种不同的体验。代理模式在 IDE 中执行任务，反馈周期短，便于交互。Cloud agent 则被设计为结对编程的同伴，像团队成员一样异步工作，通过 issue 和拉取请求与你交互。

在本练习中，你将了解：

- Copilot 代理模式如何探索项目、识别相关文件并进行协调一致的更改。
- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- 如何在合并到代码库之前，审阅 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表不断增长，你希望允许用户按类别和发行商筛选。你已在上一个练习中添加了 publishers 辅助函数，现在将使用 Copilot 代理模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

在进行任何更改之前，先浏览 Tailspin Toys 网站，了解其当前功能。

该网站是一个面向开发者主题桌游的众筹平台，用户可以列出游戏并查看详情。网站是一个单一的 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在其 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可重用的数据访问辅助函数位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型、限定作用域的 Astro `<script>` 添加。

### 启动网站

为便于运行网站，项目提供了 `npm run dev` 任务，用于启动单一的 Astro 开发服务器。可以在 GitHub Codespace 中按以下步骤运行：

1. 返回 codespace。上一个练习结束后，应当已回到 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）在 Codespace 中打开新的终端窗口。
3. 为本次筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，应会看到显示 URL 的横幅，类似于：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框提示在浏览器窗口中打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux）并选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从 Codespace 终端选择 localhost URL 链接会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是通往 codespace 的私有隧道，此时 codespace 正在托管你的 Web 服务器！

### 浏览网站

网站运行后，可以浏览其功能。网站的主要功能包括：

- **主页**：显示桌游列表，包含标题、图片和描述。
- **游戏详情页**：选择某个游戏后，会进入详情页，其中包含该游戏的更多信息，包括标题、描述、发行商和类别。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习之前，返回 codespace，打开 Copilot Chat 面板，并选择 **New Chat** 开始一次全新的对话。模式和模型的选择因练习而异，各步骤会在需要时指出。

网站的初始实现可以正常运行，但我们希望通过添加新功能来增强它。先从查看待办事项开始。通过模板设置项目时，系统已自动创建了一组 issue 作为待办事项，让 GitHub Copilot 把这些条目展示出来。

1. 在 Chat 视图的代理下拉列表中选择 **Agent**。**Agent** 会自主规划并实现跨文件的更改、运行终端命令并调用工具。

   ![展示 Chat 视图中代理选择器的截图。](../../../_images/shared-chat-mode-selector.png)

2. 在可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊的作者并不偏向某个模型。构建本工作坊时使用的是 Claude Sonnet 4.5，因此在说明中采用了它，希望你收到的代码建议相对一致，从而获得良好的体验。不过，由于 LLM 是概率性的，你可能会发现收到的建议与工作坊中的描述不同，这是完全正常的。

> [!NOTE]
> 由于 LLM 的概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问 issue 待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 运行命令，列出所有 issue。
5. 查看生成的 issue 列表。

注意，Copilot 甚至已根据它认为对用户最有用的程度，为各项排好了优先级。

## 查看指令文件

在启动代理生成代码之前，先查看将为 Copilot 的工作提供上下文的指令文件。你将使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)指令文件，其中包含如何为网站添加功能的上下文。

1. 在 codespace 中，打开 `.github/instructions/ui.instructions.md`。
2. 留意关于如何添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和可访问性的原则。
   - 指向各类文件的专项指令文件的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指令文件既可以引用其他指令文件，也可以引用项目中的文件，路径相对于指令文件所在位置。这样便于重用、将复杂的指令拆分为更易管理的小块，并提供示例和模板。

## 实现筛选功能

要完成筛选，至少需要对应用程序进行三项独立的更新：

- 在数据层（`src/lib/`）添加或完善其余的筛选逻辑
- 添加或更新筛选行为的测试
- 更新游戏列表页面，引入筛选 UI

此外，合并到代码库之前，测试必须运行并通过。Copilot 代理模式可以替你完成这些任务！来添加这项功能。

1. 可以在当前与 Copilot 的对话中继续，也可以选择 **New Chat** 开始新对话。
2. 依次选择 **Add Context**、**Instructions**，并选择 **ui** 作为指令文件。

   ![展示选择 UI 指令文件示例的截图](../../../_images/ex3-select-instructions-file.png)

3. 确保 Chat 视图的代理下拉列表中仍选中 **Agent**。

   ![展示 Chat 视图中代理选择器的截图。](../../../_images/shared-chat-mode-selector.png)

4. 确保模型仍选中 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办事项中的相关 issue 实现该功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 先探索项目，找到与所需功能相关的文件。应该会看到它找到数据层辅助函数、UI 和测试，然后开始修改文件并运行测试。

   ![展示 Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> 你会注意到 Copilot 执行多项任务，如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，会为所有更改提供统一的 **Keep** 或 **Undo**，因此无需在工作进行中选择它们。

7. 按 Copilot 的提示，选择 **Continue** 运行测试。

   ![展示 Copilot Chat 窗格中对话框的截图，对话框询问用户是否确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会出现一些停顿，甚至有些测试会失败，这没有问题！Copilot 会在代码生成和测试之间反复迭代，直到完成任务且未检测到错误。

   ![展示 Copilot 代理模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 请记住，审阅 Copilot 或任何 AI 工具生成的代码始终很重要。

10. 返回运行网站的浏览器，体验新功能！
11. 确认一切正常并审阅代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！在本练习中，我们探索了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。我们了解了：

- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- Copilot 代理模式如何探索项目、识别相关文件并进行协调一致的更改。
- 如何在合并到代码库之前，审阅 Copilot 代理模式生成的更改和测试。

接下来，[使用 Playwright MCP 服务器测试功能][next-lesson]并为其打开拉取请求。

### 附加探索练习：实现分页

随着游戏列表增长，需要启用分页。运用在本练习中学到的技能，提示 Copilot 更新网站以实现分页。代码方面的一些考虑因素包括：

- 遵循现有的最佳实践，包括使用现有的指令文件。
- 考虑希望如何实现分页，是允许用户选择每页大小，还是写死。
- 编写提示时，确保为 Copilot 提供必要的指导，以按期望的方式实现。
- 可能需要与 GitHub Copilot 迭代，提出更改要求并提供上下文。这是使用 Copilot 的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：作用及使用时机][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
