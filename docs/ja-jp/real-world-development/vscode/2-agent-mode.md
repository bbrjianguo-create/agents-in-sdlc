---
title: "演習 2 - Copilot エージェントモードで新機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの最も単純な更新でも、通常は複数のファイルの更新と、テストの実行などの操作が必要になります。開発者の作業は、必要なファイルをすべて探し、変更を加え、テストを実行し、デバッグし、見落としたファイルを特定し、再び更新する、というように延々と続きます。

ここで役立つのが、Copilot エージェントモードです。

Copilot エージェントモードは、IDE でより自律的に動作するよう作られています。開発者に似た動きをし、まず既存のプロジェクト構造を調査し、必要な更新を実行し、テストなどのタスクを実行して、見つかった誤りを自動的に修正します。エージェントモードを使って、サイトに新機能を導入する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は、2 種類の異なる体験向けに作られています。エージェントモードは IDE 内でタスクを実行するため、素早いフィードバックのサイクルとやり取りが可能です。cloud agent は、ペアプログラマーとして設計されており、チームのメンバーのように非同期で作業し、Issue や pull request を通じてやり取りします。

この演習では、次の内容を学習します。

- Copilot エージェントモードが、プロジェクトを調査し、関連するファイルを特定して、協調した変更を加える方法。
- GitHub Copilot エージェントモードが、UI とデータレイヤーにまたがって新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えるにつれて、ユーザーがカテゴリーとパブリッシャーで絞り込めるようにしたくなります。前の演習でパブリッシャーのヘルパーはすでに追加しました。ここでは、残りのデータレイヤー、UI、テストの作業を Copilot エージェントモードで仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを調べて、現在の機能を把握しましょう。

この Web サイトは、開発者向けテーマのボードゲームのクラウドファンディングプラットフォームです。ユーザーは、ゲームを一覧表示し、その詳細を確認できます。Web サイトは単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。ページは、フロントマターで Drizzle ORM を通じてローカルの SQLite データベースに直接クエリを実行します。個別のバックエンド API やクライアント側の UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、対話機能は、標準の DOM API を使った、範囲を限定した小さな Astro の `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるように、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で、次の手順で実行できます。

1. codespace に戻ります。前の演習の後なので、`main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内で新しいターミナルウィンドウを開きます。
3. このフィルター機能の作業用に、新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して、Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のように URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザーウィンドウで開くよう求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して、Web サイトを開きます。

> [!NOTE]
> codespace を使用している場合、Codespace のターミナルで localhost の URL のリンクを選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは、Web サーバーをホストしている codespace への、プライベートなトンネルです。

### Web サイトを調べる

Web サイトが実行されたら、その機能を調べられます。Web サイトの主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧を、タイトル、画像、説明とともに表示します。
- **ゲーム詳細ページ**: ゲームを選択すると詳細ページが開き、タイトル、説明、パブリッシャー、カテゴリーなど、そのゲームの詳しい情報を確認できます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な箇所で、各手順が説明します。

Web サイトの初期実装は機能していますが、新しい機能を追加して強化したいと考えています。まず、バックログを確認しましょう。テンプレートからプロジェクトを設定したときに、Issue のバックログが自動的に作成されています。GitHub Copilot に、その項目を表示するよう依頼します。

1. Chat ビューのエージェントのドロップダウンから **Agent** を選択します。**Agent** は、ファイルをまたいだ変更を自律的に計画して実装し、ターミナルコマンドを実行し、ツールを呼び出します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作者は、特定のモデルを推奨しているわけではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にそのモデルを記載しています。得られるコードの提案が比較的一貫したものになり、良い体験につながることを期待しています。ただし、LLM は確率的に動作するため、得られる提案がワークショップの記載と異なる場合があります。これはまったく正常で、想定された動作です。

