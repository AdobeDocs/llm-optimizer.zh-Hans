---
title: Optimize at Edge
description: 了解如何在无需进行任何内容创作更改的情况下，通过内容传递网络边缘在 LLM Optimizer 中交付优化。
feature: Opportunities
autotag-review: '2026-05-15T17:55:41.072Z'
TQID: 'https://experienceleague.adobe.com/kMxoKtrfyzxIpLJP9nt-rq6GP37ICCNe4XienUKqDZE'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: a0b5a505-2fd7-4c3d-b61c-b557fb6f0558
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
  - id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2:
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: e9001ce2-5245-4a8e-8601-dd958009072f
source-git-commit: 559e77adedb1a93215090441c93c2aa6dc664e5f
workflow-type: tm+mt
source-wordcount: 2931
ht-degree: 98%

---


# Optimize at Edge

本页面详细介绍了如何在无需进行内容创作更改的情况下，在内容传递网络边缘交付优化。 内容涵盖加入流程、可用的优化机会，以及如何在边缘实现自动优化。

## 什么是 Optimize at Edge？

Optimize at Edge 是 LLM Optimizer 中的一项基于边缘的部署功能，用于向 LLM 用户代理提供对 AI 友好的更改。 在当前语境中，“Edge”指的是在内容传递网络层应用优化。 由于优化是在内容传递网络层交付，因此无需在内容管理系统（CMS）中进行任何创作更改，您的来源 CMS 保持不变。 这种分离方式使您能够在不更改现有发布工作流程的情况下提升 LLM 可见度。 该功能仅针对代理式流量，不会影响真人用户或 SEO 机器人。 当 LLM Optimizer 检测到页面存在可优化机会时，用户可以直接在内容传递网络边缘部署修复。

相比需要复杂工程投入的传统修复方式，Optimize at Edge 是一种更快速、更精简的替代方案。 如前所述，在完成一次性设置后，应用更改无需进行平台改动或经历漫长的开发周期。 您可以在数分钟内发布改进，而无需开发人员参与。 这是一种无需编写代码即可为 AI 代理优化网站的方式。

Optimize at Edge 专为营销、SEO、内容和数字战略团队中的业务用户设计。 它支持业务用户在 LLM Optimizer 中完成完整流程：识别优化机会、理解优化建议，并轻松部署修复。 借助 Optimize at Edge 功能，用户可以预览更改，在内容传递网络边缘快速部署，并验证优化是否已生效。 相关表现可在 LLM Optimizer 生态系统中进行跟踪。

### 主要优势

* **仅面向 AI 的交付：**&#x200B;仅向 AI 代理提供优化后的 HTML，不会影响真人访客或 SEO 机器人。
* **更快的发布周期：**&#x200B;以分钟为单位发布更改，而非数周。 无需进行平台改动或经历漫长的工程周期。
* **可回滚：**&#x200B;支持一键回滚功能，可在数分钟内将页面恢复至之前状态。
* **无性能影响：**&#x200B;基于边缘的优化与缓存机制确保网站延迟不受影响。
* **兼容任意内容传递网络和 CMS：**&#x200B;无论使用何种内容管理系统，均可适配任意内容传递网络配置和前端设置。

### Optimize at Edge 支持哪些优化机会？

所有可提升代理式 Web 体验的优化机会均可通过 Optimize at Edge 功能实现。 您可在[机会仪表板](/help/dashboards/opportunities-overview.md)页面和当前页面的机会部分中详细了解每个机会。

## 加入

<!--You should reach out to either your Adobe account team or the FDE team to start the onboarding process. Your IT or CDN team is also required to complete the pre-requisites and setup process. Additionally, you can also contact `llmo-at-edge@adobe.com` for further onboarding assistance.-->

在您的 LLM Optimizer 帐户中开始加入流程：

