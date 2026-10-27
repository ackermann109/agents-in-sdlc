---
title: "第 12 课 - 使用 GitHub Pages 部署网站"
description: "使用 GitHub Copilot app 配置 Pages 工作流，审查其拉取请求，并验证 Space Quiz 是否已上线。"
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

使用 GitHub Pages 部署 Space Quiz，为它提供公开统一资源定位符（URL）。Copilot 可以准备工作流并引导部署，但仍需自行审查更改并确认网站已上线。

在本课中，你将：

- 让 Copilot 配置 GitHub Pages 部署。
- 在合并前审查工作流和拉取请求。
- 跟进部署并验证已发布的网站。

## 场景

Space Quiz 已在 GitHub 存储库中，但其他人还需要一种简单的方式来试用它。使用 GitHub Pages 发布这个单文件网站，无需添加服务器或运行时依赖项，即可通过公开 URL 访问。

## 让 Copilot 部署网站

1. 在 `space-quiz` 的 Copilot app 会话中，发送以下提示词：

   ```plaintext
   为此存储库启用 GitHub Pages。创建一个 GitHub Actions 工作流；如有需要，先构建网站，然后将其部署到 GitHub Pages。如果存储库中尚无 README，请添加一个，介绍此应用以及如何在本地运行它。使用这些更改创建拉取请求，并在必需检查通过后合并。持续监控部署，直到成功，然后报告实时网站的最终 URL。如果你没有启用 Pages 或合并的权限，请准确说明我需要批准或执行的操作；在网站上线之前，不要报告成功。
   ```

![三阶段示意图：已合并的 space-quiz 拉取请求；成功运行的 GitHub Actions“Deploy website”，其中包含“Build website”“Upload artifact”和“Deploy to Pages”步骤；以及已上线的 Space Quiz，地址为 yourname.github.io/space-quiz/。](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> 此提示词要求代理创建并合并拉取请求，以及更改存储库的 Pages 设置。继续操作前，请审查差异、部署权限和所有 GitHub 审批提示。

## 审查工作流并验证部署

1. 确认只有在存储库尚无 README 时才添加 README。
2. 审查拉取请求中的更改，包括工作流的构建和部署步骤。
3. 仅在必需的拉取请求检查通过后合并。
4. 跟进 GitHub Actions 部署运行，直到成功。
5. 打开报告的 URL，确认 Space Quiz 已加载。
6. 如果代理无法启用 Pages 或合并拉取请求，请按照它提供的具体说明操作。只有部署成功且网站可以正常加载后，任务才算完成。

## 总结和后续步骤

Space Quiz 现已作为实时网站发布，并且你已亲自验证部署，而不是只依赖代理的报告。你：

- 让 Copilot 配置了 GitHub Pages 部署。
- 在合并前审查了工作流和拉取请求。
- 跟进部署并验证了已发布的网站。

继续学习[第 13 课：回顾与后续步骤][next-lesson]。

[next-lesson]: ../13-review/
