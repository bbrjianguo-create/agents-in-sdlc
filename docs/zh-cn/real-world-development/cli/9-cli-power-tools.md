---
title: "第 9 课 - GitHub Copilot CLI 中的斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话以及可选的 cloud agent 委托的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与任何优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多用于交互的斜杠命令。这些命令提供高级功能、幕后信息和额外的配置选项。你已经用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。再来探索几个其他实用的命令。

在本课中，你将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用的模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树和委托给 cloud agent 的可选命令。

## 场景

你已经完成了 CLI 的核心工作流。再来看几项额外的能力：管理上下文、切换模型、共享会话，以及可选地将工作委托给 [Copilot cloud agent][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会触及模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，也可以通过斜杠命令自行查看或压缩。

1. 返回 Codespace，如果 Copilot CLI 尚未打开，请在存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 留意模型、当前 token 用量，以及上下文在系统说明、工具、消息和可用空间之间的分配。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> 窗口填满时，Copilot CLI 会自动压缩上下文。想自行选择时机时，请使用 `/compact`。切换到无关任务并希望开始全新对话时，请使用 `/clear` 或 `/new`。

## 选择模型

不同的模型各有所长，不同的开发人员也各有偏好。Copilot CLI 允许列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 浏览可用的模型和用量信息。
3. 保持当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作并分享心得，有助于每个人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享的链接，或发布 GitHub gist。

1. 输入 `/help`，查看所安装版本中的 `/share` 选项。
2. 如果想共享此会话，请选择符合需求的目标，例如使用 `/share file` 导出本地 Markdown。
3. 在发送给他人或发布之前，请先检查导出的内容。会话导出可能包含提示、回复和项目详情。

发布链接或 gist 是可选的。请勿发布团队不打算共享的存储库或对话内容。

## 可选：扩展或委托

核心工作坊已经完成。Copilot CLI 还提供了适用于更大任务的命令：

- `/fleet` 可将相互独立的子任务分配给多个子智能体并行运行。
- `/worktree` 可为单独的任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot cloud agent，它以异步方式工作，并可能打开拉取请求。

这些命令是可选的，因为它们可能创建额外的工作树或远程工作。尝试之前，请先开始一个新的、范围明确的任务，并通过常规工作流审查结果。如果想深入了解异步智能体工作，请继续学习 [cloud agent 工作坊][cloud-workshop]。

## 总结与后续步骤

使用 Copilot CLI 中的斜杠命令，可以配置它、共享会话，并了解幕后发生的事情。在本课中，你：

- 使用 `/context` 和 `/compact` 了解了 Copilot 如何管理对话上下文。
- 使用 `/model` 查看了可用的模型。
- 了解了如何使用 `/share` 导出或共享会话。
- 探索了用于并行工作、工作树和委托给 cloud agent 的可选命令。

Copilot CLI 还有更多斜杠命令等待探索！让我们通过[回顾所学内容][next-lesson]和后续学习步骤，为这段旅程收尾。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
