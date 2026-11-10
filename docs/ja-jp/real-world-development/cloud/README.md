---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウド上で非同期に作業できます。GitHub で作業を割り当てると、cloud agent がバックグラウンドでリポジトリを調べ、変更を加え、プルリクエストを作成します。その間、別の作業を続けられます。

各演習では、まず cloud agent が従うカスタム指示を追加し、その後 GitHub Issue を割り当てて実装を任せます。カスタムエージェントをレビューしてアプローチを調整し、agents ダッシュボードからセッションを監視・誘導したうえで、プルリクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと Codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加します |
| [2. Cloud Agent][ex2] | 非同期エージェント | Issue を GitHub Copilot cloud agent に割り当てます |
| [3. カスタムエージェント][ex3] | 特化エージェント | カスタムエージェントをレビューして使用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントのセッションを監視して誘導します |
| [5. 反復][ex5] | レビュー | PR をレビューし、Copilot の作業を改善して次の手順を選択します |

## 前提条件

このワークショップに参加する前に、次の条件を満たしていることを確認してください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは Organization で GitHub Copilot cloud agent が有効になっていること

> [!TIP]
> 有料プランを利用していませんか？認証済みの学生は[GitHub Education][callout-student-plan-education]から GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使う agent、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どのハーネスも完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は **Copilot Student、Pro、Pro+、Business、Enterprise** で利用できます。Copilot Free では利用できません。Copilot Business または Enterprise では、管理者が Organization で有効にする必要があります。

> [!NOTE]
> このハーネスでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハーネス](../cli/)または [VS Code ハーネス](../vscode/)を参照してください。

## 始める

**[演習 0: 前提条件から始める →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
