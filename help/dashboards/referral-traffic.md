---
title: 反向链接流量
description: 了解如何使用反向链接流量仪表板，以查看访客如何从外部平台、AI引用和反向链接到达您的网站。
source-git-commit: a6d050516e681094d97b25fcdc59e9ed4e60e104
workflow-type: tm+mt
source-wordcount: '599'
ht-degree: 0%

---


# 反向链接流量

反向链接流量显示访客如何从外部平台、AI引用和反向链接到达您的网站。 它跟踪和分析来自外部网站和平台的流量源、反向链接模式和转化量度。 这有助于您了解哪些源、地区和页面产生的参与度最高的流量。 <!--Data is sourced from the CDN logs, a privacy-preserving source that does not capture personal user data.-->还有可自定义的筛选器帮助您优化显示的数据。

![推荐页面](/help/dashboards/assets/referral-traffic.png)

本页详细介绍以下内容：

* [设置](#setup)
* [过滤器](#filters)
* [总体推荐业绩](#overall-performance)
* [主要引用URL](#top-referrals)
* [反向链接流量详细信息](#traffic-details)

## 设置 {#setup}

首次登录时，反向链接流量仪表板可能显示为空白。 要查看您的数据，您必须通过选择[转到配置](/help/dashboards/customer-configuration.md#cdn-configuration)来配置&#x200B;**CDN日志转发**。

![反向链接设置](/help/dashboards/assets/referral-setup1.png)

<!--- 1. Select your Source (either CDN logs or AEM Operational Telemetry).
2. Enter a primary contact email.
3. Click **Request activation** to enable data ingestion. Hiding this until confirmation from PM-->

激活后，仪表板将填充引用流量量度。

## 过滤器 {#filters}

在页面顶部，您可以应用过滤器来优化视图。 您选择的筛选器将影响仪表板上的&#x200B;**所有**&#x200B;部分。 您可以自定义以下内容：

* **日期范围** — 为显示的数据选择时间范围。 例如，最近4周。 您还可以通过选择&#x200B;**自定义周**&#x200B;选项来自定义时间段。
* **平台** — 选择特定流量源，如Google、OpenAI或社交媒体。
* **页面意图** — 按用户意图筛选反向链接流量。
* **渠道Source** — 按渠道源筛选。 相关选项包括：LLM、已获取、已付或混合型转介渠道。
* **设备类型** — 按访客的设备类型（桌面、移动设备或所有设备）分析流量。
  **区域** — 查看跨不同地理位置的引用模式。

选择所需的筛选器后，单击&#x200B;**应用筛选器**&#x200B;以将所选内容应用到仪表板。

## 总体推荐业绩 {#overall-performance}

仪表板通过显示关键量度来突出显示总体反向链接性能，包括：

* **反向链接流量总计** — 来自所有源的反向链接流量总计。
* **同意率** — 接受同意提示的访客百分比。
* **跳出率** — 来自没有参与事件的反向链接源的会话百分比。

![推荐页面](/help/dashboards/assets/referral-traffic.png)

除了上述总体性能量度之外，**热门区域**&#x200B;面板还按地理位置划分流量。 同时，**热门反向链接源**&#x200B;面板会显示促成最多访问的平台。 量度的趋势指示器显示这些值与上一时段相比如何随时间的变化。

<!--## Top Referral URLs {#top-referrals}

The Top Referral URLs list surfaces your site’s most visited pages from referrals.

![Top Referral URLs](/help/dashboards/assets/top-url.png)-->

## 反向链接源详细信息和URL性能分析 {#traffic-details}

“反向链接源详细信息”和“URL性能分析”表可帮助您评估流量和质量。 单击下面的每个选项卡以了解更多详细信息：

![引用流量详细信息](/help/dashboards/assets/traffic-details.png)

>[!BEGINTABS]

>[!TAB 反向链接源详细信息]

“反向链接源详细信息”视图划分来自不同平台(如OpenAI、Microsoft、Google和Perplexity)的流量。 它显示访问次数、跳出率和渠道类型等关键量度，帮助您了解哪些AI和搜索来源为您的网站带来了最多参与流量。

* **Source** — 反向链接流量的来源。
* **访问次数** — 每个源的访问总数。
* **跳出率** — 来自反向链接源且没有参与事件的会话的百分比。
* **渠道** — 来源的渠道，可以免费获取，也可以付费获取，或者两者都有。

>[!TAB URL性能分析]

URL性能分析视图根据LLM和其他源的引用流量对性能最佳的页面进行排名。 它突出显示流量、跳出率、同意率和页面意图等量度，帮助您识别哪些页面吸引并保留AI驱动型反向链接中最参与的访客。 该表具有用于快速访问主题的搜索字段。

>[!ENDTABS]

在这两个表中，您可以使用&#x200B;**导出**&#x200B;选项来下载表.csv，并与团队共享见解，或者在执行报表中包含反向链接流量表。
