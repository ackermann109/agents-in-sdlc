---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "絞り込み機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

絞り込み機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて 1 つのブランチにあります。まとめてレビューし、プルリクエストを作成します。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にプロセスを任せます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。ワークショップを簡潔にするため、絞り込みと品質の作業全体を 1 つのセッションとブランチにまとめ、この PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びます。
- 機能 PR 全体と検証の根拠を確認します。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認します。

## シナリオ

絞り込み機能の作業を通じて、Copilot で計画、実装、検証を行いました。Tailspin Toys は、マージの許可を開発者の管理下に置いたまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、GitHub Copilot app でプルリクエストをマージするための残りの作業を自動化します。有効にすると、アプリのセッションが PR を読み取り、失敗した継続的インテグレーション (CI) チェックの修正、レビュー コメントへの対応、必要に応じたリベースなど、マージを妨げている問題に対処します。GitHub が許可するとマージし、バックグラウンドで実行されます。アプリを再起動しても継続し、PR がマージされると自動的に停止します。

ここまでは自分で **Merge pull request** を選択していました。Agent Merge に任せることもできますが、コードの編集やマージには明示的な許可が必要です。許可する操作と作業を確認してからマージを許可してください。

## Agent Merge で PR を管理する

コードの作成とレビューが完了したら、Agent Merge に PR のプロセスを管理させます。

1. agent picker で **Default agent** を選択します。
2. **Create PR** の横にあるドロップダウンを選択します。
3. **Agent Merge** を選択します。ボタンが **Agent Merge** に変わります。
4. **Agent Merge** ボタンを選択して処理を開始します。ドロップダウンの選択だけでは始まりません。

Agent Merge は次の作業を行います。

- タイトルと説明を含む PR を作成します。
- Issue からセッションを開始した場合は、説明本文で関連 Issue に言及します。
- ターゲット ブランチにリベースするか、マージ競合に対処します。
- CI を監視し、すべてのチェックが成功することを確認します。
- 他の開発者や Copilot code review からのフィードバックを監視し、コメントを解決する変更を行います。
- すべて成功した後、必要に応じて PR を自動的にマージします。

チェックが成功したら PR もマージするよう Agent Merge に許可します。

5. マージを許可する前に、PR の差分と検証の根拠をレビューします。
6. **Agent Merge** の横にあるドロップダウンを選択します。
7. **Merge pull request** の横にチェックが付いていることを確認します。
8. 次のプロンプトでエージェントに明示的にマージを依頼します。

   ```plaintext
   Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
   ```

9. 処理の進行状況を確認し、報告された問題を解決します。続行前に PR の状態が **Merged** であることを確認します。マージ権限の有効化やエージェントの実行中表示だけでは、マージ完了の確認になりません。

> [!IMPORTANT]
> Agent Merge は、リポジトリの保護や権限不足を回避しません。続行前に問題を解決してください。

## まとめと次のステップ

コード生成、テストと検証、プルリクエストのプロセスなど、開発の複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージ ライフサイクルを自動化する方法を学びました。
- 機能 PR 全体と検証の根拠を確認しました。
- レビュー後にのみ Agent Merge を許可し、PR がマージされたことを確認しました。

次は[既存のキャンバスを使い、トリアージ キャンバスを作成します][next-lesson]。エージェントと一緒に作業を確認、計画、視覚化する方法を学びます。

## リソース

- [GitHub Copilot app で Issue とプルリクエストを管理する][managing-issues-prs]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
