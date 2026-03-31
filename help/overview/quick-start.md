---
title: 快速入门
description: 了解如何载入您的品牌名称和域、从Experience Hub或Experience Cloud激活您的试用版，以及完成Adobe LLM Optimizer的设置。
feature: Quickstart, Onboarding
source-git-commit: dcbeb1c61dd9dcefd83908f65f8303d36c0fb78e
workflow-type: tm+mt
source-wordcount: '1208'
ht-degree: 50%

---


# 快速入门

要开始使用LLM Optimizer，您需要完成载入流程。 完成新用户引导后，您将能够自定义类别、主题、提示并配置日志转发，以便获得更准确的见解并完全访问[LLM Optimizer的功能板](/help/dashboards/dashboards-overview.md)和其他功能。

## 加入概述

载入流程从载入您的域和品牌名称开始。 下面详细描述了入门培训历程的每个部分，以及有关如何尽快开始使用LLM Optimizer的有用提示。

### 允许 Adobe LLM Optimizer 访问公开页面

为了提供准确的内容和技术建议，Adobe LLM Optimizer 需要访问您的公开页面。 该访问通过一个安全的内部爬虫（Spacecat/1.0 用户代理）实现。

配置要求：

* 将Spacecat/1.0用户代理添加到您站点的robots.txt文件或bot-traffic管理规则中的。
* 确保在域或CDN级别上均不会阻止页面。 遭到阻止的页面无法索引，这意味着无法为其生成优化任务和相关洞察。

如果仪表板中显示内容可见度较低，请确认爬虫是否可以访问您的域名。 访问受限是导致索引不完整的常见原因。

## 步骤1：载入您的品牌名称和域 {#step-1-onboard-your-domain}

要开始使用LLM Optimizer，请首先激活您的试用版（如果适用），然后加入您的品牌名称和域。

### 激活您的试用版

激活流程因您的Adobe产品而异。

#### AEM Cloud客户

要激活试用版，作为AEM Cloud客户，您可以：

