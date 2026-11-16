---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使う際は、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを確実に参照できるようにしておく必要があります。Copilot に特定のコンテキストを共有する方法はいくつかあります。中でも重要なのが[指示ファイル][instruction-files]で、コード生成に関するガイダンスを提供する手段です。

この演習では、次の内容を行います。

- プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメントの標準が、リポジトリのカスタム指示とパスを限定した指示ファイルを通じて Copilot にどのように渡されるかを確認します。
- *現在の*指示のもとで、フィルター機能の最初のデータスライス (パブリッシャーのヘルパー) を生成します。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用される新しい標準を追加します。
- 同じプロンプトをもう一度実行し、生成されるコードが新しい標準に従う様子を確認します。
- 次の演習で Copilot が更新後のガイダンスを使えるように、指示の更新とフィルター機能のスライスを `main` にコミットします。

> [!CAUTION]
> 生成されるコードは、設定した標準の一部から外れる場合があります。Copilot は非決定的です。この演習の目的は、指示の更新後に動作が変わる*傾向*を確認することであり、出力を 1 文字ずつ一致させることではありません。

## 指示ファイル

### シナリオ

優れた開発チームの例に漏れず、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには、常に単体テストが必要です。
- UI はダークモードで、モダンな印象にします。
- ドキュメントは、TSDoc のドキュメントコメントの形式でコードに追加します。
- 各ファイルの先頭に、そのファイルの内容を説明するコメントブロックを追加します。

指示ファイルを使うことで、Copilot がこれらのプラクティスに沿ってタスクを実行するための適切な情報を持てるようにします。

### カスタム指示

