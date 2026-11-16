---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を調べるために使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでにも触れたように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、さらに追加の自動化を Copilot が利用できるようにする必要もあります。そこで役立つのが MCP サーバーです。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能を超えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次の内容を学習します。

- Model Context Protocol (MCP) とは何か、Copilot CLI がそれをどのように使うかを理解する。
- Playwright MCP サーバーが利用できない場合は追加する。
- ブラウザーを操作してフィルター機能を調べるようエージェントに依頼する。

## シナリオ

単体テストやエンドツーエンドテストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。作業中の Web サイトを、ユーザーと同じように Copilot に使わせることで、変更の作成をさらに自動化し、更新が期待どおりに動作するという確信を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための手段を提供します。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムで通信できます。これにより、最新の情報にアクセスし、ユーザーの代わりに操作を実行できます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスの橋渡しをする MCP サーバーを通じてアクセスします。各 MCP サーバーは、AI エージェントがアクセスできる、異なるツールとリソースのセットを表します。

よく使われる既存の MCP サーバーには、次のようなものがあります。

- **[GitHub MCP サーバー][github-mcp]**: GitHub のリポジトリ、Issue、pull request を管理するための API へのアクセスを提供します。
- **[Playwright MCP サーバー][playwright-mcp-server]**: Playwright を使ったブラウザー自動化の機能を提供します。

他にも多くの MCP サーバーが公開されています。GitHub は、エコシステムの発見しやすさと貢献を促進するために、[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクトの他の依存関係と同じように扱ってください。使用する前に、ソースコードをレビューし、公開元を確認し、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、Copilot がユーザーと同じように Web サイトを操作できるよう追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP が設定されている場合は、これらのセットアップ手順をスキップして、次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを <kbd>Enter</kbd> を押して確認し、<kbd>Tab</kbd> を押します。
5. 次の内容を **Command** ダイアログに貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して、MCP ダイアログを閉じます。

## Playwright 経由で機能を調べるよう Copilot に依頼する

以前は、機能が期待どおりに動作することを手動で確認しました。今度は、Copilot で利用できる Playwright MCP サーバーを使って、同じ確認を行いましょう。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に指示します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> MCP サーバーを使うよう Copilot に指示する必要は、実はありません。通常は Copilot が自分で判断します。ただし、使うべきものがわかっている場合は、方向を示しておくと一貫した結果を得やすくなり、トークンも少し節約できます。

2. Copilot が、機能の動作を確認するためにブラウザーで実行している各手順を一覧表示する様子を確認します。
3. レポートを読み、すべてが期待どおりに動作していることを確認します。

Copilot は、サーバーを起動し、Playwright で Web サイトを操作し、サーバーを停止して、レポートを提示します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を調べることができました。ここまでの内容を振り返ります。

- Model Context Protocol (MCP) とは何か、Copilot CLI がそれをどのように使うかを学習した。
- Playwright MCP サーバーが利用できない場合は追加した。
- ブラウザーを操作してフィルター機能を調べるようエージェントに依頼した。

次は、スキルとブラウザーツールを組み合わせた専門の役割として、[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ皆が話題にしているのか][mcp-blog-post]
- [Microsoft Playwright MCP サーバー][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
