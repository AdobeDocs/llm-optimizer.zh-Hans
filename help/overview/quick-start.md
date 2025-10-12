---
title: 快速入门
description: 开始使用Adobe LLM Optimizer — 加入您的品牌，解锁AI可见性洞察，并探索功能板以提高搜索性能。
source-git-commit: 4cbfbe420a8419a04c2d6c465b6a290ee00ff3d4
workflow-type: tm+mt
source-wordcount: '908'
ht-degree: 0%

---


# 快速入门

要开始使用LLM Optimizer，您需要完成载入流程，如以下步骤所述。 完成该过程后，您将拥有[LLM Optimizer功能板](/help/dashboards/dashboards-overview.md)和其他功能的完全访问权限。

## 载入概述

载入流程从载入您的域开始。 根据您是否是AEM Cloud客户，此过程会有所不同。 完成该过程后，您需要为CDN日志转发提供信息，并最终自定义类别、主题和提示。 此过程的每个部分详见下文，其中包含有关如何尽快开始使用LLM Optimizer的有用提示。

## 步骤1：载入域

### 购买之前请尝试

AEM Cloud(Cloud Service、Managed Services、Edge Delivery服务)客户可以选择使用“购买前尝试”选件。 它是LLM Optimizer的免费试用版，最多可提供200个免费提示。 使用200多个提示需要单独的许可协议。 访问是按“原样”和“可用”提供的，并可由Adobe随时修改、限制或删除。

有些产品功能在免费版本中不可用：

* 试用仅限于一个域。 完成设置后，您将无法更改提供的域。
* 对部署优化的支持将不可用。

有关如何激活免费试用版和加入域的详细信息，请参阅以下部分。

### AEM Cloud客户

如果您是AEM Cloud客户，则可以选择使用[Experience Hub](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/experience-hub/experience-hub)中的产品公告卡来尝试LLM Optimizer。

>[!NOTE]
>在处理完成之前，新添加的提示将不会显示在[Brand Presence功能板](/help/dashboards/brand-presence.md)中。 AEM Cloud客户(Cloud Service、Managed Services/Edge Delivery服务)可以使用LLM Optimizer的免费试用版。 使用200多个提示需要单独的许可协议。 访问是按“原样”和“可用”提供的，并可由Adobe随时修改、限制或删除。 有关更多信息，请联系您的客户代表。

![LLM Optimizer试用版](/help/overview/assets/llm-trial.png)

单击&#x200B;**尝试LLM Optimizer**&#x200B;按钮后，您将被重定向到[https://llmo.now](https://llmo.now)。 然后，您将需要通过IMS登录。 登录后，您将通过提供域和品牌名称开始载入流程。

![LLM Optimizer域](/help/overview/assets/domain.png)

>[!NOTE]
>您提供的域将由贵组织的每个人使用，无法更改。

要触发Brand Presence Analysis，您需要提供类别、主题和提示。

![品牌状态分析](/help/overview/assets/bp-analysis.png)

此外，您还需要配置[CDN日志转发](#step-4)以进行流量分析。 LLM Optimizer需要从代理和反向链接流量获得Brand Presence数据和见解，以识别机会并提供规范性建议，从而提高AI可见性。

### 非AEM Cloud客户

业务协议完成后，您将通过slackbot命令登录到要登录到LLM Optimizer的域。 完成此载入后，您将能够通过[https://llmo.now](https://llmo.now)登录到LLM Optimizer。

### 第2步：自定义类别、主题和提示

要触发Brand Presence分析并在功能板中填充有关您的品牌可见性的洞察，您需要自定义“类别”、“主题”和“提示”。 此配置在[客户配置仪表板](/help/dashboards/customer-configuration.md)上创建。

![客户配置信息板](/help/overview/assets/prompt-creation.png)

从该功能板，您可以：

* 添加与您的业务优先级一致的&#x200B;**新类别**。 类别可以是与域相关的广泛内容领域。
* 输入要跟踪的&#x200B;**自定义主题**&#x200B;或子主题。 主题可以是与您的域关联的大量非品牌关键字关联的特定主题。
* 创建&#x200B;**您的提示**&#x200B;以监视特定查询中的可见性。 提示是提供基线可见性的查询（标记和非标记）。 根据您提供的类别和主题，只会自动生成有限数量的提示。
* 定义提及&#x200B;**别名**&#x200B;以确保捕获并记录品牌的所有提及。
* 定义&#x200B;**竞争对手别名**&#x200B;以准确跟踪竞争对手。

>[!NOTE]
>您询问LLM的确切提示不会公开提供，因为LLM不公开这些提示。

### 步骤3：自动预填充见解

一旦您的域上线并提供了类别和主题，LLM Optimizer就会自动触发Brand Presence分析。

### 步骤4：提供有关CDN日志转发的信息 {#step-4}

要解锁代理流量和反向链接流量洞察，您需要为CDN日志转发提供信息。 通过导航到[CDN配置](/help/dashboards/customer-configuration.md)选项卡并单击&#x200B;**板载CDN**，可以从&#x200B;**客户配置仪表板**&#x200B;添加它。

![客户配置CDN](/help/overview/assets/cc-cdn.png)

或者，如果事先未添加CDN提供商，则在首次访问代理和反向链接流量功能板时，系统会提示您添加CDN日志转发。 有关更多详细信息，请参阅：

* [代理流量](/help/dashboards/agentic-traffic.md#cdn-setup)
* [反向链接流量](/help/dashboards/referral-traffic.md#setup#setup)

### 步骤5：浏览功能板并采取行动

提供CDN日志转发的信息后，您可以：

* 查看[Brand Presence](/help/dashboards/brand-presence.md)仪表板，查看可见性得分并跟踪您相对于竞争对手的表现。
* 如果配置了CDN日志转发，请浏览[代理](/help/dashboards/agentic-traffic.md)和[引用流量](/help/dashboards/referral-traffic.md)仪表板。
* 使用[机会](/help/dashboards/opportunities.md)确定内容和技术改进。
* 导出数据并与团队协作，或邀请同事使用该产品。

最后，为了充分了解LLM优化器的功能，请探索所有可用的[仪表板](/help/dashboards/dashboards-overview.md)。