カスタム指示を使うと、Copilot にコンテキストと好みを伝えられるため、Copilot があなたのコーディングスタイルと要件をよりよく理解できます。これは強力な機能で、より関連性の高い提案やコードスニペットを得るために Copilot を誘導できます。好みのコーディング規約、ライブラリ、コードに含めたいコメントの種類まで指定できます。指示は、リポジトリ全体向けにも、特定の種類のファイル向けにタスクレベルのコンテキストとしても作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md`: リポジトリに対する**すべての**リクエストで Copilot に送信される、単一の指示ファイルです。このファイルには、プロジェクトレベルの情報、つまりチャットや CLI で Copilot に送信されるほとんどのリクエストに関連するコンテキストを記述します。使用している技術スタック、構築中の内容の概要、ベストプラクティス、その他の全体的なガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイルの種類向けに作成できます。特定の言語 (TypeScript や Astro など) や、UI コンポーネントや新しい単体テストの作成といったタスク向けのガイドラインを提供できます。

> [!NOTE]
> IDE で作業している場合、指示ファイルは Copilot Chat でのコード生成にのみ使用され、コード補完や次の編集の提案には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コードを生成する際に、リポジトリレベルのファイルと `*.instructions.md` ファイル (`applyTo` フロントマター付き) の両方を使用します。
>
> また、Copilot は、AGENTS.md や CLAUDE.md ファイルなど、[他の標準を使った指示ファイルにも対応しています][custom-instructions-support]。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成に関する詳細な議論は、このワークショップの範囲外です。ただし、サンプルプロジェクトに用意されている例は、代表的な方法を示しています。概要は次のとおりです。

- `copilot-instructions.md` の指示は、構築中の内容の説明、プロジェクトの構成、全体的なコーディング標準など、プロジェクトレベルのガイダンスに絞ります。
- `*.instructions.md` ファイルは、ファイルの種類 (単体テスト、Astro コンポーネント、データレイヤー) や特定のタスク向けの指示に使用します。
- 自然な言葉で、明確に記述します。コードがどうあるべきか (そしてどうあるべきでないか) の例を示します。

指示ファイルの作成方法は、AI の使い方と同じく、1 つだけではありません。試行錯誤を通じて、プロジェクトに最適な方法が見つかります。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトには、充実した指示ファイルのコレクションが必要です。このプロジェクトのファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまな種類のタスク向けのファイルがあることがわかります。
>
> Copilot に指示ファイルの生成を任せることもできます。方法はサーフェスごとに異なります (たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init`)。必要な箇所で、使用しているサーフェスのレッスンが説明します。
>
> テンプレートや出発点をお探しの場合は、[awesome-copilot][awesome-copilot] をご覧ください。指示ファイル、カスタムエージェント、その他のリソースが豊富にそろったリポジトリです。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれている指示ファイルを読んでみましょう。中核となる `copilot-instructions.md` が 1 つと、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. ファイルを確認し、プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションに注目します。**Code standards** の下にある、入れ子になった **GitHub Actions Workflows** のガイダンスにも注目してください。これらは、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて、中身を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに対する指示があることがわかります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドに注目してください。これは、指示を適用するファイルを決める、(リポジトリルートからの相対の) glob を設定します。ここでは、あらゆる TypeScript のテストファイル (たとえば `**/*.test.ts` に一致するもの) が対象になります。
5. このプロジェクトの単体テストの作成に固有の指示に注目します。
6. 最後に、`.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。他の指示ファイル (`unit-tests.instructions.md` など) とプロジェクト内の既存ファイルへのリンクに注目してください。これにより、大きな指示セットを再利用可能な小さなファイルに分割し、コード生成時に従うべき例を Copilot に示せます。(そこでのパスは、リポジトリルートではなく、指示ファイルからの相対パスです。)

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションには、プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須になっていません。次の手順で、TSDoc ドキュメントコメントとファイルコメントヘッダーのルールを追加します。

## 変更用のブランチを作成する

ベストプラクティスに従って、変更用のブランチを作成しましょう。

1. 前の演習で使用した codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前*に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な箇所で、各手順が説明します。

カスタム指示の影響を確かめるために、まず現在の指示ファイルのままプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 以前の演習で開いたエディターのタブをすべて閉じ、必要なコンテキストだけが Copilot に渡るようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に伝えます。
3. Copilot がファイルの変更を適用できるように、Chat ビューのエージェントのドロップダウンから **Agent** を選択します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調査してコードの更新を適用します。多くの場合、ヘルパーファイルとそのテストにまたがります。
6. 提案されたヘルパーが、第 1 引数に `db` クライアントを受け取り、型付きのパブリッシャーの配列を返す、型付きの関数であることに注目します。これは、`.github/instructions/drizzle.instructions.md` (`src/lib/*.ts` に適用) のデータレイヤーの規約によるものです。
7. 提案されたコードに、TSDoc ドキュメントコメントとファイルレベルのコメントヘッダーが**含まれていない**ことに注目します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合でも問題ありません。指示の更新後に*一貫性*が向上することが、この演習の要点です。

## リポジトリの新しい標準を追加する

前述のとおり、`.github/copilot-instructions.md` は、プロジェクトレベルの情報を Copilot に提供するためのものです。リポジトリのコーディング標準を文書化して、コードの提案を改善しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関するルールがまだないことに注目してください。生成されたヘルパーにドキュメントコメントがなかったのは、これが理由です。
3. 既存の標準のすぐ下に、次の Markdown を追加して、ファイルコメントヘッダーと TSDoc ドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルは、全体的なガイダンス用にリポジトリレベル (`.github/copilot-instructions.md`) で作成することも、特定の言語、ファイルの種類、タスク向けに `*.instructions.md` ファイルとして作成することもできます。今追加したドキュメントコメントのルールのような、プロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して、バッファーをクリアします。
2. `src/lib/publishers.ts` を選択して、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが付いていることに注目します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることに注目します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot を誘導して新しいプロジェクト標準に従わせ、次の演習の土台となる実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルは、リポジトリ内の他のアセットと同じように、他の項目と同じソース管理の方法で管理します。ブランチをコミットして、リポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新と、ヘルパーの土台のために Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルで、ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチをプッシュしたら、pull request を作成し、バックログにあるドキュメント標準の Issue に関連付けます。手動でも行えますが、プロジェクトにすでに接続されている GitHub のツールを使って、Copilot に代行させることもできます。Issue を探し、その Issue をクローズする PR を作成し、マージするよう Copilot に依頼しましょう。

> [!NOTE]
> これは、Copilot があなたの代わりに GitHub 上で操作を行う最初の演習です。プロジェクトテンプレートはすでに GitHub のツールをワークスペースに接続しているため、設定は不要です。Copilot が初めてツールを使用するとき、VS Code から GitHub へのサインインを求められる場合があります。プロンプトに従って許可してください。この接続の仕組み (Model Context Protocol) は、後の演習で学習します。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関連する Issue を探し、現在のブランチから、指示の更新と新しいパブリッシャーのヘルパーの土台の両方を説明する PR を作成するよう、Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue の検索と PR の作成を開始します。
5. Copilot があなたの代わりに GitHub の操作を実行する許可を求められたら、コマンドを確認し、適切であれば **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使い、PR をマージして、ブランチを main に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot があなたの代わりに GitHub の操作やシェルコマンドを実行する許可を求められたら、コマンドを確認し、適切であれば **Allow** を選択します。

GitHub Copilot の助けを借りて、pull request を作成してマージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat を使って、次の作業を行いました。

- コード生成のプロンプトを送信し、*既存の*指示で Copilot が生成する内容を確認した。
- `.github/copilot-instructions.md` に、リポジトリ全体に適用される新しい標準を追加した。
- 同じプロンプトを再実行し、提案されるコードが新しい標準に従う様子を確認した。
- 次の演習の土台となるように、指示の更新とヘルパーの土台を `main` にコミットした。

次は、Copilot がコードベース全体に新機能を追加する [エージェントモード][next-lesson]で、これらの指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズのための指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot - 指示ファイルなどのリソースのコレクション][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
