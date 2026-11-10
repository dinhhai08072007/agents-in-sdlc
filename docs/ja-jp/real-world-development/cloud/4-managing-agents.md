---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、ユーザーエクスペリエンスの改善や機能追加を目的とする 3 つのタスクを GitHub Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に作業しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の内容を学びます。

- エージェントページで cloud agent のタスクを監視する。
- 実行中のセッションに追加機能を依頼する。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加する良い機会だと考えました。サイトのスタイルと切り替え機能をすでに更新しているため、ライトモードも含めるのが自然です。エージェントに作業範囲を追加で伝え、ハイコントラストに加えてライトモードも実装してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインのタスクリストを確認します。次のような、Copilot に割り当てたタスクが表示されます。
    - コードのドキュメントを更新する。
    - ゲームを更新するための API を生成する。
    - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような作業が表示されます。
    - リポジトリからコードをチェックアウトする。
    - Copilot の作業環境を作成する。
    - MCP サーバーを設定する。
    - 割り当てられたタスクを完了するための各種手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用したアプローチによって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、進行状況を監視できます。

## cloud agent に追加の指示を送る

実行中のタスクを確認したので、cloud agent がハイコントラストモードを作成している間に、ライトモードの切り替えも追加するよう依頼します。

1. ハイコントラストモードを追加するセッションを選択します。Copilot が付けた名前や作業状況によって、タイトルは異なります。

    ![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業を開始したことが表示されるまで数分間セッションを確認します。開始すると、次のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページの cloud agent タスクで、Steer active session while Copilot is working ダイアログが強調表示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業計画に反映することを確認します。

## Copilot の作業を待つ

前回と同じように、Copilot が更新されたタスクに取り組みます。メッセージを送信した時点で実行中だった手順が終わると、新しい依頼が作業に反映されます。

今回も完了まで数分かかります。これまでに学習し、確認した内容を振り返りましょう。

## まとめと次のステップ

Copilot cloud agent のタスクを監視し、誘導するための中心的な場所である Copilot agents ページを確認しました。この mission control では、実行中および完了したタスクを追跡し、作業内容を確認して、作業中のタスクの範囲を変更したり追加の指示を送ったりできます。

次の内容を学びました。

- mission control とエージェントページで cloud agent のタスクを監視する。
- 実行中のセッションに追加機能を依頼する。

Copilot がアクセシビリティ機能を完成させている間に、[Copilot が作成したプルリクエストを改善する][next-lesson]方法を確認します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
