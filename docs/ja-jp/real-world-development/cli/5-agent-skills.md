---
title: "レッスン 5 - quality-checks スキルを調整して使う"
description: "既存の quality-checks スキルを調べ、レポート形式をカスタマイズして、絞り込み機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くこと以外にも、テスト、lint、継続的インテグレーション (CI) の他の作業があります。これまでは手動でコードを確認し、指示ファイルでコーディング標準に従うようガイドしました。こうした作業には**エージェント スキル**が適しています。スキルを使うと、Copilot に適切な操作方法を伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを調べます。
- 結果のレポート形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームは結果を読みやすくするため、レポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの指示、実行可能なスクリプト、サポート リソースをまとめられます。基本的には、スキル名のフォルダーと、その中の `SKILL.md` で構成されます。Markdown には、スキルの内容を定義する `name` と `description` を含む frontmatter、機能の概要、呼び出すタイミングのガイダンスを記載します。必要に応じて、スクリプトなどのリソース用サブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、既存のプロジェクト コマンドを使うため、`SKILL.md` のみで構成されています。

スキルは、チームで共有して再利用できるプロジェクトの `.github/skills` フォルダー、またはユーザー用の `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックの実行に使う `quality-checks` スキルを確認します。

1. Codespace に戻り、Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。description は Copilot がスキルをいつ呼び出すか判断する助けになります。
3. 指示を読み、Copilot がテストと lint を実行する方法を確認します。
4. まだ **Results output formatting** セクションがないことを確認します。

## 変更前にスキルを実行する

Copilot CLI では、スキルを直接呼び出すことも、自然言語で依頼することもできます。3 種類のチェックを実行するよう依頼します。

1. Interactive モードの絞り込み会話に戻ります。
2. 次のプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが報告した内容を把握できるようにレポートを改善します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルが自動的に保存されたことを確認します。

## 更新したスキルを実行する

Copilot CLI は会話を再起動せずに、編集したスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを送信します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されたレポートを読み、最初の結果と比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして実行しました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェックを実行する `quality-checks` スキルを調べました。
- 結果のレポート形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は絞り込みの機能 PR に含めます。次は [Playwright MCP サーバー][next-lesson]を使い、Copilot がサイトを直接操作できるようにします。

## スキルの例

コミュニティの次の例は参考情報であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューション ワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
