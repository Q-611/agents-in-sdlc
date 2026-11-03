---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、任意のクラウド委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

ほかの CLI ツールと同様に、GitHub Copilot CLI にも多くのスラッシュコマンドがあります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドをいくつか確認します。

このレッスンでは、次の内容を学習します。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認する。
- `/model` で利用できるモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の主なワークフローが完了しました。次に、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent] への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、スラッシュコマンドで状態を確認したり、手動で圧縮したりできます。

1. Codespace に戻り、まだ起動していない場合はリポジトリルートから Copilot CLI を起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がまだ短い場合は、大きな違いがないこともあります。

> [!NOTE]
> Copilot CLI はコンテキストウィンドウが埋まると、自動的にコンテキストを圧縮します。圧縮するタイミングを選びたい場合は `/compact` を使います。関連のない別のタスクに切り替えて新しい会話を始めたい場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルによって得意なタスクが異なり、開発者によって好みも異なります。Copilot CLI では、利用できるモデルを一覧表示し、使用するモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 使用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールをより効果的に使えるようになります。`/share` コマンドでは、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力して、インストール済みバージョンの `/share` オプションを確認します。
2. セッションを共有する場合は、たとえばローカルに Markdown をエクスポートする `/share file` など、目的に合った方法を選びます。
3. エクスポートした内容を、送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有するつもりのないリポジトリや会話の内容を公開しないでください。

## オプション: 作業の並列化と委任

ワークショップの主要な内容は完了しました。Copilot CLI には、大きなタスクに対応する次のコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並列で実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、非同期に作業してプルリクエストを作成する場合もある Copilot cloud agent にタスクを送信できます。

追加の worktree やリモート作業が発生するため、これらのコマンドは任意です。試すときは、範囲を明確にした新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業をさらに確認する場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、CLI の構成、セッションの共有、内部動作の確認ができます。このレッスンでは、次の内容を学習しました。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認しました。
- `/model` で利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI にはほかにも多くのスラッシュコマンドがあります。最後に、これまでの内容を[振り返り][next-lesson]、学習を続けるための次のステップを確認します。

## リソース

- [Copilot CLI のコマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
