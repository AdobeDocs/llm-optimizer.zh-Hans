---
title: 恢复内容可见性
description: 了解LLM Optimizer如何识别关键内容对AI代理隐藏的页面，以及如何使用基于边缘的优化恢复该可见性。
feature: Opportunities
autotag-review: '2026-05-15T17:56:37.098Z'
TQID: 'https://experienceleague.adobe.com/rHqJL4RrJr1ghsy4fhXe-JLDrWruNSZgVhXQeRN-iyA'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: a0b5a505-2fd7-4c3d-b61c-b557fb6f0558
  - id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2:
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 928
ht-degree: 1%

---


# 恢复内容可见性

AI代理只能引用其可访问的内容。 当页面上的关键内容隐藏在客户端渲染和动态加载（例如产品描述、用户评级、菜谱和评论）后面时，人工智能代理会完全忽略它们，从而使可能引述它们的系统看不到有价值的内容。

恢复内容可见度机会可识别您网站上存在这种可见性差距的页面。 对于每个受影响的页面，它都会精确显示AI代理视图中缺少哪些内容，突出显示差距，并允许您应用修复程序，而无需任何CMS更改或开发人员参与。

它一眼便会显示三个关键量度：

- **URL** — 识别为内容可见度间隙的页数。
- **估计的内容增益** — 可以通过应用优化恢复的内容估计乘数。
- **平均内容可见度** — AI代理当前在受影响的页面中可见内容的平均百分比。

![恢复内容可见度仪表板](/help/dashboards/opportunities/assets/recover-content-visibility-overview.png)

有关此机会的视频概述，您可以观看[恢复内容可见度](https://www.youtube.com/watch?v=BigPyJssFCw)。

通过使用Edge上的[优化](/help/dashboards/optimize-at-edge/overview.md)，可以优化此机会。 优化仅交付给AI代理，对人类访客没有影响（仅限机器人交付）。 优化然后可以在CDN层应用，无需CMS更改，并且可在几分钟内生效，无需开发人员参与 — 这使得它成为一种快速、低风险的部署。

## 工作原理

LLM Optimizer通过将人工智能代理可以访问的内容与页面上实际存在的内容进行比较，来分析您的页面。 接收高代理流量但具有较低内容可见度的页面出现在带有建议&#x200B;**表的** URL中，按代理流量排定优先级。

对于每个受影响的URL，LLM Optimizer提供：

- **AI分析** — 有关哪些内容缺失以及它对LLM可访问性重要原因的描述，以及可恢复的内容引用列表
- **内容可见度** — 该页面上AI代理当前可见内容的百分比
- **内容增益比** — 如果应用了优化，则估计的可恢复内容的乘数
- **预览** — 并排的HTML比较，其中显示页面现在与优化后的外观，因此您可以在部署之前验证更改

此修复程序是使用[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)应用的 — Adobe的基于边缘的部署功能，此功能为CDN层的LLM用户代理提供完全预渲染、对AI友好的HTML快照，恢复以前隐藏的内容而不会接触您的CMS。

<!-- [URLs with suggestions](/help/dashboards/opportunities/assets/recover-content-visibility-urls.png)-->

## 包含建议的 URL

带有建议&#x200B;**的** URL表列出了所有受影响的页面，并可按分类进行过滤。 对于每个URL，您可以：

- **展开行**&#x200B;以查看AI分析，包括缺少什么内容以及内容重要的原因。
- **预览**&#x200B;当前页面与优化后版本的HTML并排比较。
- 一旦问题得到解决，**标记为已修复**。
- **忽略不相关的**&#x200B;建议。

建议分为三个视图：**当前建议**、**固定建议**&#x200B;和&#x200B;**忽略的建议**。 部署建议后，该建议将移至状态为&#x200B;**已优化**&#x200B;的固定建议和&#x200B;**查看实时**&#x200B;操作，以验证对代理流量进行的优化是否已启动。 修复的建议也可以随时回滚。

![已修复状态为“已优化”的建议](/help/dashboards/opportunities/assets/recover-content-visibility-fixed.png)

## 部署优化

查看建议并选择要优化的URL后，单击&#x200B;**部署优化**&#x200B;以在CDN边缘发布修补程序。 **部署到Edge**&#x200B;确认对话框显示选定的URL、其类型(Prerender)以及正在应用的建议。 部署后，确认屏幕会确认哪些URL已成功优化。

>[!NOTE]
>
>部署优化需要完成Optimize at Edge载入流程。 如果您尚未载入，单击&#x200B;**部署优化**&#x200B;会将您引导至载入流程。 有关在Edge中优化的工作方式、支持的CDN提供商以及载入流程的完整详细信息，请参阅[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)页面。

![部署到Edge对话框](/help/dashboards/opportunities/assets/recover-content-visibility-deploy.png)

## 在演示中尝试

使用Frescopa演示环境查看正在进行的“恢复”内容可见度机会。

[在Frescopa演示中查看恢复内容可见度](https://play.llmo.now/org/demo-org/opportunities/prerender/75729489-756a-4c2b-9905-155b1715da5c)

## 常见问题解答

**为什么对AI代理隐藏我的页面内容？**

大多数现代网站依赖JavaScript在初始页面请求后动态加载内容。 人工智能代理通常不执行JavaScript，因此呈现客户端的内容（产品列表、用户评论、博客文章链接和类似元素）永远不会被人工智能代理看到，即使人类访客可以完全看到该内容。

**此优化是否会影响我的人类访客或SEO机器人？**

不。 在Edge中优化仅针对AI用户代理。 人类访客和SEO机器人将完全按照之前接收原始页面，并且不会更改其体验或性能。

**是否需要更改我的CMS或让开发人员参与？**

不。 优化在CDN边缘应用，无需创作更改、代码部署或开发人员参与。 在Edge上线优化后，您可以在几分钟内直接从LLM Optimizer界面部署和回滚更改。

**如果我部署后页面内容发生更改，会发生什么情况？**

对于恢复内容可见度，LLM Optimizer使用低缓存TTL设置，因此网站上的任何内容更新都会在几分钟内触发刷新。 AI代理将始终接收最新版本的内容。

**如何开始使用Edge中的优化功能？**

有关完整载入流程、CDN配置指南和先决条件，请参阅[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)页面。
