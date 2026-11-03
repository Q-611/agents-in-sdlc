---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行したい場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。中でも重要なのは、コード生成のガイダンスを伝える[指示ファイル][instruction-files]です。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパスで対象を指定する指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認する。
- 現在の指示を使って、絞り込み用の最初のデータ部分 (発行元ヘルパー) を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認する。
- 次の演習で Copilot が更新したガイダンスを利用できるよう、指示の更新と絞り込みの土台を `main` にコミットする。

> [!CAUTION]
> 生成コードが設定した標準の一部に従わないことがあります。Copilot は確率的に動作します。この演習では、出力を一字一句一致させるのではなく、指示を更新した後に動作がどのように変わるかを確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードで、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って Copilot に必要な情報を伝え、強調したプラクティスに沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルを Copilot に理解させ、コンテキストや設定を伝えられます。コーディング規約、ライブラリ、コードコメントの種類などを指定することで、より適切な提案やコードスニペットを得られます。リポジトリ全体に適用する指示も、特定のファイル種類やタスクに適用する指示も作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送られます。技術スタック、開発対象の概要、ベストプラクティスなど、Copilot Chat や CLI でのほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に使えます。TypeScript などの言語や、UI コンポーネントやユニットテストの作成などのタスクに関するガイドラインを記載します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することはこのワークショップの範囲外ですが、サンプルプロジェクトの例は代表的なアプローチを示しています。基本として、次の点を意識してください。

- `copilot-instructions.md` には、作成するものやプロジェクトの構造、全般的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種類や、特定のタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスと例を示します。コードの望ましい例と避けるべき例を含めます。

指示ファイルの作成方法は 1 つではありません。AI の使い方と同様に、プロジェクトで試行錯誤して最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、充実した指示ファイルを用意すると役立ちます。このプロジェクトには、[UI の指示][ui-instructions]や [Astro の指示][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。各画面で方法は異なります (例: VS Code の **Configure Chat → Generate Agent Instructions**、Copilot CLI の `/init`)。このハーネスでは、該当する箇所で説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みます。コアの `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針もあります。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを決める glob を指定します。この例では `**/*.test.ts` に一致する TypeScript のテストファイルが対象になります。
5. このプロジェクトでユニットテストを作成するときの指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾を確認します。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示を再利用可能な小さなファイルに分け、コード生成の参考例を Copilot に示す方法を確認できます (パスはリポジトリのルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイルのコメントヘッダーに関するルールを追加します。

## 変更用のブランチを作成する

ここではベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. ファイルを変更できるよう、Chat ビューのエージェントドロップダウンで **Agent** を選択します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べ、ヘルパーファイルとテストなどのコードを更新します。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを取り、発行元の型付き配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> 確率的な動作のため、指示しなくても Copilot がドキュメントコメントを追加する場合があります。その場合も問題ありません。この演習では、指示を更新した後の動作が一貫することを確認します。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案の品質を高めるため、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関するルールがありません。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは、グローバルなガイダンス用のリポジトリレベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種類、タスク向けの `*.instructions.md` にも作成できます。ここで追加したドキュメントコメントのルールなど、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに集中できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Tailspin Toys クラウドファンディングプラットフォームの発行元データアクセスヘルパーです。
    * データベースから発行元情報を取得する関数を提供します。
    */
   ```

5. 関数に次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * ID と名前を含む発行元の一覧を返します。
    *
    * @param db - Drizzle データベースクライアントです。
    * @returns 発行元オブジェクトの配列で解決される Promise です。
    */
   ```

Copilot をガイドして新しいプロジェクト標準に従わせ、次の演習で使うコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもリポジトリ内のほかのファイルと同様に、ソース管理で管理します。指示の更新をコミットし、ブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. ターミナルで次を実行し、指示の更新とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基盤として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の関連変更 (例: テスト) が表示される場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けてプルリクエストを作成します。GitHub ツールはすでにプロジェクトに接続されているため、Copilot に依頼して Issue を探し、Issue をクローズする PR を作成してマージできます。

> [!NOTE]
> この演習では Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートでは GitHub のツールがあらかじめワークスペースに接続されています。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従って許可します。この接続方式 (Model Context Protocol) は後の演習で学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイル更新の Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、今後の絞り込み作業のために追加した発行元ヘルパーの基盤を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. GitHub の操作を Copilot に許可するよう求められたら、コマンドを確認して適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトを使って PR をマージし、ローカルのブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェルコマンドの実行を Copilot に許可するよう求められたら、コマンドを確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、Copilot Chat を使って次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` に新しい標準を追加しました。
- 同じプロンプトを再実行し、生成コードに標準が反映されることを確認しました。
- 次の演習で機能を拡張できるよう、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は、Copilot がコードベース全体に機能を追加するときに、指示を[エージェントモード][next-lesson]で活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
