---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看存储库指令，添加文档规范，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相关指导。[指令文件][instruction-files]不仅说明希望编写什么代码，还说明代码应如何组织。完成筛选功能后，现在可以查看 Copilot 使用的指令、添加文档规范并将其应用于代码。

在本课中，你将：

- 查看存储库指令和按路径应用的指令文件如何传递给代理。
- 更新指令文件，确保遵循编码规范。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 记录用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应记录其 `Props` 接口约定；相关代码更改时，注释也应保持更新。
- 应保留现有格式和 lint 指南。

通过使用指令文件，可以让 Copilot 获取正确的信息，并遵循这些实践完成任务。

## 指令文件

自定义指令可为 Copilot 提供上下文和偏好，帮助它理解你的编码风格和要求。可以借此引导 Copilot 提供更相关的建议和代码片段，例如指定编码规范、库以及希望添加的注释类型。可以为整个存储库或特定文件类型创建指令，以提供任务级上下文。

指令文件有两种：

- `.github/copilot-instructions.md` 是发送给存储库中每个请求的单个指令文件。它应包含适用于大多数 Copilot Chat 或命令行界面（CLI）请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供指导，例如 TypeScript 或 Astro 文件、UI 组件或新的单元测试。

> [!NOTE]
> 其他指令格式及其支持情况因开发环境而异。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

入门项目已包含一组指令文件。先查看现有内容，再进行更改，以便观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件，留意项目简介和编码指导。此文件中的指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里包含针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置一个 glob，用于确定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和现有项目文件的链接。通过这种方式，可以将较大的指令集拆分成可复用的小文件，并提供可供 Copilot 参考的示例。

## 更新指令文件以符合团队规范

现有文件已经提供了良好基础，但仍有不足。修改核心文件 `copilot-instructions.md`，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约位于第 35 行。
2. 在该部分最后添加以下条目：

   ```markdown
   - 所有新编写的 TypeScript 代码都应包含用于文档说明的 TSDoc 注释。
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存指令更新后，退出并恢复筛选功能对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上，运行以下命令以恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选功能对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   我们刚刚更新了指令和代码指导。请根据这些指导更新你生成的代码。
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认它们准确说明了代码。

## 总结和后续步骤

你查看了 Copilot 如何从指令文件中获取上下文，并将一项新规范应用于功能代码。具体来说，你：

- 查看了存储库指令和按路径应用的指令文件如何传递给代理。
- 更新了指令文件，以确保遵循编码规范。
- 查看了指令文件对代码的影响。

接下来，自定义并运行可复用的 `quality-checks` 技能，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件和其他资源集合][awesome-copilot]

[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
