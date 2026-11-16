---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。打开拉取请求之前，应先确认它在浏览器中确实可用。与其自己在应用中逐项点击，不如连接 **Playwright MCP 服务器**，让 Copilot 驱动真实的浏览器替你测试功能，然后发布分支并打开 PR。

在本练习中，你将：

- 了解模型上下文协议 (Model Context Protocol, MCP) 是什么，以及 MCP 服务器如何通过新工具扩展 Copilot，
- 将 Playwright MCP 服务器添加到工作区，
- 让 Copilot 使用它在浏览器中手动测试筛选功能，
- 发布分支并为筛选工作打开拉取请求。

## 什么是模型上下文协议 (MCP)？

当代理模式能够触及编辑器之外时，会变得强大得多。模型上下文协议 (MCP) 正是 Copilot 实现这一点的方式，它是代理与外部工具和服务通信的标准方法。

![示意图，展示代理模式的内部工作方式，以及它如何与上下文、LLM 和工具交互，其中包括由 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)为 AI 代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时与外部工具和服务通信，从而获取最新信息（通过资源），并代表你执行操作（通过工具）。

这些工具和资源通过 MCP 服务器访问，它充当 AI 代理与外部工具和服务之间的桥梁。MCP 服务器负责管理 AI 代理与外部工具（如现有 API 或 NPM 包等本地工具）之间的通信。每个 MCP 服务器代表 AI 代理可以访问的一组不同的工具和资源。

几个常用的现有 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：该服务器提供用于管理 GitHub 存储库的一组 API 访问，使 AI 代理可以执行创建新存储库、更新现有存储库以及管理 issue 和拉取请求等操作。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：该服务器使用 Playwright 提供浏览器自动化能力，使 AI 代理可以执行导航到网页、填写表单和点击按钮等操作。

还有许多其他 MCP 服务器提供对不同工具和资源的访问。GitHub 托管了一个 [MCP 注册表](https://github.com/mcp)，以提升发现性并促进生态系统贡献。

> [!CAUTION]
> 在安全方面，对待 MCP 服务器应像对待项目中的任何其他依赖项一样。使用 MCP 服务器之前，请仔细审查其源代码、核实发布者，并考虑安全影响。只使用信任的 MCP 服务器，并谨慎授予对敏感资源或操作的访问权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置此 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 应该会看到已为你配置好的 `github` 服务器：

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

这个 `github` 条目随项目模板提供，所以 Copilot 才能在上一个练习中读取你的 issue 待办事项。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，因此无需在本地安装任何内容：VS Code 通过 HTTP 连接到它，并在 Copilot 首次使用其工具时使用 GitHub 进行验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供一个可控制的浏览器，这正是测试功能所需要的。

1. 在 `.vscode/mcp.json` 中，在 `github` 旁边添加 `playwright` 条目，使文件如下所示：

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

与远程的 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 通过运行 `npx @playwright/mcp@latest` 在你的计算机上启动它。`--headless` 标志让 Playwright 在没有可见窗口的情况下运行浏览器，这在没有桌面可供显示的 codespace 中是必需的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此 Playwright 所需的浏览器通常已经安装。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

## 启动并信任服务器

VS Code **按需**启动 MCP 服务器：Copilot 首次需要某个服务器的工具时，VS Code 会启动它，并要求你确认是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会出现 **Start** 操作。选择它可立即启动服务器，并确认配置正确。
2. 如果 VS Code 要求确认是否信任该服务器，请审查配置，并选择信任，以便服务器启动。

手动启动是可选的。如果跳过，VS Code 会在下一步 Copilot 首次需要其工具时启动服务器（并提示你信任它）。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供了一个可驱动的真实浏览器。无需你在应用中逐项点击来检查工作，代理可以打开页面、导航、应用筛选并将结果反馈给你，然后总结它看到的内容。这是在不离开对话的情况下，确认功能是否按预期工作的最快方式。

在底层，Playwright MCP 服务器基于页面的[无障碍树][playwright-mcp-server]而非屏幕截图工作。这意味着代理像辅助技术一样，针对结构化、带标签的元素（按钮、链接、列表项）进行推理，因此一次快速的功能检查同时也是一次轻量的无障碍合理性检查。

服务器连接且应用正在运行后，让 Copilot 测试你刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，逐步执行并反馈发现。请对照 issue 中的验收标准阅读它的总结。如果有异常，请继续提问，或让它先修复代码，再打开拉取请求。

> [!NOTE]
> 此测试要求应用在 `http://localhost:4321` 运行。如果已停止开发服务器，请先重新启动，再发送提示。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器。如果它报告缺少浏览器，请让它运行 `npx playwright install chromium` 后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以打开拉取请求 (PR)，让团队审阅。第一步是发布 `filtering-vscode` 分支。

1. 打开 codespace 中的 **Source Control** 面板，查看 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![展示 Source Control 面板中所做更改的截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

创建拉取请求有多种方式，包括通过 github.com 和 GitHub 命令行界面 (CLI)。但既然已在使用 GitHub Copilot，就让它来创建 PR 吧！它可以找到相关 issue，并创建与之关联的 PR。

1. 打开 Copilot Chat 面板，选择 **New Chat** 开始新会话。
2. 确保在代理下拉列表中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 为你创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行收集信息和操作所需的任务。首次使用 GitHub 工具时，可能会提示使用 GitHub 验证，请按提示允许。
5. 注意 Copilot 如何搜索 issue、找到正确的 issue 并创建 PR。
6. 选择 Copilot 生成的链接查看拉取请求，但**请先不要合并**。

## 总结与后续步骤

恭喜！在本练习中，你：

- 了解了模型上下文协议 (MCP) 是什么，以及 MCP 服务器如何通过新工具扩展 Copilot，
- 将 Playwright MCP 服务器添加到工作区，
- 在交付之前，使用它在浏览器中手动测试了筛选功能，
- 发布了分支，并为筛选工作打开了拉取请求。

你使用 MCP 服务器来*测试*功能，但 MCP 只是扩展 Copilot 的方式之一。接下来，[创建自定义代理][next-lesson]，简化无障碍审查等专项任务。

## 资源

- [MCP 到底是什么？为什么大家都在谈论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
