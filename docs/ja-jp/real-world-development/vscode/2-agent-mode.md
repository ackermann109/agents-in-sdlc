---
title: "演習 2 - Copilot agent mode で機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

小さなアプリケーションの変更でも、複数のファイルを更新し、テストを実行する必要があることがよくあります。開発者は通常、関連ファイルを探し、変更し、テストを実行して、問題を修正します。Copilot agent mode を使うと、こうした作業を IDE 内でより自律的に進められます。

Copilot はプロジェクト構成を調べ、必要なファイルを更新し、テストなどのタスクを実行して、見つかった問題を修正します。agent mode を使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、agent mode と cloud agent は異なる用途向けです。agent mode は IDE 内で作業し、素早くやり取りできます。cloud agent は Issue とプルリクエストを通じて非同期に作業し、チームの一員のように連携します。

この演習では、次のことを学びます。

- Copilot agent mode がプロジェクトを調べ、関連ファイルを見つけ、複数ファイルにまたがる変更を行う方法。
- GitHub Copilot agent mode が UI とデータ層にまたがる機能を実装する方法。
- コードベースにマージする前に、Copilot agent mode が生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリと publisher で絞り込めるようにします。前の演習では publisher ヘルパーを追加しました。今回は Copilot agent mode を使って、残りのデータ層、UI、テストの作業を完了します。

## Tailspin Toys サイトを実行する

変更を加える前に、Tailspin Toys サイトの現在の機能を確認します。

このサイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲーム一覧と詳細を表示する単一の Astro アプリで、ページはビルド時に静的 HTML として生成されます。ページの frontmatter から Drizzle ORM を使ってローカル SQLite データベースを直接参照します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、必要なインタラクティブ機能は標準 DOM API を使う小さな Astro `<script>` で追加します。

### サイトを起動する

`npm run dev` タスクを使うと、Astro の開発サーバーを起動できます。

1. codespace に戻ります。前の演習の後なので、`main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、codespace で新しいターミナルを開きます。
3. 絞り込み作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行してサイトを起動します。

   ```bash
   npm run dev
   ```

   Astro の開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くよう促すダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルの Astro サーバー アドレス `http://localhost:4321` を選択します。Mac では <kbd>Command</kbd>、Windows/Linux では <kbd>Ctrl</kbd> を押しながら選択します。

> [!NOTE]
> codespace では、ターミナルの localhost URL を選択すると `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは、Web サーバーを実行中の codespace へのプライベート トンネルです。

### サイトを確認する

サイトを起動したら、現在の機能を確認します。

- **ホーム ページ**には、ボードゲームの一覧、タイトル、画像、説明が表示されます。
- ゲームを選択すると**ゲーム詳細ページ**が開き、タイトル、説明、publisher、カテゴリなどが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なり、必要な場合は各手順で説明します。

テンプレートからプロジェクトを作成すると、Issue のバックログも自動的に用意されます。Copilot に項目を確認させます。

1. Chat ビューの agent 選択メニューで **Agent** を選択します。agent は複数ファイルにまたがる変更を計画、実装し、ターミナル コマンドやツールを実行できます。

   ![Chat ビューの agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップは特定のモデルを推奨するものではありません。作成時に Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案を一定にしやすくすることが目的ですが、LLM は確率的に動作するため、異なる提案が出る場合があります。これは想定内です。

> [!NOTE]
> Copilot が異なる MCP コマンドを使う場合がありますが、タスクを完了できれば問題ありません。

3. 次のプロンプトを送信し、GitHub リポジトリの Issue バックログを表示するよう Copilot に依頼します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての Issue を一覧表示するコマンドを実行するため、**Continue** を選択します。
5. 表示された Issue の一覧と優先順位を確認します。

## 指示ファイルを確認する

Copilot にコード生成を依頼する前に、コンテキストを与える指示ファイルを確認します。Web サイトへの機能追加方法を説明する [user interface (UI)][ui-instructions] ファイルを使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関する全体的なガイダンスを確認します。アーキテクチャの概要、コンポーネント設計、テスト容易性とアクセシビリティの原則、Astro、Tailwind CSS、Drizzle などの指示ファイルへのリンクが含まれています。

> [!TIP]
> 指示ファイルでは、別の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、大きな内容を小さく分け、例やテンプレートを示せます。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 絞り込み機能を実装する

絞り込み機能を完成させるには、複数箇所を更新します。データ層のロジックを追加または調整し、テストを更新し、ゲーム一覧ページに絞り込み UI を追加します。マージ前にテストも実行します。Copilot agent mode にこれらを依頼します。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューの agent 選択メニューで **Agent** が選択されていることを確認します。

   ![Chat ビューの agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、データ層、UI、テストなどの関連ファイルを特定して変更し、テストを実行する様子を確認します。

   ![Copilot がプロジェクトのファイルを調べるスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、完了まで数分かかることがあります。作業中にエディターに **Keep** と **Undo** が表示されても、すぐに選択する必要はありません。完了すると、変更ごとに選択できるようになります。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログのスクリーンショット](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、エラーを検出しなくなるまで進めます。
9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot や他の AI ツールが生成したコードは、必ずレビューしてください。

10. ブラウザーで実行中のサイトに戻り、新しい機能を試します。
11. 動作とコードを確認したら、Copilot Chat で **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot agent mode を使って Tailspin Toys に機能を追加しました。次のことを確認しました。

- GitHub Copilot agent mode がプロジェクトを調べ、関連ファイルを見つけ、複数ファイルにまたがる変更を行う方法。
- GitHub Copilot agent mode が UI とデータ層にまたがる機能を実装する方法。
- コードベースにマージする前に、生成された変更とテストをレビューする方法。

次は[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### おまけの演習 - ページネーションを実装する

ゲーム一覧が増えるとページネーションが必要になります。この演習で学んだ方法を使って、Copilot に実装を依頼してみましょう。次の点を検討します。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定にするかを決めます。
- 希望する実装に必要な情報をプロンプトに含めます。
- Copilot に変更を依頼し、コンテキストを追加して反復することもできます。これは通常の進め方です。

## リソース

- [Copilot の ask、edit、agent mode: それぞれの役割と使い分け][choose-mode]
- [VS Code の agent mode][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
