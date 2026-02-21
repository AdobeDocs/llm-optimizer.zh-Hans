---
title: 快速入门
description: 开始使用 Adobe LLM Optimizer —— 加入您的品牌，解锁 AI 可见性洞察，并探索仪表板以提升搜索表现。
feature: Quickstart, Onboarding
source-git-commit: 82830e66d43ddd9741617cdf6daab63cd259554b
workflow-type: tm+mt
source-wordcount: '1152'
ht-degree: 93%

---


# 快速入门

要开始使用LLM Optimizer，您需要完成载入流程，如以下步骤所述。 完成该流程后，您将获得对 [LLM Optimizer 的仪表板](/help/dashboards/dashboards-overview.md)及其他功能的完整访问权限。

## 加入概述

加入流程从加入您的域名开始。 根据您是否为 AEM Cloud 客户，流程会有所不同。 完成该流程后，您需要提供内容传递网络日志转发信息，并最终自定义类别、主题和提示词。 以下将详细介绍流程的每个部分，并提供帮助您尽快开始使用 LLM Optimizer 的实用建议。

### 允许 Adobe LLM Optimizer 访问公开页面

为了提供准确的内容和技术建议，Adobe LLM Optimizer 需要访问您的公开页面。 该访问通过一个安全的内部爬虫（Spacecat/1.0 用户代理）实现。

配置要求：

* 将 Spacecat/1.0 用户代理添加到您网站的 robots.txt 文件或机器人流量管理规则中的允许列表。
* 确保页面在域名或内容传递网络层级未设置访问限制。 遭到阻止的页面无法索引，这意味着无法为其生成优化任务和相关洞察。

如果仪表板中显示内容可见度较低，请确认爬虫是否可以访问您的域名。 访问受限是导致索引不完整的常见原因。

## 步骤 1：加入您的域名

### 先试用后购买

AEM Cloud(Cloud Service、Managed Services、Edge Delivery服务)客户可以选择使用&#x200B;**在购买之前尝试**&#x200B;选件。 该方案为 LLM Optimizer 的免费试用版本，最多包含 200 个免费提示词。 使用超过 200 个提示词需要单独签署许可协议。 该访问权限以“按现状”和“按可用性”方式提供，Adobe 可随时对其进行修改、限制或取消。

免费版本中有部分产品功能不可用：

* 试用仅限一个域名。 完成设置后，您将无法更改所提供的域名。
* 部署优化的功能可在早期访问中获得。 若要了解详情，请访问[在Edge中优化常见问题解答](https://experienceleague.adobe.com/zh-hans/docs/llm-optimizer/using/resources/optimize-at-edge/overview#frequently-asked-questions)。

有关如何激活免费试用版本并加入域名的详细信息，请参阅下方内容。

### AEM Cloud 客户

如果您是 AEM Cloud 客户，可以通过 [Experience Hub](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/experience-hub/experience-hub) 中的“产品公告”卡片试用 LLM Optimizer。

>[!NOTE]
>新添加的提示词在处理完成之前不会显示在[品牌存在感仪表板](/help/dashboards/brand-presence.md)中。 AEM Cloud 客户可以使用 LLM Optimizer 的免费试用版本。 使用超过 200 个提示词需要单独签署许可协议。 该访问权限以“按现状”和“按可用性”方式提供，Adobe 可随时对其进行修改、限制或取消。 如需更多信息，请联系您的客户代表。

![LLM Optimizer 试用版](/help/overview/assets/llm-trial.png)

单击&#x200B;**试用 LLM Optimizer** 按钮后，系统会将您重定向至 [https://llmo.now](https://llmo.now)。 随后，您需要通过 IMS 登录。 登录后，您将通过提供域名和品牌名称来启动加入流程。

![LLM Optimizer 域名](/help/overview/assets/domain.png)

>[!NOTE]
>您提供的域名将供组织内所有成员使用，且无法更改。

在加入阶段，将自动生成一小组类别、主题和提示词。 在网站完成加入后不久，即可查看基于这些提示词生成的品牌存在感分析。

<!--![Brand Presence Analysis](/help/overview/assets/bp-analysis.png)-->

此外，您还需要配置[内容传递网络日志转发](#step-4)以进行流量分析。 LLM Optimizer 需要品牌存在感数据以及来自代理式和引荐流量的洞察，以识别机会并提供可执行的优化建议，从而提升 AI 可见性。

### 非 AEM Cloud 客户

在完成商业协议后，您将使用希望加入 LLM Optimizer 的域名进行加入。 完成加入后，您将可以通过 [https://llmo.now](https://llmo.now) 登录 LLM Optimizer。

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

要解锁代理式流量和引荐流量洞察，您需要提供内容传递网络日志转发信息。 您可以在[客户配置仪表板](/help/dashboards/customer-configuration.md#cdn-configuration)中，通过导航至&#x200B;**内容传递网络配置**&#x200B;选项卡并单击&#x200B;**引入内容传递网络**&#x200B;来添加该信息。

![客户配置内容传递网络](/help/overview/assets/cc-cdn.png)

或者，如果此前尚未添加内容传递网络提供商（如上所述），则在首次访问代理式和引荐流量仪表板时，系统会提示您添加内容传递网络日志转发。 有关更多详细信息，请参阅：

* [代理式流量](/help/dashboards/agentic-traffic.md#cdn-setup)
* [引荐流量](/help/dashboards/referral-traffic.md#setup#setup)

## 步骤 5：探索仪表板并采取行动

在您提供内容传递网络日志转发信息后，您可以：

* 查看[品牌存在感](/help/dashboards/brand-presence.md)仪表板，查看您的可见度分数，并跟踪您相对于其他品牌的表现。
* 如果已配置内容传递网络日志转发，请探索[代理式](/help/dashboards/agentic-traffic.md)和[引荐流量](/help/dashboards/referral-traffic.md)仪表板。
* 使用[机会](/help/dashboards/opportunities.md)识别内容和技术优化改进点。
* 导出数据，与您的团队协作，或邀请同事使用该产品。

最后，为了全面了解 LLM Optimizer 的功能，您应探索所有可用的[仪表板](/help/dashboards/dashboards-overview.md)。
