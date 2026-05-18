---
title: 产品详细信息页面扩充
description: 了解 LLM Optimizer 如何识别其目录数据对 AI 代理隐藏的产品页面，以及如何通过由 Adobe Commerce 提供支持的基于边缘的优化和产品目录洞察来恢复这种可见性。
feature: Opportunities
autotag-review: '2026-05-15T17:46:41.487Z'
TQID: 'https://experienceleague.adobe.com/l4hTGNNg1NW40ceI00P41KZBSGcqmr-t1RWM-NXtRV4'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2:
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 1210
ht-degree: 100%

---


# 扩充产品详细信息页面

AI 代理只能推荐它们能完全理解的产品。 在大多数商务店面，产品页面是为人类购物者设计的。 因此，这些产品会使用 JavaScript 渲染的选项卡、可展开的面板、购物向导、交互式模态以及会显示产品型号变体、规格和功能的链接。 AI 代理不会在产品详细信息页面的整个深度上解析，这意味着这些丰富的产品数据虽然人类访客可以完全看到，但 LLM 爬虫看不到，也就无法支持 AI 驱动的发现。

“扩充产品详细信息页面”机会能够识别您的 Adobe Commerce 目录中存在可见性差异的产品页面。 它在 Adobe Commerce 目录的支持下，将 AI 代理在店面上可以访问的内容与您的目录中可用的完整产品数据进行比较，显示在 AI 代理视图中缺少的所有属性、变体和产品特征深度。

它可一目了然地显示以下几个关键量度：

- **产品页面**：识别到存在目录数据可见性差异的所有产品详细信息页面的列表。
- **代理式流量**：代表用户执行发现、检索、与内容互动等操作的自主 AI 代理（如 LLM 支持的助手或机器人）所发起和驱动的网站访问和交互总数。

![扩充产品详细信息页面仪表板](/help/dashboards/opportunities/assets/enrich-product-detail-pages-overview.png)

