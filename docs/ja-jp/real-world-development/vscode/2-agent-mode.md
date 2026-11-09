---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの小さな更新でも、複数のファイルを変更し、テストを実行する必要があることがよくあります。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、問題をデバッグし、更新し忘れたファイルを見つけて、さらに変更します。作業は際限なく続くことがあります。

このようなときに役立つのが Copilot のエージェントモードです。

Copilot のエージェントモードは、IDE 内でより自律的に作業します。開発者と同様に、プロジェクトの構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使って、サイトに新しい機能を追加しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途の機能です。エージェントモードは IDE 内で動作し、すばやいフィードバックとやり取りを可能にします。Cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じて連携します。

この演習では、次の内容を学習します。

- Copilot エージェントモードでプロジェクトを調べ、関連するファイルを見つけ、複数の変更を調整する方法。
- GitHub Copilot エージェントモードで UI とデータ層にまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリーや出版社で絞り込めるようにします。前の演習では出版社ヘルパーを追加しました。次は Copilot エージェントモードを使って、残りのデータ層、UI、テストを仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。Astro アプリケーションはビルド時にページを静的 HTML として生成します。ページは frontmatter で Drizzle ORM を通じてローカル SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス用ヘルパーは `src/lib/` に配置し、インタラクティブな機能は標準 DOM API を使う小規模で限定的な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後なので、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. フィルター機能用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL のバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` のブラウザーウィンドウを開くダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは、Web サーバーをホストしている Codespace へのプライベートなトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認します。主なページは次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、出版社、カテゴリーなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

Web サイトは動作していますが、新しい機能を追加して改善しましょう。まずバックログを確認します。テンプレートからプロジェクトを作成したときに、Issue のバックログも自動的に用意されています。GitHub Copilot に内容を表示させます。

1. Chat ビューのエージェントのドロップダウンで **Agent** を選択します。エージェントは自律的に変更を計画して複数のファイルに適用し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは、特定のモデルを推奨するものではありません。ワークショップの作成時には Claude Sonnet 4.5 を使ったため、指示にも記載しています。提案の一貫性を高め、学びやすくするためです。LLM は確率的に動作するため、説明と異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択してすべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot はユーザーに最も役立つと思われる項目を優先して表示します。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に与える指示ファイルを確認します。Web サイトの機能を追加する際のガイダンスを含む[ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加する際の全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル向け指示へのリンク。

> [!TIP]
> 指示ファイルから、他の指示ファイルとプロジェクトファイルの両方を参照できます。パスは指示ファイルからの相対パスです。指示を再利用し、大きな内容を小さく扱いやすいファイルに分け、Copilot に例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つを変更する必要があります。

- データ層 (`src/lib/`) の残りのフィルター処理を追加または調整します。
- フィルターの動作を確認するテストを追加または更新します。
- ゲームの一覧ページにフィルター UI を追加します。

また、コードベースにマージする前に、テストを実行して成功を確認する必要があります。Copilot エージェントモードにこれらの作業を依頼しましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 関連する Issue の要件に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを見つけて、データ層のヘルパー、UI、テストを更新してテストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、作業に数分かかる場合があります。その間、エディターに **Keep** と **Undo** のボタンが表示されることがあります。作業が終わると、変更ごとに **Keep** または **Undo** を選べるようになるため、作業中に選択する必要はありません。

7. Copilot の指示に従い、**Continue** を選択してテストを実行します。

   ![テストの実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に待機状態になったり、テストが失敗したりすることがあります。Copilot はタスクを完了し、エラーが見つからなくなるまでコード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了したチャットセッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェントモードを使い、Tailspin Toys の Web サイトに機能を追加しました。この演習では、次の内容を学習しました。

- GitHub Copilot エージェントモードで UI とデータ層にまたがる機能を実装する方法。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを見つけて変更を調整する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### ボーナス演習 - ページングを実装する

ゲームの一覧が増えるとページングが必要になります。この演習で学んだことを使い、Copilot にページングを追加するよう依頼します。次の点を検討してください。

- 指示ファイルなど、既存のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかなど、ページングの方式を検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更を依頼したりコンテキストを提供したりして反復する必要がある場合があります。これは Copilot を使うときの通常の流れです。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
