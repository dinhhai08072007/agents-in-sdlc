---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、用意されたフィルターの Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作する強力なエージェント型コーディングアシスタントです。コードベースを調査し、コードを生成し、コマンドを実行し、外部ツールと連携できます。すべてコマンドラインから行えるため、タスクを任せたり、変更を依頼したりしながら、作業に集中できます。最初のステップは Copilot CLI のインストールです。すでに使い慣れたツールで簡単に行えます。

このレッスンでは、次の内容を学びます。

- npm で GitHub Copilot CLI をインストールする。
- GitHub アカウントで認証する。
- ワークショップのリポジトリを信頼し、簡単な会話を試す。
- 組み込みの GitHub MCP サーバーを使って、フィルターの Issue を見つける。

## シナリオ

チームは、増え続けるバックログに取り組むために AI エージェントを使い始めています。Copilot CLI は、多くの開発者が日常的に使うターミナルでその機能を利用できるようにします。このレッスンでは、インストールと認証を済ませ、ワークショップの残りで使える状態にします。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされており、バージョン要件を満たしていることを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以上が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. 次のコマンドでインストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI を初めて起動すると、GitHub アカウントでの認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 求められた場合は、デバイスコードの手順に従って認証し、Copilot CLI のアクセスを許可します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. パスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して「はい」と答えます。

> [!NOTE]
> Codespace では、GitHub のセッションを通じてすでに認証されている場合があります。Copilot CLI が認証を求めずに起動した場合は、そのまま進めて問題ありません。

## 使い方を確認する

通常のシェルプロンプトに入力したコマンドは、Codespace で直接実行されます。Copilot CLI の起動後は、自然言語をエージェントに送り、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力し、インストール済みのバージョンで利用できるコマンドを確認してから、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. Copilot が正しく動作していることを確認するため、簡単な質問をします。

   ```plaintext
   What are the key files in this project?
   ```

4. 回答を読み、Copilot が回答の前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用できることを確認します。
6. Copilot にフィルターの Issue を探してもらいます。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開き、Issue を読みます。最初の小さな変更を完了した後に、この Issue を使います。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトされているブランチを使います。自動的にワークツリーは作成されません。変更を始める前に、機能ブランチを作成します。

## ワークショップのショートカットを使う

通常、Copilot CLI は設定済みの許可範囲外でツールを使う前に確認します。このワークショップでは、`--yolo` を使って Copilot CLI を再起動します。ユーザーが許可したこのショートカットにより、Codespace 内では承認の確認がなくなり、演習に集中できます。

> [!CAUTION]
> `--yolo` は、すべての自動権限 (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM などの分離された環境でのみ使い、日常の開発で使う既定のエイリアスにはしないでください。詳細は[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは、`--enable-all-github-mcp-tools` を使って、後のレッスンで Issue やプルリクエストを操作するための読み書き可能な GitHub MCP ツールを有効にします。Codespace はローカルコンピューターへのアクセスを制限しますが、認証された GitHub リソースは実際のものです。変更を公開またはマージする前にレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリのルートから、次のコマンドで再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 会話が動作することを確認するため、プロジェクトについて簡単な質問をします。その後、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で指示を変更したりエージェントを追加したりした後、`copilot --resume` を使って同じ機能の会話とブランチに戻ります。

## まとめと次のステップ

このレッスンでは、次の作業を行いました。

- npm で GitHub Copilot CLI をインストールした。
- GitHub アカウントで認証した。
- ワークショップのリポジトリを信頼し、簡単な会話を試した。
- 組み込みの GitHub MCP サーバーでフィルターの Issue を見つけた。

次は[最初の変更に取り組み][next-lesson]、Copilot CLI でゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
