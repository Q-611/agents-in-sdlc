---
slug: ja-jp/real-world-development/app
title: "GitHub Copilot app"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** は Copilot CLI を基盤とするデスクトップアプリケーションで、エージェント主導の開発を単一の作業用ワークスペースで実現します。並列エージェントセッション、切り替え可能なセッションモード、共有キャンバス、GitHub Issue とプルリクエストのネイティブ管理機能を備えています。さらに、リベース、レビューのフィードバック、継続的インテグレーション (CI) の修正、マージまでプルリクエストを導く **Agent Merge** も利用できます。

このワークショップでは、Tailspin Toys の一連のワークフローに沿って作業します。

1. プロジェクトを準備し、アプリをインストールしてリポジトリを接続し、ワークスペースと用意されたバックログを確認します。
2. 星評価の小さな変更を加え、ブラウザーで確認して、最初のプルリクエスト (PR) を手動でマージします。
3. 絞り込みの Issue から始め、**Plan** モードでアプローチを決め、**Autopilot** モードで構築して、**Interactive** モードでレビューします。
4. リポジトリの指示を更新し、絞り込みの作業に適用します。
5. 既存の `quality-checks` スキルをカスタマイズし、プロジェクトのチェックを実行します。
6. Playwright Model Context Protocol (MCP) サーバーを追加し、ブラウザーで絞り込みを確認します。
7. 品質保証 (QA) のカスタムエージェントを作成し、要件、テスト範囲、検証の根拠を確認します。
8. 絞り込みの変更全体をレビューし、2 つ目の PR に Agent Merge を使います。
9. キャンバスが利用できるか確認してから、Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストします。必要な機能を利用できない場合は、ブロッカーを記録し、読み取り専用の代替手段を使うか、まとめのレッスンに進みます。

このワークショップでは、PR を 2 つ作成します。星評価の PR と、指示の更新、スキルの更新、QA プロファイル、テストを含む絞り込みの PR です。それぞれ、更新済みの `main` から開始します。絞り込みと品質のワークフローでは、1 つのセッション、worktree、ブランチを共有するため、ツールを学びながら作業を積み重ねられます。最後のキャンバス演習はそのセッション内で行い、PR ワークフローを繰り返さずに共有スペースの作成とテストに集中します。

## レッスン

| レッスン | トピック | 説明 |
|--------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | Node.js をインストールし、Tailspin Toys プロジェクトの自分用コピーを作成する |
| [1. Copilot app のインストール][ex1] | セットアップ | アプリをインストールしてプロジェクトを接続し、ワークスペースを確認する |
| [2. 星評価を追加する: 手早い改善][ex2] | 最初の変更 | 既存の評価と null の場合の代替表示を確認し、1 つ目の PR をマージする |
| [3. エージェントモード: Plan と Autopilot][ex3] | エージェントモード | Issue から機能を計画し、Autopilot で構築して、Interactive モードでレビューする |
| [4. カスタム指示で Copilot をガイドする][ex4] | コンテキスト | 指示を確認して更新し、絞り込みに適用する |
| [5. quality-checks スキルをカスタマイズして使う][ex5] | 繰り返し可能なチェック | 既存のスキルを確認し、レポート形式を変更して実行する |
| [6. Playwright MCP で機能を検証する][ex6] | ブラウザーによる確認 | Customize から MCP を構成し、絞り込みの動作を調べる |
| [7. QA エージェントを作成して使う][ex7] | 要件とテスト範囲 | 専門家プロファイルを作成して選択し、最終検証の根拠を集める |
| [8. 機能の PR を作成してマージする][ex8] | レビューとマージ | 絞り込み、指示、スキル、QA プロファイル、テストをレビューし、2 つ目の PR に Agent Merge を使う |
| [9. キャンバスを確認して作成する][ex9] | コラボレーション | Database Explorer を使い、リポジトリに保存するトリアージキャンバスを作成してテストする |
| [10. 振り返りと次のステップ][ex10] | まとめ | ワークフロー、成果物、追加のリソースを振り返る |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] **macOS、Linux、Windows** を実行するコンピューター
- [ ] コンピューターに[インストールされた Git][install-git]

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

> [!NOTE]
> Copilot app は codespace ではなく自分のコンピューターで動作します。[前提条件のレッスン][ex0]では、アプリをインストールする前に Node.js をインストールし、プロジェクトの自分用コピーを作成します。

> [!NOTE]
> Copilot Business または Copilot Enterprise を使用している場合、管理者は **GitHub Copilot app** ポリシーを有効にする必要があります。これは Copilot CLI ポリシーとは別の設定です。

## はじめる

**[前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
