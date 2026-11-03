---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、複数のファイルを変更し、テストを実行する必要があります。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、問題を修正し、見落としたファイルを見つけて再び更新する、という作業を繰り返します。

ここで Copilot のエージェントモードが役立ちます。

Copilot のエージェントモードは、IDE 内でより自律的に作業します。開発者と同じように、まず既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる利用体験を提供します。エージェントモードは IDE 内で動作し、すばやいフィードバックとやり取りを可能にします。Cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次の内容を学習します。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを見つけ、複数の変更を調整する。
- UI とデータレイヤーにわたって機能を実装する。
- 生成された変更とテストをレビューしてからコードベースにマージする。

## シナリオ

ゲームの一覧が増えてきたため、利用者がカテゴリや発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加しました。次は、Copilot エージェントモードを使って残りのデータレイヤー、UI、テストを実装します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームを一覧表示し、詳細を表示できます。サイトは単一の Astro アプリで、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接検索します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、必要なインタラクティブ機能は標準 DOM API を使う小さなスコープ付き Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後なので、`main` に戻っているはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、Codespace で新しいターミナルを開きます。
3. 絞り込みの作業用に、新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルで Astro サーバーのアドレス `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択します。

> [!NOTE]
> Codespace のターミナルで localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的にリダイレクトされます。これは Web サーバーを実行している Codespace へのプライベートトンネルです。

### Web サイトを確認する

サイトを起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明を一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な手順で説明します。

Web サイトの最初の実装は動作しますが、さらに機能を追加します。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、Issue のバックログが自動的に作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェントドロップダウンで **Agent** を選択します。**Agent** は複数ファイルを自律的に計画・変更し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨するものではありません。ワークショップ作成時に Claude Sonnet 4.5 を使用したため、手順に含めています。よく似た体験を提供することを意図していますが、LLM は確率的に動作するため、手順と異なる提案が表示されることがあります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は別の MCP コマンドを使う場合がありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信して、Issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての Issue を一覧表示するコマンドを実行するため、**Continue** を選択します。
5. 生成された Issue の一覧を確認します。

Copilot が利用者にとって役立つと思われる順に項目を並べ替えたことを確認します。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot にコンテキストを与える指示ファイルを確認します。Web サイトへの機能追加方法に関するガイダンスを含む [ユーザーインターフェイス (UI)][ui-instructions] ファイルを使います。

1. Codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の方法に関する全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle などのファイル種類に関する指示へのリンク

> [!TIP]
> 指示ファイルでは、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示を再利用したり、複雑な内容を小さく分けたり、Copilot に例やテンプレートを示したりできます。

## 絞り込み機能を実装する

絞り込みを完了するには、アプリケーションに少なくとも 3 つの別々の変更が必要です。

- データレイヤー (`src/lib/`) に絞り込みロジックを追加または調整する。
- 絞り込みの動作をテストするテストを追加または更新する。
- ゲーム一覧ページに絞り込み UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot エージェントモードに作業を任せます。

1. Copilot との現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログ内の関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを見つけて、ファイルの変更とテストの実行を始めます。

   ![Copilot がプロジェクトのファイルを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、変更、テスト実行など複数の作業を行います。タスクやコードベースの複雑さによって数分かかることがあります。処理中に **Keep** と **Undo** が表示されても、作業中に選択する必要はありません。Copilot が完了すると、変更ごとに **Keep** または **Undo** を選択できます。

7. Copilot に求められたら、テストを実行するため **Continue** を選択します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了し、エラーがないと判断するまでコード生成とテストを繰り返します。

   ![Copilot エージェントモードの完了したチャットセッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の AI ツールが生成したコードをレビューすることが重要です。

10. Web サイトを起動したブラウザーに戻り、新しい機能を試します。
11. 動作を確認し、コードをレビューしたら Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys に機能を追加する方法を確認しました。次の内容を学びました。

- GitHub Copilot エージェントモードで UI とデータレイヤーにわたる新機能を実装する。
- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを特定し、複数の変更を調整する。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナスの演習 — ページ分割を実装する

ゲーム一覧が増えるとページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割を実装するよう依頼してみましょう。次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズを利用者が選択できるようにするか、固定にするかなど、ページ分割の方法を検討します。
- 望む実装に必要なガイダンスを含むプロンプトを作成します。
- GitHub Copilot に変更を依頼し、コンテキストを追加しながら反復する必要がある場合があります。これは Copilot を使う際の通常の作業手順です。

## リソース

- [Copilot の ask、edit、agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
