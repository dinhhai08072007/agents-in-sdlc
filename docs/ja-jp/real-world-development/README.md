---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を繰り返し行います。

## シナリオ

ボードゲームのクラウドファンディングを提供する架空の会社、Tailspin Toys に新しく加わった開発者として作業します。チームのバックログは GitHub Issue として登録済みで、すぐに着手できます。フィルターやページネーションなどの機能開発に加えて、アクセシビリティやコーディング規約などの品質改善も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、普段の作業環境で利用できます。自分の開発スタイルに合う環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright MCP、Agent Merge、実用的なスラッシュコマンドを扱います。

### [GitHub Copilot app][app]

複数のエージェントセッションを並行して実行し、セッションモードを切り替え、Canvas で共同作業し、デスクトップアプリで GitHub Issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub Issue を非同期エージェントに割り当て、作業を指示し、進行状況を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使用します。エディターを離れずに、Copilot Chat の agent mode、MCP サーバー、カスタムエージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