* 导航到[Experience Hub](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/experience-hub/experience-hub)并使用产品公告卡激活LLM Optimizer。 选择&#x200B;**尝试LLM Optimizer**&#x200B;后，您将被重定向到[https://llmo.now](https://llmo.now)。 通过IMS登录，然后输入域和品牌名称以开始载入流程。
* 或直接转到[https://llmo.now](https://llmo.now)并登录。

![LLM Optimizer 试用版](/help/overview/assets/llm-trial.png)

#### Adobe Analytics客户

如果您是Adobe Analytics客户，则会在Experience Cloud主页上看到一条横幅。

![Experience Cloud主页上的“开始Adobe LLM Optimizer试用”横幅](/help/overview/assets/experience-cloud-llmo-trial-banner.png)

您可以通过以下方式之一激活试用版：

* 在横幅中选择&#x200B;**开始您的Adobe LLM Optimizer试用版**。
* 直接转到[https://llmo.now](https://llmo.now)并登录。

一旦试用版生效，请继续载入您的品牌名称和域。

>[!NOTE]
>
> * **免费试用版：** AEM Cloud和Adobe Analytics客户可以使用LLM Optimizer的免费试用版。
> * **在2026年4月1日或之后激活试用版的客户**&#x200B;最多可以使用100个提示、一个域，并且可以为单个机会类型跨最多10个URL部署优化。
> * **在2026年4月1日之前激活该试用版的客户**&#x200B;按照其现有条款，可继续访问最多200个提示。
>
>超出所包括的限制的使用需要单独的许可协议。 访问是按“原样”和“可用”提供的，可随时修改、限制或删除。 有关更多信息，请与您的客户代表联系。

#### 载入您的品牌名称和域

载入您的品牌名称和域以开始使用LLM Optimizer。

1. 输入您的品牌名称和关联的域。

   * 这应该是您要分析和优化内容的主域。

1. 完成入门。

   * 提交后，LLM Optimizer将开始分析您的域并生成见解。

![LLM Optimizer 域名](/help/overview/assets/domain.png)

>[!NOTE]
>新添加的提示词在处理完成之前不会显示在[品牌存在感仪表板](/help/dashboards/brand-presence.md)中。

>[!NOTE]
>您提供的域名将供组织内所有成员使用，且无法更改。

在加入阶段，将自动生成一小组类别、主题和提示词。 在网站完成加入后不久，即可查看基于这些提示词生成的品牌存在感分析。

还提供了在Edge部署优化的功能。 请参阅[在Edge中优化 — 常见问题解答](https://experienceleague.adobe.com/zh-hans/docs/llm-optimizer/using/resources/optimize-at-edge/overview#frequently-asked-questions)以了解详情。

此外，配置[CDN日志转发](#step-4)以进行流量分析。 LLM Optimizer需要来自代理和引荐流量的品牌存在感数据和见解，以识别机会并提供提高AI可见性的规范性建议。

### 非AEM云客户

在您的组织最终确定业务协议后，您将登记到您组织所选的域的LLM Optimizer。 完成载入时，登录到[https://llmo.now](https://llmo.now)。

## 步骤 2：自定义类别、主题和提示词

在您的网站完成加入后，您可以基于加入阶段自动生成的一小组提示词查看品牌存在感分析。 接下来，您可以为您的品牌自定义类别、主题和提示词。 此配置在[客户配置仪表板](/help/dashboards/customer-configuration.md)中创建。

![客户配置仪表板](/help/overview/assets/prompt-creation.png)

在此仪表板中，您可以：

* 添加与您的业务重点相匹配的&#x200B;**新类别**。 类别可以是与您的域名相关的广泛内容领域。
* 输入您希望跟踪的&#x200B;**自定义主题**&#x200B;或子主题。 主题可以是与您的域名相关的高搜索量非品牌关键词所对应的具体主题。
* 创建&#x200B;**您的提示词**，以监测特定查询中的可见度。 提示词是用于提供基准可见度的查询（包括品牌和非品牌）。 系统仅会根据您提供的类别和主题自动生成数量有限的提示词。
* 定义提及&#x200B;**别名**，以确保捕获并统计品牌的所有不同提及形式。
* 定义&#x200B;**其他别名**，以准确跟踪其他品牌。

>[!NOTE]
>您向 LLM 提出的具体提示词不会公开，因为 LLM 不会披露这些内容。

>[!NOTE]
>
> 有关如何设置类别、主题和提示词的更多信息，请参阅[配置类别、主题和提示词的最佳做法](/help/overview/best-practices-topics-prompts.md)页面。

## 步骤 3：品牌存在感洞察

在您的域名完成加入后，您将在品牌存在感视图中看到基于加入阶段自动生成提示词的初始洞察。 当您自定义类别、主题和提示词后，LLM Optimizer 将自动对您提供的提示词触发品牌存在感分析，并在 24 小时内提供结果。

## 步骤 4：提供内容传递网络日志转发信息 {#step-4}

若要解锁代理流量和引荐流量分析，请从[客户配置仪表板](/help/dashboards/customer-configuration.md#cdn-configuration)添加CDN日志转发信息。 打开&#x200B;**CDN配置**&#x200B;选项卡并选择&#x200B;**板载CDN**。

![客户配置内容传递网络](/help/overview/assets/cc-cdn.png)

或者，如果此前尚未添加内容传递网络提供商（如上所述），则在首次访问代理式和引荐流量仪表板时，系统会提示您添加内容传递网络日志转发。 有关更多详细信息，请参阅：

* [代理式流量](/help/dashboards/agentic-traffic.md#cdn-setup)
* [引荐流量](/help/dashboards/referral-traffic.md#setup)

>[!NOTE]
>有关使用客户自管内容传递网络（BYOCDN）进行日志转发的详细信息，请参阅 [BYOCDN 日志转发概述](/help/overview/log-forwarding/log-forwarding-overview.md)

## 步骤 5：探索仪表板并采取行动

在您提供内容传递网络日志转发信息后，您可以：

* 查看[品牌存在感](/help/dashboards/brand-presence.md)仪表板，查看您的可见度分数，并跟踪您相对于其他品牌的表现。
* 如果配置了CDN日志转发，请浏览[代理](/help/dashboards/agentic-traffic.md)和[引荐流量](/help/dashboards/referral-traffic.md)仪表板。
* 使用[机会](/help/dashboards/opportunities.md)识别内容和技术优化改进点。
* 导出数据，与您的团队协作，或邀请同事使用该产品。

最后，为了全面了解 LLM Optimizer 的功能，您应探索所有可用的[仪表板](/help/dashboards/dashboards-overview.md)。
