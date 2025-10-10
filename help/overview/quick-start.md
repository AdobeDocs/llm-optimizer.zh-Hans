---
title: 快速入门
description: 这是快速入门文章。
source-git-commit: 5dbf794b87df92583daec83ab02063821ee7a412
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 0%

---


# 快速入门

要开始使用LLM Optimizer，您需要完成载入流程。 之后，您将拥有LLM Optimizer功能板和所有功能的完全访问权限。

## 载入概述

载入流程从载入您的域开始。 根据您是否是AEM Cloud客户，此过程会有所不同。 完成该过程后，您需要为CDN日志转发提供信息，并最终自定义类别、主题和提示。

### 步骤1：载入域

### AEM Cloud客户

AEM Cloud客户(Cloud Service/Managed Services/Edge Delivery服务)将看到用于通过[Experience Hub](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/experience-hub/experience-hub)中的产品公告卡试用LLM Optimizer的选项。

>[!NOTE]
>在处理完成之前，新添加的提示将不会显示在品牌在线状态中。 AEM Cloud客户(Cloud Service、Managed Services/Edge Delivery服务)可以使用LLM Optimizer的免费试用版。 使用200多个提示需要单独的许可协议。 访问是按“原样”和“可用”提供的，并可由Adobe随时修改、限制或删除。 请联系您的[帐户代表]以获取更多信息。

![LLM Optimizer试用版](/help/overview/assets/llm-trial.png)

单击&#x200B;**尝试LLM Optimizer**&#x200B;按钮后，您将被重定向到[https://llmo.now](https://llmo.now)。 然后，您将需要通过IMS登录。 登录后，您将通过提供域和品牌名称开始载入流程。

![LLM Optimizer域](/help/overview/assets/domain.png)

>[!NOTE]
>您提供的域将由贵组织的每个人使用，无法更改。

要触发Brand Presence Analysis，您需要提供类别、主题和提示。

![品牌状态分析](/help/overview/assets/bp-analysis.png)

此外，您还需要配置CDN日志转发以进行流量分析。 LLM Optimizer需要从代理和反向链接流量获得Brand Presence数据和见解，以识别机会并提供规范性建议，帮助客户提升其AI可见性。

### 非AEM Cloud客户

在您签署合同后，将通过slackbot命令为您希望在LLM Optimizer上载入的域进行载入。 完成此载入后，您将能够通过[https://llmo.now](https://llmo.now)登录到LLM Optimizer。

### 步骤2：自动预填充见解

您的域载入后，LLM Optimizer将自动填充以下内容：

* **类别** — 与您的域相关的广泛内容领域。
* **主题** — 与域关联的高容量非品牌关键字关联的特定主题。
* **提示** — 查询（标记和非标记）以提供基线可见性。

这可确保您甚至在添加自定义配置和输入之前，就已看到对品牌可见性的初步洞察。

### 步骤3：自定义类别、主题和提示

单击[客户配置仪表板](/help/dashboards/customer-configuration.md)开始自定义您的类别、主题和提示。

![客户配置信息板](/help/dashboards/assets/customer-config.png)

从该功能板，您可以：

* 添加与您的业务优先级一致的新类别。
* 输入需要跟踪的自定义主题或子主题。
* 创建提示以监视特定查询中的可见性。
* 定义提及别名，以便捕获所有提及。
* 定义竞争对手别名以准确跟踪竞争对手。

### 步骤4：提供有关CDN日志转发的信息

要解锁代理流量和反向链接流量洞察，您需要为CDN日志转发提供信息。 有关如何配置日志转发的更多详细信息，请参阅每个特定页面：

* [代理流量](/help/dashboards/agentic-traffic.md)
* [反向链接流量](/help/dashboards/referral-traffic.md#setup#cdn-setup)

### 步骤5：浏览功能板并采取行动

提供CDN日志转发的信息后，您可以：

* 查看[Brand Presence](/help/dashboards/brand-presence.md)仪表板，查看可见性得分并跟踪您相对于竞争对手的表现。
* 浏览[代理](/help/dashboards/agentic-traffic.md)和[引用流量](/help/dashboards/referral-traffic.md)仪表板。
* 使用[机会](/help/dashboards/opportunities.md)确定内容和技术改进。
* 导出数据并与团队协作，或邀请同事使用该产品。

要全面了解LLM优化器的功能，请探索所有可用的[功能板](/help/dashboards/dashboards-overview.md)。
