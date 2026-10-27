---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务必须以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它可以访问这些上下文。[指令文件][instruction-files]可为 Copilot 提供指导，让它不仅了解你希望完成什么，也了解你希望如何完成。

在本练习中，你将：

- 了解项目上下文、编码指南和文档规范如何通过存储库自定义指令及按路径应用的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一项新的存储库级规范。

> [!NOTE]
> 与 VS Code 和命令行界面（CLI）开发环境不同，这里不会运行更改前后的对比提示词。Copilot 云智能体会在 GitHub 议题上异步工作，因此难以实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，可以看到这些指令的作用。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面应采用深色模式并具有现代风格。
- 应使用 TypeScript 文档注释（TSDoc）记录代码。
- 每个文件开头都应添加注释块，说明文件的用途。

借助指令文件，可以确保 Copilot 获取正确的信息，并按照这些实践完成任务。

### 自定义指令

自定义指令可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。可以借此引导 Copilot 提供更相关的建议和代码片段，例如指定编码规范、库以及希望添加的注释类型。可以为整个存储库或特定文件类型创建指令，以提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是发送给存储库中每个请求的单个指令文件。它应包含适用于大多数 Copilot Chat 或 CLI 请求的项目级信息，例如所用技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供指导，例如 TypeScript 或 Astro 文件、UI 组件或新的单元测试。

> [!NOTE]
> 在集成开发环境中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令文件，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

创建指令文件的完整流程超出了本研讨会的范围，但示例项目展示了具有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目简介、项目结构和全局编码规范。
- 使用 `*.instructions.md` 文件指导特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务。
- 使用自然语言，确保指导清晰，并提供代码示例说明推荐或不推荐的写法。

指令文件没有唯一的编写方式，正如人工智能（AI）也没有唯一的使用方式。通过试验，你会逐渐找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应维护完善的指令文件。查看本项目中的文件时，你可能会发现有些文件针对特定任务，例如[用户界面（UI）更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同开发环境提供的方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。对应开发环境的课程会在相关处介绍这些功能。
>
> 如果需要模板或起点，可以查看 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

花些时间查看本存储库附带的指令文件：其中包括一个核心文件 `copilot-instructions.md`，以及一组适用于各种任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。特别查看 **Code standards** 下的 **GitHub Actions Workflows** 指导。这些内容适用于在此存储库中与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里包含针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于确定指令适用的文件。这里匹配任何 TypeScript 测试文件，例如符合 `**/*.test.ts` 的文件。
5. 查看该文件中针对本项目单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。留意指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。通过这种方式，可以将较大的指令集拆分成可复用的小文件，并提供可供 Copilot 参考的示例。此处的路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求添加代码文档。接下来，你将为 TSDoc 文档注释和文件注释头添加规范。

## 添加新的存储库规范

接下来只需编辑一个文件：添加一项项目级规则，要求使用 TSDoc 文档注释并在文件开头添加注释头。稍后本系列练习中的云智能体会据此处理你分配给它的议题。

编辑文件前，先创建一个分支（云智能体会从议题指定的分支读取指令，但仍建议单独提交这些更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码规范可以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分。它应位于第 27 行附近，用于记录项目编码规范；目前尚无代码文档规则，这也是生成的辅助函数没有文档注释的原因。
3. 在现有规范下方添加以下 Markdown 内容，以要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - 每个导出的函数都应包含 TSDoc 注释，说明其用途、参数和返回值。
   - 在导入语句或其他代码之前，为文件添加注释块以说明其用途。
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如前一课所示，可以在存储库级文件（`.github/copilot-instructions.md`）中编写全局指导，也可以在 `*.instructions.md` 文件中编写适用于特定语言、文件类型或任务的指导。你刚添加的文档注释规则属于项目级规范，因此适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。在下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将指令更改合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并将其合并。之后，云智能体处理下一练习分配的议题时就会读取这些指令。

> [!TIP]
> 如果想在本研讨会中直接使用 `main`，可以跳过分支操作并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

本练习中，你：

- 了解了项目上下文、编码指南和文档规范如何通过存储库自定义指令及按路径应用的指令文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加了一项新的存储库级规范。

接下来整个开发环境的练习中，云智能体创建的拉取请求会体现该规范。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
