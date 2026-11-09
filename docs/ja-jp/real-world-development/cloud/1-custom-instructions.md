---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使うときには、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot にガイダンスを伝え、何をしてほしいかだけでなく、どのように実行してほしいかを理解させられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、この演習では指示の更新前後に同じプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、リアルタイムに並べて効果を示すのが難しいためです。この環境の後半で、cloud agent が作成するプルリクエストをレビューし、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のようなものです。

- データ層には常にユニットテストが必要です。
- UI はダークモードで、モダンな見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発手法に沿ってタスクを実行するための情報を提供します。

### カスタム指示

カスタム指示を使うと、コンテキストや好みを Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。命名規則、ライブラリ、コメントの書き方などを指定でき、より適切な提案やコードスニペットを得られます。リポジトリ全体に適用する指示も、特定のファイルやタスクに適用する指示も作成できます。

指示ファイルには、次の 2 種類があります。

- `.github/copilot-instructions.md`: リポジトリに関する**すべて**のリクエストで Copilot に送られる単一の指示ファイルです。多くのチャットや CLI のリクエストに関係するプロジェクト全体の情報を記載します。技術スタック、開発するものの概要、ベストプラクティス、全体に適用するガイダンスなどが含まれます。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイルの種類向けに作成できます。TypeScript や Astro などの言語、UI コンポーネントやユニットテストの作成などのタスクについてガイドできます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの形式の指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法をすべて説明することは、このワークショップの範囲外です。サンプルプロジェクトの例から、一般的な方法を確認できます。

- `copilot-instructions.md` には、開発するものの説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルでは、ユニットテスト、Astro コンポーネント、データ層などのファイルの種類や、特定のタスクについて具体的な指示を提供します。
- 自然な言葉で、明確なガイダンスを書きます。コードの良い例と悪い例を示します。

指示ファイルの作成に唯一の正解はありません。AI の使い方と同様に、試してみることでプロジェクトやシナリオに適した方法が分かります。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトで、指示ファイルを充実させることをお勧めします。このプロジェクトには、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示ファイルがあります。
>
> Copilot に指示ファイルを生成させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、各環境で手順は異なります。このワークショップでは、該当する箇所で説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェントなどがある [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

リポジトリには、中心となる `copilot-instructions.md` と、タスクごとの `*.instructions.md` ファイルが用意されています。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** など、プロジェクトの簡単な説明とセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指示もあります。これらは、Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの相対 glob を指定します。ここでは、`**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、Copilot が参照する例を示せます。ここでのパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順では、TSDoc のドキュメントコメントとファイルコメントのヘッダーに関する規則を追加します。

## リポジトリ全体に適用する標準を追加する

ここで行う編集は、コード内のドキュメントとファイルレベルのコメントヘッダーを要件とするプロジェクト全体の規則を追加することです。Cloud agent は、この環境の後半で割り当てる Issue に取り組むときに、この規則を参照します。

編集の前に、作業用のブランチを作成します。Cloud agent は Issue が対象とするブランチの指示を読みますが、ここでは変更をきれいにコミットします。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトの標準が記載されていますが、コード内のドキュメントに関する規則はありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、ファイルコメントのヘッダーと TSDoc のドキュメントコメントを Copilot に追加させる次の行を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体のガイダンスは `.github/copilot-instructions.md` に、言語、ファイルの種類、特定のタスクへの指示は `*.instructions.md` に記載できます。ここで追加したドキュメントコメントの規則など、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

Cloud agent は、Issue が対象とするブランチの指示ファイルを読みます。次の演習では Issue を Copilot に割り当てると `main` からブランチが作成されるため、cloud agent が指示を利用できるよう、変更を `main` にマージする必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチの作成を省略して、変更を `main` にコミットします。ブランチを使うのは、実際のプロジェクトに近い手順で作業するためです。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。この環境の残りのレッスンで、cloud agent が生成するプルリクエストにその標準が反映されます。

次は[最初の Issue を GitHub Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
