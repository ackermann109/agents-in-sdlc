---
title: "レッスン 1 - GitHub Copilot app をインストールする"
description: "GitHub Copilot app をインストールし、テンプレートから作成したリポジトリを接続して、ワークスペースを確認し、クイックチャットを試します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** は、エージェント主導の開発に使うデスクトップ アプリです。GitHub Copilot CLI を基盤とし、GitHub とネイティブに統合されているため、リポジトリ、ブランチ、継続的インテグレーション (CI) パイプラインをすぐに利用できます。複数のエージェントを個別のワークスペースで並行して動かし、繰り返し作業を自動化するワークフロー向けに設計されています。Node.js をインストールし、プロジェクトのコピーを用意したので、次はアプリをインストールしてリポジトリを接続します。

このレッスンでは、次のことを行います。

- GitHub Copilot app をインストールしてサインインします。
- GitHub リポジトリからプロジェクトをアプリに追加します。
- テンプレートで準備されたバックログなど、ワークスペースを確認します。
- クイックチャットを試し、アプリについて質問します。

## シナリオ

チームは、増え続けるバックログに取り組むために AI エージェントを導入しています。Copilot app では、Issue の選択、エージェントの実行、変更のレビュー、プルリクエストのマージを一か所から指示できます。このレッスンでは、アプリをインストールして接続し、プロジェクトについての会話を始めます。

> [!NOTE]
> 対象となる Copilot プランが必要です。Copilot Free、Copilot Student、または有料プラン (Pro、Pro+、Business、Enterprise) を利用してください。Copilot Business または Copilot Enterprise の場合、管理者が **GitHub Copilot app** ポリシーを有効にする必要があります。

## GitHub Copilot app をインストールして構成する

GitHub Copilot app は Windows、macOS、Linux で利用できます。アプリをインストールして認証し、Tailspin Toys リポジトリを追加します。

1. ブラウザーで [GitHub Copilot app のランディング ページ][download-app]を開きます。
2. 使用しているプラットフォーム向けのアプリをダウンロードし、ページの手順に従ってインストールします。
3. インストール後、アプリを開きます。
4. **Sign in to GitHub** を選択し、画面の案内に従って認証します。データ所在地機能を使う GitHub Enterprise Cloud の場合は、**Use GitHub Enterprise** を選択し、要求されたら `*.ghe.com` ホスト名を入力します。GitHub Enterprise Server はサポートされていません。
5. オンボーディングでリポジトリを選択するよう求められたら、テンプレートから作成した Tailspin Toys リポジトリを選びます。リポジトリの選択をスキップし、後から追加することもできます。
6. テーマを選択するよう求められたら選び、オンボーディングを完了します。
7. サイドバーの **Sessions** でリポジトリを探します。すでに表示されていれば、再度追加する必要はありません。
8. リポジトリが接続されていない場合は、**Sessions** の横にある **+** を選択して新しいセッションを作成し、**Add GitHub repository** を選択します。前提条件のレッスンで控えた所有者とリポジトリ名を使います。
9. リポジトリがサイドバーに表示されたことを確認します。ここで使うレイアウトでは、`tailspin-toys` などのリポジトリ フォルダーが **Sessions** の下にまとまり、その下に個々のセッションが表示されます。

## ワークスペースを確認する

プロジェクトを接続したら、サイドバーにある各領域を確認します。

- **New** - Copilot との新しいチャット セッションを開始します。
- **Pull requests** - アプリの GitHub 統合でプルリクエストを表示します。アプリを離れずに参照、絞り込み、CI 状態の確認、レビューができます。
- **Issues** - Issue を参照、絞り込み、Issue からセッションを開始します。
- **Automations** - スケジュールまたはオンデマンドで実行する、保存済みのエージェント タスクです。やることリストや定期的な保守などを任せるのに役立ちます。まとめでは次のステップとして紹介しますが、別の演習ではありません。
- **Customize** - MCP サーバー、プラグイン、スキルなどを追加します。後で Playwright MCP を設定するときに使います。
- **Chats** - 独自のブランチやワークスペースを必要としない、簡単な質問やブレインストーミングのための会話です。最初のチャットを始めると表示されます。表示されていない場合は、**Sessions** の横の **+** を選択して最初のチャットを始めます。
- **Sessions** - リポジトリ フォルダーと、そのエージェント セッションをまとめます。リポジトリ名 (例: `tailspin-toys`) を選ぶとリポジトリ ビューが開き、その下のセッションを選ぶと会話に戻ります。各セッションは独立したワークスペースで実行され、変更を競合させずに複数のセッションを同時に動かせます。星評価を追加するレッスンで最初のセッションを開始します。

ワークショップを進めながら、ワークスペースの機能を確認していきます。

> [!TIP]
> 操作方法がわからない場合や、何かが可能か知りたい場合は、Copilot に質問してください。手順を案内してくれます。

### 用意されたバックログを確認する

Tailspin Toys のバックログを確認します。このバックログはテンプレートからリポジトリを作成したときに用意されています。

1. サイドバーの **Sessions** で、個別のセッションではなく Tailspin Toys のリポジトリ フォルダーを選択します。リポジトリの上部ナビゲーションで **Issues** を選択します。
2. Issue 番号を前提にせず、次のタイトルを探します。
   - Allow users to filter games by category and publisher
   - Update our repository coding standards
3. Issue を選択して詳細を読みます。各 Issue はエージェント セッションの開始点にもなります。簡単な最初の変更が終わったら、絞り込み Issue から作業を始めます。

> [!NOTE]
> **Issues** と **Pull requests** の一覧には、Copilot app に追加したリポジトリの項目だけが自動的に表示されます。他のリポジトリの作業項目を確認するには、そのリポジトリもアプリに追加してください。

## クイックチャットを試す

**quick chat** を使うと、ブランチや worktree を作成せずに質問やブレインストーミングができます。セッションを必要としない簡単な質問に適しています。

1. サイドバーの **Chats** の横にある **+** を選択し、新しいチャットを開きます。**Chats** がまだ表示されていない場合は、**Sessions** の横にある **+** で最初のチャットを始めます。チャット後に **Chats** が表示されます。
2. アプリのセッションについて質問します。

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 会話ビューで回答を読みます。各セッションは独立した Git worktree で実行されるため、変更を競合させずに複数のエージェントを並行して動かせます。会話は続けることも、新しいチャットを始めることもできます。

## まとめと次のステップ

GitHub Copilot app をインストールし、プロジェクトを接続して、ワークスペースを確認しました。このレッスンでは、次のことを行いました。

- GitHub Copilot app をインストールして GitHub にサインインしました。
- GitHub リポジトリからプロジェクトを追加しました。
- ワークスペースを確認し、**Issues** で用意されたバックログを見つけました。
- クイックチャットで簡単な質問をしました。

次は[最初のエージェント セッションを開始し][next-lesson]、ゲーム カードに星評価を表示します。

## リソース

- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app の使用を開始する][getting-started]
- [GitHub Copilot app のエージェント セッションを操作する][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
