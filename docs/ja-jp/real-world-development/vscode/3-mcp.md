---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードで、フィルター機能を構築しました。pull request を作成する前に、ブラウザーで実際に動作することを確認する必要があります。アプリを自分で操作して確認する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot をどのように拡張するかを学習します。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルター機能を手動でテストします。
- ブランチを公開し、フィルター機能の作業の pull request を作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にも手が届くようになると、はるかに強力になります。Model Context Protocol (MCP) は、Copilot がそれを実現するための仕組みで、エージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードの内部の仕組みと、コンテキスト、LLM、ツール (MCP サーバーと VS Code 拡張機能が提供するツールを含む) との関係を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための手段を提供します。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。これにより、(リソースを使って) 最新の情報にアクセスし、(ツールを使って) ユーザーの代わりに操作を実行できます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスの橋渡しをする MCP サーバーを通じてアクセスします。MCP サーバーは、AI エージェントと外部のツール (既存の API や、NPM パッケージのようなローカルのツールなど) の間の通信を管理します。各 MCP サーバーは、AI エージェントがアクセスできる、異なるツールとリソースのセットを表します。

よく使われる既存の MCP サーバーには、次のようなものがあります。

- **[GitHub MCP サーバー](https://github.com/github/github-mcp-server)**: このサーバーは、GitHub リポジトリを管理するための一連の API へのアクセスを提供します。AI エージェントは、新しいリポジトリの作成、既存のリポジトリの更新、Issue や pull request の管理などの操作を実行できます。
- **[Playwright MCP サーバー](https://github.com/microsoft/playwright-mcp)**: このサーバーは、Playwright を使ったブラウザー自動化の機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などの操作を実行できます。

他にも、さまざまなツールやリソースへのアクセスを提供する MCP サーバーが数多くあります。GitHub は、エコシステムの発見しやすさと貢献を促進するために、[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点では、MCP サーバーは、プロジェクトの他の依存関係と同じように扱ってください。MCP サーバーを使用する前に、ソースコードを慎重にレビューし、公開元を確認し、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを使用し、機密性の高いリソースや操作へのアクセスを許可する際は慎重に判断してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルは、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されていることを確認します。

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

この `github` のエントリはプロジェクトテンプレートに含まれており、前の演習で Copilot が Issue のバックログを読み取れたのはそのためです。[リモートの GitHub MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールするものはありません。VS Code が HTTP で接続し、Copilot が最初にそのツールを使用するときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できるブラウザーを提供します。これは、機能のテストにまさに必要なものです。

1. `.vscode/mcp.json` で、`github` と並べて `playwright` のエントリを追加し、ファイルを次のようにします。

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

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**のサーバーです。VS Code は、`npx @playwright/mcp@latest` を実行して、お使いのマシンで起動します。`--headless` フラグは、ブラウザーをウィンドウなしで実行するよう Playwright に指示します。これは、表示するデスクトップがない codespace 内では必須です。

> [!NOTE]
> Tailspin Toys のプロジェクトは、エンドツーエンドテストにすでに Playwright を使用しているため、Playwright に必要なブラウザーは通常すでにインストールされています。後で Copilot がブラウザーが不足していると報告した場合は、`npx playwright install chromium` を実行して、もう一度試すよう依頼してください。

## サーバーを起動して信頼する

VS Code は、MCP サーバーを**必要に応じて**起動します。Copilot が初めてサーバーのツールを必要としたときに、VS Code がサーバーを起動し、信頼するかどうかの確認を求めます。

1. `.vscode/mcp.json` の `playwright` エントリの上に、**Start** アクションが表示されます。これを選択すると、サーバーがすぐに起動し、構成が正しいことを確認できます。
2. サーバーを信頼するかどうかの確認を VS Code から求められた場合は、構成を確認し、サーバーを起動できるよう信頼することを選択します。

手動で起動するのは任意です。起動しなかった場合は、次の手順で Copilot が初めてそのツールを必要としたときに、VS Code がサーバーを起動し、信頼するかどうかを確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。作業内容を確認するためにアプリを自分で操作する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を読み取り、確認した内容を要約して報告します。会話から離れることなく、機能が期待どおりに動作することを確認できる、最も速い方法です。

内部的には、Playwright MCP サーバーは、スクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]に基づいて動作します。つまり、エージェントは、支援技術と同じように、構造化されたラベル付きの要素 (ボタン、リンク、リスト項目) に基づいて判断します。そのため、簡単な機能確認が、簡易的なアクセシビリティの確認も兼ねることになります。

サーバーを接続し、アプリを実行した状態で、構築したフィルター機能を試すよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は、Playwright MCP サーバーを通じてブラウザーを起動し、各手順を順に実行して、確認した内容を報告します。報告を、Issue の受け入れ基準と照らし合わせて読んでください。おかしな点があれば、追加の質問をするか、pull request を作成する前にコードの修正を依頼します。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再度起動してください。Copilot が初めて Playwright MCP サーバーを使用するときに、ブラウザーのダウンロードが必要になる場合があります。ブラウザーが不足していると報告された場合は、`npx playwright install chromium` を実行して、もう一度試すよう依頼してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開して pull request を作成する

機能が動作することを確認できたので、チームがレビューできるように pull request (PR) を作成します。最初の手順は、`filtering-vscode` ブランチの公開です。

1. codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して、変更をステージします。
3. **Sparkle** ボタンを使って、コミットメッセージを生成します。

   ![行われた変更を示す Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## pull request を作成する

pull request の作成方法には、github.com や GitHub のコマンドラインインターフェース (CLI) を使う方法など、いくつかあります。ただ、すでに GitHub Copilot を使っているので、PR の作成を任せましょう。Copilot は、関連する Issue を見つけて、それに関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して、新しいセッションを開始します。
2. Copilot が GitHub のツールを使えるように、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. PR の作成を Copilot に依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の収集と操作の実行に必要なタスクを Copilot に許可します。初めて GitHub のツールを使用するときは、GitHub での認証を求められる場合があります。プロンプトに従って許可してください。
5. Copilot が Issue を検索し、適切なものを見つけて、PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択して pull request を確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot をどのように拡張するかを学習した。
- Playwright MCP サーバーをワークスペースに追加した。
- それを使って、リリース前にブラウザーでフィルター機能を手動でテストした。
- ブランチを公開し、フィルター機能の作業の pull request を作成した。

MCP サーバーを使って機能を*テスト*しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティのレビューのような、焦点を絞ったタスクを効率化するために、[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ皆が話題にしているのか][mcp-blog-post]
- [Microsoft Playwright MCP サーバー][playwright-mcp-server]
- [GitHub MCP サーバー][github-mcp-server]
- [GitHub MCP レジストリ][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
