---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタムエージェント][previous-lesson] |
|:--|

これまでの演習では、ユーザーエクスペリエンスの改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。Cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。このコントロールセンターでは、開いているプルリクエスト (PR) のあるエージェントタスクを一覧できます。実行された操作を確認し、進行中のセッションに指示を追加して、作業の方向を調整できます。

このレッスンでは、次の内容を学習します。

- エージェントページを開き、cloud agent のタスクを監視します。
- 進行中のセッションに追加の機能を依頼します。

## シナリオ

エージェントにハイコントラストモードを作成するよう依頼した後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイルと切り替え機能を更新しているため、両方を実装するのが合理的です。ハイコントラストに加えてライトモードも追加するよう、エージェントの作業を方向付けます。

## Copilot cloud agent のタスクを確認する

GitHub Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインのタスクリストを確認します。次のようなタスクが表示されます。
   - コードのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択して、Copilot が実行した作業を確認します。次のような操作が含まれる場合があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot が作業する環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するための各手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認して、作業をさらに監視できます。

## Cloud agent に指示を追加する

実行中のタスクを確認したので、ハイコントラストモードを作業中の Copilot に、ライトモードも追加するよう依頼します。

1. ハイコントラストモードの追加に関するセッションを選択します。Copilot が付けた名前や作業状態によって、タイトルは異なります。

   ![コントロールセンターのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![エージェントページのタスクで、作業中のセッションへの追加指示ダイアログが強調表示されている](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を受け取り、作業に追加することを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業していたステップを完了した後、新しい依頼をワークフローに追加します。

今回も数分かかります。これまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、Copilot cloud agent のタスクを監視して方向付ける中心的な場所である Copilot agents ページを確認しました。コントロールセンターを使うと、進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、進行中のタスクを方向転換したり追加のガイダンスを伝えたりできます。

このレッスンでは、次の内容を学習しました。

- コントロールセンターとエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに依頼を追加し、機能を拡張しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成したプルリクエストを反復して改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
