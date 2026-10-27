---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: カスタム指示][previous-lesson]

技術的負債には、未解決のセキュリティ問題、更新が必要なレガシー コード、実装の時間が取れずにバックログに残った機能要望などがあります。GitHub Copilot cloud agent はコード更新や機能追加などを自律的に実行できます。作業が終わると、人間がレビューできるドラフトのプルリクエスト (PR) を作成します。時間のかかる作業を委任し、開発者がより大きな課題に集中できるようにします。

この演習では、Copilot cloud agent で次のことを行います。

- コード生成環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 範囲を明確に定めた Issue の重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には、対応が後回しになりやすい技術的負債があります。最初の開発を担当したチームがコードのドキュメントを整備していないため、まずアプリケーションのエクスポートされた関数に TSDoc ドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに同じカテゴリの他のゲームを表示し、UX の受け入れテストに使える状態にします。これは作業を妨げているものの、他に優先度の高い Issue があります。

このような優先順位が下がりやすい作業は、Copilot cloud agent に適しています。非同期で処理を任せ、開発者は別の作業を進めてから、結果をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) はバックグラウンドで作業します。[GitHub Issue を Copilot に割り当てる][assign-issue]と、Copilot は進行状況を追跡するドラフトのプルリクエスト (PR) を作成し、環境を準備して作業を始めます。実行中または完了後にセッションを確認できます。レビューの準備ができると、プルリクエストで通知されます。

## 明確な指示が重要な理由

GitHub Copilot を使っても、開発者の判断が不要になるわけではありません。一見簡単な作業でも、詳しく見ると複雑さがあります。[cloud agent に依頼するタスクの範囲を明確にしてください][cloud-agent-best-practices]。AI ペア プログラマーとして、段階を踏んで学び、試し、調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示が Copilot に与える影響を確認しました。まだ確認していない場合は、cloud agent に作業を割り当てる前に、リポジトリに含まれる指示ファイルを読み取り専用で確認します。

GitHub Web UI または起動済みの codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクションの TypeScript の規約と TSDoc コメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` に適用されることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てる際、生成された PR に TSDoc コメント、TypeScript の規約、ファイル ヘッダーが反映されているか確認します。

## Copilot cloud agent の開発環境を準備する

コードの作成には、必要なツールや依存関係を整える環境が必要です。cloud agent は作業時に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` の[特別なセットアップ ワークフロー][setup-workflow]で、開発ツールや依存関係を準備できます。このラボ用のワークフローは、Node.js、プロジェクトの依存関係、エンドツーエンド テスト用ブラウザー、移行とシードが済んだ SQLite データベースを用意します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます。
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 必要な手順に対して、可能な限り少ない権限を設定します。Copilot の操作には専用トークンが使われます。
    permissions:
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      - name: Set up the database
        run: npm run db:setup
```

このワークフローには、重要な点が 2 つあります。

- `copilot-setup-steps` という 1 つのジョブが、Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、Copilot の実行を待たずにリポジトリの **Actions** タブからワークフローを手動実行してテストできます。

## ドキュメントを追加する

ドキュメントが古い、または不足しているプロジェクトは多く、保守や新しい開発者の参加が難しくなります。Copilot にドキュメント作成を依頼し、後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` などのタイトルの PR を開きます。表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後に Copilot の TODO リストが表示されたら、内容と作業計画を確認します。

> [!NOTE]
> TODO リストが表示されるまで数分かかる場合があります。Copilot は、前述のワークフローの実行、プロジェクトの分析、問題の解決方針の決定を行っています。

13. PR のタイムラインを下にスクロールし、Copilot が Issue の作業を始めたことを確認します。
14. **View session** ボタンを選択します。

   ![Copilot のセッション ビュー](../../../_images/ex4-view-session.png)

15. Copilot のセッションを読み、コードの調査、計画、コード作成の様子を確認します。表示が更新されない場合は、ブラウザーを更新します。

cloud agent が非同期に作業している間、別の作業を進められます。次は関連ゲームの表示を別の Issue として割り当てます。

## ゲーム詳細ページに関連ゲームを追加する

cloud agent に別の作業を任せると、開発者は他のタスクに集中できます。ゲーム詳細ページに関連ゲームを追加する作業を Copilot に依頼します。

1. github.com のリポジトリに戻ります。
2. **Issues** タブ、次に **New issue** を選択します。
3. **Blank issue** を選択します。
4. **Title** に `Show related games on the game details page` と入力します。
5. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

6. **Create** を選択して Issue を作成します。
7. 右側にある **Assign to Copilot** を選択し、割り当てダイアログを開きます。
8. **Assign** を選択します。

Issue の最初のコメントに 👀 が表示されると、Copilot が作業を始めています。cloud agent は SWE と同じように非同期で作業するため、常に監視する必要はありません。作業が完了したらレビューします。次はカスタム エージェントを作成して使います。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents] を使うと、Issue を割り当てて非同期に作業させることができます。技術的負債への対応、新機能の作成、コードのフレームワーク移行などを依頼できます。

この演習では、次のことを行いました。

- コード生成環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確に定めた Issue の重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、次はカスタム エージェントを作成して使います。[Copilot cloud agent は Model Context Protocol (MCP) サーバーも利用でき][cloud-agent-mcp]、前の演習で確認したカスタム指示も使います。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