1. 在&#x200B;**客户配置**&#x200B;仪表板中，选择&#x200B;**内容传递网络配置**&#x200B;选项卡。
1. 单击&#x200B;**加入内容传递网络**。
   ![内容传递网络配置选项卡](/help/overview/assets/cc-cdn.png)
1. 对于 AEM 云服务托管的 Fastly 客户，路由设置采用自助方式，可以直接在 LLM Optimizer UI 中完成。 对于使用其他内容传递网络提供商的客户，需由您的 IT/内容传递网络团队完成相关设置和前置条件配置。 您也可以参考下方提供的内容传递网络示例指南以获取更多指导。

>[!NOTE]
>请参阅下方逐步指南，了解完整的加入流程。 如遇指南未覆盖的问题，可联系 `llmo-at-edge@adobe.com`。

对您的 IT/内容传递网络团队的要求：

* 将 `*AdobeEdgeOptimize/1.0*` 用户代理添加到您网站的 robots.txt 文件或机器人流量管理规则中的允许列表。
* 确保页面在域名或内容传递网络层级未设置访问限制。
* 在内容传递网络中添加 Optimize at Edge 路由规则。
* 如果您的内容传递网络有 WAF 或 Bot Manager 规则，请将 `*AdobeEdgeOptimize/1.0*` 用户代理列入允许列表。 如果需要其他验证，请配置 `x-edgeoptimize-fetcher-key` 标头。 下面的每个 BYOCDN 指南都包含了这些步骤。
* 在 LLM Optimizer 界面中确认 Optimize at Edge 路由已生效。

下图说明了请求如何通过 Optimize at Edge 流过 BYOCDN 设置：

![BYOCDN 请求流](/help/assets/optimize-at-edge/byocdn-request-flow.png)

>[!IMPORTANT]
>路由必须在外部内容传递网络（即离客户端最近的内容传递网络）上进行配置。 如果您拥有多个内容传递网络，路由只能在外部内容传递网络上进行。

要指导这个设置过程，请在下面选择您的内容传递网络提供商，然后按照相应的配置指南操作。 请注意，这些示例需根据您的实际生产环境配置进行调整。 建议您先在低级环境中应用更改。

### 内容传递网络配置指南

| 内容传递网络提供者 | 类型 | 指南 |
|---|---|---|
| AEM 云服务托管的内容传递网络 (Fastly) | 由 Adobe 管理 | [查看设置指南](/help/dashboards/optimize-at-edge/aemcs-managed-cdn.md) |
| Fastly (BYOCDN) | 自带内容传递网络 | [查看设置指南](/help/dashboards/optimize-at-edge/fastly-byocdn.md) |
| Akamai (BYOCDN) | 自带内容传递网络 | [查看设置指南](/help/dashboards/optimize-at-edge/akamai-byocdn.md) |
| Cloudflare (BYOCDN) | 自带内容传递网络 | [查看设置指南](/help/dashboards/optimize-at-edge/cloudflare-byocdn.md) |
| CloudFront (BYOCDN) | 自带内容传递网络 | [查看设置指南](/help/dashboards/optimize-at-edge/cloudfront-byocdn.md) |
| Azure前门(BYOCDN) | 自带内容传递网络 | [查看设置指南](/help/dashboards/optimize-at-edge/azure-front-door-byocdn.md) |

>[!NOTE]
>
>如果上面未列出您的内容传递网络提供商，或者您在 LLM Optimizer UI 中未找到您的域或电子邮件，请联系 `llmo-at-edge@adobe.com` 获取加入帮助。 完成设置配置后，您即可在 LLM Optimizer 中部署 Optimize at Edge 相关优化建议。

上述每个内容传递网络设置指南的最后都包含详细的验证步骤，以确认代理式流量被正确路由以及人工流量不受影响。

## 机会

下表列出了可提升代理式 Web 体验且受 Optimize at Edge 支持的优化机会。

