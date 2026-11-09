---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織は、技術的負債に悩まされています。未解決のセキュリティ課題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。時間のかかる作業を委任して開発を加速し、開発者がより重要な作業に集中できるようになります。

Copilot cloud agent を使って、次の内容を学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決すべき技術的負債があります。最初のサイトを作成した委託先は、ドキュメントを適切に整備しませんでした。実際、コードにドキュメントがほとんどありません。最初の取り組みとして、アプリケーションのエクスポート関数すべてに TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーに属する関連ゲームを表示し、ユーザーがゲームを見続けられるようにします。洗練された実装はまだ必要ありません。UX の受け入れテストに使えるものを求めています。この作業はブロッカーですが、ほかに優先度の高い Issue があります。

このようなタスクは優先順位が下がりやすく、Copilot cloud agent に割り当てるのに適しています。Cloud agent が非同期で作業する間、開発者は別のタスクに取り組めます。後から作業結果をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業します。[GitHub Issue を Copilot に割り当てる][assign-issue]など、複数の方法でタスクを依頼できます。割り当て後、Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境を準備してタスクに取り組みます。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 明確な範囲の指示が重要

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。単純に見える操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲を明確にしてください][cloud-agent-best-practices]。AI ペアプログラマーとして、段階的に作業し、学び、試し、状況に応じて調整します。生成 AI を利用しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイダンスに対する役割を学びました。すでにカスタム指示の演習を完了している場合は、`.github/copilot-instructions.md` の動作を確認したはずです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み、後でその影響を確認しましょう。

GitHub の Web UI または起動中の Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントを確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てると、指示ファイルから取得した TSDoc コメント、TypeScript の規約、ファイルヘッダーが PR に反映されます。後の演習で PR をレビューするときに、改めて確認します。

## Copilot cloud agent の開発環境をセットアップする

コードの作成では、誰が作業する場合でも、適切な状態を保つために特定の環境やセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、cloud agent はソフトウェアエンジニアのように作業します。

Cloud agent は、作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定した[セットアップワークフロー][setup-workflow]を作成すると、作業を始める前に必要な開発ツールや依存関係をインストールできます。ラボの進行と学習のため、あらかじめ設定されています。単一の Astro アプリケーション向けに、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションと初期データを投入した SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローと同じ形式ですが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブがあります。Copilot が PR の作業を始める前に、GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。リポジトリの **Actions** タブからワークフローを手動実行できるため、Copilot の作業を待たずにセットアップをテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、古い情報しかない、またはドキュメントがまったくないプロジェクトも多くあります。このような技術的負債が放置されると、生産性が低下し、コードベースの保守や新しい開発者の受け入れが難しくなります。Copilot はドキュメント作成が得意なため、cloud agent に依頼するのに適したタスクです。バックグラウンドで必要なドキュメントを生成します。後の演習で作業結果を確認します。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで、数分かかる場合があります。Copilot は環境を作成して (前述のワークフローを実行)、プロジェクトを分析し、問題に取り組む最適な方法を検討しています。

13. リストと、実行しようとしているタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示を更新するために、ブラウザーをリロードする必要がある場合があります。

16. ライブセッションをスクロールして、Copilot がどのように問題を解決しているかを確認します。コードを調べて状態を理解し、考えて適切な計画を決め、コードを作成する様子を確認できます。

作業には数分かかります。Copilot cloud agent の主な目的は、非同期でタスクを実行し、その間にほかの作業ができるようにすることです。この機能を使って、もう 1 つのタスクを Copilot cloud agent に割り当ててから、アプリの機能を追加する作業に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分けられることです。デザインチーム向けの関連ゲームの表示は短時間で実装できるかもしれませんが、その時間をほかのタスクに使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

その後、Issue の最初のコメントに 👀 が表示され、Copilot が作業を始めたことを確認できます。

![目の絵文字で Copilot が Issue の作業中であることを示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はありません。完了後に結果をレビューします。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使い、Issue を Copilot に割り当てて非同期に作業させる方法を学びました。Cloud agent は、技術的負債の解消、新しい機能の作成、別のフレームワークへのコード移行などに利用できます。

この演習では、次の内容を学習しました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- 範囲を明確に定めた Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

Cloud agent がバックグラウンドで作業している間に、カスタムエージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで学んだカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
