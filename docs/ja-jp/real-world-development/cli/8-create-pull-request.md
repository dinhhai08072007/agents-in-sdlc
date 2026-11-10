---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルター、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルターの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチに保存されています。これらをまとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** にマージプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡略化するため、フィルター機能と品質ワークフロー全体を 1 つの会話とブランチにまとめ、この PR に含めます。

このレッスンでは、次の内容を学びます。

- Agent Merge とは何か、マージのライフサイクルを自動化する方法を理解する。
- 機能の変更全体と検証結果を確認する。
- フィルターの PR を作成する。
- レビューの後に Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルター機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys は、マージを許可する権限を開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージ可能にするための残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗を修正し、レビューコメントに対応し、必要に応じてリベースするなど、PR をブロックしている問題に取り組みます。その後、リポジトリが許可していれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せられます。作業をレビューし、マージの準備ができていることを確認してから Agent Merge を有効にしてください。

## Agent Merge で PR を管理する

コードの作成がすべて終わりました。まとめてレビューして PR を作成し、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功したら、マージを実行します。
7. `PR #14 was squash-merged successfully.` のようなメッセージが Copilot から表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、必要な権限を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合はレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発プロセスの一部を自動化しました。このレッスンでは、次の作業を行いました。

- Agent Merge とは何か、マージのライフサイクルを自動化する方法を理解した。
- 機能の変更全体と検証結果を確認した。
- フィルターの PR を作成した。
- レビューの後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に使える [Copilot CLI のスラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
