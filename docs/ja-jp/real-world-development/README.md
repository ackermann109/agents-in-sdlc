---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更の計画、コードの作成、動作のテスト、プルリクエストのレビュー、エージェントを使った作業管理を繰り返し実践します。

## シナリオ

開発者として、ボードゲームのクラウドファンディングを提供する架空の会社 Tailspin Toys に加わります。チームのバックログは GitHub issue として登録済みで、すぐに取り組めます。ゲームの絞り込みやページネーションなどの機能に加え、アクセシビリティやコーディング標準などの品質改善が含まれています。

## 作業環境を選ぶ

GitHub Copilot は、開発者が作業する環境で利用できます。希望する開発環境を選んでください。各ワークショップは個別のセットアップから始まり、共通の Tailspin Toys シナリオを使用します。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、命令、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実用的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業を案内して進行状況を確認し、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。Copilot Chat の agent mode、Model Context Protocol (MCP) サーバー、カスタム エージェントをエディターから離れずに扱います。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
