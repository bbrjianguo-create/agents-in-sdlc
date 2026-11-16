---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクやドメインに合わせた、専門の AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルとしてエージェントを定義することで、特定の種類の作業をより効果的に実行できるよう導く、的を絞った指示、ベストプラクティス、コーディングパターン、ドメイン固有の知識を Copilot に提供できます。チームの専門知識を、再利用可能なエージェントとして体系化できます。たとえば、[WCAG][wcag] への準拠を徹底するアクセシビリティエージェント、セキュアコーディングのプラクティスに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントなどです。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門性、指示を定義する Markdown のプロンプトを記述します。

### カスタムエージェントとエージェントスキルの比較

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、論理的に重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に操作の実行方法を伝えます。両者を区別する最も明快な方法は、**カスタムエージェント**が作業者であり、**スキル**がツールであると考えることです。

カスタムエージェントは独自のコンテキストウィンドウを持ち、作業の一環としてスキル (さらには他のエージェント) をオーケストレーションするよう作られています。このラボでは、アクセシビリティのカスタムエージェントが、アクセシビリティのガイドラインに照らしてサイトをレビューし、更新します。その作業の一環として、pull request のワークフローのスキルや、テストを実行および管理するスキルなどを呼び出す場合があります。

> [!NOTE]
> カスタムエージェントの作成に、唯一の「正しい」方法はありません。AI に関するあらゆることと同様に、テストと反復を通じて、環境とシナリオに合った方法を見つけてください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにすることに取り組んでいます。最近のユーザーからのフィードバックで、文字と背景色のコントラストが不十分なため、現在のダークテーマが読みにくいと感じるユーザーがいることがわかりました。このアクセシビリティの課題に対処するため、デザインチームから、ユーザーがオンとオフを切り替えられるハイコントラストモードの実装が依頼されました。

アクセシビリティは非常に重要なため、できるだけ早く実装したいと考えています。カスタムエージェントを使って、この機能を生成します。

この演習では、次の内容を行います。

- 既存のアクセシビリティのカスタムエージェントをレビューします。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティのカスタムエージェントを確認する

アクセシビリティ向けのカスタムエージェントは、すでに用意されています。その内容を確認して、Copilot がどのように導かれるかを理解しましょう。

codespace に戻り、ターミナルを開いて、`main` からアクセシビリティの作業用の新しいブランチに切り替えます (演習 3 のフィルター機能の PR とは別に管理します)。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを持つ YAML フロントマターに注目します。

> [!CAUTION]
> `name` と `description` を持つフロントマターは、カスタムエージェントに必須です。

3. その後、次のセクションを確認します。内容は次のとおりです。
   - アクセシブルな Web サイトのコードを生成する際の中心的な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある落とし穴と間違いの一覧。

## Copilot Chat でカスタムエージェントを使う

VS Code は、`.github/agents` で定義されたすべてのカスタムエージェントを、Copilot Chat ビューの下部にあるエージェントのドロップダウンに表示します。カスタムエージェントを選択すると、チャットセッションを、そのエージェントの指示とツールに限定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な箇所で、各手順が説明します。

1. Chat ビューのエージェントのドロップダウンで、**Agent** が選択されていない場合は選択します。

   ![Chat ビューのエージェントピッカーを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. チャットビューの下部にあるエージェントのドロップダウンを選択します (アクティブなエージェントが表示されており、既定では **default** です)。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトを、アクセシビリティエージェントに送信します。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat が作業を開始します。コードベースを調査し、変更を提案して、プロジェクトのファイルに編集を適用します。各編集は、ファイルパスとレビューできる差分とともに、チャット内にインラインで表示されます。
6. 編集が反映されると、**Files changed** インジケーターが更新され、エージェントが変更した作業セットを確認できます。

> [!NOTE]
> この処理には、数分かかる可能性があります。Copilot はリポジトリに実際の変更を加えています。作業中、Astro コンポーネント、CSS、関連するテストなどの既存のファイルを編集します。

進行中のこの作業のレビューと調整は、次の演習で行います。

## まとめと次のステップ

このレッスンでは、GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクやドメインに合わせた、専門の AI アシスタントです。カスタムエージェントを使うと、チームの専門知識と標準を再利用可能なエージェントとして体系化し、特定の種類の作業をより効果的に実行するよう Copilot を導けます。

次の概念を確認しました。

- カスタムエージェントがどのように定義されるか。
- Copilot Chat のエージェントモードでカスタムエージェントを使用する方法。

次は、[エージェントの作業を監視して調整します][next-lesson]。変更が行われるたびにレビューし、同じセッションでライトモードの切り替えを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