| 机会 | 类型 | 自动识别 | 自动建议 | 自动优化 |
|---------|----------|----------|----------|----------|
| [恢复内容可见度](/help/dashboards/opportunities/recover-content-visibility.md) | 技术性 GEO | 检测关键内容对 AI 代理不可见的页面。 显示受影响的 URL 以及可恢复的预期内容。 | 突出显示可向 AI 代理开放的内容，并建议为这些页面启用预渲染。 | 向代理式流量提供完全渲染的、对 AI 友好的 HTML 快照，从而恢复此前隐藏的内容。 |
| [扩充产品详细信息页面](/help/dashboards/opportunities/enrich-product-detail-pages.md) | 技术 GEO | 为 Adobe Commerce 店面，将完整目录数据与 AI 代理在每个产品详细信息页面上可以访问的内容进行比较；显示代理可见的 HTML 中缺少变体、规范、属性和相关目录字段的 PDP，按代理式流量确定优先级。 | 突出显示代理视图中缺少的可恢复的产品目录信息，以及为什么这对 LLM 驱动的产品发现很重要。 | 在内容传递网络边缘为代理式流量提供完全预渲染的 AI 友好的 HTML 快照，使代理在无需更改 CMS 或您的产品目录的情况下即可从目录中获得丰富的产品上下文。 |
| [添加对 LLM 友好的摘要](/help/dashboards/opportunities/add-llm-friendly-summaries.md) | 内容优化 | 识别出那些页面或分区层面上缺乏简洁的摘要和结构化关键点的高流量页面，这导致 AI 代理难以扫描和解读这些页面。 | 提出 AI 基于现有内容生成的简短摘要和关键点的建议。 | 将摘要和关键点插入到相关的 HTML 分区，提升模型对页面内容的理解和描述能力。 |
| [添加相关常见问题（FAQ）](/help/dashboards/opportunities/add-relevant-faqs.md) | 内容优化 | 识别那些缺少与您的提示词集相符的结构化问答内容的高流量页面，这使 AI 代理难以将用户问题与您的页面进行匹配。 | 提出与用户意图及现有页面主题相匹配的 AI 生成的 FAQ 内容建议。 | 将 FAQ 内容注入 HTML，使页面在 AI 驱动的答案中更易被发现且更具相关性。 |
| [简化复杂内容](/help/dashboards/opportunities/simplify-complex-content.md) | 内容优化 | 标记包含复杂文本、可能影响 AI 理解的页面。 | 在保留原始含义的前提下，提供 AI 生成的简化版本文本。 | 重写页面中的复杂内容区块，提升 AI 可读性。 |
| [添加目录](/help/dashboards/opportunities/add-table-of-contents.md) | 技术 GEO | 识别那些缺少清晰的结构化组织或导航标题的页面，这使 AI 代理难以解析内容，不易将内容映射到用户查询。 | 建议一个结构化目录，其中包含能反映页面各主要部分的锚点链接标题。 | 将目录插入 HTML，改进页面结构，使 AI 模型可以更轻松地提取、映射和引用相关部分。 |
| [添加多媒体文字记录摘要](/help/dashboards/opportunities/add-multimedia-transcript-summaries.md) | 内容优化 | 识别那些视频或其他媒体中嵌入了关键信息，但没有机器可读的文字记录或摘要的页面，这使 AI 代理难以使用相关内容。 显示受影响的 URL 和推荐的文本。 | 建议 AI 基于媒体和页面生成的文字记录摘要。 | 将文字记录摘要插入到 HTML 中，使代理式流量能够获得机器可读的文本（例如在相关视频的旁边）。 |

### 其他工具

