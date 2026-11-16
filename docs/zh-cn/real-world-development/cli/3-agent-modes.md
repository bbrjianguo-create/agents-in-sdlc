---
title: "第 3 课 - 智能体模式：计划与 Autopilot"
description: "使用计划模式确定方案，使用 Autopilot 根据 issue 构建筛选功能，并在交互模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们先为项目添加了一个小功能，但更大的更改需要更稳健的流程。好在 GitHub Copilot CLI 可以融入组织现有的工作流，确保以正确的方式构建正确的内容。这是几节课中的第一节，将遵循典型的智能体驱动开发流程：从 issue 开始生成新功能，确保代码有效、功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 在继续完成该功能的整个流程中，将使用同一个对话和同一个分支。通常，不同类型的文件会使用不同的分支或 PR，但这里走一条捷径，以便专注于核心概念。

首先，在本课中，你将：

- 从 GitHub issue 开始一次新的 Copilot 对话。
- 在计划模式下确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成该功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的目录不断增长，访客需要按类别和发行商缩小游戏范围。待办事项中的 issue 描述了该功能，但组合多个类别等细节需要在编码前达成一致。你将使用计划模式敲定这些决定，然后通过 Autopilot 授权一次有边界的实现。

## 背景

将 AI 编码智能体引入开发流程不会改变基本原则，这些原则反而更加重要！大多数开发人员的工作流类似于：

1. 从描述待办工作的已创建 issue 开始。
2. 制定需要构建内容的计划。
3. 构建并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码经过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体细节因团队和组织而异，但大多是上述流程的变体。

坚持这一标准方法，可以确保 AI 生成的代码满足既定需求，并与手写代码经过同样的审核流程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在各模式间切换：

- **Interactive（交互）**：你和智能体协同工作。智能体提出更改建议，并在继续前等待你的输入。
- **Plan（计划）**：智能体先制定计划，且不能编辑项目文件。
- **Autopilot（自动驾驶）**：智能体自主工作：编写代码、运行测试并迭代，直至任务完成。

先从计划模式开始，审查计划，然后使用 Autopilot 实现。

## 从 issue 开始

开始筛选工作之前，先返回 Codespace，确认存储库和终端已准备就绪。

1. 返回 Codespace。如果已停止，请先重新启动再继续。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd>，打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用箭头键高亮标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将该 issue 添加到提示中，并返回 **Session** 选项卡。

注意，提示现在以 `#7`（或类似编号）开头。`#` 可以将 GitHub 上的 issue 或拉取请求 (PR) 引入上下文。

## 规划筛选功能

规划让你在交给 Copilot 之前，先确定实现功能或执行任务的方法。对于任何复杂的工作，花一点时间规划总是好主意。切换到计划模式，让 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到计划模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的 issue 引用之后，输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 开始制定计划！它会先探索项目，然后确定最佳方案。

3. 过程中，Copilot 可能会询问筛选功能应如何工作。请根据自己的偏好回答，这里没有错误答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动，审查计划。
6. 如果计划中有与你的决定不一致的部分，请让 Copilot 修改。

## 批准 Autopilot

计划已写好并经过审查，可以开始实现了！使用 Autopilot 让 Copilot 来完成。

Autopilot 允许 Copilot 持续迭代，直到它认为问题已解决。

1. 选择 **Accept plan and build on autopilot (recommended)**，或所安装版本中类似标签的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按既定计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，与其他代码一样，需要先审查再合并。审查代码并运行网站，确保一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入交互模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 将结果与 issue 以及规划期间做出的决定进行对比。
4. 代码审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并让 Copilot 解决所有失败项。

## 体验新功能

代码看起来不错，但能运行吗？像之前一样启动应用，并通过 Codespaces 转发的端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及在计划中约定的组合筛选。
4. 确认重置和无结果时的行为符合 issue 以及你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结与后续步骤

你已使用不同的对话模式构建并审查了一项功能。在本课中，你：

- 从 GitHub issue 开始了一次新的 Copilot 对话。
- 在计划模式下确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，通过[使用自定义说明][next-lesson]深入了解代码是如何生成的，并确保其遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
