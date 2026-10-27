---
title: "演習 4 - エージェントを監視して管理する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の演習では、GitHub Copilot cloud agent にユーザー体験の改善と機能追加を依頼しました。cloud agent は非同期かつ自律的に作業しますが、進行中のタスクを監視することも重要です。

GitHub.com の[エージェント ページ][agents-page]など、cloud agent に割り当てたタスクを管理するためのツールがあります。mission control では、未完了のプルリクエスト (PR) があるエージェントのタスクを確認し、実行した操作を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

高コントラスト モードの実装を依頼した後、チームはライトモードも追加したいと考えました。すでにサイトのスタイルと切り替え機能を変更しているため、同じ作業に含めるのが合理的です。エージェントに追加で指示し、高コントラストに加えてライトモードも作成させます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の領域に表示されるタスクを確認します。コードのドキュメント追加、ゲームを変更する API の生成、Web サイトへの高コントラスト モード追加などのタスクが表示されます。
3. 実行中のタスクを選択し、Copilot が行った操作を確認します。リポジトリのチェックアウト、環境の作成、MCP サーバーの設定、タスクを完了するための操作などが含まれます。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) パネルで、PR と変更されたファイルを確認します。

## cloud agent に指示を追加する

進行中の作業を確認したので、高コントラスト モードにライトモードも追加するよう Copilot に依頼します。

1. 高コントラスト モード追加のセッションを選択します。タイトルは Copilot が付けた名前や作業状態によって異なります。

   ![mission control の Accessibility セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まるまで、セッションを数分確認します。作業開始後はメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![cloud agent タスクの画面で、作業中のセッションに指示するダイアログを示すスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot が依頼を認識し、作業に含めたことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で実行中の手順が終わると、新しい依頼が作業に反映されます。

## まとめと次のステップ

Copilot agents ページを使って、cloud agent のタスクを監視し、作業中に方向付けしました。mission control では、進行中または完了したタスクを追跡し、作業をレビューし、進行中のセッションの範囲を調整できます。

このレッスンでは、次のことを行いました。

- mission control とエージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は Copilot が作成したプルリクエストを[反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