通过 [Optimize at Edge](https://experienceleague.adobe.com/zh-hans/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge) 可以优化这个机会。 这些优化专门传递给 AI 代理，不会对人类访客造成任何影响（仅限机器人的传递方式），在内容传递网络层应用，无需更改 CMS 或目录，只需几分钟即可生效，无需开发人员参与，因此这是一种快速、低风险的大产品目录部署路径。

## 工作原理

Adobe Commerce 目录代理会读取您的完整产品目录数据，包括：变体、更深层的产品关系、属性、方面、类别元数据和所有产品特征。 然后，它将这些数据与 AI 代理在相应店面 PDP 上实际可访问的数据进行比较。 **包含建议的 URL** 表格中会列出目录数据对 AI 爬虫隐藏的页面，并按代理式流量确定优先级。

对于每一个受影响的产品页面，LLM Optimizer 会提供：

- **AI 分析预览**：AI 代理视图中缺少的目录信息的完整列表，以及为什么它们对 LLM 驱动的产品发现很重要，包括可恢复数据点的列表，如产品型号变体、尺寸选项、材料规格和兼容性详细信息等。

修复的应用是通过 [Optimize at Edge](https://experienceleague.adobe.com/zh-hans/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge) 实施：Adobe 的基于边缘的部署功能在内容传递网络层为 LLM 用户代理提供完全预渲染的 AI 友好的 HTML 快照。 这可恢复所有以前隐藏的目录数据（包括产品变体、技术规格和功能详细信息），不会对您的 Commerce 目录或人类可见的店面 UI 产生丝毫影响。

![“包含建议的 URL”表](/help/dashboards/opportunities/assets/enrich-product-detail-pages-suggestions.png)

## 包含建议的 URL

**包含建议的 URL** 表中列出了所有识别到的可获益于优化的产品页面。 对于每一个产品 URL，您可以：

- **预览**&#x200B;查看 AI 分析，包括缺少哪些目录信息以及为什么它们对 AI 驱动的可发现性很重要
- 在部署并验证优化后，将其&#x200B;**标记为已修复**
- **忽略**&#x200B;与您的促销策略无关的建议

这些建议分成三个视图：**当前建议**、**修复的建议**&#x200B;和&#x200B;**忽略的建议**。 建议部署后，会移至“修复的建议”，状态变为&#x200B;**已优化**，通过&#x200B;**实时查看**&#x200B;操作可以验证这种扩充方法是否已对代理式流量生效。 修复的建议可以随时回滚。

## 部署优化

如果您审阅了建议并选择了要优化的产品页面后，可点击&#x200B;**部署优化**，在内容传递网络边缘发布扩充。 **部署到边缘**&#x200B;确认对话框中会显示选定的产品 URL、优化类型和将要应用的扩充。 部署后，确认屏幕中会确认哪些产品页面已成功优化。

优化会通过内容传递网络边缘层专门传递给 AI 用户代理。 人类访客会继续像之前一样看到您现有的店面体验，您无需更改 PDP 设计，页面表现或品牌体验也不会发生变化。

>[!NOTE]
>
>部署优化需要 (1) 将 LLM Optimizer 连接到 Adobe Commerce，(2) 还需要完成 Optimize at Edge 加入过程。

如果您的 Commerce 实例尚未连接到 LLM Optimizer，您就会在应用扩充之前被定向到连接设置。

如果您尚未加入，点击&#x200B;**部署优化**&#x200B;会引导您完成加入过程。 关于 Optimize at Edge 如何工作、受支持的内容传递网络提供程序以及加入过程的完整详细信息，请参阅 [Optimize at Edge](https://experienceleague.adobe.com/zh-hans/docs/llm-optimizer/using/resources/optimize-at-edge/overview#what-is-optimize-at-edge) 页面。

![”部署到边缘“对话框](/help/dashboards/opportunities/assets/enrich-product-detail-pages-deploy.png)

## 在演示中尝试此操作

使用 Frescopa 演示环境了解“扩充产品详细信息页面”机会的实际运作。

[在 Frescopa 演示中查看扩充产品详细信息页面](https://play.llmo.now/org/demo-org/opportunities/commerce-product-page-enrichment/4e8b0428-0893-4864-a00e-fc1d77fb3372?siteId=9ae8877a-bbf3-407d-9adb-d6a72ce3c5e3)

## 常见问题

**为什么我的产品目录数据对 AI 代理是隐藏的？**

Commerce 店面是为人类购物者构建的。 产品特征、型号、尺寸选项、材料详细信息和其他技术规格通常都通过 JavaScript 驱动的交互方式呈现出来，如选项卡、可折叠面板、弹出窗口模式、链接和购物向导。 AI 代理不会在产品详细信息页面的整个深度上解析，因此所有这些数据虽然完全显示在您的产品目录中，但是 LLM 爬虫看不到。 这就导致 AI 代理只能根据实际可用的产品信息的一小部分做出产品推荐。

**这个优化会恢复哪些类型的产品数据？**

目录代理可恢复您的 Adobe Commerce 目录中 AI 代理当前在店面中无法访问的所有产品信息。 这包括产品特征、关系、型号变体（尺寸、颜色、配置）、技术规格和属性、兼容性详细信息、类别元数据和方面值。

**这个优化是否会影响我的人类访客、SEO 机器人或店面性能？**

不。 Optimize at Edge 仅针对 AI 用户代理。 人类访客和 SEO 机器人会获得与之前完全相同的原始产品页面，他们的体验、页面加载性能和店面设计不会有任何变化。

**我需要更改 Commerce 目录、CMS 或者需要开发人员参与吗？**

不。 优化的应用发生在内容传递网络边缘层，无需更改 Adobe Commerce 目录，无需部署代码，也不需要开发人员参与。 加入 Optimize at Edge 后，您可以在几分钟内直接在 LLM Optimizer 界面中部署，也可以随时撤消扩充。

**如果部署之后我的产品数据发生变化，该怎么办？**

LLM Optimizer 为“扩充产品详细信息页面”机会使用低缓存 TTL 设置，因此您的 Commerce 目录中发生任何产品更新都会在几分钟内触发刷新。 AI 代理将一直能获得所提供的最新产品数据。
