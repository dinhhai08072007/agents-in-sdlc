---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルター機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くことは、コードを記述するだけではありません。データや外部サービスを扱い、Copilot が使える自動化機能を追加する必要もあります。そこで MCP サーバーを使います。MCP サーバーは Copilot の CLI に組み込まれていないツールやサービスを提供し、機能を拡張します。

このレッスンでは、次の内容を学びます。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを理解する。
- まだ利用できない場合は、Playwright MCP サーバーを追加する。
- エージェントにブラウザーを操作させ、フィルター機能を試す。

## シナリオ

ユニットテストや E2E テストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように作業中の Web サイトを使えるようにし、変更が期待どおりに動作するかを自動的に確認します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信し、最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。各 MCP サーバーは、AI エージェントが利用できるツールやリソースのセットを提供します。

よく使われる MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

他にも多くの MCP サーバーがあります。GitHub は、エコシステム内のサーバーを見つけやすくし、コントリビューションを促すために [MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーは、プロジェクト内の他の依存関係と同じように扱ってください。使用する前に、ソースコードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

まだ Playwright MCP サーバーを利用できない場合は追加し、ユーザーと同じように Copilot が Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP が設定されている場合は、セットアップ手順を省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーの追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. サーバーの種類が **STDIO** であることを確認し、<kbd>Enter</kbd> を押してから <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次のテキストを貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を試すよう Copilot に依頼する

これまでは機能が期待どおりに動作するかを手動で確認しました。次は、Copilot が利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを見つけて使うため、明示的に指示する必要はありません。使うツールが分かっている場合は、指定すると結果の一貫性を高め、トークンを節約できます。

2. Copilot がブラウザーで機能を検証するために実行する手順を確認します。
3. レポートを読み、すべてが期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、サーバーを停止して結果をレポートします。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を試しました。次の内容を学びました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを理解した。
- まだ利用できない場合に Playwright MCP サーバーを追加した。
- エージェントにブラウザーを操作させ、フィルター機能を試した。

次は、スキルとブラウザーツールを組み合わせる [QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
