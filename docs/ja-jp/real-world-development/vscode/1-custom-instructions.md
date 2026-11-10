---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。コード生成のガイダンスを提供する[指示ファイル][instruction-files]が、その主な方法の 1 つです。

この演習では、次の内容を学びます。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認する。
- 現在の指示を使って、フィルター用の最初のデータスライス (パブリッシャーヘルパー) を作成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトをもう一度実行し、生成コードが新しい標準に従う様子を確認する。
- 次の演習で Copilot が更新したガイダンスを使えるよう、指示の更新とフィルタースライスを `main` にコミットする。

> [!CAUTION]
> 生成されたコードが、設定した標準に従わない場合があります。Copilot は確率的に動作します。この演習の目的は、指示を更新した後の動作の*傾向*を確認することであり、出力の一字一句を一致させることではありません。

## 指示ファイル

### シナリオ

優れた開発会社である Tailspin Toys には、開発方法に関するガイドラインと要件があります。具体的には、次のようなものです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc の doc コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コンテキストや設定を Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。関連性の高い提案やコードスニペットを得るために、好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティスなど、リポジトリに関するほとんどのチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式に使う指示ファイルです。TypeScript などの言語や、UI コンポーネントまたはユニットテストの作成などのタスクに指示を設定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使います。
>
> さらに、Copilot は AGENTS.md や CLAUDE.md など、他の規格を使った指示ファイルにも対応しています。[対応する指示ファイルの規格][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成について詳しく説明することはこのワークショップの範囲外ですが、サンプルプロジェクトには代表的な構成例があります。基本的には、次のようにします。

- `copilot-instructions.md` には、開発対象の説明、プロジェクト構成、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル形式や、特定のタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスとコードの良い例・悪い例を示します。

指示ファイルの作り方は一通りではなく、AI の使い方にも決まった方法はありません。試しながら、自分のプロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスクに使う指示ファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。各環境で操作は異なります。たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。詳細は、該当する環境のレッスンで説明します。
>
> テンプレートや出発点を探していますか？指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意された指示ファイルを確認します。コアとなる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には、**GitHub Actions Workflows** に関するガイダンスもあります。これらの指示は、Copilot を使うすべてのやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリルートからの相対パスで対象ファイルを指定する glob であることを確認します。ここでは、たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトでユニットテストを作成するための指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの他の指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな指示セットを小さく再利用しやすいファイルに分割し、参考例を示せます。ここに記載されたパスは、リポジトリルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメント作成はまだ必須ではありません。次の手順で、TSDoc の doc コメントとファイルコメントヘッダーに関するルールを追加します。

## 変更用のブランチを作成する

ベストプラクティスに従って、変更用のブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前*に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新し、同じプロンプトをもう一度送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの作成場所を Copilot に伝えます。
3. Chat ビューのエージェント選択メニューから **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを更新します。多くの場合、ヘルパーファイルとテストの両方が変更されます。
6. 提案されたヘルパーが、`db` クライアントを最初の引数に取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc の doc コメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc コメントを追加することがあります。その場合も問題ありません。指示を更新した後の*一貫性*の改善が、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

前の説明のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準が記載されていますが、コード内ドキュメントに関するルールがないため、生成されたヘルパー関数に doc コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown 行を追加します。ファイルヘッダーと TSDoc doc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する指示は `.github/copilot-instructions.md` に、言語やファイル形式、タスクに応じた指示は `*.instructions.md` に置けます。ここで追加した doc コメントのルールのように、プロジェクト全体の標準はリポジトリレベルのファイルに記載します。

## プロンプトを再実行して変更を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再び開き、Copilot が適切なファイルに集中できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

これで新しいプロジェクト標準を Copilot に伝え、次の演習で使うコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリの他のファイルと同じように、ソース管理で管理します。変更をコミットしてブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の更新と、ヘルパーの基盤のために Copilot が変更したすべてのファイルをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する Issue と関連付けたプルリクエストを作成します。手動で作成することもできますが、接続済みの GitHub ツールを使って Copilot に作成してもらいます。Issue を検索し、それをクローズする PR を作成してから、マージを依頼します。

> [!NOTE]
> Copilot に GitHub の操作を依頼するのは、これが最初の演習です。プロジェクトテンプレートでは、GitHub ツールがすでにワークスペースに接続されているため、追加のセットアップは必要ありません。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められることがあります。表示される手順に従ってください。次の演習では、Model Context Protocol を使った接続方法を学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。指示の更新と、フィルター作業に向けたパブリッシャーヘルパーの基盤の両方を記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot は Issue を検索し、PR の作成を開始します。
5. GitHub の操作を許可するよう Copilot から求められたら、コマンドを確認して必要に応じて **Allow** を選択します。
6. PR が作成されたら、次のプロンプトで PR のマージと、ローカルブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作とシェルコマンドを許可するよう Copilot から求められたら、コマンドを確認して必要に応じて **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送信し、Copilot の出力を確認した。
- `.github/copilot-instructions.md` に新しい標準を追加した。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認した。
- 指示の更新とヘルパーの基盤を `main` にコミットし、次の演習で使えるようにした。

次は [agent mode][next-lesson]で指示を使い、Copilot にコードベース全体にわたる新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot 用の個人向けカスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
