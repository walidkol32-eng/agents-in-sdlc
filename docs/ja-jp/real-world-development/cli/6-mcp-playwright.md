---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスを扱い、Copilot が使える自動化機能を追加する必要があります。MCP サーバーを使うと、Copilot CLI に組み込まれている機能を超えて、さらに多くのツールやサービスを利用できます。

このレッスンでは、次の内容を学習します。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを理解します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルター機能を確認します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。ユーザーと同じように作業中の Web サイトを Copilot が使えるようにすると、変更が期待どおりに動作するかを確認する作業をさらに自動化できます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を利用すると、AI エージェントは外部ツールやサービスとリアルタイムに通信し、最新の情報にアクセスしたり、依頼に応じて操作したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。各 MCP サーバーは、エージェントが利用できる異なるツールやリソースのセットを提供します。

よく使われている MCP サーバーには、次のものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API を利用できます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザーの自動操作機能を利用できます。

ほかにも多くの MCP サーバーがあります。GitHub がホストする [MCP レジストリ][mcp-registry]では、エコシステム内のサーバーを見つけたり、コントリビュートしたりできます。

> [!CAUTION]
> MCP サーバーは、プロジェクトで使うほかの依存関係と同じように扱ってください。使用する前にソースコードを確認し、公開元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加します。これで Copilot がユーザーと同じように Web サイトを操作できます。

> [!NOTE]
> Playwright MCP が Copilot CLI ですでに設定されている場合は、このセットアップ手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力して、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を確認するよう Copilot に依頼する

これまでは機能が期待どおり動作することを手動で確認しました。次は、Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は MCP サーバーを明示しなくても、Copilot が自動的に判断します。ここでは使うツールを把握しているので、明示してもかまいません。方向性を示すと結果がより安定し、トークンも節約できます。

2. Copilot がブラウザーを操作して機能を確認する各ステップを表示する様子を見ます。
3. レポートを読み、すべて期待どおり動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止した後、レポートを提示します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認しました。このレッスンでは、次の作業を行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように利用するかを学びました。
- Playwright MCP サーバーがまだ利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルター機能を確認しました。

次は[QA カスタムエージェントを作成し][next-lesson]、スキルとブラウザーツールを専門的な役割にまとめます。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
