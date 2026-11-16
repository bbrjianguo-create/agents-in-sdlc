---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "了解现有的 quality-checks 技能，自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。我们已经手动验证了代码可以运行，并使用说明文件确保其遵循标准。但测试呢？lint 呢？持续集成 (CI) 的其他环节呢？

对于这类任务，**智能体技能**最为合适！技能帮助 Copilot 了解如何正确执行这类操作。

在本课中，你将：

- 了解现有的 `quality-checks` 技能。
- 自定义其结果的格式。
- 重新加载并运行该技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，使结果更易阅读。

## 说明、脚本和资源

智能体技能将可重用的任务说明、可执行脚本和支持资源打包在一起，由智能体按需加载。从本质上说，技能是一个以技能名称命名的文件夹，其中包含一个名为 `SKILL.md` 的 Markdown 文件。该 Markdown 的 frontmatter 包含用于定义技能的名称和描述，正文则概述技能的功能以及应在何时调用。文件夹中还可以包含子文件夹，存放技能调用时使用的脚本和其他资源。

> [!NOTE]
> 技能不要求有额外的文件夹和文件。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有的命令。

技能可以位于项目的 `.github/skills` 文件夹中，成为团队共享和重用的存储库资产，也可以位于用户技能文件夹 `~/.copilot/skills` 中。

## 了解技能

来看看 Tailspin Toys 团队创建的、用于运行单元测试、lint 和类型检查的技能 `quality-checks`。

1. 返回 Codespace。在 Codespaces 编辑器中，打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。描述帮助 Copilot 了解何时调用该技能。
3. 阅读说明，留意它们如何引导 Copilot 完成测试和 lint 流程。
4. 注意该技能目前还没有 **Results output formatting** 部分。

## 更改前先运行技能

技能可以直接通过 Copilot CLI 调用，也可以使用自然语言调用。让 Copilot 运行该技能的三项检查。

1. 返回处于交互模式的筛选对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意末尾的报告。

## 自定义报告

我们希望得到更好的报告，说明运行了什么、是否成功，以及工具实际报告了什么。更新技能来生成这样的报告！

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 将以下部分添加到文件末尾：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改完成，看看实际效果！Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意末尾的报告，并与第一份报告比较。

## 总结与后续步骤

你已自定义并使用了现有的智能体技能。在本课中，你：

- 了解了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了其结果的格式。
- 重新加载并运行了该技能。

这项更改将与筛选功能一起进入功能 PR。接下来，将[通过 Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外的任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表及附带的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
