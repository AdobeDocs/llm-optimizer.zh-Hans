---
title: 优化机会
description: 了解如何使用“机会”仪表板，自动检测您的网站可改进之处，以提升品牌可见度。
feature: Opportunities
source-git-commit: 82830e66d43ddd9741617cdf6daab63cd259554b
workflow-type: tm+mt
source-wordcount: '575'
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
| 长段落摘要优化 | 内容（站内） | 检测超过建议长度阈值的段落。 显示受影响的 URL 及超长文本片段。 | 创建摘要，或将长文本拆分为更短、便于快速浏览的段落。 |
| 推荐结构化内容（FAQ） | 内容（站内） | 检测高热度提示但缺少对应 FAQ 条目的情况。 显示相关提示、类别及受影响的 URL。 | 添加包含简明回答的 FAQ 架构模块，以匹配常见问题。 |
| 检测缺失的 hreflang | 内容（站内） | 标记缺少 hreflang 属性的页面。 提供受影响的 URL 及预期的语言/区域覆盖范围。 | 实施 hreflang 标记，以指示正确的本地化版本。 |
| 检测缺失的规范 | 内容（站内） | 扫描未设置规范标记或存在冲突标记的页面。 列出受影响的 URL 及重复页面。 | 添加规范标记，指向每个页面的首选版本。 确保在不同版本间保持一致的使用方式。 |
| 检测被阻止的代理式流量 | 技术性 GEO | 分析内容传递网络日志，识别来自已知 AI 代理（例如 GPTBot、PerplexityBot）的遭到阻止的请求。 报告受影响的 URL 和相关代理。 | 在适当情况下更新 robots.txt 或服务器配置，以允许受支持的 AI 爬虫访问。 |
| 检测 404 / 403 / 5xx 错误问题 | 技术性 GEO | 监控内容传递网络日志中的错误响应。 报告错误发生频率、受影响的 URL 以及预计丢失的访问次数。 | 修复失效链接、更新访问权限，并解决服务器端问题，使关键内容返回 200 状态代码。 |
| 恢复内容可见度（抢先体验） | 技术性 GEO | 标记对 AI 代理隐藏关键内容的页面。 显示受影响的 URL 以及可恢复的预期内容。 | 对页面进行预渲染，使 AI 代理在无需执行 JavaScript 的情况下即可获取更多内容。 |

## 自动优化 {#auto-optimization}

自动优化支持一键部署推荐的优化项，从而减少人工操作并缩短实现价值的时间。 优化可应用于内容源端或内容传递网络边缘。 基于边缘的自动优化目前处于抢先体验阶段，仅支持部分优化机会。 有关更多详细信息，请参阅 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 页面。

<!--### Recover Content Visibility Opportunity {#recover-contet}

As stated above, the content visibility opportunity, flags pages where key content is lost for AI agents due to client-side rendering. For each identified page, it shows you exactly which content is missing from the AI agent view, helping you pinpoint visibility gaps. It's also supported by an edge-based pre-rendering capability that can serve more HTML content to agentic traffic without requiring Content Management System (CMS) changes. This functionality is currently in Early Access and requires setup from the LLM Optimizer team. Please contact `llmo-at-edge@adobe.com` to activate the content visibility opportunity.-->

### 其他工具

[LLM 可见度检查器](https://chromewebstore.google.com/detail/is-your-webpage-citable/jbjngahjjdgonbeinjlepfamjdmdcbcc)是一款 Chrome 扩展，可帮助您准确了解网页内容中有多少可由 LLM 访问，以及哪些内容仍处于隐藏状态。 该工具为免费、独立的诊断工具，无需产品许可证或额外设置。 只需单击一次，用户即可评估任意网站的机器可读性，并查看 AI 代理与人类用户所见内容的并排对比。 同时评估通过使用 LLM Optimizer 可恢复的内容比例。
