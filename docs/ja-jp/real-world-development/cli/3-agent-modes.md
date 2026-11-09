---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で Issue に基づくフィルター機能を構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に小さな機能を追加しました。より大きな変更には、さらにしっかりとしたプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って使えるため、適切な方法で必要な機能を構築できます。このレッスンを含む一連のレッスンでは、Issue から新機能を作り、コードの妥当性と機能の動作を確認して、最終的にプロジェクトへマージする、一般的なエージェント主導の開発プロセスを学びます。

> [!NOTE]
> この機能のワークフローでは、同じ会話とブランチを使います。通常は、扱うファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主要な概念に集中するため、手順を簡略化します。

このレッスンでは、次の内容を学習します。

- GitHub Issue から Copilot の新しい会話を始めます。
- Plan モードで要件を定義します。
- Autopilot モードで新しい機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動で検証します。

この機能の開発を続ける中で、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がゲームをカテゴリーや出版社で絞り込める必要があります。バックログの Issue には機能が説明されていますが、複数カテゴリーを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードでこれらを決めた後、範囲を定めて Autopilot に実装を任せます。

## 背景

AI コーディングエージェントを開発ワークフローに導入しても、基本的な手順は変わりません。むしろ、より重要になります。多くの開発者は、次のような手順で作業します。

1. 実装する内容を説明する Issue から始めます。
2. 実装内容の計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、多くの場合は上記の流れを調整したものです。

この標準的な方法を守ることで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じレビューのプロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントに許可する自律性の度合いを決めます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次の作業に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を作成します。プロジェクトファイルは編集できません。
- **Autopilot**: エージェントが自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能に取り組む前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) から始まることを確認します。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) を会話のコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、機能の実装やタスクの委任を行う前に、採用する方法を決められます。複雑な作業では、時間をかけて計画を立てることをお勧めします。Plan モードに切り替え、Copilot に計画を作成させましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 直前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、最適な方法を検討し、計画を作成します。

3. フィルター機能の動作について質問された場合は、希望に基づいて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 上下にスクロールして計画を確認します。
6. 自分の決定と合わない箇所があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せましょう。

Autopilot では、Copilot がタスクを完了したと判断するまで、問題に取り組み続けます。

1. **Accept plan and build on autopilot (recommended)**、またはインストールしているバージョンで同様のラベルの選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画をレビューしてください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と見なす前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードを生成した後、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して動作も確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** が表示されていないことを確認します。
2. `/diff` を入力し、フィルター機能の実装とテストを確認します。
3. 結果を Issue と計画時の決定に照らし合わせます。
4. コードをレビューしたら、<kbd>Esc</kbd> を押して差分表示を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードに問題がないように見えても、実際に動くでしょうか。前回と同じようにアプリを起動して、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. Plan で合意したカテゴリーと出版社のフィルター、およびその組み合わせを試します。
4. リセットと結果が空になる場合の動作が、Issue と決定した内容に一致することを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

会話モードを切り替えて、機能を作成しレビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から Copilot の新しい会話を始めました。
- Plan モードで要件を定義しました。
- Autopilot モードで新しい機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は[カスタム指示を使って][next-lesson]、コードが文書化された開発手法に従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
