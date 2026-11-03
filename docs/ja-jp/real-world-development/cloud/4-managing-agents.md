---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

これまでの 2 つの演習では、Copilot cloud agent にユーザー体験の改善や機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクの進行状況を監視することも重要です。

GitHub.com の[agents ページ][agents-page]など、cloud agent に割り当てたタスクを管理するためのツールがいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。エージェントが行った操作を確認し、進行中のセッションに追加の指示を出すこともできます。

このレッスンでは、次の内容を学習します。

- agents ページで cloud agent のタスクを監視する。
- 実行中のセッションに追加の機能を依頼する。

## シナリオ

ハイコントラストモードの作成をエージェントに割り当てた後、チームはライトモードも追加するよい機会だと考えました。サイトのスタイルと切り替え機能をすでに変更しているため、両方を追加するのが自然です。エージェントにライトモードも実装するよう追加指示を出します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン領域と左側の領域にあるタスクの一覧を確認します。割り当てたタスクとして、コードのドキュメントの更新、ゲームを変更する API の生成、Web サイトへのハイコントラストモードの追加などが表示されます。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。リポジトリのチェックアウト、作業環境の作成、MCP サーバーの設定、割り当てられたタスクを完了するための各種手順などが含まれます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示されるプルリクエスト (PR) 領域も確認します。PR や変更されたファイルを監視できます。

## Cloud agent に追加の指示を出す

ライトモードをハイコントラストモードと同時に追加するよう依頼します。

1. ハイコントラストモードを追加するセッションを選択します。表示されるタイトルは、Copilot が付けた名前や作業状況によって異なります。

    ![Mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を開始したことが分かるメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Agents ページで、セッションへの追加指示ダイアログが表示された cloud agent タスク](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が指示を確認し、作業に取り込むことを確認します。

## Copilot に作業を続けてもらう

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中だった手順を終えた後、新しい依頼を作業の流れに反映します。

これまでと同様に、完了まで数分かかります。ここで少し休憩して、学んだことを振り返りましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視してガイドする中心的な場所である、Copilot agents ページを確認しました。この mission control では、進行中と完了済みのすべてのタスクを追跡し、作業をレビューし、進行中のタスクに追加の指示を出せます。

このレッスンでは、次の内容を学びました。

- mission control と agents ページで cloud agent のタスクを監視しました。
- 実行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は [Copilot が作成したプルリクエストを反復して改善する][next-lesson]方法を確認します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
