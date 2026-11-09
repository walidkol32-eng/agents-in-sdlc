---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときには、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot にコード生成のガイダンスを伝える主な方法の 1 つが[指示ファイル][instruction-files]です。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `src/lib/publishers.ts` に、フィルター機能の最初のデータ層を作成します。最初は既存の指示を使います。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従う様子を確認します。
- 次の演習で更新したガイダンスを利用できるよう、指示の更新とフィルター機能の一部を `main` にコミットします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確率的です。この演習の目的は、指示を更新した後の傾向の変化を確認することであり、出力のすべての文字を一致させることではありません。

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

- `.github/copilot-instructions.md`: リポジトリに関するすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くのチャットや CLI のリクエストに関係するプロジェクト全体の情報を記載します。技術スタック、開発するものの概要、ベストプラクティス、全体に適用するガイダンスなどが含まれます。
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
2. **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** など、プロジェクトの簡単な説明とセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指示もあります。これらは、このリポジトリで Copilot を使うすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示を確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの相対 glob を指定します。ここでは、`**/*.test.ts` に一致するファイルなど、TypeScript のテストファイルに適用されます。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示を再利用できる小さなファイルに分け、コード生成時に Copilot が参照する例を示せます。ここでのパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント作成はまだ必須ではありません。次の手順では、TSDoc のドキュメントコメントとファイルコメントのヘッダーに関する規則を追加します。

## 変更用のブランチを作成する

ベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディターのタブを閉じ、Copilot が必要なコンテキストだけを使えるようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に伝えます。
3. Chat ビューのエージェントのドロップダウンで **Agent** を選び、ファイル変更を適用できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認してコードを適用します。ヘルパーファイルとテストなど、複数のファイルに変更が及ぶ場合があります。
6. 生成されたヘルパーが、型付けされた関数であることを確認します。最初の引数として `db` クライアントを受け取り、出版社の型付き配列を返します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層の規約によるものです。
7. 生成コードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示の更新後に一貫性が高まることがこの演習の目的です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` には、プロジェクトレベルの情報を記載します。コードの提案を改善するため、コーディング標準を文書化します。

1. `.github/copilot-instructions.md` を開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内のドキュメントについての規則がないため、生成されたヘルパーにはドキュメントコメントがありません。
3. 既存の標準の直後に、ファイルコメントのヘッダーと TSDoc のドキュメントコメントを Copilot に追加させる次の行を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体のガイダンスは `.github/copilot-instructions.md` に、言語、ファイルの種類、特定のタスクへの指示は `*.instructions.md` に記載できます。ここで追加したドキュメントコメントの規則など、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を再度選び、Copilot が正しいファイルを対象にするようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 生成されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot が新しいプロジェクト標準に従うようにして、次の演習で使う実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも、ほかのアセットと同じようにソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. `git status` を実行して、指示とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基礎となる Copilot の変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する Issue を関連付けてプルリクエストを作成します。接続済みの GitHub ツールを使い、Issue を探して、Issue を完了する PR を作成し、マージするよう Copilot に依頼します。

> [!NOTE]
> Copilot が GitHub 上で操作するのは、この演習が初めてです。プロジェクトテンプレートで GitHub のツールがあらかじめ接続されているため、セットアップは不要です。Copilot がツールを初めて使うときに、VS Code で GitHub へのサインインを求められることがあります。表示される手順に従って許可してください。この接続 (Model Context Protocol) の仕組みは、後の演習で学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントの選択欄で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を見つけ、現在のブランチから PR を作成して、指示の更新と出版社ヘルパーの基盤の両方を説明するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を許可するよう求められたら、コマンドを確認して **Allow** を選びます。
6. PR が作成されたら、PR をマージしてローカルブランチを `main` に戻すよう、次のプロンプトを使って Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェルコマンドの実行を許可するよう求められたら、コマンドを確認して **Allow** を選びます。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

指示ファイルから Copilot がコンテキストを得る方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示でコード生成のプロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再送信し、生成コードが新しい標準に従うことを確認しました。
- 次の演習で使えるよう、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は[エージェントモード][next-lesson]を使い、Copilot にプロジェクト全体にまたがる新機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
