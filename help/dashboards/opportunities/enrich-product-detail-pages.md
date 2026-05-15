---
title: 产品详细信息页面扩充
description: 了解LLM Optimizer如何识别对AI代理隐藏目录数据的产品页面，以及如何使用由Adobe Commerce提供支持的基于边缘的优化和产品目录分析来恢复该可见性。
feature: Opportunities
autotag-review: '2026-05-15T17:46:41.487Z'
TQID: 'https://experienceleague.adobe.com/l4hTGNNg1NW40ceI00P41KZBSGcqmr-t1RWM-NXtRV4'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2: id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 1210
ht-degree: 0%

---


# 丰富产品详细信息页面

AI代理只能推荐他们能够完全理解的产品。 在大多数商业店面，产品页面是为人类购物者设计的。 因此，这些产品依赖于JavaScript渲染的选项卡、可展开的面板、购物向导、交互式模型以及指向表面产品变体、规范和功能的链接。 AI代理不会分析产品详细信息页面的深度，这意味着这些丰富的产品数据永远不会被支持AI驱动发现的LLM爬虫查看，即使人类访客完全可以看到。

“丰富产品详细信息页面”机会可识别Adobe Commerce目录中存在可见性差距的产品页面。 它由Adobe Commerce Catalog提供支持，将AI代理在店面上可以访问的内容与目录中可用的完整产品数据进行比较，并显示在AI代理视图中缺少的所有属性、变体和产品特征深度。

它一览即可显示以下关键量度：

- **产品页面** — 已识别具有目录数据可见性差距的所有产品详细信息页面的列表。
- **代理流量** — 由代表用户执行发现、检索或参与内容操作的自主AI代理（如LLM支持的助手或机器人）发起和驱动的网站访问和交互总数。

![丰富产品详细信息页面仪表板](/help/dashboards/opportunities/assets/enrich-product-detail-pages-overview.png)

通过使用Edge上的[优化](https://experienceleague.adobe.com/en/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge)，可以优化此机会。 优化专门交付给AI代理，不会对人类访客造成影响（仅限机器人交付），在CDN层应用，无需进行CMS或目录更改，并且可在几分钟内生效，无需开发人员参与 — 使其成为大型产品目录的快速、低风险部署途径。

## 工作原理

Adobe Commerce Catalog Agent读取您的完整产品目录数据，包括：变体、更深入的产品关系、属性、彩块化、类别元数据和所有产品特征。 然后，它将数据与人工智能代理在相应店面PDP上实际可访问的数据进行比较。 对AI爬虫隐藏目录数据的页显示在带有建议&#x200B;**表的** URL中，按代理流量排定优先级。

对于每个受影响的产品页面，LLM Optimizer都提供：

- **AI Analysis预览** — AI代理视图中缺少的目录信息以及它们对LLM驱动的产品发现重要的原因，包括可恢复数据点的完整列表，如产品变型、大小选项、材料规格和兼容性详细信息等。

此修复是使用[在Edge中优化](https://experienceleague.adobe.com/en/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge)应用的 — Adobe的基于边缘的部署功能，为CDN层的LLM用户代理提供完全预呈现、人工智能友好的HTML快照。 这将恢复所有以前隐藏的目录数据（包括产品变型、技术规范和功能详细信息），而不会接触您的Commerce目录或人类可见的店面UI。

带有建议表的![个URL](/help/dashboards/opportunities/assets/enrich-product-detail-pages-suggestions.png)

## 包含建议的 URL

带有建议&#x200B;**表的** URL列出了所有已识别的产品页面，这些页面得益于优化。 对于每个产品URL，您可以：

- **预览**&#x200B;以查看AI分析，包括缺少哪些目录信息以及它们对AI驱动的可发现性重要的原因
- 部署并验证优化后，**标记为已修复**
- **忽略**&#x200B;与您的促销策略无关的建议

建议分为三个视图：**当前建议**、**固定建议**&#x200B;和&#x200B;**忽略的建议**。 部署建议后，该建议将移至状态为&#x200B;**已优化**&#x200B;的固定建议和&#x200B;**查看实时**&#x200B;操作，以验证对代理流量进行的扩充是否处于活动状态。 可以随时回退已修复的建议。

## 部署优化

审查建议并选择要优化的产品页面后，单击&#x200B;**部署优化**&#x200B;以在CDN边缘发布扩充。 **部署到Edge**&#x200B;确认对话框将显示所选产品URL、优化类型和正在应用的扩充。 部署后，确认屏幕会确认哪些产品页面已成功优化。

优化通过CDN边缘层专门交付给AI用户代理。 人类访客会继续像之前一样看到您现有的店面体验，而不会更改您的PDP设计、页面性能或品牌体验。

>[!NOTE]
>
>部署优化需要(1)将LLM Optimizer连接到Adobe Commerce和(2)完成Optimize at Edge载入流程。

如果您的Commerce实例尚未连接到LLM Optimizer，则会在应用增强功能之前将您定向到连接设置。

如果您尚未载入，单击&#x200B;**部署优化**&#x200B;会将您引导至载入流程。 有关在Edge中优化的工作方式、支持的CDN提供商以及载入流程的完整详细信息，请参阅[在Edge中优化](https://experienceleague.adobe.com/en/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge)页面。

![部署到Edge对话框](/help/dashboards/opportunities/assets/enrich-product-detail-pages-deploy.png)

## 在演示中尝试

使用Frescopa演示环境查看正在进行的“丰富产品详细信息页面”机会。

[在Frescopa演示中查看扩充产品详细信息页面](https://play.llmo.now/org/demo-org/opportunities/commerce-product-page-enrichment/4e8b0428-0893-4864-a00e-fc1d77fb3372?siteId=9ae8877a-bbf3-407d-9adb-d6a72ce3c5e3)

## 常见问题解答

**为什么我的产品目录数据对AI代理隐藏？**

Commerce的店面是为人类购物者建造的。 产品特性、变体、尺寸选项、材料详细信息和其他技术规范通常通过JavaScript驱动的交互呈现，如选项卡、可折叠面板、弹出模式、链接和购物向导。 AI代理不会深入分析产品详细信息页面，因此LLM爬虫不会看到所有这些数据，即使它们完全显示在产品目录中。 结果是，AI代理会根据实际产品信息的一部分进行产品推荐。

**此优化将恢复哪些类型的产品数据？**

目录代理可恢复您的Adobe Commerce目录中当前无法在AI代理的店面访问的所有产品信息。 这包括产品字符、关系、变体（大小、颜色、配置）、技术规范和属性、兼容性详细信息、类别元数据和方面值。

**此优化是否会影响我的人工访客、SEO机器人或店面性能？**

不。 在Edge中优化仅针对AI用户代理。 人类访客和SEO机器人获得的产品页面与之前完全相同，并且不会更改其体验、页面加载性能或店面设计。

**我需要更改Commerce目录、CMS还是让开发人员参与？**

不。 优化在CDN边缘层应用，无需更改Adobe Commerce目录，无需部署代码，也不需要开发人员参与。 在Edge上线到“优化”页面后，您可以在几分钟内直接从LLM Optimizer界面部署和回滚增强功能。

**如果我部署后产品数据发生更改，会发生什么情况？**

对于扩充产品详细信息页面销售机会，LLM Optimizer使用低缓存TTL设置，因此Commerce目录中的任何产品更新都会在几分钟内触发刷新。 AI代理将始终接收可用的最新产品数据。
