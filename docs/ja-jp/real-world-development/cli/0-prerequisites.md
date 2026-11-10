---
title: "レッスン 0 - 前提条件"
description: "Tailspin Toys の自分用コピーを作成し、Copilot CLI ワークショップ用に GitHub Codespace を準備します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI のレッスンを始める前に、必要な環境を準備します。Tailspin Toys リポジトリの自分用コピーを作成し、[Codespace][codespaces] を起動します。次のレッスンでは、統合ターミナルを使って Copilot CLI をインストールし、実行します。

このレッスンでは、次の内容を学びます。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成する。
- Codespace を作成し、プロジェクトの準備ができていることを確認する。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを使って作業します。[テンプレートリポジトリ][tailspin-template]からコピーを作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれています。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. **Use this template** を選択し、次に **Create a new repository** を選択して、自分用のコピーを作成します。
3. GitHub または Microsoft が主催するイベントの一環としてワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる Organization に新しいリポジトリを作成します。
4. 後でワークショップ内で使うため、作成したリポジトリのパス (`organization-or-user-name/repository-name`) を記録します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップ全体を通してこれらの Issue を使うため、自分で作成する必要はありません。

ワークショップのテンプレートの新しいコピーを使用してください。リポジトリの指示、アプリケーションコード、テスト、`quality-checks` スキル、バックログが含まれています。古いコピーを使用する場合は、必要なファイルが含まれているか進行役に確認してください。

## Codespace を作成する

次に、ワークショップの演習に使う Codespace を作成します。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードの作成、実行、デバッグができるクラウドベースの開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した高機能なエディターを利用できます。

1. 作成したリポジトリを開きます。
2. **Code** を選択します。
3. **Codespaces** タブを選び、**Create codespace on main** を選択します。
4. Codespace のセットアップが完了するまで待ちます。テンプレートがプロジェクトの依存関係、Playwright Chromium、ローカルデータベースをインストールします。
5. **Do you trust the authors of the files in this folder?** と表示された場合は、**Trust Folder & Continue** を選択します。
6. リポジトリルートでターミナルを開き、次のコマンドでアプリケーションを起動します。

   ```bash
   npm run dev
   ```

7. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択し、Tailspin Toys のサイトが読み込まれることを確認します。
8. ターミナルに戻り、<kbd>Ctrl</kbd>+<kbd>C</kbd> を押して開発サーバーを停止します。

## まとめと次のステップ

準備が整いました。このレッスンでは、次の作業を行いました。

- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成した。
- Codespace を作成し、プロジェクトの準備ができていることを確認した。

次は Codespace に[GitHub Copilot CLI をインストールし][next-lesson]、GitHub アカウントで認証します。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces の使用を開始する][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-install-copilot-cli/
[codespaces]: https://github.com/features/codespaces
