---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルター機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルター機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、1 つのブランチに保存されています。これらをまとめてレビューし、pull request を作成します。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスの管理を任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを、いくつかの PR に分割します。ワークショップを効率よく進めるため、フィルター機能と品質に関するワークフロー全体を 1 つの会話とブランチにまとめており、すべての作業がこの PR に含まれます。

このレッスンでは、次の内容を学習します。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを理解する。
- 機能の変更全体と、検証の証拠を確認する。
- フィルター機能の PR を作成する。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルター機能のワークフロー全体を通じて、Copilot を使って機能の計画、実装、検証を行いました。Tailspin Toys では、残りの PR 作業を自動化しつつ、マージの承認は開発者の管理下に置きたいと考えています。

## Agent Merge の紹介

**Agent Merge** は、pull request をマージするために残っている作業を自動化します。有効にすると、Copilot は PR を妨げている要因に対処します。失敗している継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースを行い、リポジトリで許可されている場合は GitHub の自動マージを有効にします。

ここまでは、**Merge pull request** を自分で選択してきました。Agent Merge は、その役割を引き受けられます。Agent Merge を有効にする前に、作業内容をレビューし、準備ができていると判断してください。

## Agent Merge で PR を管理する

すべてのコードが作成できたので、まとめてレビューし、PR を作成して、残りのプロセスの管理を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力して、エージェントのダイアログを開きます。
3. オプションの一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで、新しい PR を作成します。
5. `/pr agentmerge` で Agent Merge を有効にします。
6. Copilot は、PR の継続的インテグレーションのプロセスを監視します。すべて成功すると、マージを実行します。
7. 「PR #14 was squash-merged successfully.」のようなメッセージが Copilot から表示されることを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリの設定、権限の不足を回避しません。ブロックされた場合は、報告された理由を読み、リポジトリで許可されていれば、レビュー済みのマージを手動で完了してください。

## まとめと次のステップ

コードの生成、コードのテストと検証、そして pull request のプロセスまで、開発プロセスのいくつかの部分を自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学習した。
- 機能の変更全体と、検証の証拠を確認した。
- フィルター機能の PR を作成した。
- レビュー後にのみ Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、オプションのクラウドへの委任に役立つ、[Copilot CLI のスラッシュコマンドをさらに確認します][next-lesson]。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
