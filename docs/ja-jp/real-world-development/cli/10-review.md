---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用できるカスタマイズ、関連リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連のワークフローに取り組みました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みのフィルター機能の Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで方針を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーでの検証結果を評価しました。
- フィルター機能全体をレビューし、その PR で Agent Merge を許可しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作り、2 つの PR を作成します。

1. **星評価**: ゲームカードに、既存の `starRating` と未評価であることを示す表示を追加します。
2. **フィルター機能と品質ワークフロー**: フィルター機能を実装し、指示を更新して機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能の計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、作業全体を 1 つの PR にまとめました。

## さまざまな方法で検証する

自動テスト、自分で行うブラウザーの確認、MCP 経由で Copilot が行うブラウザーの確認など、複数の方法でコードを検証しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、更新した形式で結果を報告しました。QA はこれらの結果に要件とテストカバレッジの確認を加え、PR を作成する前のチェックをまとめました。

追加するテストは、実際に不足しているカバレッジを補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証の証拠を確認してください。変更を加えた場合は、影響する検証をやり直します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに応じて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築の前に方針を検討するときは **Plan**、焦点を絞った変更で常に状況を把握したいときは **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように作るかを明確に説明すると、結果が大きく変わります。

## さらに試す

中心的なワークフローを学びました。Copilot CLI には、ほかにも試せる機能があります。

- `/review` を使って、コードレビューエージェントに変更を分析させます。
- `/rubber-duck` を使って問題を整理し、別の視点を得ます。
- `/fleet` を使って、独立したサブタスクを並行して調整します。
- `/worktree` を使って、別のタスクを分離します。
- `/delegate` を使って、タスクを Copilot cloud agent に送信します。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。本番コードや趣味のコードに使ったり、長年作りたいと思っていた小さなアプリを形にしたりしてください。学んだことをチームと共有し、チームの知見も取り入れましょう。ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの[Foundry Backer Concierge シリーズ][foundry]に進みます。別の環境も確認するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を参照してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
