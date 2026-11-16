---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、基本操作を確認して、用意されているフィルター機能の Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースの探索、コード生成、コマンド実行、外部ツールとの連携をすべてコマンドラインから行えます。タスクを任せたり、変更を依頼したりしながら、集中を保って作業できます。最初のステップは、想像どおりツールをインストールすることです。幸い、すでによく知っているツールを使って実行できます。

このレッスンでは、次の内容を学習します。

- npm を使って GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーを通じて、フィルター機能の Issue を見つける。

## シナリオ

チームでは、増え続けるバックログに対応するために AI エージェントを使い始めています。Copilot CLI は、その機能を、多くの開発者が日常的に作業するターミナルに持ち込みます。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は、[npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、npm を使用します。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されるはずです。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されるはずです。

## GitHub で認証する

初回起動時に、Copilot CLI は GitHub アカウントでの認証を求めます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、デバイスコードの指示に従って、Copilot CLI を認証し、承認します。
3. Copilot CLI に、次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して、信頼することを回答します。

> [!NOTE]
> Codespace では、GitHub のセッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 基本操作を確認する

通常のシェルプロンプトで入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語がエージェントに送られ、スラッシュコマンドが会話を制御します。

1. `/model` を入力し、方向キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、お使いのバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot に簡単な質問をして、正しく動作していることを確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答する前にリポジトリを調査する様子を確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. フィルター機能の Issue を見つけるよう Copilot に依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開いて、Issue を読みます。最初の簡単な変更を完了した後で使用します。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチで作業します。worktree は自動的には作成されません。変更を加えるたびに、事前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は、通常、確立された権限の範囲外のツールを使用する前に確認を求めます。このワークショップでは、`--yolo` を付けて再起動します。これは、ユーザーが承認するショートカットで、Codespace 内でそれらの承認プロンプトを表示しないようにし、演習に集中できるようにします。

> [!CAUTION]
> `--yolo` は、すべての権限を自動的に有効にします (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`)。Codespace や VM のような、分離された環境でのみ使用し、日常の開発で既定になるようエイリアスを設定することは絶対にしないでください。詳細については、[ツールの使用を許可および拒否する][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` により、後続のレッスンで Issue と pull request の操作に使用する、読み取りと書き込みの GitHub MCP ツールが有効になります。Codespace はローカルコンピューターへのアクセスを制限しますが、認証済みの GitHub のリソースは実際のものです。公開やマージの前に、変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリルートから、再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて、もう一度簡単な質問をして会話が機能していることを確認し、`/exit` で終了します。

Copilot は、会話を自動的に保存します。後で、指示を変更したりエージェントを追加したりした後に、`copilot --resume` を使うと、同じ機能の会話とブランチに戻れます。

## まとめと次のステップ

このレッスンでは、次の作業を行いました。

- npm を使って GitHub Copilot CLI をインストールした。
- GitHub アカウントで認証した。
- ワークショップのリポジトリを信頼し、簡単な会話を試した。
- 組み込みの GitHub MCP サーバーを通じて、フィルター機能の Issue を見つけた。

次は、[最初の目的を絞った変更を開始し][next-lesson]、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI のインストール][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
