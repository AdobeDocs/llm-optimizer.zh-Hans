---
title: 恢复内容可见性
description: 了解 LLM Optimizer 如何识别那些关键内容对 AI 代理隐藏的页面，以及如何使用基于边缘的优化恢复这种可见性。
feature: Opportunities
autotag-review: '2026-05-15T17:56:37.098Z'
TQID: 'https://experienceleague.adobe.com/rHqJL4RrJr1ghsy4fhXe-JLDrWruNSZgVhXQeRN-iyA'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: a0b5a505-2fd7-4c3d-b61c-b557fb6f0558id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2: id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 928
ht-degree: 100%

---


# 恢复内容可见性

AI 代理只能引用那些它能访问的内容。 如果页面上的关键内容隐藏在客户端渲染和动态加载（例如产品描述、用户评分、方法和评论）后面，AI 代理会完全忽略它们，这使可能引用它们的系统看不到这些有价值的内容。

“恢复内容可见度”机会可识别出您的网站上存在这种可见性差异的页面。 对于每一个受影响的页面，它会准确地为您显示 AI 代理视图中缺少哪些内容，并突出显示这一差异，使您在无需任何 CMS 更改或开发人员参与的情况下应用修复程序。

它会一目了然地显示三个关键量度：

- **URL**：识别到存在内容可见度差异的页面数。
- **估计的内容增益**：可以通过应用优化而恢复的内容的估计倍增值。
- **平均内容可见度**：当前对 AI 代理可见的内容在所有受影响的页面中的平均百分比。

![恢复内容可见度仪表板](/help/dashboards/opportunities/assets/recover-content-visibility-overview.png)

关于这个机会的视频概述，您可以观看[恢复内容可见度](https://www.youtube.com/watch?v=BigPyJssFCw)。

通过 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 可以优化这个机会。 优化仅传递给 AI 代理，对人类访客没有任何影响（仅限机器人的传递方式）。 之后可以在内容传递网络层上应用优化，无需 CMS 更改，几分钟内即会生效，无需开发人员参与，因此这是一种快速、低风险的部署方式。

## 工作原理

LLM Optimizer 会分析您的页面，将 AI 代理可以访问的内容与页面上实际存在的内容进行比较。 那些获得很高代理式流量，但内容可见度较低的页面会列在&#x200B;**包含建议的 URL**&#x200B;表格中，并按代理式流量确定优先级。

LLM Optimizer 会为每一个受影响的 URL 提供：

- **AI 分析**：说明哪些内容缺失以及为什么它对 LLM 可引用性很重要，并提供一个可恢复内容的引用列表
- **内容可见度**：此页面上当前对 AI 代理可见的内容百分比
- **内容增益率**：应用优化后可以被恢复的内容的估计倍增值。
- **预览**：并排比较 HTML，显示目前的页面与优化后的外观，使您可以在部署之前验证更改效果

修复的应用是通过 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md)，这是 Adobe 基于边缘的部署功能，在内容传递网络层上为 LLM 用户代理提供一个完整预渲染的 AI 友好的 HTML 快照，在不更改您的 CMS 的情况下恢复以前隐藏的内容。

<!-- [URLs with suggestions](/help/dashboards/opportunities/assets/recover-content-visibility-urls.png)-->

## 包含建议的 URL

**包含建议的 URL** 表中会列出所有受影响的页面，并可按类别筛选。 对于每一个 URL，您可以：

- **展开此行**，查看 AI 分析，包括缺少哪些内容以及为什么这很重要。
- **预览**&#x200B;并排比较当前页面与优化后版本的 HTML。
- 问题解决后，就可以&#x200B;**标记为已修复**。
- **忽略**&#x200B;那些不相关的建议。

这些建议分成三个视图：**当前建议**、**修复的建议**&#x200B;和&#x200B;**忽略的建议**。 建议部署后，会移至“修复的建议”，状态变为&#x200B;**已优化**，通过&#x200B;**实时查看**&#x200B;操作可以验证此优化是否已对代理式流量生效。 修复的建议也可以随时回滚。

![修复的建议，状态变为“已优化”](/help/dashboards/opportunities/assets/recover-content-visibility-fixed.png)

## 部署优化

审阅了建议并选择了要优化的 URL 后，点击&#x200B;**部署优化**&#x200B;就可以在内容传递网络边缘发布修复程序。 **部署到边缘**&#x200B;确认对话框中会显示选定的 URL 及其类型（预渲染器），以及将要应用的建议。 部署后会显示一个确认屏幕，用于确认哪些 URL 已成功优化。

>[!NOTE]
>
>部署优化过程需要完成 Optimize at Edge 加入过程。 如果您尚未加入，点击&#x200B;**部署优化**&#x200B;会引导您完成加入过程。 关于 Optimize at Edge 如何工作、受支持的内容传递网络提供程序以及加入过程的完整详细信息，请参阅 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 页面。

![”部署到边缘“对话框](/help/dashboards/opportunities/assets/recover-content-visibility-deploy.png)

## 在演示中尝试此操作

使用 Frescopa 演示环境了解“恢复内容可见度”机会的实际运作。

[在 Frescopa 演示中查看“恢复内容可见度”](https://play.llmo.now/org/demo-org/opportunities/prerender/75729489-756a-4c2b-9905-155b1715da5c)

## 常见问题解答

**为什么我的页面内容对 AI 代理是隐藏的？**

大多数现代网站在获得初始页面请求后需要 JavaScript 动态加载内容。 AI 代理通常不执行 JavaScript，因此内容渲染的客户端（产品列表、用户评论、博客文章链接和类似元素）AI 代理是看不到的，虽然人类访客可以完全看到。

**此优化是否会影响我的人类访客或 SEO 机器人？**

不。 Optimize at Edge 仅针对 AI 用户代理。 人类访客和 SEO 机器人将获得与之前完全一样的原始页面，他们的体验或性能不会发生任何变化。

**我需要更改 CMS 或者让开发人员参与吗？**

不。 优化会在内容传递网络边缘应用，无需进行创作更改、代码部署，也无需开发人员参与。 加入 Optimize at Edge 后，您可以在几分钟内直接在 LLM Optimizer 界面中部署，也可以随时撤消更改。

**如果部署之后我的页面内容发生变化，该怎么办？**

为恢复内容可见度，LLM Optimizer 使用低缓存 TTL 设置，因此您网站上的任何内容更新都会在几分钟内触发刷新。 AI 代理会一直获得最新版本的内容。

**如何开始使用 Optimize at Edge？**

请参阅 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 页面，了解完整的加入过程、内容传递网络配置指南和先决条件。
