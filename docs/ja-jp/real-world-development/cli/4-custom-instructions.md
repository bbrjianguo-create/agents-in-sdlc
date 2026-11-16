---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメントの標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際は、コンテキストが重要です。特定の方法で作業する必要がある場合は、そのガイダンスを Copilot が利用できるようにしておく必要があります。[指示ファイル][instruction-files]は、*どのような*コードが必要かだけでなく、それを*どのように*構成すべきかも記述します。フィルター機能を構築したので、Copilot が使用した指示を確認し、ドキュメントの標準を追加して、コードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの指示と、パスを限定した指示ファイルがエージェントにどのように渡されるかを確認する。
- 指示ファイルを更新して、コーディング標準が守られるようにする。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

優れた開発チームの例に漏れず、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントは、コードの内容を繰り返すのではなく、意図や自明ではない判断を説明する。
- `db/` と `src/lib/` のエクスポートされた関数には、TSDoc/JSDoc で目的、パラメーター、戻り値を記述する。注入可能な `db` 引数がある場合は、それも含める。
- 再利用可能な Astro コンポーネントには `Props` のコントラクトを記述し、関連するコードを変更したときはコメントを最新の状態に保つ。
- 既存のフォーマットと lint のガイダンスを維持する。

指示ファイルを使うことで、Copilot がこれらのプラクティスに沿ってタスクを実行するための適切な情報を持てるようにします。

## 指示ファイル

カスタム指示を使うと、Copilot にコンテキストと好みを伝えられるため、Copilot があなたのコーディングスタイルと要件をよりよく理解できます。これは強力な機能で、より関連性の高い提案やコードスニペットを得るために Copilot を誘導できます。好みのコーディング規約、ライブラリ、コードに含めたいコメントの種類まで指定できます。指示は、リポジトリ全体向けにも、特定の種類のファイル向けにタスクレベルのコンテキストとしても作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md`: リポジトリに対する**すべての**リクエストで Copilot に送信される、単一の指示ファイルです。このファイルには、ほとんどのリクエストに関連するプロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスク向けのガイドラインを提供するファイルです。

> [!NOTE]
> その他の指示の形式とサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

最初の手助けとして、一連の指示ファイルがスタータープロジェクトにあらかじめ含まれています。変更を加えて影響を確かめる前に、すでにある内容を確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく)、`.github/copilot-instructions.md` を開きます。
3. ファイルを確認し、プロジェクトの簡単な説明とコーディングのガイダンスに注目します。これらの指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどに対する指示があることがわかります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドに注目してください。これは、指示を適用するファイルを決める glob を設定します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクトファイルへのリンクに注目します。これにより、大きな指示セットを再利用可能な小さなファイルに分割し、従うべき例を Copilot に示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

すでにあるファイルは良い出発点ですが、まだ不足があります。中核となる `copilot-instructions.md` ファイルを変更して、新しく生成される TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` で、**Code formatting guidance** というセクションを探します。35 行目付近にあります。
2. そのセクションの最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。更新した指示を保存したら、フィルター機能の会話を終了して再開します。これにより、機能のコンテキストを失わずに、新しいガイダンスが適用された状態で Copilot を開始できます。

1. フィルター機能の会話がまだ開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は、フィルター機能の会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを読みます。新しく生成された TSDoc コメントに注目し、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次の作業を行いました。

- リポジトリの指示と、パスを限定した指示ファイルがエージェントにどのように渡されるかを確認した。
- 指示ファイルを更新して、コーディング標準が守られるようにした。
- 指示ファイルがコードに与える影響を確認した。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot - 指示ファイルなどのリソースのコレクション][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
