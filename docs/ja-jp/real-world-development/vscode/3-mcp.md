---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: Agent mode][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

agent mode でフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を作成します。

この演習では、次の内容を学びます。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を理解する。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルター機能を手動テストする。
- ブランチを公開し、フィルター機能のプルリクエストを作成する。

## Model Context Protocol (MCP) とは

agent mode は、エディターの外部に接続できると、さらに強力になります。Copilot がそのために使う仕組みが Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準です。

![agent mode の内部構造と、コンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) との連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、エージェントはリソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わって操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、既存の API や NPM パッケージなどのローカルツールを含む外部ツールとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールやリソースのセットを提供します。

よく使われる MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を提供します。新しいリポジトリの作成、既存のリポジトリの更新、Issue やプルリクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。エージェントは Web ページへの移動、フォームの入力、ボタンの選択などを実行できます。

他にも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、コントリビューションを促すために [MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内の他の依存関係と同じように扱ってください。使用する前に、ソースコードを慎重に確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密リソースや操作へのアクセスを許可する際には注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースで利用する MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトテンプレートに含まれています。そのため、前の演習で Copilot がバックログの Issue を読み取れました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うため、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次は 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot が操作できるブラウザーを提供できます。機能をテストするために必要なツールです。

1. `.vscode/mcp.json` で `github` と並べて `playwright` の設定を追加し、ファイルが次のようになるようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、Playwright は表示ウィンドウなしでブラウザーを実行します。デスクトップ環境がない Codespace 内では、この指定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトは E2E テストにすでに Playwright を使っているため、Playwright が必要とするブラウザーは通常インストール済みです。後で Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行するよう依頼し、もう一度試してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて起動**します。Copilot がサーバーのツールを初めて使うときに VS Code が起動し、サーバーを信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. サーバーを信頼するか確認されたら、構成を確認し、信頼して起動できるようにします。

手動で起動する必要はありません。ここで起動しなかった場合は、次の手順で Copilot がツールを初めて使うときに VS Code が起動し、信頼するかどうかを尋ねます。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して確認する代わりに、エージェントにページを開かせ、移動し、フィルターを適用して、その結果を読み上げてもらえます。その後、確認内容をまとめてもらえます。会話を中断せずに、機能が期待どおりに動作するか確認する手軽な方法です。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。これにより、エージェントはボタン、リンク、リスト項目などの構造化されたラベル付き要素を、支援技術と同じように解釈できます。簡単な機能テストを、アクセシビリティの基本確認にも活用できます。

サーバーを接続し、アプリを実行した状態で、次のプロンプトを Copilot に送信して、作成したフィルター機能を試します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、手順に沿って操作して結果を報告します。レポートを Issue の受け入れ基準と比較してください。問題があれば、プルリクエストを開く前に追加の質問をしたり、コードを修正するよう依頼したりします。

> [!NOTE]
> このテストには、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止した場合は、プロンプトを送信する前に再起動します。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーがダウンロードされることがあります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行するよう依頼して、もう一度試します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できました。次はチームでレビューするため、プルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更をステージングします。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。GitHub Copilot を使っているので、PR の作成を依頼しましょう。関連する Issue を探し、関連付けて PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. Copilot が GitHub ツールを使えるように、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の収集や操作の実行を Copilot に許可します。GitHub ツールを初めて使うときは、GitHub での認証を求められることがあります。表示される手順に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成する様子を確認します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。**まだマージしないでください**。

## まとめと次のステップ

この演習では、次の作業を行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールで Copilot を拡張する方法を学んだ。
- Playwright MCP サーバーをワークスペースに追加した。
- Playwright MCP サーバーを使い、リリース前にブラウザーでフィルター機能をテストした。
- ブランチを公開し、フィルター機能のプルリクエストを作成した。

MCP サーバーを使って機能を*テスト*しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティレビューなど、対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
