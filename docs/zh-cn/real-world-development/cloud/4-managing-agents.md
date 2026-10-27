---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体处理了三项任务，分别用于改进用户体验和添加功能。云智能体可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个控制台中，可以查看所有当前有开放拉取请求（PR）的代理任务、了解它执行的操作，甚至引导正在运行的会话。

在本课中，你将：

- 查看代理页面并监控云智能体任务。
- 引导正在运行的会话，请求添加其他功能。

## 场景

将实现高对比度模式的任务分配给代理后，团队意识到也可以同时添加浅色模式。既然已经开始更新网站样式并添加切换功能，将其一并纳入实现很合理。现在，你将引导代理，要求它在实现高对比度模式的同时添加浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务及其状态。

1. 转到 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。这里应显示分配给 Copilot 的任务，包括：
   - 更新代码库文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置模型上下文协议（MCP）服务器。
   - 执行完成分配任务所需的各项操作。

> [!NOTE]
> 显示的具体步骤会因 Copilot 当前的工作状态和执行方式而异。

4. 同时留意右侧显示的拉取请求（PR）面板。你可以在此查看 PR 和已更改的文件，以便进一步监控。

## 引导云智能体

你已查看正在进行的任务。现在，在代理实现高对比度模式的同时，要求它添加浅色模式切换功能。

1. 选择负责添加高对比度模式的会话。确切标题会因 Copilot 使用的名称和当前工作状态而异。

    ![代理控制台中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，直到代理完成环境设置并开始工作。出现类似以下内容的消息时，表示它已开始处理任务。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示词：

    ```plaintext
    在实现高对比度模式时，也请添加浅色模式。此模式也应提供一个开关，供用户选择所需的显示模式。
    ```

    ![代理页面上的云智能体任务截图，其中高亮显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认收到提示词并将其纳入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务。它会在完成你发送消息时正在执行的步骤后，将新要求纳入工作流程。

这同样需要几分钟。等待期间，可以暂停一下，回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，也就是监控和引导 GitHub Copilot 云智能体任务的中心位置。你可以用这个控制台跟踪所有进行中和已完成的任务、审查正在执行的工作，以及重定向进行中的任务来调整范围或提供额外指导。

你了解了以下内容：

- 使用代理控制台和代理页面监控云智能体任务。
- 重定向正在运行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
