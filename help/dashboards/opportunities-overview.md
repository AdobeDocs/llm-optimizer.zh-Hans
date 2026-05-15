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
source-wordcount: 1227
ht-degree: 31%

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
| [添加LLM友好的摘要](/help/dashboards/opportunities/add-llm-friendly-summaries.md) | 内容（站内） | 识别在页面或部分级别缺乏简洁摘要和结构化关键点的高流量页面，使AI代理更难扫描和解读品牌声明。 显示受影响的URL以及建议摘要的位置。 | 审查基于现有内容的AI生成的摘要和要点，然后使用Edge中的优化在CDN边缘部署，以便代理接收更清晰、可浏览的上下文。 |
| [添加相关常见问题解答](/help/dashboards/opportunities/add-relevant-faqs.md) | 内容（站内） | 识别缺少与您的提示集对齐的结构化问答内容的高流量页面，使AI代理更难将用户问题与您的页面进行匹配。 显示受影响的URL以及推荐的常见问题解答。 | 审查基于现有页面材料的AI生成、与意图一致的常见问题解答内容，然后使用Edge中的优化在CDN边缘进行部署，以便代理接收更清晰的问答上下文。 |
| [添加多媒体成绩单摘要](/help/dashboards/opportunities/add-multimedia-transcript-summaries.md) | 内容（站内） | 标识关键信息嵌入到视频或其他媒体中的页面，这些页面没有机器可读的转录或摘要，这使得人工智能代理难以使用该内容。 显示受影响的URL和推荐的文本。 | 查看以媒体和页面为基础的AI生成的成绩单摘要，然后在CDN边缘上使用在Edge优化进行部署，以便代理接收机器可读的文本（例如，相关视频附近）。 |
| [被robots.txt阻止的流量](/help/dashboards/opportunities/traffic-blocked-by-robots.md) | 技术性 GEO | 分析您的robots.txt文件，查找有选择地阻止AI代理访问其他可公开访问内容的规则。 报告受影响的URL和受阻的代理。 | 更新您的robots.txt文件，以便在适当时允许访问支持的AI爬虫。 |
| [代理流量错误](/help/dashboards/opportunities/agentic-traffic-errors.md) | 技术性 GEO | 监视返回给AI代理的404、403和5xx错误响应的CDN日志。 报表会影响URL和总点击量。 | 修复失效链接、更新访问权限，并解决服务器端问题，使关键内容返回 200 状态代码。 |
| [简化复杂内容](/help/dashboards/opportunities/simplify-complex-content.md) | 内容（站内） | 识别高流量页面，其中密集型或复杂副本低于可读性阈值，使AI代理更难解释关键信息。 显示受影响的URL以及建议使用简化文本的位置。 | 审查基于现有页面内容的AI生成的改进文本，然后使用Edge中的优化在CDN边缘部署，以便代理接收更清晰、更易于扫描的章节。 |
| [恢复内容可见度](/help/dashboards/opportunities/recover-content-visibility.md) | 技术性 GEO | 标记对 AI 代理隐藏关键内容的页面。 显示受影响的 URL 以及可恢复的预期内容。 | 使用Edge中的优化在CDN层预呈现页面，以便人工智能代理无需执行JavaScript即可使用更多内容。 |
| [添加目录](/help/dashboards/opportunities/add-table-of-contents.md) | 技术性 GEO | 检测缺少清晰的结构化组织或导航标题的页面，这会使AI代理难以解析内容并将其映射到用户查询。 显示受影响的URL以及建议使用结构化目录的位置。 | 查看建议的结构化目录，该目录具有反映页面主要部分的锚点链接标题，然后使用Edge中的优化在CDN边缘进行部署，从而将目录插入到HTML中，从而改进页面结构，使模型可以更轻松地提取、映射和引用相关部分。 |
| [维基百科分析](/help/dashboards/opportunities/wikipedia-analysis.md) | 非现场 | 跨引用、部分、内容长度、图像和信息框完整性分析您公司的Wikipedia页面与行业竞争对手的对比。 确定页面低于行业基准的特定差距。 | 审查AI生成的战略建议以提高您的维基百科存在，包括添加引用、丰富您的信息框、扩展部分以及提高文章质量。 |
| [情绪分析(Beta)](/help/dashboards/opportunities/youtube-sentiment-analysis.md) | 异地、社交和社区 | 分析为您的品牌存在感提示集引用的YouTube视频，其中包含品牌提及、情绪、声音份额和重复出现的主题。 仅当检测到YouTube视频作为提示集的引文时才会显示。 | 审查优先推荐，以提高YouTube内容中的品牌认知度，包括建议的操作以及负责实施这些操作的团队。 |
| [Reddit情绪分析(Beta)](/help/dashboards/opportunities/reddit-sentiment-analysis.md) | 异地、社交和社区 | 分析针对您的品牌存在感提示集引用的Reddit线程，这些提示集包含品牌提及、情绪、声音份额和循环主题。 仅当检测到Reddit线程作为提示集的引用时才会显示。 | 审查优先推荐，以提高Reddit内容中的品牌认知度，包括建议的操作以及负责实施这些操作的团队。 |
| [已引用的情绪分析(Beta)](/help/dashboards/opportunities/cited-sentiment-analysis.md) | 异地、社交和社区 | 分析在品牌提及、情绪、声音份额和重复主题的品牌存在感提示集中检测到的热门引用URL。 | 在响应有关您的品牌提示时，查看人工智能系统最常引用的优先级建议，以改善整个页面的品牌感知。 |
| [扩充产品目录(Beta)](/help/dashboards/opportunities/enrich-product-catalog.md) | 内容（现场），Adobe Commerce | 标识其名称或描述过于宽泛、技术上过于密集或不明确，以供LLM解释的Commerce目录产品。 显示已评估的PDP、代理流量上下文和人工智能生成的叙述扩充功能。 | 查看并编辑建议的产品名称和描述，然后部署优化以直接将更新发布到Adobe Commerce目录（可从固定建议回滚）。 |
| [丰富产品详细信息页面](/help/dashboards/opportunities/enrich-product-detail-pages.md) | 技术地理位置，Adobe Commerce | 对于Adobe Commerce店面，比较完整目录数据与AI代理在每个产品详细信息页面上可以访问的内容；显示PDP，其中代理可见的HTML中缺少变体、规范、属性和相关目录字段，并按代理流量划分优先级。 | 突出显示代理视图中缺少的可恢复目录信息以及它对LLM驱动的产品发现重要的原因；使用Edge中的优化进行部署，向CDN边缘的代理流量提供完全预呈现、对AI友好的HTML快照，以便代理无需更改CMS或目录即可从目录中接收丰富的产品上下文。 |

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
