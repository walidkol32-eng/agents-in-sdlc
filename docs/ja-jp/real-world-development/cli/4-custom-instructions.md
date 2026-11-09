---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント作成の基準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときには、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、作成するコードだけでなく、その構成方法も説明できます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメントの基準を追加して、コードに適用します。

このレッスンでは、次の内容を学習します。

- リポジトリの指示と、パスに応じた指示ファイルがエージェントに適用される方法を確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- コメントでは、コードの内容を繰り返すのではなく、目的やすぐには分からない判断の理由を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数を注入できる場合は、その引数についても記述します。
- 再利用可能な Astro コンポーネントでは、`Props` の契約を説明します。関連するコードを変更した場合は、コメントも最新の状態に保ちます。
- 既存の書式設定と lint のガイダンスを維持します。

指示ファイルを使い、Copilot がチームの開発手法に沿ってタスクを行うための情報を提供します。

## 指示ファイル

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。命名規則、ライブラリ、コメントの書き方などを指定でき、より適切な提案やコードスニペットを得られます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くのリクエストに関連するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスク向けのガイドラインを記載します。

> [!NOTE]
> ほかの形式の指示に対するサポートは、環境によって異なります。特定の形式を使用する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、すでに指示ファイルが用意されています。変更を加える前に内容を確認して、影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングのガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、そのファイルを確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示を適用するファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、Copilot が参照する例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルは出発点として十分ですが、まだ改善できます。TypeScript を新しく生成するときに TSDoc コメントを追加するよう、主要な `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** という見出しのセクションを探します。およそ 35 行目にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話を始めるときにリポジトリの指示を読み込みます。指示を保存した後、機能のコンテキストを保ったままフィルター機能の会話を終了して再開します。

1. フィルター機能の会話を開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートから `game-filters-cli` ブランチで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新した指示を適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい基準を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリの指示と、パスに応じた指示ファイルがエージェントに適用される方法を確認しました。
- コーディング標準に従うよう、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は[再利用可能な `quality-checks` スキルをカスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