[AI 内容可见度检查器](https://chromewebstore.google.com/detail/ai-content-visibility-che/jbjngahjjdgonbeinjlepfamjdmdcbcc)扩展会显示 LLM 可以访问多少网页内容以及哪些内容仍然隐藏。 该工具为免费、独立的诊断工具，无需产品许可证或额外设置。

只需单击一次，即可评估任意网站的机器可读性。 您可以并排查看 AI 代理与真人用户所见内容的差异，并估算通过使用 LLM Optimizer 可恢复的内容量。 请参阅 [AI 能读取您的网站吗？](https://business.adobe.com/cn/blog/introducing-the-llm-optimizer-chrome-extension) 页面以了解更多信息。

## 优化机会详解

在下文各章节中，您可查看每项受 Optimize at Edge 支持的优化机会的更多详细信息。

### 恢复内容可见性

此机会标记因客户端渲染而为AI代理隐藏关键内容的页面。对于每个标识的页面，它都会精确显示AI代理视图中缺少哪些内容，突出显示可见性差距，并允许您直接应用更改以恢复隐藏的内容。当您使用Edge中的优化来部署此机会时，为LLM用户代理提供预呈现的、AI优化的页面版本，以便他们无需执行Javascript即可访问整个上下文。
这可确保该页面首先对AI代理完全可见。在该预呈现的HTML之上应用了其他增强功能。

>[!IMPORTANT]
>当通过 Optimize at Edge 部署下述所有优化机会时，预渲染功能将自动生效，以确保页面对 AI 代理完全可见。

请参阅[恢复内容可见度](/help/dashboards/opportunities/recover-content-visibility.md)，了解仪表板讲解、部署步骤和常见问题。

### 扩充产品详细信息页面

这个机会能够针对那些购物者可用通过交互式店面体验查看完整的产品上下文，而 AI 代理只能获得一个简略的 HTML 快照的 Adobe Commerce 产品详细信息页面。 目录代理会将您的权威 Commerce 目录与代理可见的 PDP 进行比较，列出每一个有意义的差异（例如不显示在静态 HTML 中的变体或规范），允许您部署一个仅机器人的边缘响应，从而在不更改目录记录或人类 UI 的前提下恢复对 LLM 爬虫的对等性。

请参阅[扩充产品详细信息页面](/help/dashboards/opportunities/enrich-product-detail-pages.md)，了解仪表板讲解、部署步骤和常见问题。

### 添加 LLM 友好的摘要

这个机会可识别那些可获益于简洁的摘要和结构化的关键点，使 LLM 能够快速理解页面上的声明的高流量页面。 对于每一个页面，它会检测哪里最需要添加摘要，并基于现有内容在页面或分区层面上提出 AI 生成的摘要（以及相关的关键点）的建议。 如果您通过 Optimize at Edge 进行部署，这个内容会插入 AI 代理检索的 HTML 中，使您的品牌能更准确地展示在 AI 回答中。

请参阅[添加 LLM 友好的摘要](/help/dashboards/opportunities/add-llm-friendly-summaries.md)，了解关于这个机会的更多详细信息。

### 添加相关常见问题（FAQ）

这个机会能够标记那些可通过添加问答内容更好匹配用户意图和 AI 驱动的发现的高流量页面。 对于每一个页面，它会提出与您的提示词集和页面内容相关的 AI 生成的常见问题分区的建议。 通过 Optimize at Edge，这些常见问题被插入到 HTML，使您的页面对 AI 更友好，使 AI 回答能更直接地反映您的内容指导。

请参阅[添加相关常见问题（FAQ）](/help/dashboards/opportunities/add-relevant-faqs.md)，了解仪表板讲解、部署步骤和常见问题。

### 简化复杂内容

此优化机会会识别包含长篇复杂段落、可能降低 AI 理解能力的页面。 对于超出可读性阈值的页面，该功能会生成更简洁、便于快速浏览的 AI 内容版本，同时保留原始含义。 在边缘层部署后，提供给代理式流量的简化内容将有助于 LLM 更准确地理解和概括您的内容。

请参阅[简化复杂内容](/help/dashboards/opportunities/simplify-complex-content.md)，了解仪表板讲解、部署步骤和常见问题。

### 添加目录

这个机会可以识别那些因标题和分区结构不明确或缺失而使 AI 代理难以导航的页面。 对于每一个受影响的页面，它会建议一个锚点链接条目与主要部分一致的结构化目录。 如果您通过 Optimize at Edge 进行部署，这个目录会插入到 HTML 中，使模型可以更可靠地将用户查询映射到页面的适当部分并引用它们。

请参阅[添加目录](/help/dashboards/opportunities/add-table-of-contents.md)，了解仪表板讲解、部署步骤和早期访问指南。

### 添加多媒体文字记录摘要

这个机会可以针对那些重要信息仅存在于视频播放中，没有可供 AI 代理读取的文字记录或文本摘要的页面。 对于每一个页面，它会提出 AI 生成的媒体中的文字记录和关键点简短摘要的建议。 通过 Optimize at Edge，这些摘要作为机器可读的文本添加到 HTML 中，使代理可以使用与人类访客观看视频所获得的相同内容。

请参阅[添加多媒体文字记录摘要](/help/dashboards/opportunities/add-multimedia-transcript-summaries.md)，了解仪表板讲解、部署步骤和常见问题。

## 在边缘层自动优化

针对每个优化机会，您可以在边缘层进行预览、编辑、部署、实时查看和回滚操作。

>[!VIDEO](https://video.tv.adobe.com/v/3477994/?captions=chi_hans&learn=on&enablevpops)

### 预览

**预览**&#x200B;可让您在建议上线前查看其影响。 它会并排展示当前页面与应用建议后预期生成的 AI 优化版本之间的差异。 该视图采用与线上流量相同的 Optimize at Edge 逻辑，但运行于独立的预览模式。 由于这是用于审查的只读模拟，因此不会影响线上流量。

![预览](/help/assets/optimize-at-edge/preview.png)

### 编辑

**编辑**&#x200B;允许您在部署前优化或完全重写自动生成的建议。 您无需直接接受建议，而是可通过编辑流程保持完全控制。 该视图会在结构化编辑器中显示建议更改，您可以修改文本，使其更符合您的原始意图。 部署后，编辑后的版本将提供给 AI 代理。

![编辑](/help/assets/optimize-at-edge/edit.png)

### 部署

**部署**&#x200B;会发布所选建议，使优化后的体验可从边缘层提供给 AI 代理。 如果内容传递网络已完成全量路由，域名下的所有页面通常会在数分钟内上线新更改。 如果仅为部分路径配置了路由，则只有加入允许列表的页面会应用优化并上线。

![部署](/help/assets/optimize-at-edge/deploy.png)

### 实时查看

**查看实时效果**&#x200B;允许您验证优化是否已上线，并确认其在代理式流量中的表现是否符合预期。这种视图通常较难直接访问。 您可以在“已修复建议”下查看实时页面，该页面以 AI 代理所见方式进行渲染。

![查看实时效果](/help/assets/optimize-at-edge/view-live.png)

### 回滚

回滚可安全撤销之前已部署的优化。 仅面向 AI 的页面版本通常会在数分钟内恢复至先前状态，因此在需要时可安全进行优化试验。

![回滚](/help/assets/optimize-at-edge/rollback.png)

## 其他资源

有关 Optimize at Edge 功能的更多详细信息，请观看以下播放列表 [LLM Optimizer — Optimize at Edge](https://www.youtube.com/playlist?list=PLzbVcr6JHocVSMWBCaCw4xxjQ_VFVvFh0)。

## 常见问题解答

问：试用用户是否可以使用 Optimize at Edge？

可以，试用用户可使用一个优化机会，并最多在 10 个页面上部署。 默认机会为“恢复内容可见度”，可使 AI 代理访问页面内容的完整版本。

问：Optimize at Edge 针对哪些类型的 LLM？

目标用户代理列表由您在加入流程中自行定义。

<!--
Q. What does "Edge" in Optimize at Edge mean?

In our context, "Edge" means that the optimization is applied at the CDN layer and not inside your CMS.

Q. Why does this optimization require a CDN?

The CDN is where the optimized version of the page is assembled and delivered to AI agents. We leverage the CDN to ensure your origin CMS remains unchanged. This separation lets you improve LLM visibility without altering your existing publishing workflows.
-->

问：如果我尚未完成 Optimize at Edge 加入，会发生什么？

如果您在完成必要设置之前点击&#x200B;**部署优化**，您的网站不会应用任何更改。 系统会弹出提示框，引导您联系 `llmo-at-edge@adobe.com` 获取加入支持。 在完成加入之前，您仍可查看检测到的优化机会和建议，但一键部署流程将保持不可用状态。

问：当源站内容更新时会发生什么？

只要基础源页面未发生更改，我们就会从缓存中提供页面的优化版本。但是，当&#x200B;**恢复内容可见度**&#x200B;的源发生更改时，我们的系统会自动刷新，以便AI代理始终接收最新的内容。这是因为我们使用低缓存生存时间(TTL)设置（以分钟为单位），因此您网站上的任何内容更新都会触发该窗口内的新优化。对于诸如&#x200B;**添加LLM友好型摘要**&#x200B;等内容机会，LLM Optimizer会监控源页面是否有更改。如果检测到更改，我们会暂停优化并将其标记为人工审核，以防止代理可见页面和人类可见页面之间的内容漂移。
<!--As there is no universal TTL that fits every site, we can configure this TTL based on your cache invalidation rules to ensure both systems stay in sync.-->

问：Optimize at Edge 是否仅适用于使用 Adobe Edge Delivery Service（EDS）的网站？

不。 Optimize at Edge 与内容传递网络无关，可兼容任意前端架构，而不仅限于部署在 Adobe EDS 技术栈上的网站。

问：Optimize at Edge 的预渲染与传统服务器端渲染（SSR）有何不同？

两者解决的问题不同，且可协同使用。 传统 SSR 渲染服务器端内容，但不包含随后在浏览器中加载的内容。 Optimize at Edge 预渲染会在 JavaScript 和客户端数据加载完成后捕获页面，在内容传递网络边缘层生成完整组装版本。 SSR 侧重于提升真人用户体验，而 Optimize at Edge 则专注于提升面向 LLM 的 Web 体验。

问：恢复内容可见度（即预渲染）是伪装技术吗？ 听起来好像是给 AI 代理提供页面的另一个版本。

不。 预渲染可确保 AI 代理能够看到人类访客和 SEO 机器人可看的相同内容。 许多网站使用 JavaScript 加载有意义的内容，而常见 AI 代理不执行 JavaScript，因此代理可能会错过页面的很多部分。 预渲染会生成一个捕获完整文本的静态快照，使代理能够获得与人类和搜索引擎看到的相同信息。 它能为 LLM **恢复**&#x200B;内容对等性，而不会添加或更改实际内容。

问：那么其他的内容机会呢？例如会在页面上显示提供给代理的新文案的“添加 LLM 友好的摘要”。 这是伪装技术吗？

不。 Optimize at Edge 不会引入人类用户和 SEO 爬虫无法访问的信息。 这个服务会重新组织或总结页面上已有的内容，使 AI 代理可以更轻松地进行解读。 如果有人通过 AI 回答中的链接访问您的网站，他们仍可以在实时页面上找到相同的底层信息。

问：如果我为域中的某些 URL，而不是所有 URL 部署优化功能，会怎样？

只有您明确进行优化的 URL 才会更改。 对于已经部署了机会的 URL，AI 代理会获得优化过的版本。 对于未部署机会的 URL，我们的服务会按原样代理原始页面，不会应用更改，也不会将其存储在我们的优化缓存层中。 这样可以确保您能够选择性地部署优化，而不影响网站的其余部分。
