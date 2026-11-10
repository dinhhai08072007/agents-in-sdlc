---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加などのタスクを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。手間のかかるタスクを任せて開発を加速し、開発者がより大きな課題に集中できるようにします。

Copilot cloud agent を使って、次の内容を学びます。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- Issue の範囲を明確にすることの重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを開発した請負業者が残したドキュメントの状態は良くなく、実際にはドキュメントがほとんどありません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc の doc コメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリの他のゲームを表示し、閲覧を続けられるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものを求めています。現在はブロッカーですが、優先度の高い Issue が他にもあります。

こうしたタスクは後回しになりやすく、Copilot cloud agent に任せるのに適しています。cloud agent に非同期で作業させれば、開発者は別の作業に集中し、あとから結果をレビューして期待どおりか確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub Issue を Copilot に割り当てる][assign-issue]など、いくつかの方法で作業を依頼できます。割り当てると、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境を準備してタスクを開始します。作業中も完了後もセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく調べると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、必要に応じて調整します。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。カスタム指示の演習を終えていれば、`.github/copilot-instructions.md` が機能することを確認済みです。まだの場合は、ここで簡単に確認しましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリの指示ファイルを読み取り専用で確認しておくと、あとでその効果が分かります。

GitHub の Web UI または起動済みの Codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc doc コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターで、指示の対象が `**/*.test.ts` ファイルに限定されていることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成されたプルリクエストに TSDoc doc コメント、TypeScript の規約、コメントヘッダーが反映されているか確認します。これらは指示ファイルによるものです。後の演習で PR をレビューするときに、もう一度確認します。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードを作成するには通常、適切な状態を保つための環境とセットアップスクリプトが必要です。SWE と同様に作業する Copilot にタスクを割り当てる場合も同じです。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を準備できます。このラボでは学習を円滑に進めるため、事前に設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、E2E テスト用のブラウザーバイナリ、マイグレーションとシードを済ませた SQLite データベースを Copilot が利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

他の GitHub ワークフローファイルと同じように見えますが、重要な点がいくつかあります。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot がプルリクエストの作業を開始する前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーがあるため、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot の実行を待たずに、ワークフローが正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトでは情報が古かったり、まったくなかったりします。このような技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しいメンバーの受け入れを難しくします。Copilot はドキュメントの作成を得意としているため、cloud agent に依頼するのに適した Issue です。バックグラウンドで必要なドキュメントを作成させ、後の演習でその結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようなものです。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認できます。

> [!NOTE]
> Copilot の TODO リストが PR に表示されるまで数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を準備し、プロジェクトを分析して、問題に取り組む方法を検討しています。

13. リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するには、ウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールし、Copilot がどのように問題を解決しているか確認します。コードを調査して状態を把握する様子、考えて適切な計画を決めるために一時停止する様子、コードを作成する様子を確認できます。

完了まで数分かかることがあります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期で実行し、他の作業に集中できるようにすることです。この機能を活用し、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションに機能を追加する作業を進めます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の大きな利点は、作業を分担できることです。デザインチームのために関連ゲームのセクションを追加する作業に時間がかからないとしても、その時間を別の作業に使えるかもしれません。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業中であることを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue の作業中であることを目の絵文字で示している画面](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent は SWE と同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当て、非同期で作業させることができます。技術的負債の解消、新しい機能の作成、別のフレームワークへのコード移行などを依頼できます。

次の内容を学びました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- Issue の範囲を明確にすることの重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用できます][cloud-agent-mcp]。前のモジュールで説明したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローに関するベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
