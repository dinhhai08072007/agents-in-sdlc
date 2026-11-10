---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

ワークスペースを確認し、クイックチャットを試しました。次は**エージェントセッション**を開始し、プロジェクトに最初の変更を加えます。変更は小さくします。ゲームのデータにはすでに星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次の内容を学びます。

- エージェントセッションを開始し、セッションの構成を理解する。
- プロジェクトに小さく、範囲の明確な変更を加えるようエージェントに依頼する。
- ワークスペースの差分ビューで変更をレビューする。
- アプリをローカルで実行し、ブラウザーで変更を確認する。
- 最初のプルリクエストを作成してマージする。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホームページのゲームカードにはタイトル、カテゴリ、パブリッシャー、説明しか表示されていません。ウォームアップとして、各カードに既存の評価を表示するようエージェントに依頼します。これは小規模で自己完結した変更なので、最初のセッションにぴったりです。

## セッションの構成

**セッション**は、分離された独自のワークスペースで実行されるエージェントとの会話です。各セッションには**専用の git worktree とブランチ**が割り当てられます。そのため、一方で機能を追加し、もう一方でバグを修正するなど、変更を競合させずに複数のセッションを同時に実行できます。セッションはリポジトリごとにグループ化されてサイドバーに表示され、選択すると切り替えられます。

セッション内では、エージェントとの**会話**、ファイルを調査・編集する際の**ツールアクティビティ**、差分を含む**変更されたファイル**の一覧を確認できます。

## セッションを開始して変更を依頼する

新しいセッションを開始して、プロジェクトを調べ、機能を実装します。[アプリのセットアップ][prior-lesson]で、GitHub リポジトリからプロジェクトを追加しました。そのリポジトリに新しいセッションを作成し、変更を依頼します。

1. GitHub Copilot app に戻ります。アプリを閉じている場合は開きます。
2. サイドバーの上部にある **+** を選択してセッションを開始します。
3. リポジトリとして `tailspin-toys` を選択します。
4. プロンプトボックスの下で **new working tree** と **Interactive** モードを選びます。次のプロンプトを使って変更を依頼します。

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. <kbd>Enter</kbd> を押して、プロンプトを Copilot に送信します。

Copilot app は、新しい worktree (プロジェクトの分離されたコピー) を作成して作業を開始します。次にプロジェクトを調べ、新機能を追加するために必要なファイルを見つけ、コードを作成します。Copilot app を使って新機能を追加できました。

## 差分をレビューする

AI が生成した変更は、どれほど小さくてもマージ前にレビューします。Copilot app 内で変更を確認しましょう。

1. アプリの右上隅にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルと差分を確認します。

    ![Create PR ボタンの右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲームの表示に使う主要ファイル `GameCard.astro` にコードが追加されていることを確認します。評価があれば表示し、`starRating` が `null` なら `No rating yet` を表示する、次のような小さなブロックです。

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot を含む生成 AI ツールは確率的に動作し、決定論的ではないため、実際のコードは上記と異なる場合があります。ただし、比較的似た実装になるはずです。

## 変更を確認する

ブラウザーを開く前に、エージェントが実行した自動チェックの結果をレビューします。テストが数値の `starRating` と、`null` の場合の代替表示の両方を確認していることを確認します。前提条件が不足していたり、チェックがスキップされていたりする場合は成功ではありません。インストールの要求は内容を確認してから承認します。

コードを読んだだけで動くと判断するのではなく、Copilot に Web サイトを起動させ、ブラウザーキャンバスで更新された UI を確認します。

> [!TIP]
> キャンバスは、Copilot app 内で利用できるインタラクティブなウィジェットです。後のレッスンではカスタムキャンバスを調べて、自分でも作成します。まずは組み込みのブラウザーキャンバスを使います。

1. 次のプロンプトを使い、アプリを起動してブラウザーキャンバスでページを開くよう Copilot に依頼します。

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザーウィンドウが開きます。
3. 評価済みのゲームカードに、5 点満点の評価が表示されることを確認します。
4. 確認が終わったら、次のプロンプトを使い、このセッションで起動した開発サーバーを停止するよう Copilot に依頼します。

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 最初のプルリクエストを作成してマージする

機能を作成できました。次は、新しいコードを既存のコードベースにマージするためのプルリクエスト (PR) を作成します。

1. 右上隅にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、画面の指示に従って認証します。
3. リポジトリのポリシーにより、PR を作成する前に一致する Issue が必要だと Copilot から説明された場合は、その手順に従うよう確認します。
4. Copilot が PR を作成します。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインで PR を開きます。ここで必要に応じてレビューできます。
6. 準備ができたら **Ready to merge** を選択します。
7. 新しいダイアログで **Merge pull request** を選択し、プルリクエストをマージします。

## まとめと次のステップ

GitHub Copilot app で最初の変更をリリースしました。このレッスンでは、次の作業を行いました。

- エージェントセッションを開始し、セッションの構成を学んだ。
- ゲームカードに小さく、範囲の明確な変更を加えるようエージェントに依頼した。
- ワークスペースの差分ビューで変更をレビューした。
- アプリをローカルで実行し、ブラウザーで星評価を確認した。
- PR 1 を作成し、チェックをレビューして、明示的にマージした。

次は[フィルターの Issue から始めて、Plan と Autopilot モードを使い][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェントセッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
