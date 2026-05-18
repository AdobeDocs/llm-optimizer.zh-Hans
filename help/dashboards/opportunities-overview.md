---
title: 优化机会
description: 了解如何使用“机会”仪表板，自动检测您的网站可改进之处，以提升品牌可见度。
feature: Opportunities
autotag-review: '2026-05-15T17:53:48.623Z'
TQID: 'https://experienceleague.adobe.com/FAbQhzuyT-kIitIaoVQ47dam-TpN-deU5Vbo1nmK5CA'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: a0b5a505-2fd7-4c3d-b61c-b557fb6f0558id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2: id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 564171851fdccee43afd233da143d66182464889
workflow-type: tm+mt
source-wordcount: 1290
ht-degree: 100%

---


# 优化机会

优化机会是自动检测生成的洞察，展示您的网站及外部呈现在哪些方面可以改进，从而提升在 AI 搜索中的品牌可见度。

这些优化包括页面内容修正（如添加结构化内容、规范链接或摘要）、技术调整（如解除对 AI 爬虫的限制或修复错误），以及优化第三方权威网站上的相关内容。 落实这些优化机会，有助于确保品牌准确呈现，并提升在生成式回答中得到引用的可能性。

![优化机会](/help/dashboards/assets/oport.png)

## 机会仪表板

仪表板中展示的优化机会会根据竞争差距、趋势主题和性能数据进行优先排序，并以列表形式呈现。 您可以通过搜索字段查找特定的优化机会。 此外，优化机会按标记分组，您可以直接点击标记，查看该标记下的所有机会。

点击&#x200B;**详细信息**&#x200B;将打开一个单独窗口，显示更多信息和补充指导。

## 支持的优化机会

下方为当前支持的优化机会列表：

| 机会 | 类型 | 已识别问题 | 修复建议 |
|---------|----------|----------|----------|
| [添加 LLM 友好的摘要](/help/dashboards/opportunities/add-llm-friendly-summaries.md) | 内容（站内） | 识别出那些页面或分区层面上缺乏简洁的摘要和结构化关键点的高流量页面，这导致 AI 代理难以扫描这些页面，不易解读品牌声明。 显示受影响的 URL 以及所建议的摘要。 | 审阅 AI 基于现有内容生成的摘要和关键点，然后通过 Optimize at Edge 在内容传递网络边缘进行部署，使代理能够获得更清晰、可扫描的上下文。 |
| [添加相关常见问题（FAQ）](/help/dashboards/opportunities/add-relevant-faqs.md) | 内容（站内） | 识别那些缺少与您的提示词集相符的结构化问答内容的高流量页面，这使 AI 代理难以将用户问题与您的页面进行匹配。 显示受影响的 URL 以及在哪里推荐常见问题。 | 审阅 AI 基于现有页面材料生成的与意图一致的常见问题内容，然后通过 Optimize at Edge 在内容传递网络边缘进行部署，使代理能够获得更清晰的问答上下文。 |
| [添加多媒体文字记录摘要](/help/dashboards/opportunities/add-multimedia-transcript-summaries.md) | 内容（站内） | 识别那些视频或其他媒体中嵌入了关键信息，但没有机器可读的文字记录或摘要的页面，这使 AI 代理难以使用相关内容。 显示受影响的 URL 和推荐的文本。 | 审阅 AI 基于媒体和页面生成的文字记录摘要，然后通过 Optimize at Edge 在内容传递网络边缘进行部署，使代理能够获得机器可读的文本（例如在相关视频的旁边）。 |
| [robots.txt 阻止了流量](/help/dashboards/opportunities/traffic-blocked-by-robots.md) | 技术性 GEO | 分析您的 robots.txt 文件中那些选择性地阻止 AI 代理访问本可公开访问的内容的规则。 报告受影响的 URL 和受阻的代理。 | 在适当的情况下更新您的 robots.txt 文件，以允许受支持的 AI 爬虫访问相关内容。 |
| [代理式流量错误](/help/dashboards/opportunities/agentic-traffic-errors.md) | 技术性 GEO | 监控内容传递网络日志中记录的返回给 AI 代理的 404、403 和 5xx 错误响应。 报告受影响的 URL 和流失访问总数。 | 修复失效链接、更新访问权限，并解决服务器端问题，使关键内容返回 200 状态代码。 |
| [简化复杂内容](/help/dashboards/opportunities/simplify-complex-content.md) | 内容（站内） | 识别那些其中密集或复杂的文本未达到可读性阈值的高流量页面，这使 AI 代理难以解读关键信息。 显示受影响的 URL 以及在哪里建议使用简化文本。 | 审阅 AI 基于现有页面内容生成的改进文本，然后通过 Optimize at Edge 在内容传递网络边缘进行部署，使代理能够获得更清晰、更易于扫描的段落。 |
| [恢复内容可见度](/help/dashboards/opportunities/recover-content-visibility.md) | 技术性 GEO | 标记对 AI 代理隐藏关键内容的页面。 显示受影响的 URL 以及可恢复的预期内容。 | 通过 Optimize at Edge 在内容传递网络层预渲染页面，使 AI 代理无需执行 JavaScript 即可使用更多内容。 |
| [添加目录](/help/dashboards/opportunities/add-table-of-contents.md) | 技术 GEO | 识别那些缺少清晰的结构化组织或导航标题的页面，这使 AI 代理难以解析内容，不易将内容映射到用户查询。 显示受影响的 URL 以及在哪里建议使用结构化目录。 | 审阅所建议的带有反映页面主要部分的锚点链接标题的结构化目录，然后通过 Optimize at Edge 在内容传递网络边缘进行部署，将目录插入到 HTML 中，从而改进页面结构，使模型可以更轻松地提取、映射和引用相关部分。 |
| [维基百科分析](/help/dashboards/opportunities/wikipedia-analysis.md) | 站外 | 分析您公司的维基百科页面与行业竞争对手在引用、分区、内容长度、图像和信息框完整性等各方面的对比。 确定您的页面低于行业基准的具体差距。 | 审阅 AI 生成的策略性建议，以改进您的维基百科页面，包括添加引用、扩充您的信息框、扩展分区、提高文章质量。 |
| [YouTube 情绪分析（试用版）](/help/dashboards/opportunities/youtube-sentiment-analysis.md) | 站外、社交和社区 | 分析在使用您的品牌存在感提示词集时引用的 YouTube 视频，分析品牌提及、情绪、声音份额和定期出现的主题。 仅当识别到为您的提示词集引用了 YouTube 视频时才会显示。 | 审阅高优先级的建议，以提高 YouTube 内容中的品牌感知，包括建议的操作以及负责实施这些操作的团队。 |
| [Reddit 情绪分析（试用版）](/help/dashboards/opportunities/reddit-sentiment-analysis.md) | 站外、社交和社区 | 分析在使用您的品牌存在感提示词集时引用的 Reddit 会话，分析品牌提及、情绪、声音份额和定期出现的主题。 仅当识别到为您的提示词集引用了 Reddit 会话时才会显示。 | 审阅高优先级的建议，以提高 Reddit 内容中的品牌感知，包括建议的操作以及负责实施这些操作的团队。 |
| [引用的情绪分析（试用版）](/help/dashboards/opportunities/cited-sentiment-analysis.md) | 站外、社交和社区 | 分析在使用您的品牌存在感提示词集时识别到的引用最多的 URL，分析品牌提及、情绪、声音份额和定期出现的主题。 | 审阅高优先级的建议，在那些 AI 系统在回答有关您品牌的提示词时引用最多的页面上提高品牌感知。 |
| [扩充产品目录（试用版）](/help/dashboards/opportunities/enrich-product-catalog.md) | 内容（站内），Adobe Commerce | 识别那些因名称或描述过于笼统、技术性文字过于密集或模糊，使 LLM 难以解读的 Commerce 目录产品。 显示评估过的 PDP、代理式流量上下文和 AI 生成的扩充叙述。 | 审阅和编辑所建议的产品名称和描述，然后部署优化，将更新直接发布到您的 Adobe Commerce 目录（可从“修复的建议”回滚）。 |
| [扩充产品详细信息页面](/help/dashboards/opportunities/enrich-product-detail-pages.md) | 技术 GEO，Adobe Commerce | 为 Adobe Commerce 店面，将完整目录数据与 AI 代理在每个产品详细信息页面上可以访问的内容进行比较；显示代理可见的 HTML 中缺少变体、规范、属性和相关目录字段的 PDP，按代理式流量确定优先级。 | 突出显示代理视图中缺少的可恢复的目录信息，以及为什么此信息对于 LLM 驱动的产品发现很重要；通过 Optimize at Edge 进行部署，在内容传递网络边缘为代理式流量提供完整预渲染的 AI 友好的 HTML 快照，使代理无需更改 CMS 或目录即可从您的目录中获得丰富的产品上下文。 |

