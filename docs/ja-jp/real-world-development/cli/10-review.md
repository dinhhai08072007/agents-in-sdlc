---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

継続的な Tailspin Toys の開発ワークフロー全体で GitHub Copilot CLI を使いました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されているフィルターの Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの Issue から始めて、Plan モードでアプローチを決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テストカバレッジ、スキルの結果、ブラウザーでの検証結果を評価しました。
- フィルター機能の変更全体をレビューし、Agent Merge による PR のマージを許可しました。
- コンテキスト、モデル、共有、任意の cloud agent への委任に使うスラッシュコマンドを確認しました。

## リリースした内容

このワークショップでは、更新した `main` からそれぞれ別のブランチを作成し、2 つの PR を作成します。

1. **星評価:** 既存の `starRating` と未評価の状態をゲームカードに表示します。
2. **フィルターと品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加します。

フィルターの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡略化するため、作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザーでの確認、MCP 経由の Copilot によるブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、追加した形式で結果を報告しました。QA はこれらの結果と要件およびテストカバレッジのレビューをまとめて PR の確認に使いました。

追加するテストは実際の不足を補うものである必要があります。QA を実行しても新しいテストが不要と判断される場合があります。マージを許可する前に、コードと根拠を確認し、変更後は影響を受ける検証結果を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。実装前にアプローチを考えるには **Plan**、範囲の絞られた変更を一緒に行うには **Interactive**、範囲の明確なタスクを任せるには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラと同じくらいコンテキストも重要です。何を作るのか、なぜ必要か、どのように作るかを明確に伝えると、出力が大きく変わります。

## さらに試す

主要なワークフローを確認しました。次の Copilot CLI の機能も試してみてください。

- `/review` でコードレビューエージェントに変更を分析してもらう。
- `/rubber-duck` で問題について相談し、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して調整する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

どのツールも、使い続けることで上達します。実際のコード、趣味のコード、長い間作ろうと思っていた小さなアプリなどで試してみてください。学んだことをチームと共有し、メンバーからも学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys をさらに拡張したい場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
