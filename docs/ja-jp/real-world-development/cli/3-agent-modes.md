---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan でアプローチを決め、Autopilot で Issue に基づく絞り込み機能を構築し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトに小さな機能を追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の作業手順に合わせて動作できるため、適切なものを適切な方法で構築できます。このレッスンから、エージェント主導の一般的な開発プロセスに沿って進めます。Issue から機能を作成し、コードの妥当性と機能の動作を確認して、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能開発のワークフローでは、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中できるよう簡略化しています。

このレッスンでは、次の内容を学習します。

- GitHub Issue から Copilot の会話を始める。
- Plan モードで要件を定義する。
- Autopilot モードで新しい機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動で検証する。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して QA エージェントを作成し、機能の PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えてきたため、利用者がカテゴリや発行元でゲームを絞り込めるようにします。バックログの Issue には機能が記載されていますが、カテゴリを組み合わせる方法など、コーディング前に合意しておく必要のある詳細があります。Plan モードで決定し、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発ワークフローに加えても、基本は変わりません。むしろ、より重要になります。多くの開発者は次のような流れで作業します。

1. 実装内容を記載した Issue から始めます。
2. 何を構築するか計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> チームや組織によって具体的な手順は異なりますが、ほとんどの場合は上記の流れを多少変更したものです。

この標準的なアプローチに従うことで、AI が生成したコードが要件を満たし、手作業で書いたコードと同じ検証プロセスを通るようにします。

## 会話モード

**会話モード**によって、エージェントに与える自律性の程度を制御できます。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントはまず計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントは自律的にコードを書き、テストを実行し、タスクが完了するまで反復します。

最初に Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

絞り込み機能の作業を始める前に、Codespace とターミナルの準備を確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、絞り込み用のブランチを作成します。

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
8. 矢印キーを使って **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトが `#7` (または同様の番号) で始まることを確認します。`#` を使うと、GitHub 上の Issue やプルリクエスト (PR) をコンテキストに含められます。

## 絞り込み機能を計画する

計画を立てると、Copilot に任せる前に機能の実装方法やタスクの進め方を決められます。複雑な作業では、時間をかけて計画を立てることをお勧めします。Plan モードに切り替えて計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** であることを確認します。
2. 前の手順で追加した Issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて、最適なアプローチを決めながら計画を作成します。

3. 絞り込み機能の動作について質問された場合は、希望に応じて回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を上下にスクロールして確認します。
6. 決定した内容と合わない部分があれば、Copilot に計画の修正を依頼します。

## Autopilot を承認する

計画を作成して確認したら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot がタスクを完了したと判断するまで反復して作業します。

1. **Accept plan and build on autopilot (recommended)** (またはインストール済みバージョンで同様の名前の項目) を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** であることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まることがあるため、先に計画を確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と見なす前にセットアップの問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないか検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** でないことを確認します。
2. `/diff` を入力して、絞り込みの実装とテストを確認します。
3. 結果を Issue および計画時に決めた内容と照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に解決を依頼します。

## 新しい機能を試す

コードを確認できました。次は動作を確認します。前と同じようにアプリを起動し、Codespaces のポート転送を使ってサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で決めたカテゴリ、発行元、組み合わせの絞り込みを試します。
4. リセットと該当結果がない場合の動作が、Issue と決定内容に合っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の作業を行いました。

- GitHub Issue から Copilot の会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新しい機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動で検証しました。

次は、カスタム指示を使ってコード生成がドキュメント化されたプラクティスに従うようにします。[カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI のコマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
