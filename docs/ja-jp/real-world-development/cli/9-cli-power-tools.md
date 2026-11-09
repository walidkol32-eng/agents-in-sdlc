---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、クラウドへの任意の委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI にも操作に使えるスラッシュコマンドが用意されています。コマンドを使うと、高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次の内容を学習します。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使い、利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、クラウドエージェントへの委任に使う任意のコマンドを確認します。

## シナリオ

Copilot CLI の中心的なワークフローを終えました。ここでは、コンテキスト管理、モデルの切り替え、セッションの共有などの機能を確認します。また、[Copilot cloud agent][about-cloud-agent] に作業を委任することもできます。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組んでいると、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように割り当てられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分のタイミングで圧縮する場合は `/compact` を使います。関係のない別のタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルによって得意分野が異なり、開発者ごとに好みも違います。Copilot CLI では、利用できるモデルを一覧表示して、使うモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、AI ツールをより効果的に使えるようになります。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown 形式で出力する `/share file` など、用途に合った方法を選びます。
3. エクスポートした内容を、誰かに送信したり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する予定のないリポジトリや会話の内容は公開しないでください。

## オプション: 作業を拡張または委任する

ワークショップの中心的な内容は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクを複数のサブエージェントに分割し、並行して実行します。
- `/worktree` は、別のタスク用に独立した Git worktree を作成します。
- `/delegate` は、タスクを Copilot cloud agent に送信します。エージェントは非同期で作業し、プルリクエストを作成する場合があります。

これらのコマンドを使うと追加の worktree やリモート作業が発生するため、任意の操作です。試すときは、範囲を明確に定めた新しいタスクから始め、通常のワークフローを通じて結果をレビューしてください。非同期エージェントによる作業について詳しく知るには、[クラウドエージェントのワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次の内容を学習しました。

- `/context` と `/compact` を使い、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使い、利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、クラウドエージェントへの委任に使う任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュコマンドがあります。次は、これまでに学んだことを[振り返り、今後の手順を確認します][next-lesson]。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
