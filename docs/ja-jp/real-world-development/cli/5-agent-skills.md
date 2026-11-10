---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、レポート形式をカスタマイズして、フィルター機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことは、コードを記述するだけではありません。手動でコードの動作を確認し、指示ファイルでコーディング標準に従うようにしました。テストや lint、継続的インテグレーション (CI) の他の作業はどうでしょうか。

このようなタスクには、**エージェントスキル**が適しています。スキルを使うと、こうした操作を正しく実行する方法を Copilot に伝えられます。

このレッスンでは、次の内容を学びます。

- 既存の `quality-checks` スキルを確認する。
- 結果のレポート形式をカスタマイズする。
- スキルを再読み込みして実行する。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルには、再利用可能なタスクの指示、実行可能なスクリプト、補助リソースが含まれます。基本的には、`SKILL.md` という名前の Markdown ファイルを含むスキル名のフォルダーです。Markdown には、スキル名と説明を定義するフロントマター、機能の概要、呼び出すタイミングに関するガイダンスを記載します。スキルのフォルダーには、呼び出されたときに使用するスクリプトやその他のリソース用のサブフォルダーも含められます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` だけで構成されています。

スキルは、チームで共有して再利用できるようプロジェクトの `.github/skills` フォルダーに置くことも、ユーザーのスキルフォルダー `~/.copilot/skills` に置くこともできます。

## スキルを確認する

Tailspin Toys のチームがユニットテスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭の `name` と `description` を読みます。説明は、Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 指示を読み、Copilot がテストと lint を実行する方法を確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更を加える前にスキルを実行する

Copilot CLI から直接、または自然言語を使ってスキルを呼び出せます。Copilot に 3 種類のチェックを実行してもらいましょう。

1. Interactive モードでフィルターの会話に戻ります。
2. 次のプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した内容を確認できるレポートに改善します。スキルを更新して、この形式のレポートを作成します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えました。実際に動作を確認しましょう。Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを入力します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次の作業を行いました。

- ユニットテスト、lint、型チェック用の `quality-checks` スキルを確認した。
- 結果のレポート形式をカスタマイズした。
- スキルを再読み込みして実行した。

この変更は、機能の PR に含まれます。次は [Playwright MCP サーバーを使って][next-lesson]、Copilot がサイトを直接操作できるようにします。

## スキルの例

コミュニティの次の例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
