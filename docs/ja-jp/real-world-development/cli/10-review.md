---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys で一連の作業に取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、用意された絞り込み Issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み Issue から始めて Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルを調整して、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込みを確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選び、要件、カバレッジ、スキルの結果、ブラウザーの根拠を評価しました。
- 絞り込みの変更全体をレビューし、絞り込み PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、省略可能な cloud agent への委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

ワークショップでは、更新済みの `main` から作成した 2 つのブランチを使いました。

1. **星評価:** ゲーム カードに既存の `starRating` と未評価状態を表示します。
2. **絞り込みと品質のワークフロー:** 絞り込み機能を実装し、指示を更新して機能に適用し、`quality-checks` レポートを調整し、QA プロファイルを作成して関連テストを追加します。

絞り込みの計画から PR 作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## 検証の種類

自動テスト、自分で行うブラウザー確認、MCP 経由の Copilot によるブラウザー確認を実施しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果に要件とテスト カバレッジのレビューをまとめました。

追加するテストは、実際に不足しているカバレッジを補うものである必要があります。QA の実行で新しいテストが不要と判断される場合もあります。マージを許可する前にコードと根拠をレビューし、変更後は関連する根拠を更新します。

## ベスト プラクティス

Copilot に与えるコンテキストとツールが作業内容に影響します。このワークショップでは、指示を更新し、スキルを調整し、QA プロファイルを作成し、MCP サーバーを設定しました。会話をまたいでこれらのカスタマイズを再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し作業を説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前に方針を決めるとき、**Interactive** は範囲の狭い変更に関与し続けるとき、**Autopilot** は範囲が明確なタスクを任せるときに使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

ツールの種類にかかわらず、コンテキストは重要です。何を、なぜ、どのように作りたいかを明確に伝えると、出力が大きく変わります。

## さらに確認する

主なワークフローを終えました。Copilot CLI には他にも便利な機能があります。

- `/review` でコードレビュー エージェントに変更を分析させます。
- `/rubber-duck` で問題を整理し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを委任します。

## 次のステップ

どのツールも、使い続けることで習熟します。本番コード、趣味のコード、以前から作りたかった小さなアプリなどで活用してください。チームと知見を共有し、ドキュメントも確認しましょう。

ターミナルから Tailspin Toys を拡張する場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
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
