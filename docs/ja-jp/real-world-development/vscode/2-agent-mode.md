---
title: "演習 2 - Copilot Agent Mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの単純な更新でも、複数のファイルの変更やテスト実行などが必要になることがあります。開発者は通常、必要なファイルを探し、変更を加え、テストを実行し、デバッグし、変更し忘れたファイルを見つけ、さらに更新するという作業を行います。必要な作業は次々に増えていきます。

このようなときに Copilot Agent Mode が役立ちます。

Copilot Agent Mode は、IDE 内でより自律的に作業できるように設計されています。開発者と同様に、既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。Agent Mode を使って、サイトに新しい機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けに作られています。agent mode は IDE 内でタスクを実行し、短いフィードバックサイクルでやりとりできます。cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じて連携します。

この演習では、次の方法を学びます。

- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにわたる変更を調整する。
- GitHub Copilot Agent Mode で UI とデータレイヤーに新しい機能を実装する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリやパブリッシャーで絞り込めるようにします。前の演習でパブリッシャーヘルパーを追加しました。次は Copilot Agent Mode を使って、残りのデータレイヤー、UI、テストの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを開いて現在の機能を確認しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームを一覧表示し、詳細を表示できます。Web サイトは単一の Astro アプリで、ビルド時に静的 HTML としてページを生成します。ページは frontmatter 内で Drizzle ORM を使ってローカルの SQLite データベースを直接クエリします。別のバックエンド API やクライアントサイドの UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準の DOM API を使った小規模で範囲の限定された Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後、`main` に戻っているはずです。
2. Codespace 内で <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
3. フィルター作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL のバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルの Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルから localhost URL のリンクを選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` にリダイレクトされます。これは Codespace へのプライベートトンネルであり、ここで Web サーバーがホストされています。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、そのタイトル、説明、パブリッシャー、カテゴリなどの情報を表示する詳細ページが開きます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習ごとに異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善したいと考えています。まず、バックログを確認します。テンプレートからプロジェクトを設定すると、Issue のバックログが自動的に作成されます。GitHub Copilot にこれらの項目を表示してもらいましょう。

1. Chat ビューのエージェント選択メニューから **Agent** を選びます。**Agent** は複数のファイルにわたる変更を自律的に計画・実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨するものではありません。ワークショップの作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案の一貫性を高め、学習しやすくすることを意図しています。ただし、LLM は確率的に動作するため、ワークショップの例とは異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が別の MCP コマンドを使用することがありますが、タスクは同様に完了できます。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える項目を優先順位付けしてくれたことを確認できます。

## 指示ファイルを確認する

エージェントにコードを生成してもらう前に、作業のコンテキストを Copilot に伝える指示ファイルを確認します。Web サイトに機能を追加する方法を説明する [user interface (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能を追加するときの全体的なガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle などのファイル形式に応じた指示ファイルへのリンク。

> [!TIP]
> 指示ファイルから、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。これにより、再利用や複雑な指示の分割が容易になり、Copilot に例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルター処理を追加または調整する。
- フィルターの動作に関するテストを追加または更新する。
- ゲーム一覧ページにフィルター UI を追加する。

さらに、コードベースにマージする前にテストを実行し、成功させる必要があります。Copilot Agent Mode にこれらの作業を任せましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニューを示すスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. 関連するバックログの Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関連するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを見つけた後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べている様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行するなど、複数の作業を行います。タスクやコードベースの複雑さによっては数分かかります。作業中、コードエディターに **Keep** と **Undo** が表示されることがあります。Copilot の作業中に選択する必要はありません。完了すると、すべての変更に対して **Keep** または **Undo** が表示されます。

7. Copilot から求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインでテストを実行してよいか確認するダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりすることがあります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクが完了してエラーが見つからなくなるまで作業します。

   ![Copilot Agent Mode のセッションが完了した状態を示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いたブラウザーに戻り、新しい機能を試します。
11. 動作を確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot Agent Mode を使って Tailspin Toys の Web サイトに新しい機能を追加する方法を確認しました。次の内容を学びました。

- GitHub Copilot Agent Mode で UI とデータレイヤーに新しい機能を実装する。
- Copilot Agent Mode でプロジェクトを調べ、関連ファイルを特定し、複数ファイルにわたる変更を調整する。
- コードベースにマージする前に、Copilot Agent Mode が生成した変更とテストをレビューする。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナスの演習 — ページ分割を実装する

ゲーム一覧が増えると、ページ分割が必要になります。この演習で学んだことを使い、Copilot にページ分割の実装を依頼します。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定値にするかなど、ページ分割の実装方法を検討します。
- 希望する実装に必要なガイダンスをプロンプトに含めます。
- Copilot に変更を依頼し、コンテキストを提供しながら反復的に進めることが必要な場合があります。これは Copilot を使う通常のワークフローです。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
