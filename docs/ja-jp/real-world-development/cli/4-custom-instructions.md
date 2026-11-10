---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、作成するコードだけでなく、その構成方法も説明できます。フィルター機能を実装したので、Copilot が使った指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の内容を学びます。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認する。
- コーディング標準に従うよう指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

優れた開発会社である Tailspin Toys には、開発方法に関するガイドラインと要件があります。具体的には、次のようなものです。

- コメントには、コードの言い換えではなく、目的や自明でない判断を記載します。
- `db/` と `src/lib/` にあるエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を追加します。該当する場合は、注入可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述します。関連するコードを変更するときは、コメントを最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるようにします。

## 指示ファイル

カスタム指示を使うと、コンテキストや設定を Copilot に伝え、コーディングスタイルや要件を理解しやすくできます。コーディング規約、ライブラリ、コメントの書き方などを指定できます。リポジトリ全体、または特定の種類のファイルに指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクに関するガイダンスを記載します。

> [!NOTE]
> 他の指示形式への対応状況は環境によって異なります。特定の形式を使う前に、[カスタム指示の対応状況][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、すでに複数の指示ファイルがあります。変更を加える前に、何が用意されているかを確認し、更新の影響を比べられるようにしましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、指示の対象ファイルを指定する glob であることを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクトファイルへのリンクを確認します。指示を小さく再利用しやすいファイルに分割し、Copilot に参考例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

すでにある指示ファイルは出発点として適切ですが、まだ不足があります。コアの `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` の 35 行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の行を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。更新した指示を保存したら、フィルター機能のコンテキストを保ったまま会話を終了して再開します。

1. フィルターの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートで `game-filters-cli` ブランチにいることを確認し、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   確認を求められたら、フィルターの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正しく説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次の作業を行いました。

- リポジトリの指示とパスに応じた指示ファイルがエージェントに適用される方法を確認した。
- コーディング標準に従うよう指示ファイルを更新した。
- 指示ファイルがコードに与える影響を確認した。

次は、再利用可能な quality-checks スキルを[カスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示の対応状況][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