## 自动优化 {#auto-optimization}

自动优化支持一键部署推荐的优化项，从而减少人工操作并缩短实现价值的时间。 优化可应用于内容源端或内容传递网络边缘。 基于边缘的自动优化目前处于抢先体验阶段，仅支持部分优化机会。 有关更多详细信息，请参阅 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 页面。

<!--
### Recover Content Visibility Opportunity {#recover-contet}

As stated above, the content visibility opportunity, flags pages where key content is lost for AI agents due to client-side rendering. For each identified page, it shows you exactly which content is missing from the AI agent view, helping you pinpoint visibility gaps. It's also supported by an edge-based pre-rendering capability that can serve more HTML content to agentic traffic without requiring Content Management System (CMS) changes. This functionality is currently in Early Access and requires setup from the LLM Optimizer team. Please contact `llmo-at-edge@adobe.com` to activate the content visibility opportunity.
-->

### 其他工具

[LLM 可见度检查器](https://chromewebstore.google.com/detail/is-your-webpage-citable/jbjngahjjdgonbeinjlepfamjdmdcbcc)是一款 Chrome 扩展，可帮助您准确了解网页内容中有多少可由 LLM 访问，以及哪些内容仍处于隐藏状态。 该工具为免费、独立的诊断工具，无需产品许可证或额外设置。 只需单击一次，用户即可评估任意网站的机器可读性，并查看 AI 代理与人类用户所见内容的并排对比。 同时评估通过使用 LLM Optimizer 可恢复的内容比例。

<!--
| Detect Missing Hreflang | Content (Onsite)| Flags pages missing hreflang attributes. Provides affected URLs and expected coverage by language/region.| Implement hreflang tags to indicate correct localized versions. |
| Detect Missing Canonicals | Content (Onsite) | Scans for pages without canonical tags or with conflicting tags. Lists affected URLs and duplicates. | Add canonical tags pointing to the preferred version of each page. Ensure consistent usage across variants. |
-->
