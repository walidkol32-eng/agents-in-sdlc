---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスの改善と機能追加を目的に、GitHub Copilot と複数のタスクに取り組みました。コードにドキュメントを追加し、デザインチーム向けの関連ゲーム機能を実装し、ハイコントラストとライトモードの切り替えなどのアクセシビリティ機能を追加するよう Copilot に依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善しましょう。

### シナリオ

これまでに何度も説明したように、生成 AI を利用してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。チームメンバーがレビューを始める前に、GitHub Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の提案を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期で監視されずにタスクを実行するため、安全を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセス権は作業に使うブランチ**のみ**に限定されます。
- Cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions のワークフローを実行するには、人間の承認が必要です。
- MCP サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を開くと、ファイアウォールによって GitHub Copilot がブロックされているという警告が表示される場合があります。Copilot は MCP サーバーへの外部呼び出しなど、外部リソースへのアクセスを既定で制限されているため、これは**想定どおり**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com で自分のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。その場合は作業が完了するまで待ちます。数分かかることがあるので、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新たに追加された TSDoc のドキュメントコメントや、その他のドキュメントを含むコードを確認します。詳細は生成結果によって異なります。

   変更を確認するときは、演習 2 の最初に確認したカスタム指示ファイルから適用される TSDoc コメント、TypeScript の規約、ファイルヘッダーに注目してください。

6. 更新内容に問題がないことを確認したら、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションにワークフローが登録されます。問題がなければ、単一の Astro アプリケーションのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot とやり取りする方法は、レビューだけではありません。チームメンバーと同様に、プルリクエストやコードのインラインコメントで Copilot をタグ付けできます。Copilot はコメントを確認し、対応するセッションを開始します。結果は確率的なため、必ず成功する定型的なプロンプトはありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、内容を簡単に説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリケーションの構成を説明する README を作成する。

1. **@copilot** をタグ付けし、生成されたドキュメントの変更をコメントで依頼します。上記の例や、コードベースに追加したいドキュメントについて別の案を使ってもかまいません。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで Copilot に修正を依頼できます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して `main` にマージします。

![PR をレビュー可能な状態にする](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

次は、ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com でリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 更新に問題がないことを確認したら、**Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちになっていることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェックセクションにワークフローが登録されます。問題がなければ、単一の Astro アプリケーションのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **オプション:** Codespace でこのブランチに切り替え、関連ゲーム機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot は関連ゲーム機能を作成しました。前の PR と同様に、Copilot cloud agent に変更を依頼し、反復して改善できます。表示する関連ゲームの数を調整する、コメントヘッダーや TSDoc のドキュメントコメントを追加するなどの依頼が考えられます。指示を更新する**前**にタスクを割り当てたため、指示への準拠を依頼する例としても使えます。PR の **Conversation** タブに新しいコメントを追加すると、Copilot はそれを確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のコントロールセンターで追加を依頼したライトモードの両方が含まれているはずです。

1. GitHub.com でリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 実装をレビューします。特に、次の点を確認してください。
   - モードを切り替える UI コンポーネント。
   - ユーザー設定を保存するための local storage の使用。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。
6. 更新内容に問題がないことを確認したら、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローが承認待ちになっていることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションにワークフローが登録されます。問題がなければ、単一の Astro アプリケーションのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **オプション:** Codespace でこのブランチに切り替え、アクセシビリティ機能を手動でテストできます。Codespace に移動してターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   次にアプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えを試します。ページを再読み込みしても設定が維持されることを確認してください。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに従って機能を実装するよう Copilot をガイドしたことを確認できます。アクセシビリティに関する懸念や改善点があれば、前の PR と同様にコメントで **@copilot** をタグ付けして変更を依頼します。

## オプションの演習 - さらに作業を委任する

実際のバックログ項目をエージェントに任せ、その間に別の作業を進めるのが cloud agent の有効な使い方です。習慣を身につけるために、リポジトリに Issue を追加して Copilot に割り当ててみましょう。次のような案があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス用ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent の環境を完了しました。各レッスンで、次の内容を学びました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し**、後で cloud agent の生成結果にどう反映されるかを確認しました。
- **Issue を Copilot cloud agent に割り当て**、環境の準備、計画、非同期での作業を確認しました。
- **アクセシビリティ用のカスタムエージェントを作成して使い**、ハイコントラストとライトモードの切り替えを追加しました。
- **Copilot agents ページをコントロールセンターとして使い**、作業中にアクセシビリティセッションを監視して方向付けました。
- **cloud agent のプルリクエストをレビューして反復し**、`@copilot` に変更を依頼して、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent の環境を完了しました。引き続き学びたい場合は、ほかの環境のワークショップも確認してください。

- 🖥️ **[VS Code の環境](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP の統合を試します。
- 💻 **[CLI の環境](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、cloud agent へ戻る `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次の方法を試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
