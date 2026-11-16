---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选实现、说明更新、技能更新、质量保证 (QA) 配置文件和测试都已保存在同一个分支上。现在该一并审查它们并打开拉取请求了。星级评分的拉取请求 (PR) 是你自己合并的，这次将交由 **Agent Merge** 来管理合并流程。

> [!NOTE]
> 通常，我们会把功能、说明更新、技能更新和 QA 智能体拆分为几个独立的 PR。为简化工作坊，你将完整的筛选和质量工作流保留在同一个对话和分支中，所有这些工作都会进入这个 PR。

在本课中，你将：

- 了解 Agent Merge 是什么，以及它如何自动化合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选 PR。
- 审查之后再启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了一项功能。Tailspin Toys 现在希望自动化其余的 PR 工作，同时让合并授权仍由开发人员掌控。

## Agent Merge 简介

**Agent Merge** 会自动完成拉取请求落地所需的其余工作。启用后，Copilot 会处理阻碍 PR 的问题：修复失败的持续集成 (CI) 检查、回应审查评论、必要时变基，然后在存储库允许时启用 GitHub 自动合并。

到目前为止，你都是自己选择 **Merge pull request**。Agent Merge 可以接手这项职责。请先审查工作并确认已就绪，再启用 Agent Merge。

## 使用 Agent Merge 管理 PR

所有代码都已创建完毕，一并审查，创建 PR，并让 Agent Merge 管理其余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 在选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 会监视 PR 上的持续集成流程。一切成功后，它将执行合并。
7. 确认看到 Copilot 发出类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的批准、分支保护、合并队列、存储库设置或权限缺失。如果被阻止，请阅读报告的原因；在存储库允许时，手动完成经过审查的合并。

## 总结与后续步骤

你已自动化了开发流程的多个环节，包括生成代码、测试和验证代码，以及现在的拉取请求流程。你：

- 了解了 Agent Merge 是什么，以及它如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选 PR。
- 审查之后才启用 Agent Merge，并确认 PR 已合并。

接下来，将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，涵盖上下文、模型、共享以及可选的 cloud agent 委托。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
