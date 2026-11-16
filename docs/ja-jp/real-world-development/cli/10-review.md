---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つの pull request、再利用可能なカスタマイズ、参考リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローを通して、GitHub Copilot CLI を使いました。具体的には、次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを調査し、用意されているフィルター機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果をレビューして、最初の pull request (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで方針を定義し、Autopilot モードで構築し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、その後、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックの実行に使用しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を調べるために使用しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの証拠を評価しました。
- フィルター機能の変更全体をレビューし、フィルター機能の PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、オプションのクラウドへの委任に使うスラッシュコマンドを確認しました。

## 作成した成果物

このワークショップには 2 つの PR のマイルストーンがあり、それぞれ更新済みの `main` から作成した専用のブランチで作業します。

1. **星評価:** 既存の `starRating` と、評価がない状態の明示的な表示を、ゲームカードに表示します。
2. **フィルター機能と品質のワークフロー:** フィルター機能を実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを含めます。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使用しました。ワークショップを効率よく進めるため、その作業を 1 つの PR にまとめています。

## さまざまな種類の検証

コードは、自動テスト、ブラウザーでの自分自身による確認、MCP を通じた Copilot のブラウザー探索など、複数の方法で確認しました。`quality-checks` スキルは、単体テスト、lint、型チェックを実行し、新しい形式で報告しました。QA は、PR の作成前に、それらの結果を要件とテストカバレッジのレビューとあわせて評価しました。

追加するテストは、実際の不足を埋めるものである必要があります。新しいテストが不要だった QA の実行結果が、正しい場合もあります。マージを承認する前に、コードと証拠をレビューし、変更後は影響を受ける証拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の質を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズの変化に合わせて調整してください。指示は標準を定め、スキルは繰り返し行うタスクを記述し、カスタムエージェントは専門の役割を定義し、MCP サーバーは外部ツールを接続します。エージェントの要約だけでなく、実際の変更とツールの結果をレビューしてください。

**モード**と**モデル**は、タスクに合わせて選択します。構築の前に方針を検討するには **Plan**、焦点を絞った変更で自分も関与し続けるには **Interactive**、範囲が明確なタスクには **Autopilot** を使用します。日常的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選びます。

コンテキストは、インフラと同じくらい重要です。*何を*、*なぜ*、*どのように*構築したいのかを明確に伝えることで、出力は大きく変わります。

## さらに探求する

中心となるワークフローは一通り学習しました。他にも、試す価値のある CLI の機能がいくつかあります。

- `/review`: コードレビューエージェントに変更の分析を依頼します。
- `/rubber-duck`: 問題を言葉にして整理し、別の視点を得ます。
- `/fleet`: 独立したサブタスクを並列でオーケストレーションします。
- `/worktree`: 別のタスクを分離します。
- `/delegate`: タスクを Copilot cloud agent に送信します。

## 次のステップ

どのツールでも、上達する最善の方法は使い続けることです。実際の業務のコードでも、趣味のコードでも、何年も作りたいと思っていながら着手できていない小さなアプリでも使ってみてください。学んだことをチームと共有し、チームからも学びましょう。そして、いつものようにドキュメントも参照してください。

ターミナルから Tailspin Toys を拡張したい場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]をご覧ください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
