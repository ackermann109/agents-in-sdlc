---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、GitHub Copilot にコードのドキュメント追加、デザインチーム向けの関連ゲーム機能、高コントラストとライトモードを含むアクセシビリティ機能を依頼しました。次はコード変更を確認し、必要に応じて Copilot に改善を依頼します。

### シナリオ

生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードは必ずレビューし、通常の DevOps プロセスに従います。Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能を確認してから、チームのレビューを有効にします。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は非同期に、監視されずに作業するため、安全性を保つ制約があります。

- Copilot はリポジトリへの読み取りアクセスと、作業ブランチへの書き込みアクセスのみを持ちます。
- cloud agent は GitHub Actions 内に一時的な環境を作成して実行します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- 既定では [外部リソースへのアクセスが制限されます][agent-firewall]。MCP サーバーへのアクセスも制限されます。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) で、コードにドキュメントを追加した変更を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR に GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。cloud agent は既定で MCP サーバーなどの外部リソースへのアクセスが制限されるため、これは想定内です。必要であれば、[cloud agent のファイアウォールを構成または無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot が作業中の場合、PR に **[WIP]** が付いています。作業が完了するまで待ちます。数分かかることがあります。

4. 準備ができたら **Files changed** タブを選択し、変更をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 追加された TSDoc ドキュメントコメントなどを確認します。変更内容は状況によって異なります。演習 2 で確認した指示ファイルによる TSDoc コメント、TypeScript 規約、ファイル ヘッダーがあるかを確認します。
6. 問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を探します。
8. ワークフローの承認が必要な場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローが追加されることを確認します。単一の Astro アプリのプロジェクト チェックが成功するまで数分待ちます。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームメンバーと同様に、PR コメントやコードのインライン コメントで Copilot にメンションすると、新しいセッションで変更に対応します。生成結果は確率的なため、必ず一致するプロンプトはありません。次のような変更を依頼できます。

- 各コード ファイルの先頭に、役割を短く説明するコメント ヘッダーを追加します。
- TypeScript と Astro ファイルに TSDoc ドキュメントコメントを追加します。
- Astro アプリの構成を説明する README を作成します。

1. PR に変更のコメントを追加し、他のメンバーと同じように **@copilot** をタグ付けします。上記の例や、コードベースのドキュメントに関する別の依頼を使います。
2. **View Session** を選択し、Copilot が更新する様子を確認します。Copilot は変更用に新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![プルリクエストに戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了したら、PR に新しいコミットがあることを確認します。
5. **Files changed** タブを選択し、変更をレビューします。

満足するまで反復できます。準備ができたら、ドラフト PR をレビュー可能な状態にして `main` にマージできます。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 変更に問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. 承認が必要な場合は **Approve and run workflows** を選択します。
8. PR のチェック セクションでワークフローが実行され、単一の Astro アプリのチェックが成功することを確認します。
9. **省略可能:** Codespace でこのブランチに切り替えて手動テストできます。ターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot は関連ゲーム機能を実装しました。PR の **Conversation** タブにコメントを追加し、関連ゲームの表示件数やファイル ヘッダー、TSDoc コメントなどの変更を依頼できます。この Issue を割り当てた時点では、カスタム指示はまだ更新していません。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントが実装した機能を確認します。この PR には、演習 3 で依頼した高コントラスト モードと、演習 4 で追加依頼したライトモードの両方が含まれます。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、PR に **[WIP]** が付いています。完了するまで数分待ちます。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点を確認します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存するローカルストレージの使用。
   - 高コントラスト モードとライトモードの CSS またはスタイルの変更。
   - Accessible Rich Internet Applications (ARIA) 属性 (ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 変更に問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. 承認が必要な場合は **Approve and run workflows** を選択します。
9. ワークフローが PR のチェック セクションに追加され、単一の Astro アプリのチェックが成功することを確認します。
10. **省略可能:** Codespace でブランチに切り替えてアクセシビリティ機能を手動テストします。ターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   アプリを起動し、ブラウザーで高コントラストとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントは、ガイドラインに沿った実装を Copilot に促します。改善点があれば、他の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 - さらに委任する

cloud agent に実際のバックログ項目を任せ、自分は他の作業に取り組む方法を練習します。いくつか Issue を作成し、Copilot に割り当ててみてください。

- ゲーム詳細ページに支援者向けの関心フォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセス ヘルパーに入力検証とエラー処理を追加します。

## まとめ

Cloud agent 環境の演習を完了しました。このレッスンでは、次のことを行いました。

- **リポジトリにあるカスタム指示ファイルを確認し、** cloud agent の出力に影響することを確認しました。
- **GitHub Issue を Copilot cloud agent に割り当て、** 環境の準備、計画、非同期の実行を確認しました。
- **カスタム エージェントを作成して使い、** 高コントラストとライトモードを追加しました。
- **Copilot agents ページを mission control として使い、** セッションを監視して作業中に方向付けました。
- **cloud agent の PR をレビューして反復し、** `@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent 環境の演習は完了です。他の環境では、ここで扱った内容に関連するシナリオに取り組めます。

- 🖥️ **[VS Code 環境](../../vscode/)** — IDE で Copilot Chat agent mode と MCP 統合を試します。
- 💻 **[CLI 環境](../../cli/)** — Plan、Autopilot、agent skill、QA カスタム エージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` を使い、ターミナルから変更をレビューしてリリースします。

自分のリポジトリで、次のアイデアを試すこともできます。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てます。
- 別の分野のカスタム エージェントを作成します。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
