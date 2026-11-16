---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用の GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な準備を整えます。Tailspin Toys リポジトリの自分用コピーを作成し、[codespace][codespaces] を立ち上げます。次のレッスンでは、その統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次の内容を学習します。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成する。
- Codespace を作成し、プロジェクトの準備ができていることを確認する。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーで作業します。[テンプレートリポジトリ][tailspin-template]から、今すぐ作成します。新しいリポジトリには、ラボに必要なすべてのファイルが含まれています。

1. 新しいブラウザーウィンドウで、[Tailspin Toys テンプレート][tailspin-template]に移動します。
2. **Use this template** を選択し、次に **Create a new repository** を選択して、リポジトリの自分用のコピーを作成します。
3. GitHub または Microsoft が主催するイベントの一環としてワークショップを受講している場合は、メンターの指示に従ってください。そうでない場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。
4. 作成したリポジトリのパス (`organization-or-user-name/repository-name`) を控えておきます。ワークショップの後半で参照します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップでは、これらの Issue に沿って作業を進めます。自分で Issue を登録する必要はありません。

ワークショップテンプレートは、新しいコピーを使用してください。リポジトリの指示、アプリケーションコード、テスト、`quality-checks` スキル、使用するバックログが含まれています。古いコピーを使用する場合は、必要なファイルが含まれているかどうかを、ファシリテーターに確認してください。

## Codespace を作成する

次に、Codespace を使ってワークショップを進めます。

[GitHub Codespaces][codespaces] は、ブラウザー上で直接コードの記述、実行、デバッグができる、クラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した、フル機能のエディターを提供します。

1. 新しく作成したリポジトリに移動します。
2. **Code** を選択します。
3. **Codespaces** タブを選択し、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートが、プロジェクトの依存関係、Playwright Chromium、ローカルデータベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と表示された場合は、**Trust Folder & Continue** を選択します。
6. リポジトリルートでターミナルを開き、アプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces からポート `4321` が利用可能になったと通知されたら、**Open in Browser** を選択し、Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> で開発サーバーを停止します。

## まとめと次のステップ

準備が整いました。このレッスンでは、次の作業を行いました。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成した。
- Codespace を作成し、プロジェクトの準備ができていることを確認した。

次は、Codespace に [GitHub Copilot CLI をインストール][next-lesson]し、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからのリポジトリの作成][template-repository]
- [Codespaces のクイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
