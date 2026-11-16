---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、オプションのクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI ツールの例に漏れず、GitHub Copilot CLI にも、操作用のスラッシュコマンドが数多く用意されています。これらのコマンドは、高度な機能、内部の情報、追加の設定オプションを提供します。`/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドは、すでに使用しました。ここでは、他の便利なコマンドをいくつか確認します。

このレッスンでは、次の内容を学習します。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストをどのように管理するかを確認する。
- `/model` を使い、利用できるモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学習する。
- 並列作業、worktree、cloud agent への委任に使うオプションのコマンドを確認する。

## シナリオ

CLI の中心となるワークフローは完了しました。ここでは、コンテキストの管理、モデルの切り替え、セッションの共有、そして必要に応じた [Copilot cloud agent][about-cloud-agent] への作業の委任といった、追加の機能を確認します。

## Copilot CLI のコンテキストを確認する

規模の大きい、または複雑なタスクに取り組むと、モデルのコンテキストウィンドウの上限に達することがあります。Copilot CLI は、必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使って、自分で確認したり圧縮したりすることもできます。

1. Codespace に戻り、Copilot CLI が開いていない場合は、リポジトリルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、そしてコンテキストがシステム指示、ツール、メッセージ、空き領域にどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話がすでに短い場合は、大きな変化がないこともあります。

> [!NOTE]
> Copilot CLI は、ウィンドウがいっぱいになると、コンテキストを自動的に圧縮します。タイミングを自分で選びたい場合は `/compact` を使用してください。無関係なタスクに切り替えて、新しい会話を始めたい場合は、`/clear` または `/new` を使用します。

## モデルを選択する

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、使用するモデルを一覧表示して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルをそのまま使うか、別のモデルを選択します。または <kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、全員が AI ツールをうまく使えるようになります。`/share` コマンドでは、セッションを Markdown ファイルまたは HTML ファイルにエクスポートしたり、共有可能なリンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、お使いのバージョンで利用できる `/share` のオプションを確認します。
2. このセッションを共有する場合は、`/share file` によるローカルの Markdown へのエクスポートなど、目的に合った出力先を選択します。
3. 誰かに送信したり公開したりする前に、エクスポートした内容を確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は、公開しないでください。

## オプション: 並列化または委任する

中心となるワークショップは完了です。Copilot CLI には、より大きなタスク向けのコマンドもあります。

- `/fleet`: 独立したサブタスクをサブエージェントに分割し、並列で実行できます。
- `/worktree`: 別のタスク用に、分離された Git worktree を作成できます。
- `/delegate`: タスクを Copilot cloud agent に送信できます。cloud agent は非同期で作業し、pull request を作成する場合があります。

これらのコマンドは、追加の worktree やリモートでの作業が発生する可能性があるため、オプションです。試す前に、新しい、範囲が明確なタスクを開始し、通常のワークフローで結果をレビューしてください。非同期のエージェント作業をさらに深く学びたい場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定の変更、セッションの共有、内部で何が起きているかの確認ができます。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストをどのように管理するかを確認した。
- `/model` を使い、利用できるモデルを確認した。
- `/share` でセッションをエクスポートまたは共有する方法を学習した。
- 並列作業、worktree、cloud agent への委任に使うオプションのコマンドを確認した。

Copilot CLI には、他にも多くのスラッシュコマンドがあり、探求できることがまだたくさんあります。最後に、[学んだことを振り返り][next-lesson]、学習を続けるための次のステップを確認して、この旅を締めくくりましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI でのコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
