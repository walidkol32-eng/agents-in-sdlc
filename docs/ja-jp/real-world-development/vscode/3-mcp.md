---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで動作を確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストし、ブランチを公開して PR を作成します。

この演習では、次の内容を学習します。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- ブラウザーでフィルター機能を手動テストするよう Copilot に依頼します。
- ブランチを公開し、フィルター機能のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にアクセスできると、より強力になります。Model Context Protocol (MCP) は、Copilot が外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードの内部構造と、コンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとのやり取りを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作したりできます。

ツールやリソースは、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じて利用できます。MCP サーバーは、既存の API や npm パッケージなどのローカルツールとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できる異なるツールやリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。AI エージェントは、新しいリポジトリの作成、既存リポジトリの更新、Issue やプルリクエストの管理などを実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動操作機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

さまざまなツールやリソースを提供する MCP サーバーがほかにもあります。GitHub がホストする [MCP レジストリ](https://github.com/mcp)で、サーバーを見つけたりエコシステムにコントリビュートしたりできます。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。使用する前にソースコードをよく確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセス許可には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されています。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot は Issue のバックログを読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できるため、機能をテストできます。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、ファイルが次のようになるようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code が `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright は表示ウィンドウなしでブラウザーを実行します。デスクトップ環境がない Codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストに Playwright を使うため、Playwright に必要なブラウザーは通常すでにインストールされています。Copilot が後でブラウザーが見つからないと報告した場合は、`npx playwright install chromium` を実行して、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて使うときにサーバーを起動し、信頼するかどうかを尋ねます。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するよう求められた場合は、構成を確認して信頼することを選び、サーバーを起動します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを必要としたときに VS Code が起動し、信頼を求めます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントにページを開かせ、移動し、フィルターを適用させて結果を読み上げさせることができます。会話を離れずに機能を確認できる、すばやい方法です。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。エージェントはボタン、リンク、リスト項目など、ラベル付きの構造化された要素を扱います。支援技術と同じように情報を処理するため、簡単な機能確認と同時にアクセシビリティも確認できます。

サーバーを接続し、アプリを実行した状態で、次のプロンプトを Copilot に送信して、作成したフィルター機能を確認します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーを使ってブラウザーを起動し、各手順を実行して結果を報告します。レポートを Issue の受け入れ条件に照らし合わせます。動作に問題があれば、追加で質問するか、プルリクエストを作成する前にコードの修正を依頼します。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で動作している必要があります。開発サーバーを停止した場合は、プロンプトを送る前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときは、ブラウザーのダウンロードが必要になる場合があります。ブラウザーが見つからない場合は `npx playwright install chromium` を実行するよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認したので、チームがレビューできるようプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルを開き、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。ここでは、GitHub Copilot に作成を依頼します。関連する Issue を見つけて PR を作成し、Issue と関連付けられます。

1. Copilot Chat パネルを開き、**New Chat** を選択して新しいセッションを始めます。
2. Copilot が GitHub ツールを使えるよう、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選び、Copilot が情報を集めて操作を実行できるようにします。GitHub ツールの初回利用時に GitHub への認証を求められることがあります。画面の指示に従って許可してください。
5. Copilot が Issue を調べ、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の内容を学習しました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが Copilot にツールを追加する仕組みを学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能を手動テストしました。
- ブランチを公開し、フィルター機能のプルリクエストを作成しました。

MCP サーバーを使って機能をテストしましたが、Copilot を拡張する方法は MCP だけではありません。次は[カスタムエージェント][next-lesson]を作成し、アクセシビリティレビューなど、目的を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
