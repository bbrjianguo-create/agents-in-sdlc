---
slug: ja-jp/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Copilot CLI のモード、カスタマイズ、MCP ツール、pull request の自動化を学びながら、Tailspin Toys の 2 つの変更を構築、検証、提供します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI][about-copilot-cli]** は、ターミナルで GitHub Copilot をエージェント型のコーディングアシスタントとして利用できるようにします。コードベースを探索し、コードを生成し、コマンドを実行し、外部ツールに接続できます。すべてコマンドラインから行えるため、グラフィカルエディターに切り替えずに作業の流れを保てます。

このワークショップでは、Tailspin Toys の一連のワークフローを通して学習します。

1. GitHub Codespaces でプロジェクトを準備し、Copilot CLI をインストールして、基本操作を確認します。
2. 目的を絞った星評価の変更を行い、ブラウザーで確認して、最初の pull request (PR) を手動でマージします。
3. フィルター機能の Issue から始め、Plan モードでアプローチを定義し、Autopilot モードで構築して、Interactive モードでレビューします。
4. リポジトリの指示を更新し、フィルター機能の作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーでフィルター機能を調査します。
7. 品質保証 (QA) 用のカスタムエージェントを作成し、要件、カバレッジ、検証結果のレビューに使用します。
8. フィルター機能の変更全体をレビューし、フィルター機能の PR に Agent Merge を使用します。
9. コンテキスト、モデル、共有、任意の cloud への委任に役立つスラッシュコマンドを確認します。

ワークショップに集中できるよう、PR は 2 つ作成します。星評価と、指示の更新、スキルの更新、QA プロファイル、テストを含むフィルター機能です。フィルター機能と品質のワークフローは、1 つの会話とブランチを共有するため、各ツールを調べながら作業を積み上げていけます。

## レッスン

| レッスン | トピック | 説明 |
| ------ | ----- | ----------- |
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成する |
| [1. Copilot CLI のインストール][ex1] | インストール | Copilot CLI をインストールして認証し、基本操作を確認する |
| [2. 星評価の追加: 簡単な成果][ex2] | 最初の変更 | 既存の評価と null 時のフォールバックを表示し、最初の PR をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューする |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を調べて更新し、フィルター機能に適用する |
| [5. quality-checks スキルのカスタマイズと使用][ex5] | 再利用可能なチェック | 既存のスキルを調べ、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーでの観察 | CLI で MCP を設定し、フィルター機能の動作を確認する |
| [7. QA エージェントの作成と使用][ex7] | 要件とカバレッジ | 専門プロファイルを作成して選択し、最終的な検証結果を収集する |
| [8. 機能の PR の作成とマージ][ex8] | レビューとマージ | 変更全体をレビューし、PR を作成して、Agent Merge を使用する |
| [9. GitHub Copilot CLI のスラッシュコマンド][ex9] | CLI 機能 | コンテキスト、モデル、共有、cloud agent への任意の委任を確認する |
| [10. まとめと次のステップ][ex10] | まとめ | ワークフロー、再利用可能なカスタマイズ、追加のリソースを確認する |
| [オプション: Foundry を組み込む][foundry] | ホスト型エージェント | モデルを準備し、カタログに基づいたエージェントをデプロイして、Web サイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] リポジトリと Codespace を作成する権限
- [ ] ターミナル / コマンドライン操作の基本的な知識

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][student-plan] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれています。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合は、管理者が Copilot CLI を有効にしていることを確認してください。

## はじめる

**[前提条件から始める →][ex0]**

[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[student-plan]: https://github.com/education/students
[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
