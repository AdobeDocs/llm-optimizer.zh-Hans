---
title: 引荐流量
description: 了解如何使用“引荐流量”仪表板，查看访客通过外部平台、AI 引用和引荐链接访问您站点的情况。
feature: Referral Traffic
source-git-commit: e50c87e8e5a669526f3c10855c1629ce82b67aef
workflow-type: ht
source-wordcount: '605'
ht-degree: 100%

---


# 引荐流量

“引荐流量”展示访客通过外部平台、AI 引用和引荐链接访问您网站的路径。该仪表板跟踪并分析来自外部网站和平台的流量来源、引荐模式及转化量度。通过这些数据，您可以了解哪些来源、区域和页面带来更高参与度的流量。<!--Data is sourced from the CDN logs, a privacy-preserving source that does not capture personal user data.--> 此外还提供可自定义的筛选条件，帮助您细化要显示的数据。

![引荐页面](/help/dashboards/assets/referral-traffic.png)

本页面包含以下内容：

* [设置](#setup)
* [筛选条件](#filters)
* [整体引荐性能](#overall-performance)
* [热门引荐 URL](#top-referrals)
* [引荐流量详细信息](#traffic-details)

## 设置 {#setup}

首次登录时，“引荐流量”仪表板可能显示为空。要查看数据，请选择&#x200B;**前往配置**&#x200B;并完成 [内容传递网络日志转发](/help/dashboards/customer-configuration.md#cdn-configuration)设置。

![引荐设置](/help/dashboards/assets/referral-setup1.png)

<!--- 1. Select your Source (either CDN logs or AEM Operational Telemetry).
2. Enter a primary contact email.
3. Click **Request activation** to enable data ingestion. Hiding this until confirmation from PM-->

完成设置后，仪表板会显示引荐流量相关量度。

## 筛选条件 {#filters}

在页面顶部，您可以应用筛选条件以优化视图。您选择的筛选条件将影响仪表板中的&#x200B;**所有**&#x200B;部分。您可以自定义以下内容：

* **日期范围**：选择显示数据的时间范围。例如，最近 4 周。您也可以选择&#x200B;**自定义周数**&#x200B;选项来自定义时间周期。
* **平台**：选择具体流量来源，例如 Google、OpenAI 或社交媒体。
* **页面意图**：按用户意图筛选引荐流量。
* **渠道源**：按渠道源进行筛选。可选项包括：LLM、获得、付费或混合引荐渠道。
* **设备类型**：按访客设备类型分析流量（桌面端、移动端或全部设备）。  **区域**：查看不同地理区域的引荐模式。

选择所需筛选条件后，单击&#x200B;**应用筛选条件**&#x200B;以将其应用到仪表板。

## 整体引荐性能 {#overall-performance}

仪表板通过以下关键量度展示整体引荐性能：

* **总引荐流量**：来自所有源的引荐流量总量。
* **来自 LLM 的引荐流量**：来自各类 LLM 的引荐流量总量。
* **同意率**：接受同意提示的访客比例。
* **跳出率**：来自引荐来源且未产生参与事件的会话比例。

![引荐页面](/help/dashboards/assets/referral-traffic.png)

除上述整体性能量度外，页面还提供三个附加面板，用于展示不同市场、引荐来源和页面意图类别之间的流量分布情况 <!-- the **Top Regions** panel breaks down traffic by geography. Meanwhile, the **Top Referral Sources** panel shows the platforms driving the most visits. Trend indicators for the metrics show how these values are changing over time compared to the previous period.-->

<!--## Top Referral URLs {#top-referrals}

The Top Referral URLs list surfaces your site's most visited pages from referrals.

![Top Referral URLs](/help/dashboards/assets/top-url.png)-->

## 引荐来源详细信息与 URL 性能分析 {#traffic-details}

“引荐来源详细信息”和“URL 性能分析”表可帮助您同时评估流量规模与质量。点击下方选项卡查看详细信息：

![引荐流量详细信息](/help/dashboards/assets/traffic-details.png)

>[!BEGINTABS]

>[!TAB 引荐来源详细信息]

“引荐来源详细信息”视图按不同平台（如 OpenAI、Microsoft、Google 和 Perplexity）拆解流量来源。该视图显示访问次数、跳出率和渠道类型等关键量度，帮助您了解哪些 AI 和搜索来源为您的网站带来更高参与度的流量。

* **来源**：引荐流量的来源。
* **访问次数**：各来源的总访问次数。
* **跳出率**：来自引荐来源且未产生参与事件的会话比例。
* **渠道**：来源所属渠道类型（获得、付费或两者都有）。

>[!TAB URL 性能分析]

“URL 性能分析”视图根据来自 LLM 及其他来源的引荐流量规模，对性能最佳的页面进行排名。该视图重点展示流量、跳出率、同意率和页面意图等量度，帮助您识别哪些页面最能吸引并留住来自 AI 引荐的高参与度访客。该表提供搜索字段，方便快速查找主题。

>[!ENDTABS]

在这两个表格中，您都可以使用&#x200B;**导出**&#x200B;选项下载 .csv 文件，与团队共享洞察，或将表格纳入管理层报告。此外，在两个表格中，您都可以点击&#x200B;**配置列**&#x200B;按钮来自定义要显示的量度。
