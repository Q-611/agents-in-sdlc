---
slug: ja-jp/real-world-development/vscode
title: "Visual Studio Code"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の **[GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)** を使うと、普段使っているコードエディターで GitHub Copilot を利用できます。Visual Studio Code と GitHub Codespaces で Copilot Chat のエージェントモードを使い、MCP で外部ツールに接続し、カスタムエージェントを活用します。IDE を離れずに作業でき、Copilot はファイル、ターミナル、問題の全体を確認できます。

最初にカスタム指示を追加し、Copilot が指示に従うことを確認します。次にエージェントモードで、UI、データレイヤー、テストにわたる絞り込み機能を構築します。続いて Playwright MCP サーバーに接続し、プルリクエストを開く前に Copilot にブラウザーで機能をテストしてもらいます。最後にアクセシビリティ用のカスタムエージェントを確認して使い、エディターを離れずに Copilot の変更を監視、誘導、反復します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成する |
| [1. カスタム指示][ex1] | コンテキスト | VS Code でカスタム指示を追加して確認する |
| [2. エージェントモード][ex2] | コード生成 | エージェントモードで絞り込み機能を構築する |
| [3. Playwright を使った MCP][ex3] | 外部ツール | Playwright MCP サーバーを使ってブラウザーで機能をテストする |
| [4. カスタムエージェント][ex4] | 特化エージェント | カスタムエージェントを確認して使う |
| [5. エージェントの管理][ex5] | 監視 | エージェントセッションを監視して方向付けする |
| [6. 反復][ex6] | レビュー | Copilot の作業をローカルで確認し、次の手順を選ぶ |
| [オプション: Foundry を組み込む][foundry-toolkit] | AI エージェント | VS Code と Foundry Toolkit を使う 3 つのモジュールで、モデルを準備し、エージェントをデプロイしてサイトに接続する |

## 前提条件

このワークショップに参加する前に、次を準備してください。

- [ ] **Copilot Student、Pro、Pro+、Business、Enterprise** のいずれかの有効なプランがある GitHub アカウント
- [ ] GitHub Codespaces へのアクセス

> [!TIP]
> 有料プランがありませんか。認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用する agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、すべてのハーネスを完了できます。

[callout-student-plan-education]: https://github.com/education/students

## はじめる

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-agent-mode/
[ex3]: 3-mcp/
[ex4]: 4-custom-agents/
[ex5]: 5-managing-agents/
[ex6]: 6-iterating/
[foundry-toolkit]: 7-foundry-toolkit/
