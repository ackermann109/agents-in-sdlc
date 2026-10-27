---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュ コマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、省略可能なクラウドへの委任に使うスラッシュ コマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

他の CLI ツールと同様に、GitHub Copilot CLI にも多くのスラッシュ コマンドがあります。これらのコマンドを使って、追加機能、内部情報、構成オプションを操作できます。ここまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。次は他の便利なコマンドを確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認します。
- `/model` で利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並行作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認します。

## シナリオ

CLI の主なワークフローが完了しました。次に、コンテキストの管理、モデルの切り替え、セッションの共有、省略可能な [Copilot cloud agent][about-cloud-agent]への委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキスト ウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、スラッシュ コマンドを使って自分で確認したり圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリ ルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力し、結果を比較します。会話が短い場合は、大きな違いがないこともあります。

> [!NOTE]
> コンテキスト ウィンドウが埋まると、Copilot CLI は自動的に内容を圧縮します。自分のタイミングで実行するには `/compact` を使います。関係のない別タスクを始める場合は、`/clear` または `/new` で新しい会話を開始します。

## モデルを選ぶ

モデルによって得意なことが異なり、開発者ごとに好みも異なります。Copilot CLI で利用可能なモデルを確認して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで作業内容や知見を共有すると、AI ツールの活用方法を互いに学べます。`/share` は、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` のオプションを確認します。
2. セッションを共有する場合は、目的に合った出力先を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. 送信または公開する前に、エクスポートした内容を確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は省略可能です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 省略可能: 作業を並行化または委任する

ワークショップの主な作業は完了しました。Copilot CLI には、大きなタスク向けのコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分割して並行実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` はタスクを Copilot cloud agent に送り、非同期に作業させたり、プルリクエストを作成させたりできます。

これらのコマンドは追加の worktree やリモート作業を発生させる場合があるため、省略可能です。使用前に新しい会話でタスクの範囲を明確にし、結果を通常のワークフローでレビューします。非同期エージェント作業について詳しく知りたい場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

スラッシュ コマンドを使って Copilot CLI を構成し、セッションを共有し、内部の状態を確認しました。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot のコンテキスト管理を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並行作業、worktree、cloud agent への委任に使う省略可能なコマンドを確認しました。

他にも多くのスラッシュ コマンドがあります。次は[ここまでの内容を振り返り][next-lesson]、さらに学ぶための次のステップを確認します。

## リソース

- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
