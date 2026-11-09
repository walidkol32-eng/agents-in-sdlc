---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、目的を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を記述すると、Copilot がその作業をより効果的に実行できます。チームの専門知識を、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングに従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントなど、再利用可能なエージェントとしてまとめられます。

カスタムエージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、その後にエージェントの動作、専門性、指示を定義する Markdown プロンプトを記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルやほかのエージェントを調整できます。このラボのアクセシビリティカスタムエージェントは、アクセシビリティのガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエストワークフローのスキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの機能と同様に、環境やシナリオに合わせて試し、反復してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のユーザーフィードバックで、テキストと背景色のコントラストが不十分なため、現在のダークテーマが読みにくいと感じるユーザーがいることが分かりました。この課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装します。カスタムエージェントを使って機能を生成しましょう。

この演習では、次の内容を学習します。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot cloud agent でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに作成されています。Copilot をどのようにガイドするか、内容を確認しましょう。

Codespace に戻り、アクセシビリティ用のタスクを Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して空の Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てのダイアログを開きます。
9. カスタムエージェントの一覧から **Accessibility agent** を選択します。

   ![カスタムエージェントの割り当て画面で、Accessibility が選択されている](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めます。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクや分野に特化した AI アシスタントです。チームの専門知識や標準を再利用できるエージェントにまとめ、Copilot が効率よく作業できるようにします。

この演習では、次の内容を学習しました。

- カスタムエージェントの定義方法。
- タスクをカスタムエージェントに割り当てる方法。

Copilot がハイコントラストモードの実装に取り組んでいる間に、[エージェントのセッションを監視して方向付け][next-lesson]ます。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを利用する準備][org-custom-agents]
- [Enterprise でカスタムエージェントを利用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
