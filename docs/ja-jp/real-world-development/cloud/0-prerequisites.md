---
title: "演習 0 - 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な準備をします。Tailspin Toys リポジトリの自分用コピーと [Codespace][codespaces] を作成します。Codespace では指示ファイルを編集し、cloud agent が作成した変更をレビューします。

## ラボ用リポジトリをセットアップする

作成するコードのために、[テンプレートリポジトリ][template-repository]からリポジトリのコピーを作成します。新しいリポジトリには、ラボに必要なファイルがすべて含まれています。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. ラボのリポジトリページで **Use this template** ボタンを選択し、続けて **Create a new repository** を選択して、自分用のコピーを作成します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる組織に新しいリポジトリを作成します。

   ![リポジトリのテンプレート設定を入力する画面](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) を控えてください。後の演習で使用します。

## Codespace を作成する

次に、Codespace を使って演習に取り組みます。

[GitHub Codespaces][codespaces] は、ブラウザーからコードの作成、実行、デバッグを行えるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応する、機能豊富な IDE が用意されています。

1. 作成したリポジトリを開きます。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択する](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい Codespace を作成します。

   ![新しい Codespace を作成する](../../../_images/ex0-create-codespace.png)

Codespace の作成には数分かかりますが、サービスを手動でインストールするよりも短時間で済みます。待っている間に GitHub Copilot の機能を確認しましょう。次の演習でそれらを扱います。

> [!CAUTION]
> 後の演習で Codespace に戻ります。今はブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは Codespace またはローカルの [dev container][dev-containers] で動作します。どちらも必要な前提条件が揃った環境を提供します。ローカルで作業する場合は、クローンしたリポジトリを VS Code で開き、求められたら **Reopen in Container** を選択してください。Codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

ラボ用リポジトリのコピーを作成し、Codespace の作成を始めました。Codespace は、Copilot cloud agent と並行して作業するときに使います。

## 次のステップ

次は cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進んでください。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
