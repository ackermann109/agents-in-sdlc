---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや領域に合わせて人工知能 (AI) アシスタントを作成できます。リポジトリの `.github/agents` にある Markdown ファイルでエージェントを定義し、専門的な指示、ベスト プラクティス、コーディング パターン、領域固有の知識を指定します。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] 準拠を確認するアクセシビリティ エージェント、安全なコーディングを支援するセキュリティ エージェント、一貫したテスト パターンを守るテスト エージェントを作成できます。

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

Tailspin Toys は、視覚能力や好みにかかわらず誰もが使えるクラウドファンディング プラットフォームを目指しています。現在のダークテーマはテキストと背景色のコントラストが不十分で、読みづらいという意見が寄せられました。デザインチームは、高コントラスト モードを切り替えられるように依頼しました。

この機能をすばやく作成するため、アクセシビリティ用のカスタム エージェントを使います。

この演習では、次のことを行います。

- 既存のアクセシビリティ カスタム エージェントを確認します。
- Issue をカスタム エージェントに割り当てます。

## アクセシビリティ カスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認して、Copilot をどのようにガイドするかを把握します。

codespace に戻り、アクセシビリティ エージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` を含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタム エージェントには `name` と `description` の frontmatter が必要です。

3. 続くセクションを確認します。アクセシブルな Web サイトのコード生成に関する主な責務、ベスト プラクティス、HTML・CSS・JavaScript のコード例、よくある問題が記載されています。

## Issue を作成して割り当てる

Mission control では、Copilot cloud agent にタスクを割り当て、進行中の作業を監視して追加の指示を送れます。まず、高コントラスト モードを作るタスクを Copilot に割り当てます。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue 作成ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択します。
9. 一覧から **Accessibility agent** を選びます。

   ![カスタム エージェントを使った cloud agent の割り当てで、Accessibility が選択された画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を始めることを確認します。

## まとめと次のステップ

GitHub Copilot の[カスタム エージェント][custom-agents]は、特定のタスクや領域向けに調整した AI アシスタントです。チームの専門知識や標準を再利用可能なエージェントにまとめ、Copilot の作業をガイドできます。

この演習では、次のことを行いました。

- カスタム エージェントの定義方法を確認しました。
- Issue をカスタム エージェントに割り当てました。

Copilot が高コントラスト モードを実装している間に、次は[mission control からセッションを監視して方向付けます][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
