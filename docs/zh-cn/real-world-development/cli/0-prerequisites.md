---
title: "第 0 课 - 先决条件"
description: "创建你自己的 Tailspin Toys 副本，并为 Copilot CLI 工作坊准备 GitHub Codespace。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

开始 Copilot CLI 课程前，需要先完成环境准备。你将创建 Tailspin Toys 存储库的个人副本，并启动一个 [codespace][codespaces]。下一课会使用其中集成的终端来安装并运行 Copilot CLI。

在本课中，你将：

- 基于模板创建你自己的 Tailspin Toys 项目副本。
- 创建 Codespace，并确认项目已准备就绪。

## 设置实验存储库

你将在自己的 Tailspin Toys 项目副本上工作。现在基于[模板存储库][tailspin-template]创建它。新存储库包含实验所需的全部文件。

1. 在新的浏览器窗口中，访问 [Tailspin Toys 模板][tailspin-template]。
2. 选择 **Use this template**，然后选择 **Create a new repository**，创建你自己的存储库副本。
3. 如果参加的是由 GitHub 或 Microsoft 主办的活动，请按照导师提供的说明操作。否则，请在你有权使用 GitHub Copilot 的组织中创建新存储库。
4. 记下创建的存储库路径（`organization-or-user-name/repository-name`），工作坊后续会用到。

> [!NOTE]
> 通过模板创建存储库时，系统会自动为你创建一组 GitHub issue 作为待办事项。整个工作坊都会围绕这些 issue 展开，无需自己创建。

请使用工作坊模板的全新副本。其中包含存储库说明、应用程序代码、测试、`quality-checks` 技能以及要使用的待办事项。如果使用的是较旧的副本，请向讲师确认其中包含所需的文件。

## 创建 Codespace

接下来，将使用 Codespace 完成工作坊。

[GitHub Codespaces][codespaces] 是一个基于云的开发环境，可以直接在浏览器中编写、运行和调试代码。它提供功能完整的编辑器，并支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择 **Code**。
3. 选择 **Codespaces** 选项卡，然后选择 **Create codespace on main**。
4. 等待 Codespace 设置完成。模板会为你安装项目依赖项、Playwright Chromium 和本地数据库。
5. 如果出现 **Do you trust the authors of the files in this folder?** 提示，请选择 **Trust Folder & Continue**。
6. 在存储库根目录打开终端，启动应用程序：

   ```bash
   npm run dev
   ```

7. 当 Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**，并确认 Tailspin Toys 网站已加载。
8. 返回终端，使用 <kbd>Ctrl</kbd>+<kbd>C</kbd> 停止开发服务器。

## 总结与后续步骤

设置完成！在本课中，你：

- 基于模板创建了自己的 Tailspin Toys 项目副本。
- 创建了 Codespace，并确认项目已准备就绪。

接下来，将在 Codespace 中[安装 GitHub Copilot CLI][next-lesson]，并使用 GitHub 账户进行验证。

## 资源

- [GitHub Codespaces 概览][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 快速入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces

