---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Agent mode →][next-lesson]

生成 AI を使うときはコンテキストが重要です。タスクを特定の方法で進めたい場合や、Copilot に伝えておく背景情報がある場合は、その情報を参照できるようにします。[指示ファイル][instruction-files]を使うと、コード生成で「何を」するかだけでなく、「どのように」進めるかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクトのコンテキスト、コーディング規約、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を使って、フィルター機能の最初のデータスライス (publisher ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従う様子を確認します。
- 次の演習で Copilot が最新の指示を使えるよう、指示の変更とフィルターの一部を `main` にコミットします。

> [!CAUTION]
> 生成コードは、設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習では、出力を一字一句一致させるのではなく、指示の更新による傾向の変化を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発に関するガイドラインと要件があります。たとえば、次のとおりです。

- データ層には必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- TSDoc のドキュメントコメントをコードに追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿ってタスクを進められるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件などのコンテキストを Copilot に伝え、より適切な提案を得られます。リポジトリ全体または特定のファイル形式やタスクに対して指示を設定できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に渡される単一の指示ファイルです。多くのリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、言語やファイル形式、UI コンポーネントの作成やユニットテストなど、特定の種類の作業に適用する指示ファイルです。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示と `applyTo` frontmatter を持つ `*.instructions.md` ファイルをコード生成に使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルも[サポートしています][custom-instructions-support]。

### 指示ファイルを管理するためのベスト プラクティス

このプロジェクトのサンプルは、指示ファイルの構成例です。基本的には、次のように使います。

- `copilot-instructions.md` には、プロジェクト概要、構成、全体的なコーディング標準など、プロジェクト レベルの情報を記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データ層など、特定のファイル形式やタスクに関する指示を記載します。
- 明確で自然な言葉を使い、望ましいコードと避けるべきコードの例を示します。

指示ファイルの作り方は一通りではありません。プロジェクトや環境に合わせて試しながら調整してください。

> [!TIP]
> GitHub Copilot を使うプロジェクトには、十分な指示ファイルを用意してください。このプロジェクトには [UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまな作業向けのファイルがあります。
>
> Copilot に指示ファイルの作成を支援させることもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、利用する環境によって操作は異なります。具体的な手順は各演習で説明します。
>
> テンプレートを探している場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトの指示ファイルを確認する

まず、スターター プロジェクトに含まれる指示ファイルを確認します。リポジトリには、基本の `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指示も確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、適用対象を決める glob です。たとえば `**/*.test.ts` に一致する TypeScript のテスト ファイルに適用されます。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にある他の指示ファイル (`unit-tests.instructions.md` など) やプロジェクト内のファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分け、Copilot に例を示せます。ここに記載されたパスはリポジトリ ルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で追加します。

## 変更用のブランチを作成する

変更を作業ブランチに分けて、指示の更新と機能の作業を管理します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻って Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なり、必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示を更新したら、同じプロンプトをもう一度送信して違いを見ます。

1. 前の演習で開いたエディター タブを閉じ、Copilot に渡すコンテキストを整理します。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置先を Copilot に示します。
3. Chat ビューの agent 選択メニューで **Agent** を選択し、Copilot がファイルを編集できるようにします。

   ![Chat ビューの agent picker のスクリーンショット。](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを確認し、ヘルパー ファイルとテストなどに変更を適用する様子を確認します。
6. 提案されたヘルパーが、`db` クライアントを最初の引数として受け取り、型付きの publisher 配列を返す関数であることを確認します。これは `.github/instructions/drizzle.instructions.md` のデータ層の規約 (`src/lib/*.ts` に適用) によるものです。
7. 提案コードに TSDoc ドキュメントコメントとファイル レベルのコメント ヘッダーがないことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が高まることを確認するのが目的です。

## リポジトリ全体の標準を追加する

`.github/copilot-instructions.md` にプロジェクト全体の情報を記載し、コード生成の指針を改善します。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。27 行目付近にあります。既存の標準を確認し、コード内のドキュメント要件がないことを確認します。
3. 既存の標準の直後に、ファイル ヘッダーと TSDoc ドキュメントコメントを追加する次のルールを追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` は全体的な指示に、`*.instructions.md` は特定の言語、ファイル形式、タスクに関する指示に使えます。今回追加したドキュメントコメントのルールのように、プロジェクト全体に適用する標準はリポジトリ レベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

指示を追加した後、同じプロンプトをもう一度送信し、生成コードを比較します。

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと同じプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しい標準を伝え、次の演習の土台となる実際のコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のファイルと同様にソース管理します。変更とヘルパーをコミットしてリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押してターミナルを開きます。
2. 指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤に関して Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログにあるドキュメント標準の Issue に関連付けてプルリクエストを作成します。GitHub ツールを使える Copilot に作成を依頼します。

> [!NOTE]
> Copilot に GitHub 上の操作を依頼するのは今回が初めてです。プロジェクト テンプレートでは GitHub ツールがワークスペースに設定済みです。初回の使用時に VS Code から GitHub へのサインインを求められた場合は、案内に従って許可してください。後の演習で、この接続に使われる Model Context Protocol (MCP) について説明します。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、agent picker で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を探し、現在のブランチからその Issue を閉じるプルリクエストを作成するよう Copilot に依頼します。指示の変更と、今後のフィルター機能で使う publisher ヘルパーの基盤を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が情報を取得して操作を行う際に、確認を求められたら内容を確認して **Allow** を選択します。
5. プルリクエストが作成されたら、現在のブランチの変更をマージし、ローカルの `main` を最新にするよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

6. GitHub 操作やシェル コマンドの実行で確認を求められたら、内容を確認して適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージしました。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 既存の指示でコード生成プロンプトを送り、Copilot の出力を確認しました。
- publisher ヘルパー用のデータアクセス コードを生成しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再実行し、提案コードが新しい標準に従う様子を確認しました。
- 次の演習で変更を活用できるよう、指示とヘルパーの基盤を `main` にコミットしました。

次は[agent mode][next-lesson]を使って、Copilot にコードベース全体にまたがる機能を追加させます。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot の指示をよりよく書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
