---
title: robots.txt 阻止流量
description: 了解 LLM Optimizer 如何识别您的 robots.txt 文件阻止了 AI 代理访问您的内容，以及如何修复这个问题。
feature: Opportunities
autotag-review: '2026-07-15T18:04:23.113Z'
TQID: 'https://experienceleague.adobe.com/rk82xEtvYBr47tTNzhC6ZbFw1Bim3Otr-D2ncBWPmwM'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
subfeature_v2:
  - id: e0ec491f-fe51-42b6-801c-1c0dfcc0e64f
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 817
ht-degree: 100%

---


# robots.txt 阻止流量

您的 `robots.txt` 文件控制着哪些爬虫可以访问您的网站。 如果选择性地阻止 AI 代理访问那些一般爬虫可以访问的内容，这些代理就无法为这些内容编制索引或引用它们，这会直接降低您的品牌在 AI 生成的回答中的可见性。

“robots.txt 阻止流量”机会能够针对您的热门页面分析您的 `robots.txt` 文件，识别那些阻止 AI 代理访问它们本应能访问的内容的规则。 它会显示单个 `robots.txt` 在行层面上的发现，这样您就可以审阅和更新特定的指令，而不用手动审核整个文件。

它会一目了然地显示出两个关键量度：

- **URL 总数**：受到您的 `robots.txt` 中的阻止规则影响的 URL 数量。
- **被阻止的代理**：被阻止访问这些 URL 的 AI 代理数量。

![robots.txt 阻止流量的仪表板](/help/dashboards/opportunities/assets/traffic-blocked-by-robots-overview.png)

## 工作原理

LLM Optimizer 会获取您的 `robots.txt` 文件，然后根据六个主要的 AI 代理用户代理检查您的热门页面：

- ClaudeBot
- GPTBot
- OAI-SearchBot
- OAI 用户
- PerplexityBot
- Perplexity 用户

只有当 URL **允许通配符 (`*`) 用户代理访问，但不允许某个具体的 AI 代理访问的情况下，才会标记这个 URL。** 如果是全部都阻止，即所有爬虫都受到相同的限制，这种情况不会报告。 这一审核专门针对“选择性排除 AI 代理”的问题，这是对于 GEO 最具可操作性的发现。

>[!NOTE]
>这个机会不使用 AI 来制定或提供建议。 这样的发现完全基于对您的 `robots.txt` 文件的直接分析。

结果会显示在两个选项卡中：**robots.txt** 和&#x200B;**各代理的被阻流量详细信息**。

## robots.txt

这个选项卡完整显示了您的 `robots.txt` 文件，并用红色突出显示其中的阻止指令。 突出显示的每一行都表示一条选择性地阻止一个或多个 AI 代理访问本应能公开访问的 URL 的规则。

![robots.txt 视图，其中突出显示了阻止指令](/help/dashboards/opportunities/assets/traffic-blocked-by-robots-robotstxt.png)

点击突出显示的一个指令，就会显示关于此指令带来的影响和问题修复建议的更多信息。

## 各代理的被阻流量详细信息

这个选项卡提供了由 AI 代理组织的被阻止流量的细分分析。 对于每一个被阻止的代理，它显示了：

- **问题描述**：说明哪个代理被阻止以及这为什么很重要。
- **解决方法**：指导如何打开 `robots.txt` 文件以及审阅每一个受影响的 URL 旁边列出的具体行号。
- 受影响 URL 的表格，其中包括每一个被阻止页面的&#x200B;**行**、**排名**&#x200B;和 **URL**。

每个代理（例如 OAI 用户、GPTBot、OAI-SearchBot）都有自己的子选项卡，这样您就可以为每个代理处理阻止问题。

## 如何修复

如要解决某个发现的被阻止代理，请打开您的 `robots.txt` 文件，然后在“各代理的被阻流量详细信息”选项卡中找到每一个受影响的 URL 旁边显示的行号。 更新或移除不允许相关 AI 代理的指令，以允许此代理访问受影响的 URL。

例如，要取消阻止 GPTBot 访问某个特定页面，请移除或更新指令：

```
User-agent: GPTBot
Disallow: /blog/cold-brewing-101
```

您将 `robots.txt` 文件更新并重新发布后，LLM Optimizer 就会在下次运行审核时检测到这一更改，会将这个建议标记为已解决。

## 在演示中尝试此操作

使用 Frescopa 演示环境了解 “Robots.txt 阻止流量”机会的实际运作。

[在 Frescopa 演示中查看 robots.txt 阻止流量](https://play.llmo.now/org/demo-org/opportunities/blocked-urls-llm-agents)

## 常见问题解答

**为什么 AI 代理被阻问题对 GEO 很重要？**

生成式引擎优化要求 AI 爬虫可以访问您的网站内容并将其编入索引。 阻止 AI 代理会直接阻止您的页面出现在 AI 生成的回答中，这会减少引用、品牌提及和总体 AI 可见性。 即使只有一个被阻止的高流量页面都可能会导致 AI 驱动的品牌曝光度大幅下降。

**全部阻止和选择性阻止两者之间有什么区别？**

全部阻止意味着所有爬虫，包括一般的 Web 爬虫，都不能访问页面。 选择性阻止意味着一般爬虫可以访问这个页面，但一些特定的 AI 代理无法访问。 这个机会只标记选择性阻止，因为这种情况是有意或意外地阻止 AI 代理访问其他爬虫可公开访问的内容，这是最具可操作性的发现。

**LLM Optimizer 检查哪些 AI 代理？**

LLM Optimizer 会检查 ClaudeBot、GPTBot、OAI-SearchBot、OAI 用户、PerplexityBot 和 Perplexity 用户。

**如果我想有意阻止某些 AI 代理，应该怎么做？**

您可以审阅每一个被标记的指令，然后选择您想保留的阻止规则。 被忽略的建议会在运行审核时保留，但不再重新显示，除非 `robots.txt` 文件发生变化，并且相关规则重新显示。

**LLM Optimizer 如何跟踪我的 robots.txt 随时间的变化？**

LLM Optimizer 使用哈希处理在每次运行时跟踪您的 `robots.txt` 内容。 如果一个之前已解决的阻止规则又重新出现，例如在某次更新 `robots.txt` 后，它将作为一个新建议重新显示。

**如何确定热门页面？**

页面的来源综合了您流量最高的 SEO 页面、内容传递网络日志中 AI 代理访问量最高的 URL 以及您的网站配置中指定的任何自定义 URL。
