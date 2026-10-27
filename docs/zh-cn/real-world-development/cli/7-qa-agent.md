---
title: "第 7 课 - 创建并使用质量保证（QA）代理"
description: "创建一个质量保证（QA）自定义代理，结合议题需求、quality-checks 技能和 Playwright 模型上下文协议（MCP）。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在，将这些能力整合到一个自定义代理中，并为它定义清晰的质量保证（QA）流程。

在本课中，你将：

- 了解自定义代理如何使用指令、技能和模型上下文协议（MCP）工具。
- 创建并检查可复用的 QA 配置文件。
- 选择 QA 代理，并根据筛选功能议题审查其结果。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，一致地审查需求、代码质量、自动化检查、测试覆盖范围和浏览器行为。自定义代理可以协调这一 QA 流程，并生成可复用的报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。在本研讨会中，你将于 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

之前使用的自定义项各有用途：存储库指令描述团队规范；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置文件则告诉 Copilot 如何使用这些能力来评估需求并报告结果。它不会取代这些工具，也不需要新建对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建可复用的 QA 配置文件。该配置文件将定义 QA 执行的检查及必须遵守的边界。

1. 返回 Codespace，并确认筛选功能对话处于 Interactive 模式。
2. 让默认代理创建新的自定义代理：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义代理。它应根据议题和已商定的需求检查功能，遵循存储库指令，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   要求它将每项需求标记为通过、失败或受阻，并提供支持证据。修改实现代码前必须征求同意，且不得提交更改或创建拉取请求。

   现在只创建配置文件，供我审查。
   ```

## 检查配置文件

使用新代理前，先检查其配置文件，确认 Copilot 捕获了预期的 QA 工作流和边界。这样可避免代理在你只想验证功能时修改功能代码，或执行超出范围的工作。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选项，但添加后可以为代理提供清晰的显示名称。
3. 阅读配置文件指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，并且不会提交更改或创建拉取请求。
5. 如果生成的配置文件遗漏了上述职责或边界，请先让默认代理修改，再继续。

## 根据议题运行 QA

Copilot 命令行界面（CLI）会在对话启动时加载项目代理。创建配置文件后，恢复同一筛选功能对话并选择 QA，以便它使用已有的议题和规划决策。

1. 使用以下提示词启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的代理可能暂时不会显示在可用代理列表中，但仍然可以使用上述命令启用它。

2. 使用以下提示词让 QA 代理审查功能：

   ```plaintext
   根据议题和计划中的决策审查筛选功能。它是否已准备好创建 PR？
   ```

3. 等待 QA 代理完成工作。
4. 阅读代理生成的报告。

## 总结和后续步骤

你已在工作流中添加可复用的专用角色，并审查了它的工作。在本课中，你：

- 了解了自定义代理如何使用指令、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 代理，并根据筛选功能议题审查了它的结果。

现在，功能实现、技能更新、QA 配置文件、测试和验证报告均已准备好，可供审查。接下来，将这些内容[整合到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
