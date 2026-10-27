---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に伝える背景情報がある場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」進めるかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、ここでは指示更新前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue で非同期に作業するため、リアルタイムで影響を比較するのは困難です。後の演習で cloud agent が作成したプルリクエストをレビューし、指示の影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの慣行に沿ってタスクを進められるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイル、要件、ライブラリ、コメントの好みなどを Copilot に伝えられます。リポジトリ全体または特定のファイル形式やタスクに指示を設定できます。

- `.github/copilot-instructions.md` は、リポジトリへのすべてのリクエストで Copilot に送られる指示ファイルです。技術スタック、プロジェクト概要、ベスト プラクティスなど、プロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語 (TypeScript や Astro など) や、UI コンポーネントまたはユニットテストの作成などのタスクに適用します。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルも[サポートしています][custom-instructions-support]。

指示ファイルの作成方法の詳細はこのワークショップの範囲外ですが、サンプル プロジェクトに代表的な構成があります。基本的には、次のように使います。

- `copilot-instructions.md` には、プロジェクトの説明、構成、全体的なコーディング標準などのプロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層など、特定のファイル形式やタスクに関する指示を記載します。
- 自然な言葉で、明確なガイダンスとコードの例を示します。

指示ファイルの使い方に決まった形はありません。プロジェクトに合う方法を試してください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業向けのファイルがあります。
>
> Copilot に指示ファイルの作成を支援させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、操作は環境によって異なります。
>
> テンプレートを探している場合は、指示ファイル、カスタム エージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、基本の `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開き、内容を確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データ層、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。`applyTo` フィールドがリポジトリ ルートからの glob であり、たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルに適用されることを確認します。
5. このプロジェクトのユニットテストに関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内ファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分け、Copilot に例を示せます。ここに記載されたパスは指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメント要件はまだありません。次の手順で TSDoc コメントとファイル ヘッダーのルールを追加します。

## リポジトリ標準を追加する

この演習で編集するのは、コード内に TSDoc ドキュメントコメントとファイル レベルのコメント ヘッダーを追加する、プロジェクト全体のルールです。まずブランチを作成します。cloud agent は Issue が対象とするブランチから指示を読み取りますが、ここでは変更を個別にコミットします。

1. codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` にプロジェクト レベルの情報を記載し、コード提案を改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクション (27 行目付近) を探し、現在の標準を確認します。コード内のドキュメントに関するルールがないため、生成されたヘルパーにドキュメントコメントが含まれていません。
3. 既存の標準の直後に、ファイル ヘッダーと TSDoc コメントを追加する次の行を挿入します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリ レベルの `.github/copilot-instructions.md` は全体的な指示に、`*.instructions.md` は特定の言語、ファイル形式、タスクに関する指示に使えます。今回のドキュメントコメントのルールのように、プロジェクト全体の標準はリポジトリ レベルのファイルに記載します。

## 指示の更新をコミット、プッシュしてマージする

cloud agent は Issue の対象ブランチにある指示ファイルを読み取ります。次の演習で Issue を割り当てたとき、Copilot は `main` からブランチを作成するため、指示の更新を `main` にマージしておきます。

1. ファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けたプルリクエストを作成してマージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップでは `main` に直接作業しても構いません。ブランチを使う手順は、実際のプロジェクトでの作業方法に近づけるために用意しています。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。残りの環境で cloud agent が作成する PR をレビューすると、この標準の影響を確認できます。

次は GitHub Issue を Copilot cloud agent に[割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot の指示をよりよく書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
