---
title: Adobe Analytics 集成
description: 了解如何将 Adobe Analytics 与 LLM Optimizer 连接，以在引荐流量仪表板中衡量 AI 驱动的发现、网站参与度以及业务成果。
feature: Referral Traffic
source-git-commit: e7c9bc1d40267dc92608baa005f85f4be21cfda1
workflow-type: ht
source-wordcount: '879'
ht-degree: 100%

---


# Adobe Analytics 集成

Adobe Analytics 集成可将 LLM Optimizer 与您组织的 Adobe Analytics 数据连接起来，使您能够衡量 AI 驱动的发现如何转化为实际的网站参与度和业务成果。集成完成后，相关数据将在&#x200B;**业务影响**&#x200B;选项卡下的&#x200B;**引荐流量**&#x200B;仪表板中显示。

通过将分析数据与 AI 可见度洞察相结合，LLM Optimizer 可帮助您跟踪：

* AI 引荐页面上的用户参与度。
* 与 AI 发现路径相关的转化信号。
* GEO 优化带来的性能影响。

该集成弥合了 AI 可见度衡量与业务绩效分析之间的差距。团队不仅可以了解品牌在 AI 响应中的呈现位置，还可以了解用户访问之后的行为。

## 可用性 {#availability}

>[!IMPORTANT]
>
>Adobe Analytics 集成功能包含在付费版 LLM Optimizer 中。使用免费试用版的客户需升级至付费版本后，方可连接 Adobe Analytics。

## 将 Adobe Analytics 连接到引荐流量仪表板 {#connect}

连接流程从[引荐流量](/help/dashboards/referral-traffic.md)仪表板开始，具体如下：

1. 打开[引荐流量](/help/dashboards/referral-traffic.md)仪表板。默认视图为&#x200B;**流量洞察**。

   ![引荐流量仪表板，“流量洞察”选项卡](/help/dashboards/assets/aa-integration-01-referral-traffic-traffic-insights.png)

1. 选择&#x200B;**业务影响**&#x200B;选项卡。 如果尚未连接分析提供商，将显示横幅：**连接以查看业务影响**，并提供&#x200B;**连接到 Analytics** 选项。

   ![提供连接到 Analytics 的业务影响选项卡](/help/dashboards/assets/aa-integration-02-business-impact-connect.png)

1. 选择&#x200B;**连接到 Analytics**。系统将在 **Analytics** 选项卡上打开[客户配置](/help/dashboards/customer-configuration.md)仪表板。

   ![客户配置，Analytics 选项卡](/help/dashboards/assets/aa-integration-03-analytics-tab.png)

1. 在&#x200B;**凭据**&#x200B;下，输入&#x200B;**客户端 ID**&#x200B;和&#x200B;**客户端密钥**，然后选择&#x200B;**验证并继续**。 请注意以下事项：

   * 仅在两个字段均已填写时，**验证并继续**&#x200B;才可用。
   * 验证成功后，将加载报告包。
   * 请使用具有所需报告包访问权限的[技术帐户](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/)的&#x200B;**客户端 ID** 和&#x200B;**客户端密钥**。

   ![Analytics 凭据以及验证并继续](/help/dashboards/assets/aa-integration-04-credentials.png)

1. 在&#x200B;**配置**&#x200B;下，选择一个&#x200B;**报告包**。

   选择报告包后，LLM Optimizer 将加载该报告包可用的&#x200B;**页面 URL 维度**&#x200B;选项。

   ![已选择报告包并加载维度](/help/dashboards/assets/aa-integration-05-report-suite.png)

1. 选择一个&#x200B;**页面 URL 维度**（报告包专属维度列表），然后选择&#x200B;**保存并启用**。

   * 在选择报告包并加载维度之前，**页面 URL 维度**&#x200B;将保持禁用状态。
   * 仅在选择页面 URL 维度后，**保存并启用**&#x200B;才可用。

   ![页面 URL 维度与保存并启用](/help/dashboards/assets/aa-integration-06-page-url-dimension.png)

