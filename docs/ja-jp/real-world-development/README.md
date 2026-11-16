---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成されたアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用する方法を学びます。変更の計画、コードの作成、動作のテスト、pull request のレビュー、エージェントによる作業の管理を、反復的に進めます。

## シナリオ

あなたは、開発者向けテーマのボードゲームのクラウドファンディングを提供する架空の企業、Tailspin Toys の新任開発者です。チームのバックログは GitHub Issue としてすでに登録されており、すぐに着手できます。フィルター機能やページネーションなどの機能開発に加えて、アクセシビリティやコーディング標準といった品質改善も含まれています。

## 環境を選ぶ

GitHub Copilot は、普段作業している場所で利用できます。開発スタイルに合った環境を選んでください。各ワークショップは、独自のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを利用します。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを使って作業します。

### [GitHub Copilot app][app]

デスクトップアプリで、並列のエージェントセッションの実行、セッションモードの切り替え、キャンバスでの共同作業、GitHub の Issue と pull request の管理を行います。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期のエージェントに割り当て、作業を誘導し、進捗を監視して、エージェントが作成した pull request をレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を利用します。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを使って作業します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
