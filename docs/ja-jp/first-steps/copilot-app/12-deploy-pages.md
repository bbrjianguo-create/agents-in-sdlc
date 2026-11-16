---
title: "レッスン 12 - GitHub Pages でサイトをデプロイする"
description: "GitHub Copilot app で Pages ワークフローを設定し、その pull request をレビューして、公開された Space Quiz を確認します。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

GitHub Pages でデプロイして、Space Quiz を公開します。Copilot はワークフローを準備し、デプロイを支援できますが、変更内容のレビューとサイトが公開されたことの確認は、引き続きあなたの責任です。

このレッスンでは、次の内容を学習します。

- GitHub Pages のデプロイを設定するよう Copilot に依頼する。
- マージする前に、ワークフローと pull request をレビューする。
- デプロイの進行を追い、公開されたサイトを確認する。

## シナリオ

Space Quiz はすでに GitHub リポジトリにありますが、誰でも気軽に試せる方法がまだありません。単一ファイルのサイトを GitHub Pages で公開すれば、サーバーやランタイムの依存関係を追加せずに、公開 URL から利用できるようになります。

## デプロイの準備を Copilot に依頼する

> [!WARNING]
> pull request を作成すると、コードがコミットおよびプッシュされる場合があります。このプロンプトを送信する前に、承認設定と送信先のリポジトリを確認してください。**Approve all** を選択している場合、個別の確認プロンプトは表示されません。

`space-quiz` の Copilot app セッションで、次のプロンプトを送信します。

```plaintext
Prepare a GitHub Pages deployment for this repository. Create a GitHub Actions workflow that builds the website if needed and deploys it to GitHub Pages after changes reach the default branch. Add a README describing the app and how to run it locally if one does not already exist. Create a pull request with these changes and report its link and any required Pages settings or permissions. Do not merge, enable auto-merge, change Pages settings, or deploy. Stop and wait for my review.
```

## ワークフローと pull request をレビューする

1. README が追加されるのは、リポジトリにまだ README がない場合だけであることを確認します。
2. ワークフローのビルドとデプロイの手順、トリガー、権限など、pull request の変更内容をレビューします。
3. ワークフローが、マージ前の pull request ではなく、デフォルトブランチからデプロイされることを確認します。
4. 修正が必要な場合は依頼し、更新された差分を再度レビューします。必須の pull request チェックがすべて成功してから、マージを承認します。

## マージとデプロイを承認する

> [!WARNING]
> 次のプロンプトは、レビュー済みの pull request のマージ、リポジトリの Pages 設定の変更、サイトの公開をエージェントに許可します。レビューを完了し、リポジトリと pull request を確認してから送信してください。承認設定によっては、追加の確認プロンプトなしでこれらの操作が実行される場合があります。

`<PR_NUMBER>` をレビューした pull request の番号に置き換えて、別のプロンプトとして送信します。

```plaintext
I have reviewed pull request <PR_NUMBER> in this repository and authorize its merge and deployment. Enable GitHub Pages using GitHub Actions, merge that pull request only after required checks pass, and monitor the deployment until it succeeds. Report the final URL of the live website. If you lack permission to enable Pages or merge, tell me exactly what I need to approve or do; do not report success until the site is live.
```

![3 段階の図。マージ済みの space-quiz pull request、Build website、Upload artifact、Deploy to Pages の各ステップが成功した GitHub Actions の Deploy website の実行、yourname.github.io/space-quiz/ で公開された Space Quiz を示しています。](../../../_images/first-steps-app-pages-deploy.svg)

## デプロイを確認する

1. GitHub Actions のデプロイの実行が成功するまで見守ります。
2. 報告された URL を開き、Space Quiz が読み込まれることを確認します。

エージェントが Pages を有効にできない場合や pull request をマージできない場合は、エージェントが示す具体的な指示に従ってください。デプロイが成功し、公開されたサイトが読み込まれるまでは、タスクが完了したとは見なさないでください。

## まとめと次のステップ

Space Quiz がライブサイトとして公開されました。エージェントの報告に頼らず、デプロイも自分で確認できました。[レッスン 13: 振り返りと次のステップ][next-lesson]に進みます。

[next-lesson]: ../13-review/
