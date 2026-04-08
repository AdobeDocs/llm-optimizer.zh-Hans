---
title: 快速入门
description: 了解如何完成品牌名称和域名的加入，从 Experience Hub 或 Experience Cloud 激活试用，并完成 Adobe LLM Optimizer 的设置。
feature: Quickstart, Onboarding
source-git-commit: dcbeb1c61dd9dcefd83908f65f8303d36c0fb78e
workflow-type: ht
source-wordcount: '1208'
ht-degree: 100%

---


# 快速入门

要开始使用 LLM Optimizer，您需要完成加入流程。完成加入后，您将能够自定义类别、主题和提示，并配置日志转发，以获得更准确的洞察，同时全面访问 [LLM Optimizer 仪表板](/help/dashboards/dashboards-overview.md)及其他功能。

## 加入概述

加入流程从添加您的域和品牌名称开始。以下将详细介绍加入历程的各个环节，并提供实用建议，帮助您尽快开始使用 LLM Optimizer。

### 允许 Adobe LLM Optimizer 访问公开页面

为了提供准确的内容和技术建议，Adobe LLM Optimizer 需要访问您的公开页面。 该访问通过一个安全的内部爬虫（Spacecat/1.0 用户代理）实现。

配置要求：

* 将 Spacecat/1.0 用户代理添加到您网站 robots.txt 文件或机器人流量管理规则的允许列表中。
* 确保页面在域或内容传递网络层均未设置访问限制。遭到阻止的页面无法索引，这意味着无法为其生成优化任务和相关洞察。

如果仪表板中显示内容可见性较低，请确认爬虫是否可以访问您的域名。 访问受限是导致索引不完整的常见原因。

## 步骤 1：加入您的品牌名称和域 {#step-1-onboard-your-domain}

要开始使用 LLM Optimizer，请先激活试用版（如符合条件），并加入您的品牌名称和域。

### 激活试用版

激活流程因您所使用的 Adobe 产品而有所不同。

#### AEM Cloud 客户

作为 AEM Cloud 客户，您可以通过以下方式之一激活试用版：

* 前往 [Experience Hub](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/experience-hub/experience-hub)，使用产品公告卡片激活 LLM Optimizer。选择&#x200B;**试用 LLM Optimizer** 后，系统会将您重定向至 [https://llmo.now](https://llmo.now)。 通过 IMS 登录，然后输入域和品牌名称以开始加入流程。
* 或者直接访问 [https://llmo.now](https://llmo.now) 并登录。

![LLM Optimizer 试用版](/help/overview/assets/llm-trial.png)

#### Adobe Analytics 客户

如果您是 Adobe Analytics 客户，您将在 Experience Cloud 主页看到横幅提示。

![Experience Cloud 主页显示“开始试用 Adobe LLM Optimizer”横幅](/help/overview/assets/experience-cloud-llmo-trial-banner.png)

您可以通过以下任一方式激活试用版：

* 在横幅中选择&#x200B;**开始试用 Adobe LLM Optimizer**。
* 或直接访问 [https://llmo.now](https://llmo.now) 并登录。

试用版激活后，请继续加入您的品牌名称和域。

>[!NOTE]
>
> * **免费试用：** AEM Cloud 和 Adobe Analytics 客户可使用 LLM Optimizer 的免费试用版本。
> * **在 2026 年 4 月 1 日及之后激活试用版的客户**，最多可使用 100 个提示、1 个域，并可针对单一机会类型在最多 10 个 URL 上部署优化。
> * **在 2026 年 4 月 1 日之前激活试用版的客户**，可根据现有条款继续使用最多 200 个提示。
>
>超出上述使用限制需另行签署许可协议。访问权限按“现状”和“可用性”提供，可能随时会修改、限制或终止。如需了解更多信息，请联系您的客户代表。

#### 加入您的品牌名称和域

加入您的品牌名称和域，以开始使用 LLM Optimizer。

1. 输入您的品牌名称及其对应的域。

   * 该域应为您希望进行内容分析和优化的主域。

1. 完成加入。

   * 提交后，LLM Optimizer 将开始分析您的域并生成洞察。

![LLM Optimizer 域名](/help/overview/assets/domain.png)

>[!NOTE]
>新添加的提示词在处理完成之前不会显示在[品牌存在感仪表板](/help/dashboards/brand-presence.md)中。

>[!NOTE]
>您提供的域名将供组织内所有成员使用，且无法更改。

在加入阶段，将自动生成一小组类别、主题和提示词。 在网站完成加入后不久，即可查看基于这些提示词生成的品牌存在感分析。

同时支持在边缘端部署优化。了解更多信息，请参阅 [Optimize at Edge — 常见问题](https://experienceleague.adobe.com/zh-hans/docs/llm-optimizer/using/resources/optimize-at-edge/overview#frequently-asked-questions)。

此外，请配置[内容传递网络日志转发](#step-4)以进行流量分析。LLM Optimizer 需要品牌存在感数据以及来自代理式流量和引荐流量的洞察，以识别优化机会并提供可执行建议，从而提升 AI 可见度。

### 非 AEM Cloud 客户

在您的组织完成商务协议后，系统将使用您的组织所选择的域为您加入 LLM Optimizer。加入完成后，请在 [https://llmo.now](https://llmo.now) 登录。

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

要解锁代理式流量和引荐流量洞察，请在[客户配置仪表板](/help/dashboards/customer-configuration.md#cdn-configuration)中添加内容传递网络日志转发信息。打开&#x200B;**内容传递网络配置**&#x200B;选项卡，然后选择&#x200B;**加入内容传递网络**。

![客户配置内容传递网络](/help/overview/assets/cc-cdn.png)

或者，如果此前尚未添加内容传递网络提供商（如上所述），则在首次访问代理式和引荐流量仪表板时，系统会提示您添加内容传递网络日志转发。 有关更多详细信息，请参阅：

* [代理式流量](/help/dashboards/agentic-traffic.md#cdn-setup)
* [引荐流量](/help/dashboards/referral-traffic.md#setup)

>[!NOTE]
>有关使用客户自管内容传递网络（BYOCDN）进行日志转发的详细信息，请参阅 [BYOCDN 日志转发概述](/help/overview/log-forwarding/log-forwarding-overview.md)

## 步骤 5：探索仪表板并采取行动

在您提供内容传递网络日志转发信息后，您可以：

* 查看[品牌存在感](/help/dashboards/brand-presence.md)仪表板，查看您的可见度分数，并跟踪您相对于其他品牌的表现。
* 完成内容传递网络日志转发配置后，您可以查看[代理式流量](/help/dashboards/agentic-traffic.md)和[引荐流量](/help/dashboards/referral-traffic.md)仪表板。
* 使用[机会](/help/dashboards/opportunities.md)识别内容和技术优化改进点。
* 导出数据，与您的团队协作，或邀请同事使用该产品。

最后，为了全面了解 LLM Optimizer 的功能，您应探索所有可用的[仪表板](/help/dashboards/dashboards-overview.md)。
