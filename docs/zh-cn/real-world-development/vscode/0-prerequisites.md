---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要先完成环境准备。你将创建 Tailspin Toys 存储库的个人副本，启动一个 [codespace][codespaces] 作为工作环境，并确认 GitHub Copilot Chat 已在编辑器中正常运行。

## 设置实验存储库

为了给即将编写的代码创建一份存储库副本，需要基于[模板][template-repository]创建一个实例。这个新实例会包含实验所需的全部文件，后续练习都会在其中完成。

1. 在新的浏览器窗口中，访问 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上，选择 **Use this template** 按钮创建自己的存储库副本。然后选择 **Create a new repository**。

    ![“Use this template”按钮](../../../_images/ex0-use-template.png)

3. 如果参加的是由 GitHub 或 Microsoft 主办的活动，请按照导师提供的说明操作。否则，可以在已启用 GitHub Copilot 访问权限的组织中创建这个新存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下创建的存储库路径（**organization-or-user-name/repository-name**），后续实验会用到它。

> [!NOTE]
> **待办事项已准备就绪**
>
> 通过模板创建存储库时，系统会自动创建一组 GitHub issue 作为待办事项。整个工作坊都会围绕这些 issue 展开，无需手动新建。

## 创建 codespace

接下来，将使用 codespace 完成实验练习。

[GitHub Codespaces][codespaces] 是一个基于云的开发环境，可以直接在浏览器中编写、运行和调试代码。它提供功能完整的 IDE，并支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

codespace 的创建需要几分钟，但仍然比手动安装所有服务快得多。等待期间，可以先了解 GitHub Copilot 的其他功能，接下来就会用到。

> [!CAUTION]
> 后续练习还会回到这个 codespace。现在先将它保留在浏览器标签页中，不要关闭。

> [!NOTE]
> 本工作坊设计为在 codespace 或本地 [dev container][dev-containers] 中运行。这两种方式都能确保环境已安装顺畅体验所需的全部先决条件。如果更希望在本地运行，请在 VS Code 中打开克隆后的存储库，并在出现提示时选择 **Reopen in Container**——VS Code 会构建与 codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和代理模式

要使用 GitHub Copilot Chat 代理模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。如果使用的是 GitHub Codespace，通常已经安装。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以[从 Visual Studio Code Marketplace 安装][copilot-chat-extension]。也可以在 Visual Studio Code 中打开扩展视图，搜索 **GitHub Copilot Chat**，然后选择 **Install**。

安装扩展后，可能需要使用 GitHub 账户完成验证才能启用。

1. 返回 codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入“Hello world”之类的消息并按 Enter。这样应该会激活 Copilot Chat。
4. 如果尚未验证，系统会提示登录 GitHub 账户，请按照说明完成验证。

    ![Copilot Chat 验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 验证完成后，应该会看到 Copilot Chat 窗口。

## 总结

恭喜，已经创建了实验存储库的副本！同时也开始了 codespace 的创建流程，开始编写代码时会用到它。

## 下一步

让 Copilot 开始工作。继续前往[练习 1 - 自定义指令][next-lesson]，教 Copilot 了解项目的约定。

## 资源

- [GitHub Codespaces 概览][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 快速入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
