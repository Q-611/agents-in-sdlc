---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

多くの組織では、技術的負債に悩まされています。解決されていないセキュリティの問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがその例です。GitHub Copilot の cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が終わると、レビュー担当者が確認できるドラフト PR を作成します。面倒な作業を任せて開発プロセスを加速し、より大きな課題に集中できます。

Copilot cloud agent を使って次の内容を学習します。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確に定義した Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託先が、ドキュメントを十分に作成しませんでした。まず、アプリケーションのエクスポートされた関数すべてに TSDoc のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの別のタイトルを「関連ゲーム」として表示します。まだ仕上げた実装は必要ありませんが、UX の受け入れテストに使えるものを求めています。これは現在ブロッカーですが、ほかに優先度の高い Issue があります。

このようなタスクは後回しになりがちです。Copilot cloud agent に割り当てると、開発者が別の作業に取り組む間に非同期で処理させ、後から戻って作業をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub Issue を Copilot に割り当てる][assign-issue]方法があります。割り当てると Copilot は進行状況を追跡するドラフトプルリクエストを作成し、環境を準備して作業を開始します。作業中または完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑なことがあります。[Copilot cloud agent のタスク範囲を明確にする][cloud-agent-best-practices]ようにしてください。AI ペアプログラマーとして扱い、段階を踏んで学習、実験、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイド方法を確認しました。カスタム指示の演習をすでに行っている場合は、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、cloud agent に作業を依頼する前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後で影響を確認できるようにします。

GitHub Web UI または起動中の codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで **Code lacks documentation** Issue を cloud agent に割り当てるとき、生成されたプルリクエストに TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーが反映されていることを確認します。後の演習で PR をレビューするときにも、これらの指示を確認します。

## Copilot cloud agent の開発環境を設定する

コードの作成では、通常、必要な開発ツールと依存関係を使えるように環境の準備とセットアップスクリプトの実行が必要です。Copilot にタスクを割り当てる場合も同様で、Copilot はソフトウェア開発エンジニアのように作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。.github/workflows/copilot-setup-steps.yml に構成する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に実行するツールや依存関係を設定できます。ラボの進行と学習機会を支援するため、このワークフローはあらかじめ設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードを適用した SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限の権限を設定します。Copilot の操作には専用のトークンが付与されます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は `contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを使えるように、ローカル SQLite データベースをマイグレーションしてシードします。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub ワークフローファイルと同じ形式ですが、次の点に注意してください。

- `copilot-setup-steps` という 1 つのジョブが含まれます。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーによって、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、ワークフローが正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、まったくなかったりするプロジェクトが多くあります。この種の技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメント作成が得意なため、cloud agent に割り当てるのに適した Issue です。バックグラウンドで必要なドキュメントを生成します。後の演習で作業をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot が PR に TODO リストを表示するまで数分かかることがあります。Copilot は環境を作成し (前述のワークフローを実行し)、プロジェクトを分析して、問題への最適な取り組み方を判断しています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するために、ブラウザーを更新する必要がある場合があります。

16. ライブセッションをスクロールして作業を確認します。Copilot がコードを調べて状態を理解し、アプローチを計画し、コードを作成する様子を確認できます。

これには数分かかることがあります。Copilot cloud agent の主な目的は、非同期にタスクを実行し、ほかの作業に取り組めるようにすることです。この機能を使ってもう 1 つのタスクを cloud agent に割り当て、その間にアプリケーションに機能を追加しましょう。

## ゲーム詳細ページに関連ゲームを追加する

デザインチーム向けに関連ゲームの表示を追加する作業は短時間で終わるかもしれませんが、その時間をほかの作業に使えます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue ダイアログを開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Issue への Copilot の割り当て](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が Issue の最初のコメントに 👀 を表示し、作業中であることを示します。

![Issue に Copilot が目の絵文字を表示し、作業中であることを示す](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア開発エンジニアと同じように動作するため、常に監視する必要はありません。作業が完了したらレビューします。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent を使うと、Issue を割り当てて非同期に作業させることができます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などを依頼できます。

この演習では、次の内容を学びました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う方法を確認する。
- 範囲を明確に定義した Issue の重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
