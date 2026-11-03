---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、登録済みの絞り込み Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースを調べ、コードを生成し、コマンドを実行し、外部ツールに接続できます。コマンドラインからタスクを任せたり、変更を依頼したりできるため、集中して作業できます。最初の手順として、使い慣れたツールを使って Copilot CLI をインストールします。

このレッスンでは、次の内容を学習します。

- npm で GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーを使って絞り込みの Issue を見つける。

## シナリオ

チームは、増え続けるバックログに取り組むため、AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が日常的に作業するターミナルでその機能を使えます。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで利用できるようにします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew でインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、この演習では npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされ、必要なバージョンを満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

初回起動時に、Copilot CLI から GitHub アカウントで認証するよう求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. プロンプトが表示されたら、デバイスコードを使った認証手順に従い、Copilot CLI を認証して許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択します。

> [!NOTE]
> Codespace では、GitHub セッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 使い方を確認する

通常のシェルプロンプトでは、コマンドが Codespace で直接実行されます。Copilot CLI を起動すると、自然言語はエージェントに送られ、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押します。続けてもう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の簡単な質問を Copilot に送り、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答の前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. 次のプロンプトで、絞り込みの Issue を探すよう Copilot に依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 表示された URL を開き、Issue を読みます。最初の変更を行った後、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使います。自動的に worktree が作成されるわけではありません。変更ごとに機能ブランチを作成します。

## ワークショップ用のショートカットを使う

通常、Copilot CLI は、設定済みの権限の範囲外でツールを使う前に確認を求めます。このワークショップでは、`--yolo` を付けて Copilot CLI を再起動します。ユーザーが承認したこのショートカットを使うと、Codespace 内では各操作の確認を省略でき、演習に集中できます。

> [!CAUTION]
> `--yolo` は、すべてのツール、パス、URL に対する自動許可 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの分離された環境でのみ使用し、日常の開発で既定の設定にしないでください。詳細は[ツールの使用を許可または禁止する方法][allow-all-warning]を参照してください。

後のレッスンで Issue やプルリクエストを扱うときに使う GitHub MCP の読み取り/書き込みツールを有効にするため、このワークショップでは `--enable-all-github-mcp-tools` も指定します。Codespace はローカルコンピューターへのアクセスを制限しますが、認証済みの GitHub リソースには引き続きアクセスできます。変更の公開やマージの前に、内容を確認してください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから、次のコマンドで再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. プロジェクトについて別の簡単な質問をして会話が動作することを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりしたら、`copilot --resume` を使って同じ会話とブランチに戻ります。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次の作業を行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーを使って絞り込みの Issue を見つけました。

次は、[最初の小さな変更を始め][next-lesson]、Copilot CLI を使ってゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI のコマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
