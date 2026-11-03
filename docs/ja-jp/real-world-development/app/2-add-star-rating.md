---
title: "レッスン 2 - 星評価を追加する: 手早い改善"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認してクイックチャットを試したので、次は**エージェントセッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードには表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の内容を学習します。

- エージェントセッションを開始し、その構成を理解する。
- プロジェクトに小規模で対象を絞った変更を加えるようエージェントに依頼する。
- ワークスペースの差分ビューで変更をレビューする。
- アプリをローカルで実行し、ブラウザーで変更を確認する。
- 最初のプルリクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。一方、ホームページのゲームカードにはタイトル、カテゴリ、発行元、説明しか表示されていません。最初の練習として、エージェントに既存の評価を各カードに表示させます。小さく独立した変更なので、最初のセッションに適しています。

## セッションの構成

**セッション**は、独立したワークスペースで実行されるエージェントとの会話です。各セッションには専用の **git worktree とブランチ**が割り当てられます。そのため、あるセッションで機能を追加し、別のセッションでバグを修正するなど、変更を競合させずに複数のセッションを実行できます。セッションはリポジトリごとにサイドバーにまとめて表示されます。選択するとセッションを切り替えられます。

セッション内には、エージェントとの**会話**、ファイルを調べて編集するエージェントの**ツールアクティビティ**、差分を含む**変更されたファイル**の一覧が表示されます。

## セッションを開始して変更を依頼する

新しいセッションを開始し、プロジェクトの調査と機能の実装に取りかかります。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成して、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリに `tailspin-toys` を選択します。
4. プロンプトボックスの下で **new working tree** と **Interactive** モードを選択し、次のプロンプトを送信します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は、まずプロジェクトの分離されたコピーである新しい worktree を作成します。次にプロジェクトを調べ、新機能を追加するために更新するファイルを見つけ、必要なコードを作成します。これで Copilot app を使って新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、どれほど小さくてもマージ前にレビューしてください。Copilot app 内で変更を確認します。

1. アプリの右上隅にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルとその差分を確認します。

    ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの詳細を表示する中心的なファイル `GameCard.astro` にコードが追加されていることを確認します。評価がある場合は表示し、`starRating` が `null` の場合は "No rating yet" と表示する、小さなブロックになるはずです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot は、すべての生成 AI ツールと同様に決定論的ではなく確率的に動作するため、コードは上記と異なる場合があります。ただし、比較的よく似たものになるはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントが自動実行したチェックの結果をレビューします。数値の `starRating` と、`null` の場合の代替表示の両方をテストしていることを確認してください。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認します。

コードを読むだけで動くと判断せず、Copilot に Web サイトを開いてもらい、更新された UI を確認します。アプリを起動し、ブラウザーキャンバスで開くよう依頼します。

> [!TIP]
> キャンバスは、Copilot app 内で利用できるインタラクティブなウィジェットです。後のレッスンではカスタムキャンバスを調べ、自分でも作成します。ここでは組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトを送信し、アプリを起動してブラウザーキャンバスで開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーウィンドウが開きます。
3. 評価済みのゲームカードに 5 点満点の評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトを使い、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。新しいコードを既存のコードベースにマージするため、プルリクエスト (PR) を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. リポジトリのポリシーにより、PR の作成前に関連する Issue が必要だと Copilot から説明された場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューパネルで PR を開きます。ここで PR をレビューできます。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app を使って最初の変更をリリースしました。このレッスンでは、次の作業を行いました。

- エージェントセッションを開始し、その構成を学習しました。
- ゲームカードに対象を絞った小さな変更を加えるようエージェントに依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- PR 1 を作成し、チェックをレビューして明示的にマージしました。

次は、[絞り込みの Issue から始めて Plan モードと Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でのエージェントセッションの操作][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app での Issue とプルリクエストの管理][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
