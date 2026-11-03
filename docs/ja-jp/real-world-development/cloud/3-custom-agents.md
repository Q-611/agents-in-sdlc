---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた、専門的な AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、指示、ベストプラクティス、コーディングパターン、分野固有の知識を定義します。たとえば、[WCAG][wcag] への準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、一貫したテストパターンを適用するテストエージェントを作成できます。チームの専門知識を再利用可能なエージェントにまとめられます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には、役割の重なる部分があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を指示します。分かりやすく言うと、**カスタムエージェントは作業者、スキルはツール**です。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキル (ほかのエージェントも含む) を調整して作業します。このラボのアクセシビリティ用カスタムエージェントは、アクセシビリティガイドラインに沿ってサイトをレビューし、更新します。その作業の中で、プルリクエストのワークフロー用スキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成方法は 1 つではありません。AI のほかの使い方と同様に、環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントを使って次の内容を学習します。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディングのプラットフォームを使えるようにしたいと考えています。最近のフィードバックによると、ダークテーマのテキストと背景色のコントラストが不十分で、読みづらいと感じる利用者がいます。このアクセシビリティの課題に対応するため、デザインチームからハイコントラストモードの切り替え機能が求められました。

アクセシビリティは重要なため、できるだけ早く実装します。カスタムエージェントを使って機能を生成します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに作成されています。Copilot をどのようにガイドするか確認しましょう。

codespace に戻り、アクセシビリティ用カスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. 必須項目の `name` と `description` を含む YAML フロントマターを確認します。

> [!CAUTION]
> カスタムエージェントには、`name` と `description` を含むフロントマターが必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する主要な責務、アクセシビリティのベストプラクティス、HTML・CSS・JavaScript のコード例、よくある落とし穴が記載されています。

## Issue を作成して割り当てる

Mission control は、この環境のすべてのエージェントを操作する場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加の指示を出せます。まず、ハイコントラストモードの作成タスクを Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択して空の Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントとアクセシビリティエージェントが選択された、cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot はバックグラウンドで作業を開始します。

## まとめと次のステップ

GitHub Copilot の[カスタムエージェント][custom-agents]について学びました。特定のタスクや分野に合わせた専門的な AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot が適切に作業できるようにします。

この演習では、次の内容を学びました。

- カスタムエージェントの定義方法。
- カスタムエージェントへのタスクの割り当て。

Copilot がハイコントラストモードを実装している間に、[mission control からエージェントセッションを監視して方向付けする][next-lesson]方法を確認しましょう。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
