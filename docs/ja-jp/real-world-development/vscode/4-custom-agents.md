---
title: "演習 4 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや領域に合わせて AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、特定の作業に役立つ指示、ベスト プラクティス、コーディング パターン、領域固有の知識を指定します。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを支援するセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントを作成できます。

カスタム エージェントは、プロジェクト内の `.github/agents` またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を記載する Markdown プロンプトが必要です。

### カスタム エージェントと agent skill の違い

カスタム エージェントと [agent skill][agent-skills-concept] には共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。大まかには、**カスタム エージェントは作業者、スキルはツール**です。

カスタム エージェントは独自のコンテキスト ウィンドウを持ち、スキルや他のエージェントを組み合わせて作業を進められます。このラボのアクセシビリティ カスタム エージェントは、アクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエストのワークフローやテスト実行を管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成方法は 1 つではありません。環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、誰もがクラウドファンディング プラットフォームを使えるようにしたいと考えています。現在のダークテーマは、テキストと背景色のコントラストが十分でなく、読みづらいという意見が寄せられました。そこでデザインチームは、高コントラスト モードを切り替えられるように依頼しました。

アクセシビリティは重要なため、カスタム エージェントを使って機能をすばやく実装します。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Copilot Chat でアクセシビリティ エージェントを使い、高コントラスト モードを実装します。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot をどのように支援するかを把握します。

codespace に戻り、ターミナルを開いて `main` からアクセシビリティ作業用の新しいブランチを作成します。演習 3 の絞り込み PR とは分けて作業します。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` の frontmatter が必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する主な責務、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題が記載されています。

## Copilot Chat でカスタム エージェントを使う

VS Code の Copilot Chat ビュー下部にある agent picker には、`.github/agents` で定義されたカスタム エージェントが表示されます。エージェントを選ぶと、その指示とツールを使ってチャットできます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なり、必要な場合は各手順で説明します。

1. Chat ビューの agent 選択メニューで **Agent** を選択します (すでに選択されている場合はそのまま進みます)。

   ![Chat ビューの agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部の agent picker を選択します。既定では有効なエージェントとして **default** が表示されます。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティ エージェントに送信します。

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat がコードベースを調べ、変更を提案してプロジェクト ファイルを編集する様子を確認します。各編集はファイル パスと差分を含めてチャットに表示されます。
6. 編集が追加されると、エージェントが変更した作業ファイルを示す **Files changed** の表示が更新されます。

> [!NOTE]
> この作業には数分かかることがあります。Copilot は Astro コンポーネント、CSS、関連テストなどのファイルを編集します。次の演習で進行中の作業をレビューし、方向付けします。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]は、特定のタスクや領域向けに調整した AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業を支援できます。

この演習では、次のことを行いました。

- 既存のアクセシビリティ カスタム エージェントをレビューしました。
- Copilot Chat でアクセシビリティ エージェントを使い、高コントラスト モードを実装しました。

次は[エージェントの作業を監視して方向付け][next-lesson]、変更を確認しながら、同じセッションにライトモードの切り替えを追加します。

## リソース

- [カスタム エージェントについて][custom-agents]
- [IDE でカスタム エージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタム エージェント][custom-agents-vscode]
- [カスタム エージェントの構成][custom-agents-config]
- [awesome-copilot のカスタム エージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
