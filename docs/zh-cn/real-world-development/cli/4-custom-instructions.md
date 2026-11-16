---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "了解存储库说明，添加文档标准，并将其应用到筛选代码上。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果某项任务需要以特定方式完成，就应让 Copilot 获得相应的指导。[说明文件][instruction-files]不仅描述需要什么样的代码，还描述代码应如何组织。现在筛选功能已经构建完成，你将了解 Copilot 使用的说明，添加文档标准，并将其应用到代码上。

在本课中，你将：

- 了解存储库说明和路径范围的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

与任何优秀的开发团队一样，Tailspin Toys 有一套开发实践的准则和要求，包括：

- 注释应说明意图和不显而易见的决定，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 记录其用途、参数和返回值，包括存在的可注入 `db` 参数。
- 可重用的 Astro 组件应记录其 `Props` 约定，相关代码更改时注释也应保持最新。
- 保留现有的格式和 lint 指导。

通过说明文件，可以确保 Copilot 拥有正确的信息，按上述实践完成任务。

## 说明文件

自定义说明让你向 Copilot 提供上下文和偏好，使其更好地理解你的编码风格和要求。这项功能很强大，可以帮助引导 Copilot 给出更相关的建议和代码片段。可以指定偏好的编码约定、库，甚至希望在代码中包含的注释类型。可以为整个存储库创建说明，也可以为特定类型的文件创建说明，以提供任务级上下文。

说明文件有两种类型：

- `.github/copilot-instructions.md`：单个说明文件，存储库的**每个**请求都会将其发送给 Copilot。该文件应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md` 文件：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他说明格式及其支持情况因环境而异。依赖某种格式之前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 了解此项目中的自定义说明文件

为便于入门，入门项目中已包含一组说明文件。在做出更改之前，先了解现有内容，以便看到更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中，打开 `.github/copilot-instructions.md`。
3. 浏览该文件，留意对项目的简要描述及其编码指导。这些说明适用于在此存储库中与 Copilot 的每一次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。可以看到针对 Astro 文件、Drizzle 数据层、测试等的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个 glob，用于确定说明适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，留意其中指向其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分为更小的可重用文件，并让 Copilot 参考示例。

## 更新说明文件以符合团队指导

现有文件是个不错的开端，但仍有缺口。修改核心的 `copilot-instructions.md` 文件，确保新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中，找到标题为 **Code formatting guidance** 的部分，大约在第 35 行。
2. 将以下内容添加为该部分的最后一项：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指导

Copilot CLI 在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，这样 Copilot 既能从新的指导开始，又不会丢失功能上下文。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认已检出 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，通读已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结与后续步骤

你了解了 Copilot CLI 如何从说明文件获取上下文，并将新标准应用到功能上。具体来说，你：

- 了解了存储库说明和路径范围的说明文件如何传递给智能体。
- 更新了说明文件，确保遵循编码标准。
- 看到了说明文件对代码的影响。

接下来，将[自定义并运行可重用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源的集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
