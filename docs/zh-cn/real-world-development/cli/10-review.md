---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可重用的自定义功能和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你在一个连贯的 Tailspin Toys 工作流中使用了 GitHub Copilot CLI。你：

- 准备了 Codespace，安装了 Copilot CLI，了解了项目，并找到了预置的筛选 issue。
- 添加了星级评分，在转发的浏览器中审查了结果，并手动合并了第一个拉取请求 (PR)。
- 从筛选 issue 入手，在计划模式下确定方案，在 Autopilot 模式下构建，并在交互模式下审查。
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加了 Playwright 模型上下文协议 (MCP) 服务器，并用它在真实浏览器中探索筛选功能。
- 创建并选择了质量保证 (QA) 自定义智能体，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查了完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索了用于上下文、模型、共享以及可选 cloud agent 委托的斜杠命令。

## 你交付的内容

工作坊有两个 PR 里程碑，每个都基于更新后的 `main` 创建独立分支：

1. **星级评分：** 在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选与质量工作流：** 实现筛选功能，更新说明并将其应用到功能上，自定义 `quality-checks` 报告，创建 QA 配置文件，并包含相关测试。

从规划筛选到打开其 PR，你始终使用同一个对话和分支。为简化工作坊，我们将这些工作合并在一个 PR 中。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、自己在浏览器中检查，以及 Copilot 通过 MCP 进行的浏览器探索。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按你的新格式报告。QA 在 PR 之前，将这些结果与需求和测试覆盖率的审查结合在一起。

新增的测试应弥补真正的缺口；QA 运行后无需新增测试，也可能是正确的结果。授权合并之前，请审查代码和证据，并在更改后刷新受影响的证据。

## 最佳实践

你提供给 Copilot 的上下文和工具决定了它的工作成果。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置文件并配置了 MCP 服务器。可以在不同对话中重用这些自定义功能，并随团队需求变化而调整。说明设定标准，技能描述可重复的任务，自定义智能体定义专家角色，MCP 服务器连接外部工具。请审查实际的更改和工具结果，而不仅仅是智能体的摘要。

让**模式和模型**与任务相匹配。使用**计划**模式在构建前梳理方案，使用**交互**模式在专注的更改中保持参与，对范围明确的任务使用 **Autopilot**。常规编辑选择更快的模型，复杂工作选择能力更强的模型。

上下文与基础设施同样重要。清楚地描述要构建*什么*、*为什么*以及*如何*构建，会显著改变输出。

## 更多探索

你已经掌握了核心工作流。还有一些值得一看的 CLI 功能：

- `/review`：让代码审查智能体分析更改。
- `/rubber-duck`：梳理问题并获得另一种视角。
- `/fleet`：并行编排相互独立的子任务。
- `/worktree`：隔离单独的任务。
- `/delegate`：将任务发送给 Copilot cloud agent。

## 后续步骤

提升任何工具使用水平的最佳方式就是持续使用！把它用于生产代码、业余代码，或者那个你构思多年却一直没有动手的小应用。与团队分享心得，也向他们学习。当然，也要常查阅文档。

如果想在终端中扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列][foundry]。如需比较其他环境，请探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
