---
title: "演習 6 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
| :-- |

## 作業をレビューする

このラボでは、ユーザー体験の改善に関するいくつかのタスクで GitHub Copilot を使いました。エージェントモードでクライアントとサーバーにわたる絞り込み機能を追加し、Playwright MCP サーバーでブラウザーから動作を手動テストし、カスタムエージェントでハイコントラストとライトモードの切り替えなどアクセシビリティ機能を実装しました。また、作業中のセッションに追加の指示を出して、機能を拡張しました。次は、このローカル作業を公開し、チームと同じ方法でレビューします。

### シナリオ

生成 AI を加えてもソフトウェア設計と DevOps の基本は変わりません。Copilot が作成したものも、実際のレビューサイクルに通します。codespace からアクセシビリティの変更をプッシュし、プルリクエストを作成して、チームに公開する前に差分を確認しましょう。

## アクセシビリティ機能を公開する

[演習 4][exercise-4] と [演習 5][exercise-5] でアクセシビリティ用カスタムエージェントを使って実装したハイコントラストとライトモードの切り替えは、コミット済みの変更として codespace にあります。ブランチにプッシュしてプルリクエストを作成し、チームにレビューしてもらいます。

1. codespace に戻ります。
2. VS Code で **Source Control** ビューを開きます。
3. アクセシビリティの変更がコミット済みであることを確認します。演習 5 で未コミットの変更がある場合は、`Add high-contrast and light-mode toggles` など、内容が分かるメッセージを付けてステージし、コミットします。
4. **Publish Branch** を選択してブランチを公開します (または **...** メニューから **Push** を選択します)。
5. VS Code から github.com の新しいブランチを開くよう求められたら、プロンプトを受け入れます。手動で開く場合は、リポジトリに移動してブランチのバナーにある **Compare & pull request** を選択します。
6. 明確なタイトル (例: `Add high-contrast and light-mode toggles`) を設定し、行ったことと理由を短く説明します。
7. **Create pull request** を選択します。
8. PR を開いたら **Files changed** タブを選択し、作業全体を確認します。特に次の点を確認します。
   - 表示モードを切り替える UI コンポーネント
   - ユーザー設定を保持する local storage の使用
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード
9. **Conversation** タブに戻ります。
10. ワークフローの承認待ちがある場合は **Approve and run workflows** を選択します。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

11. ワークフローが完了するまで待ちます。問題がなければ成功します。

> [!TIP]
> アクセシビリティの変更について別の意見が必要ですか。PR のコメントで `@copilot` にメンションし、「この PR に追加の WCAG の問題がないか確認してください」や「キーボード操作を改善する方法を提案してください」などと依頼します。Copilot はコメントに対応する新しいセッションを開始します。

## オプションの演習 - ローカルでさらに試す

IDE でエージェントと反復して作業するには、実際に繰り返し試すことが大切です。VS Code から次のようなタスクを依頼できます。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- `src/lib/` のデータアクセスヘルパーに入力検証とエラー処理を追加する。
- たとえばサイト全体のキーボードフォーカス順序を監査するなど、アクセシビリティエージェントの対象範囲を広げる。

## まとめ

おめでとうございます。VS Code ハーネスを完了しました。このラボでは、次の作業を行いました。

- **Playwright MCP で機能を手動テストしました。** Playwright MCP サーバーを追加し、プルリクエストを開く前に Copilot にブラウザーを操作させて絞り込み機能を検証しました。
- **エージェントモードでスタック全体にわたる変更を調整しました。** 1 つのセッションでクライアント、サーバー、テストに関わる絞り込み機能を追加しました。
- **カスタムエージェントを使いました。** エージェント選択メニューからアクセシビリティに特化したカスタムエージェントを選び、リポジトリにハイコントラストモードを実装する様子を確認しました。
- **エージェントセッションを管理して方向付けしました。** 提案された変更をインラインでレビューし、必要なものを採用して、ライトモードの追加プロンプトでセッションを拡張しました。
- **プルリクエストで一連の作業を完了しました。** ローカルの作業を公開し、チームと同じ方法で全体をレビューしました。

## 振り返りと次のステップ

必須の VS Code ハーネスは完了です。ここでワークショップを終了できます。

Copilot のエージェント機能をさらに確認する場合は、他のハーネスで異なる環境のシナリオを試せます。

- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、スキル、QA カスタムエージェント、Playwright MCP、Agent Merge、コンテキストを考慮した引き継ぎを使い、ターミナルからレビュー済みの変更を 2 つリリースします。
- ☁️ **[Cloud agent ハーネス](../../cloud/)** — Issue を cloud agent に割り当て、agents ページでセッションを監視し、非同期にプルリクエストを改善します。

ここまでの内容を使って開発を続けることもできます。[awesome-copilot][awesome-copilot] には、自分のプロジェクトに合わせて使える指示ファイル、カスタムエージェント、スキルが多数あります。

オプションの拡張として、[オプション: Foundry を組み込む][exercise-7]では、VS Code と Microsoft Foundry Toolkit を使ってモデルを準備し、Backer Concierge をデプロイしてサイトに接続します。

## リソース

- [GitHub Copilot][github-copilot]
- [VS Code の Copilot Chat][copilot-chat-vscode]
- [エージェントモードの使用][agent-mode]

---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
