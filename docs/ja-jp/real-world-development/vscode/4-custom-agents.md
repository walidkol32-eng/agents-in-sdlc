---
title: "演習 4 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローにおける特定のタスクや分野に特化した AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、目的を絞った指示、ベストプラクティス、コーディングパターン、分野固有の知識を記述すると、Copilot がその作業をより効果的に実行できます。チームの専門知識を、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングに従うセキュリティエージェント、一貫したテストパターンを守るテストエージェントなど、再利用可能なエージェントとしてまとめられます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダーまたはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、その後にエージェントの動作、専門性、指示を定義する Markdown プロンプトを記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を伝えます。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントには独自のコンテキストウィンドウがあり、作業の一部としてスキルやほかのエージェントを調整できます。このラボのアクセシビリティカスタムエージェントは、アクセシビリティのガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエストワークフローのスキルやテストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの機能と同様に、環境やシナリオに合わせて試し、反復してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みに関係なく、すべてのユーザーがクラウドファンディングプラットフォームを利用できるようにしたいと考えています。最近のユーザーフィードバックで、テキストと背景色のコントラストが不十分なため、現在のダークテーマが読みにくいと感じるユーザーがいることが分かりました。このアクセシビリティの課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なため、できるだけ早く実装します。カスタムエージェントを使って機能を生成しましょう。

この演習では、次の内容を学習します。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使い、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは、すでに作成されています。Copilot をどのようにガイドするか、内容を確認しましょう。

Codespace に戻り、ターミナルを開いて、アクセシビリティ作業用の新しいブランチを `main` から作成します。演習 3 のフィルター PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドがある YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。次の内容が含まれます。
   - アクセシブルな Web サイトのコード生成に関する主要な責務。
   - アクセシビリティのベストプラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントのドロップダウンには、`.github/agents` に定義したすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、その指示とツールに基づくチャットセッションを開始できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 次の手順を始める前に、Codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

1. Chat ビューのエージェントのドロップダウンで、**Agent** がまだ選択されていない場合は選択します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. チャットビュー下部にあるエージェントのドロップダウンを選択します。ここには現在のエージェントが表示されます (既定は **default**)。
3. 利用可能なエージェントから **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクトファイルに適用します。変更はファイルパスと差分とともにチャット内に表示されます。
6. 編集が適用されると、**Files changed** の表示が更新され、このセッション中にエージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリの実ファイルを変更し、Astro コンポーネント、CSS、関連テストなどを更新します。

次の演習では、作業中のセッションをレビューし、エージェントの作業に指示を追加します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクや分野に特化した AI アシスタントです。チームの専門知識や標準を再利用できるエージェントにまとめ、Copilot が効率よく作業できるようにします。

この演習では、次の内容を学習しました。

- カスタムエージェントの定義方法。
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認しながら同じセッションにライトモードを追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの設定][custom-agents-config]
- [Awesome Copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