> [!NOTE]
> LLM は確率的に動作するため、Copilot が別の MCP コマンドを使用する場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、ユーザーにとって最も役立つと考える項目に基づいて、優先順位まで付けてくれることに注目してください。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot の作業にコンテキストを提供する指示ファイルを確認しておきましょう。[ユーザーインターフェース (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) のファイルを使用します。このファイルには、Web サイトに機能を追加する際の進め方に関するコンテキストが含まれています。

1. codespace で、`.github/instructions/ui.instructions.md` に移動します。
2. 機能を追加する際の進め方に関する全体的なガイダンスに注目します。次の内容が含まれています。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - 各種ファイルの種類向けの、特定の指示ファイルへのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルでは、他の指示ファイルとプロジェクト内のファイルの両方を参照できます。パスは、指示ファイルの場所からの相対パスです。これにより、再利用、複雑な指示を小さく管理しやすい単位への分割、例やテンプレートの提供が可能になります。

## フィルター機能を実装する

フィルター機能を完成させるには、アプリケーションに対して、少なくとも 3 つの更新が必要です。

- データレイヤー (`src/lib/`) に、残りのフィルターのロジックを追加または改善する
- フィルターの動作に関するテストを追加または更新する
- ゲーム一覧ページを更新して、フィルターの UI を導入する

さらに、すべてをコードベースにマージする前に、テストを実行して成功させる必要があります。Copilot エージェントモードなら、これらのタスクを代行できます。機能を追加しましょう。

1. 現在の Copilot との会話を続けることも、**New Chat** を選択して新しい会話を開始することもできます。
2. **Add Context**、**Instructions** を選択し、指示ファイルとして **ui** を選択します。

   ![UI の指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで、引き続き **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルとして、引き続き **Claude Sonnet 4.5** が選択されていることを確認します。
5. 次のプロンプトを使い、バックログの関連する Issue に基づいて機能を実装するよう Copilot に指示します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot が、まずプロジェクトを調査し、目的の機能に関連するファイルを見つける様子を確認します。データレイヤーのヘルパーと UI、そしてテストの両方が見つかります。その後、ファイルの変更とテストの実行を開始します。

   ![Copilot がプロジェクトのファイルを調査している様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot は、プロジェクトの調査、ファイルの変更、テストの実行など、複数のタスクを実行します。タスクとコードベースの複雑さによっては、数分かかる場合があります。その間、コードエディターに **Keep** と **Undo** のボタンが表示されることがあります。Copilot の作業が完了すると、すべての変更に対する **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot の指示に従って、**Continue** を選択し、テストを実行します。

   ![テストの実行を承認するようユーザーに求める、Copilot Chat ペインのダイアログを示すスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 処理の途中で、一時停止したり、一部のテストが失敗したりすることがあります。問題ありません。Copilot は、タスクが完了してエラーが検出されなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了したチャットセッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに、問題の可能性がないか確認します。

> [!CAUTION]
> Copilot やその他の AI ツールが生成したコードは、常にレビューすることが重要です。

10. Web サイトが実行されているブラウザーに戻り、新しい機能を試します。
11. すべてが動作することを確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードを使って、Tailspin Toys の Web サイトに新しい機能を追加する方法を確認しました。次のことを学習しました。

- GitHub Copilot エージェントモードが、UI とデータレイヤーにまたがって新機能を実装できること。
- Copilot エージェントモードが、プロジェクトを調査し、関連するファイルを特定して、協調した変更を加えられること。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

次は、[Playwright MCP サーバーで機能をテスト][next-lesson]し、そのための pull request を作成します。

### ボーナス演習 - ページングを実装する

ゲームの一覧が増えると、ページングを有効にする必要が出てきます。この演習で学んだスキルを使い、サイトにページングを実装するよう Copilot に指示してください。コードについて、次の点を考慮します。

- 既存の指示ファイルの使用を含め、既存のベストプラクティスに従います。
- ページングをどのように実装するか、ユーザーがページサイズを選択できるようにするか、固定にするかを検討します。
- プロンプトを作成するときは、望む実装を Copilot が作成できるよう、必要なガイダンスを提供します。
- GitHub Copilot と反復的にやり取りし、変更を依頼してコンテキストを提供する必要がある場合があります。これは、Copilot を使う際の通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
