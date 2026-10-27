---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "GitHub Copilot app で最初のエージェントセッションを開始し、ゲームカードに小さな変更を加えて、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

前のレッスンでは、ワークスペースを確認し、クイックチャットを使いました。次は**エージェント セッション**を開始し、プロジェクトに最初の変更を加えます。ゲームのデータには星評価が含まれていますが、ホーム ページのゲーム カードにはまだ表示されていません。エージェントに表示を依頼し、変更をレビューして、最初のプルリクエストとしてマージします。

このレッスンでは、次のことを行います。

- エージェント セッションを開始し、セッションの構成を確認します。
- エージェントに小さく範囲の明確な変更を依頼します。
- ワークスペースの差分ビューで変更をレビューします。
- アプリをローカルで実行し、ブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、ゲーム詳細ページにはすでに表示されています。しかし、ホーム ページのゲーム カードにはタイトル、カテゴリ、publisher、説明しかありません。ウォームアップとして、各カードに既存の評価を表示させます。小さく独立した変更で、最初のセッションに適しています。

## セッションの構成

**セッション**は、個別のワークスペースで実行されるエージェントとの会話です。各セッションに専用の **git worktree とブランチ**が割り当てられます。そのため、あるセッションで機能を追加し、別のセッションでバグを修正するなど、変更を競合させずに並行作業できます。セッションはサイドバーでリポジトリごとにまとめられ、選択すると切り替えられます。

セッションには、エージェントとの**会話**、ファイルを調べて編集する**ツール アクティビティ**、差分を確認できる**変更ファイル一覧**が表示されます。

## セッションを開始して変更を依頼する

新しいセッションでプロジェクトを確認し、機能を実装します。[前のレッスン][prior-lesson]で GitHub リポジトリからプロジェクトを追加しました。そのリポジトリ用のセッションを作成して、変更を依頼します。

1. GitHub Copilot app に戻るか、アプリを開きます。
2. サイドバー上部の **+** を選択してセッションを開始します。
3. リポジトリに `tailspin-toys` を選びます。
4. プロンプト ボックスの下で、新しい working tree と **Interactive** モードを選択し、次のプロンプトを入力します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

5. <kbd>Enter</kbd> を押して、Copilot にプロンプトを送信します。

Copilot app は最初にプロジェクトの分離されたコピーである新しい worktree を作成し、必要なファイルを調べてコードを追加します。Copilot app を使って新機能を作成しました。

## 差分をレビューする

小さな変更でも、AI が生成した変更はマージ前にレビューします。Copilot app で差分を確認します。

1. アプリの右上にある **Toggle review panel** を選択します。開いたパネルで **Changes** を選択し、変更されたファイルと差分を表示します。

   ![Create PR の右側にある Toggle review panel ボタンを矢印で示した GitHub Copilot app の上部ツールバー](../../../_images/app-2-review-panel.png)

2. ゲーム カードの表示に使われる `GameCard.astro` にコードが追加されたことを確認します。評価がある場合は値を表示し、`starRating` が `null` の場合は `No rating yet` を表示する、次のような小さなブロックです。

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
> Copilot を含む生成 AI ツールは確率的に動作するため、実際のコードは上記と異なる場合があります。似た動作になっていることを確認してください。

## 変更を確認する

ブラウザーを開く前に、エージェントが実行した自動チェックの結果をレビューします。数値の `starRating` と、`null` の場合の代替表示をテストが対象にしていることを確認します。前提条件が不足している場合やチェックがスキップされた場合は成功ではありません。インストールの要求は内容を確認してから承認します。

コードを読むだけで動作すると判断せず、Copilot にサイトを起動させ、ブラウザー キャンバスで更新された UI を確認します。

> [!TIP]
> キャンバスは、Copilot app 内で使えるインタラクティブなウィジェットです。後のレッスンでカスタム キャンバスを調べて作成します。ここでは組み込みのブラウザー キャンバスを使います。

1. 次のプロンプトで、アプリを起動してブラウザー キャンバスを開くよう依頼します。

   ```plaintext
   Start the app and open it in the browser canvas.
   ```

2. しばらくするとアプリが起動し、Copilot app 内にブラウザー ウィンドウが開きます。
3. 評価があるゲーム カードに 5 段階評価が表示されることを確認します。
4. 終了したら、次のプロンプトでこのセッションが起動した開発サーバーを停止します。

   ```plaintext
   Stop the dev server and close the browser canvas.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成しました。新しいコードをコードベースにマージするため、プルリクエスト (PR) を作成します。

1. 右上にある **Create PR** を選択します。
2. 求められた場合は **Sign in with your browser** を選択し、案内に従って認証します。
3. Copilot から PR の作成に Issue の関連付けが必要だと説明された場合は、その手順に従うよう確認します。
4. Copilot が PR を作成するのを待ちます。
5. チャットのすぐ上にある **PR** バブルを選択し、レビューペインで PR を開きます。必要に応じて内容をレビューします。
6. 問題がなければ **Ready to merge** を選択します。
7. 表示されたダイアログで **Merge pull request** を選択し、PR をマージします。

## まとめと次のステップ

GitHub Copilot app で最初の変更をリリースしました。このレッスンでは、次のことを行いました。

- エージェント セッションを開始し、その構成を確認しました。
- エージェントにゲーム カードへの小さく範囲の明確な変更を依頼しました。
- ワークスペースの差分ビューで変更をレビューしました。
- アプリをローカルで実行し、ブラウザーで星評価を確認しました。
- 最初の PR を作成し、チェックをレビューして明示的にマージしました。

次は[絞り込み Issue から始め、Plan と Autopilot モードを使って][next-lesson]、より大きな機能を構築します。

## リソース

- [GitHub Copilot app でエージェント セッションを操作する][agent-sessions]
- [GitHub Copilot app について][about-copilot-app]
- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#github-copilot-app-をインストールして構成する
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
