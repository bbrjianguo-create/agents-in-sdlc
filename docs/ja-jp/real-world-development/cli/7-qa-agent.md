---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルター機能の体験を確認しました。今度は、これらの機能を、明確に定義された QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次の内容を学習します。

- カスタムエージェントが、指示、スキル、MCP ツールとどのように連携するかを確認する。
- 再利用可能な品質保証のプロファイルを作成して確認する。
- QA エージェントを選択し、フィルター機能の Issue に照らして指摘事項をレビューする。

## シナリオ

Tailspin Toys では、pull request (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫した方法でレビューしたいと考えています。カスタムエージェントを使えば、その QA プロセスを調整し、再利用可能なレポートを提供できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown のプロファイルで定義される、Copilot の特化したバージョンです。プロファイルには、エージェントの目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまで使ってきたカスタマイズには、それぞれ異なる役割があります。リポジトリの指示は、チームの標準を記述します。`quality-checks` スキルは、繰り返し実行するチェックをパッケージ化します。Playwright MCP は、ブラウザーツールを提供します。QA プロファイルは、これらの機能を使って要件を評価し、指摘事項を報告する方法を Copilot に伝えます。これらを置き換えるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。このプロファイルでは、QA が実施するチェックと、従うべき境界の両方を定義します。

1. Codespace に戻り、フィルター機能の会話が Interactive モードであることを確認します。
2. 既定のエージェントに、新しいカスタムエージェントの作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、プロファイルをレビューして、意図した QA ワークフローと境界を Copilot が反映していることを確認します。これにより、機能の検証だけを望んでいるときに、不完全または範囲の広すぎるエージェントが機能を変更してしまうことを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを読みます。`description` は必須です。`name` は省略可能ですが、指定するとエージェントの表示名が明確になります。
3. プロファイルの指示を読み、QA が要件から始め、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が裏付けとなる証拠を報告し、実装コードを変更する前に確認を求め、変更のコミットや pull request の作成を行わないことを確認します。
5. 生成されたプロファイルにこれらの責務や境界が不足している場合は、先に進む前に、既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は、会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後に同じフィルター機能の会話を再開し、QA を選択すると、すでにコンテキストにある Issue と計画時の決定内容を QA が利用できます。

1. 次のプロンプトで、エージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成したばかりの場合、利用可能なエージェントの一覧に表示されないことがあります。エージェントは存在しており、上記のコマンドで有効になります。

2. 次のプロンプトを使い、機能をレビューするよう QA エージェントに依頼します。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、提示されたレポートを読みます。

## まとめと次のステップ

ワークフローに再利用可能な専門の役割を追加し、その作業をレビューしました。このレッスンでは、次の作業を行いました。

- カスタムエージェントが、指示、スキル、MCP ツールとどのように連携するかを確認した。
- 再利用可能な品質保証のプロファイルを作成して確認した。
- QA エージェントを選択し、フィルター機能の Issue に照らして指摘事項をレビューした。

これで、実装、スキルの更新、QA プロファイル、テスト、検証レポートがそろい、レビューできる状態になりました。次は、[それらを機能の PR にまとめ、Agent Merge を使用します][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
