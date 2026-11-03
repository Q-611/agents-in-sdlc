---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みの絞り込み Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み Issue から始め、Plan モードで方法を決め、Autopilot モードで構築して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込みを確認しました。
- 品質保証 (QA) のカスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの検証結果を評価しました。
- 絞り込みの変更全体をレビューし、絞り込み PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## リリースした内容

このワークショップには、更新後の `main` からそれぞれブランチを作成する、2 つの PR の節目があります。

1. **星評価**: 既存の `starRating` と、評価がない場合の状態をゲームカードに表示します。
2. **絞り込みと品質ワークフロー**: 絞り込み機能を実装し、指示を更新して機能に適用します。`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成し、関連するテストを含めます。

絞り込みの計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分で行うブラウザーの確認、MCP 経由の Copilot によるブラウザー確認など、複数の方法でコードを検証しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果を、要件とテスト範囲のレビューと組み合わせて PR 前の確認を行いました。

追加するテストでは、実際に不足している範囲を補ってください。新しいテストが不要という QA の結果が正しい場合もあります。マージを許可する前に、コードと根拠を確認してください。変更があった場合は、影響する検証結果を更新します。

## ベストプラクティス

Copilot に与えるコンテキストやツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** で構築前にアプローチを検討し、範囲の明確な変更では **Interactive** で確認を続け、十分に範囲を限定したタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラストラクチャと同じくらい、コンテキストも重要です。何を、なぜ、どのように構築するのかを明確に説明すると、出力が大きく変わります。

## さらに確認する

主なワークフローは完了しました。Copilot CLI の機能をさらに確認するには、次を試してみてください。

- `/review` でコードレビューエージェントに変更を分析してもらう。
- `/rubber-duck` で問題を話し合い、別の視点を得る。
- `/fleet` で独立したサブタスクを並列に調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送る。

## 次のステップ

どのツールでも、上達する一番の方法は使い続けることです。本番コード、趣味のコード、何年も考えていたけれどまだ作っていない小さなアプリなどで活用してください。学んだことをチームと共有し、チームの経験からも学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI のコマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
