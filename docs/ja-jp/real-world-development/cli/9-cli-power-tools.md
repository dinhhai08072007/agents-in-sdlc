---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドが多数あります。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。他にも役立つコマンドをいくつか確認しましょう。

このレッスンでは、次の内容を学びます。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認する。
- `/model` で利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並行作業、ワークツリー、cloud agent への委任に使える任意のコマンドを確認する。

## シナリオ

CLI の主なワークフローを終えました。次は、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大きなタスクや複雑なタスクでは、モデルのコンテキストウィンドウの上限に達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI を開いていない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分割されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキストウィンドウがいっぱいになると、Copilot CLI は自動的にコンテキストを圧縮します。自分のタイミングで圧縮する場合は `/compact` を使います。関係のない別のタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示し、使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学んだことを共有すると、全員が AI ツールをより効果的に使えるようになります。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` オプションを確認します。
2. セッションを共有する場合は、`/share file` を使ったローカルの Markdown エクスポートなど、目的に合う方法を選びます。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡張または委任する

ワークショップの主要な内容は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行します。
- `/worktree` は、別のタスク用に分離された Git ワークツリーを作成します。
- `/delegate` は、非同期で作業してプルリクエストを開くこともある Copilot cloud agent にタスクを送信します。

追加のワークツリーやリモート作業が作成される可能性があるため、これらのコマンドは任意です。試すときは、範囲の明確な新しいタスクから始め、通常のワークフローに沿って結果をレビューしてください。非同期エージェントの作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使って、ツールを構成し、セッションを共有し、内部の処理を確認できます。このレッスンでは、次の作業を行いました。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認した。
- `/model` で利用可能なモデルを確認した。
- `/share` でセッションをエクスポートまたは共有する方法を学んだ。
- 並行作業、ワークツリー、cloud agent への委任に使える任意のコマンドを確認した。

Copilot CLI には他にも多くのスラッシュコマンドがあります。次は、学んだ内容を[振り返り][next-lesson]、学習を続けるための方法を確認します。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
