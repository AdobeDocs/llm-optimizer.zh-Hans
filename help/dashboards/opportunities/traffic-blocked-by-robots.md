---
title: robots.txt阻止的流量
description: 了解LLM Optimizer如何检测您的robots.txt文件何时阻止AI代理访问您的内容以及如何修复它。
feature: Opportunities
source-git-commit: fa8b994aa55869cf7f2607e792acc4e8abc213e7
workflow-type: tm+mt
source-wordcount: '817'
ht-degree: 1%

---


# robots.txt阻止的流量

您的`robots.txt`文件控制哪些爬虫可以访问您的网站。 当选择性地阻止AI代理访问普通爬虫原本可以访问的内容时，这些代理无法索引或引用该内容，从而直接降低您的品牌在AI生成的响应中的可见性。

Robots.txt阻止的流量机会将针对您的热门页面分析您的`robots.txt`文件，并识别阻止AI代理访问他们应能够访问的内容的规则。 它在单个`robots.txt`行级别显示查找结果，因此您可以查看和更新特定指令，而不是手动审核整个文件。

它一眼就反映了两个关键指标：

- **总URL** — `robots.txt`中受阻止规则影响的URL数。
- **阻止的代理** — 被阻止访问这些URL的AI代理数。

![被robots.txt仪表板阻止的流量](/help/dashboards/opportunities/assets/traffic-blocked-by-robots-overview.png)

## 工作原理

LLM Optimizer将获取您的`robots.txt`文件，并根据六个主要的AI代理用户代理检查您的热门页面：

- 克劳德机器人
- GPTBot
- OAI-SearchBot
- OAI — 用户
- PerplexityBot
- Perplexity — 用户

仅当通配符(`*`)用户代理允许&#x200B;**而特定AI代理**&#x200B;不允许该URL时，才会标记该URL。 所有的爬虫都受到同等限制的一揽子封堵不会报告。 此次审计专门针对选择性人工智能代理排除，这是GEO最切实可行的发现。

>[!NOTE]
>这个机会不使用AI来制定或提供建议。 调查结果完全基于对`robots.txt`文件的直接分析。

结果显示在两个选项卡中：**robots.txt**&#x200B;和&#x200B;**代理阻止的流量详细信息**。

## robots.txt

此选项卡显示完整的`robots.txt`文件，其中阻止指令以红色突出显示。 每个高亮显示的行表示一条规则，该规则选择性地阻止一个或多个AI代理访问在其他情况下可公开访问的URL。

![robots.txt视图（带有突出显示的阻止指令）](/help/dashboards/opportunities/assets/traffic-blocked-by-robots-robotstxt.png)

单击高亮显示的指令会显示有关其影响和建议修复的更多信息。

## 各代理的被阻流量详细信息

此选项卡提供AI代理组织的已阻止流量的细分。 对于每个被阻止的代理，它显示：

- **问题描述** — 有关哪个代理被阻止以及它为什么重要的说明。
- **解决方案** — 有关打开`robots.txt`文件并查看每个受影响的URL旁边列出的特定行号的指导。
- 每个被阻止页面的受影响URL的表，具有&#x200B;**行**、**排名**&#x200B;和&#x200B;**URL**。

每个代理（例如，OAI-User、GPTBot、OAI-SearchBot）都有自己的子选项卡，因此您可以为每个代理寻址块。

## 如何修复

若要解决被阻止的代理查找问题，请打开您的`robots.txt`文件，并在“按代理阻止的流量详细信息”选项卡中找到每个受影响的URL旁边显示的行号。 更新或删除相关AI代理的disallow指令，以允许访问受影响的URL。

例如，要从特定页面取消阻止GPTBot，请删除或更新指令：

```
User-agent: GPTBot
Disallow: /blog/cold-brewing-101
```

更新并重新发布`robots.txt`文件后，LLM Optimizer将在下次审核运行时检测到更改，并将建议标记为已解决。

## 在演示中尝试

使用Frescopa演示环境查看实际操作中的Robots.txt阻止的流量。

[在Frescopa演示中查看被robots.txt阻止的流量](https://play.llmo.now/org/demo-org/opportunities/blocked-urls-llm-agents)

## 常见问题解答

**为什么阻止AI代理对GEO很重要？**

生成引擎优化要求AI爬虫可以访问您的网站内容并将其编入索引。 阻止AI代理会直接阻止您的页面出现在AI生成的响应中，从而减少引用次数、品牌提及次数和总体AI可见性。 即使是一个被阻止的高流量页面，也可能意味着AI驱动的品牌曝光度大幅下降。

**一揽子封锁和选择性封锁之间有何区别？**

一揽子阻止意味着所有爬虫（包括常规Web爬虫）都限制在页面上。 选择性阻止意味着常规爬虫可以访问页面，但特定AI代理无法访问。 此机会仅标记选择性阻止，因为它表示人工智能代理有意或意外地从其他方面公开的内容中排除，这是最切实可行的发现。

**LLM Optimizer检查哪些AI代理？**

LLM Optimizer检查ClaudeBot、GPTBot、OAI-SearchBot、OAI-User、PerplexityBot和Perplexity-User。

**如果我有意阻止某些AI代理，该怎么办？**

您可以查看每个已标记的指令，并选择有意保留块。 已解除的建议会在审核运行期间保留，除非`robots.txt`文件发生更改并且规则重新显示，否则不会重新显示。

**LLM Optimizer如何跟踪一段时间内对我的robots.txt所做的更改？**

LLM Optimizer使用哈希处理跨运行跟踪您的`robots.txt`内容。 如果先前解析的阻止规则重新出现（例如在更新`robots.txt`后），它将作为新建议重新出现。

**如何确定排名最前的页面？**

页面来自流量最高的SEO页面、CDN日志中排名最前的AI代理访问的URL以及在站点配置中指定的任何自定义URL的组合。
