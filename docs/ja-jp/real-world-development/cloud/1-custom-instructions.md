---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく、作業方法も伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習ではプロンプトの「実行前と実行後」を比較しません。Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

優れた開発会社である Tailspin Toys には、開発方法に関するガイドラインと要件があります。具体的には、次のようなものです。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc の doc コメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使うことで、Copilot がこれらの開発慣行に沿ってタスクを実行するための情報を提供します。

### カスタム指示

カスタム指示を使うと、コンテキストや設定を Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。関連性の高い提案やコードスニペットを得るために、好みのコーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベストプラクティスなど、リポジトリでのチャットや CLI リクエストの大半に関係する情報を記載します。
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

## リポジトリ全体に適用する標準を追加する

ここで行う編集は 1 つです。コード内のドキュメントを TSDoc の doc コメントとファイルレベルのコメントヘッダーで記述するよう、プロジェクト全体に適用するルールを追加します。後でこのハーネスの Issue を cloud agent に割り当てると、追加したルールが適用されます。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象となるブランチから指示を読み取りますが、変更を整理してコミットするためにもブランチを作成します。

1. Codespace のターミナルで、次のコマンドを実行してブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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
> 前のレッスンで確認したように、リポジトリレベル (`.github/copilot-instructions.md`) に全体的な指示を置くことも、言語、ファイル形式、タスクに応じて `*.instructions.md` ファイルを作成することもできます。ここで追加した doc コメントのルールのように、プロジェクト全体の標準はリポジトリレベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は、Issue が対象とするブランチの指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、`main` からブランチを作成します。そのため、cloud agent が新しい指示を使えるように、変更を `main` に取り込む必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成し、マージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチを作成せず、`main` に直接コミットしてもかまいません。ここでは実際のプロジェクトでの作業方法を再現するため、ブランチの手順を示しています。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する方法を確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストにその標準が適用される様子を確認できます。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