1. 保存后，配置应显示为&#x200B;**已连接**&#x200B;状态。您可以通过&#x200B;**前往引荐流量仪表板**&#x200B;返回引荐流量仪表板。在&#x200B;**业务影响**&#x200B;选项卡上的&#x200B;**引荐流量**&#x200B;中，状态应显示为&#x200B;**已连接到 Adobe Analytics**。

   ![已在配置和业务影响中连接到 Adobe Analytics](/help/dashboards/assets/aa-integration-07-connected.png)

### 连接完成后 {#after-connect}

* LLM Optimizer 会回填&#x200B;**最近四个完整自然周**&#x200B;以及&#x200B;**当前自然周截至目前的数据**。
* 回填完成后，数据将按&#x200B;**每日**&#x200B;更新，每次拉取&#x200B;**前一整天**&#x200B;的数据。

>[!NOTE]
>
>回填过程可能需要数小时完成。

## 工作原理 {#how-it-works}

### 配置

在设置过程中，您需要指定 LLM Optimizer 用于 Adobe Analytics 数据摄入的报告包和页面维度。页面维度可以是标准的 `variables/page` 映射，也可以是自定义 `eVar`，具体取决于您的报告包。

### LLM 流量的识别方式

LLM 来源的流量通过 Adobe Analytics 的[引荐类型—对话式 AI 工具](https://experienceleague.adobe.com/zh-hans/docs/analytics/components/dimensions/referrer-type#conversational-ai-tools)进行识别。

### 摄取的数据 {#data-ingested}

LLM Optimizer 会摄取以下数据：

**维度**

* 引荐域
* 引荐类型
* 国家/地区
* 设备类型
* 所选页面维度

**量度**

* 页面浏览量
* 访问次数
* 访客
* 进入次数
* 退出次数
* 跳出次数
* 单页访问次数
* 停留时间
* 购物车数
* 加入购物车次数
* 移除购物车次数
* 购物车查看次数
* 结账次数
* 订单数
* 收入
* 销售数量

### LLM Optimizer 如何使用这些数据

该数据集为 LLM Optimizer 提供以下洞察支持：

* 页面级 LLM 流量表现。
* 不同 LLM 来源的引荐表现。
* 区域及设备层级的趋势分析。
* 后续商业转化结果。

## 常见问题解答 {#faq}

问：试用期间是否可以使用 Adobe Analytics 集成？

不。 该集成功能仅对付费版 LLM Optimizer 客户开放。

问：会收集或存储哪些数据？

请参阅上文的[摄取的数据](#data-ingested)章节。LLM Optimizer 使用的是经您组织授权的 Adobe Analytics API 提供的聚合量度，而非原始逐条点击数据。

问：数据是如何摄取的？

您的组织授权 LLM Optimizer 查询 Adobe Analytics API。与 LLM 来源相关的引荐流量数据通过这些 API 获取。

问：数据多久更新一次？

数据按&#x200B;**每日**&#x200B;更新（回填完成后，每次更新前一整天的数据）。

问：LLM Optimizer 是否会存储原始逐条点击数据？

不。 仅使用&#x200B;**聚合**&#x200B;量度来分析流量模式和趋势。

问：是否会存储完整 URL、查询字符串或页面内容？

可摄取所选页面维度对应的完整 URL；但查询字符串和页面内容不会通过此集成摄取。

问：是否会存储用户标识符（ECID、IP 地址、cookie ID）？

不。

问：数据会保留多长时间？

请注意，数据保留策略可能会发生变化。目前数据将会无限期存储。

问：数据在传输和静态存储过程中是否加密？

目前数据在传输过程中会加密，但在静态存储时不会加密。该情况可能会在未来更新中发生变化。

问：是否会回填历史数据？

是。 设置完成后，将回填最近四个完整自然周及当前自然周的数据。另请参阅[完成连接后](#after-connect)。
