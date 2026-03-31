---
title: Adobe Analytics 集成
description: 了解如何将Adobe Analytics与LLM Optimizer联系起来，在引荐流量仪表板中衡量人工智能驱动的发现、网站参与和业务成果。
feature: Referral Traffic
source-git-commit: e7c9bc1d40267dc92608baa005f85f4be21cfda1
workflow-type: tm+mt
source-wordcount: '879'
ht-degree: 4%

---


# Adobe Analytics 集成

Adobe Analytics集成将LLM Optimizer与您组织的Adobe Analytics数据连接起来，以便您可以衡量AI驱动的发现如何转化为实际的网站参与度和业务成果。 集成过程完成后，数据将在&#x200B;**业务影响**&#x200B;选项卡下的&#x200B;**引荐流量**&#x200B;仪表板中可用。

通过将Analytics数据与AI可见性分析关联，LLM Optimizer可帮助您跟踪：

* 用户在人工智能引用的页面上参与。
* 与AI发现历程关联的转换信号。
* 地理位置优化对性能的影响。

这种集成弥合了AI可见性测量与业务性能分析之间的差距。 现在，团队不仅可以查看品牌在AI响应中的显示位置，还可以查看用户到达后会出现什么情况。

## 可用性 {#availability}

>[!IMPORTANT]
>
>Adobe Analytics集成包含在付费LLM Optimizer选件中。 在升级到付费选件之前，使用免费试用版的客户将无法连接Adobe Analytics。

## 将Adobe Analytics连接到引荐流量功能板 {#connect}

连接流从[引荐流量](/help/dashboards/referral-traffic.md)仪表板开始，如下所示：

1. 打开[引荐流量](/help/dashboards/referral-traffic.md)仪表板。 默认视图为&#x200B;**流量分析**。

   ![引荐流量仪表板，“流量分析”选项卡](/help/dashboards/assets/aa-integration-01-referral-traffic-traffic-insights.png)

1. 选择&#x200B;**业务影响**&#x200B;选项卡。 如果未连接Analytics提供程序，则会显示一条横幅： **连接以查看业务影响**，同时显示&#x200B;**连接到Analytics**。

   连接到Analytics的![业务影响选项卡](/help/dashboards/assets/aa-integration-02-business-impact-connect.png)

1. 选择&#x200B;**连接到Analytics**。 这会在&#x200B;**Analytics**&#x200B;选项卡上打开[客户配置](/help/dashboards/customer-configuration.md)仪表板。

   ![客户配置，Analytics选项卡](/help/dashboards/assets/aa-integration-03-analytics-tab.png)

1. 在&#x200B;**凭据**&#x200B;下，输入&#x200B;**客户端ID**&#x200B;和&#x200B;**客户端密钥**，然后选择&#x200B;**验证并继续**。 请注意以下事项：

   * **验证并继续**&#x200B;仅在两个字段都填写后才可用。
   * 在成功验证后，将加载报表包。
   * 使用有权访问所需报表包的[技术帐户](https://developer.adobe.com/developer-console/docs/guides/authentication/ServerToServerAuthentication/)的&#x200B;**客户端ID**&#x200B;和&#x200B;**客户端密钥**。

   ![Analytics凭据并验证和继续](/help/dashboards/assets/aa-integration-04-credentials.png)

1. 在&#x200B;**配置**&#x200B;下，选择&#x200B;**报表包**。

   选择报表包后，LLM Optimizer会加载可用于该报表包的&#x200B;**页面URL Dimension**&#x200B;选项。

   ![选定的报表包和加载的维度](/help/dashboards/assets/aa-integration-05-report-suite.png)

1. 选择&#x200B;**页面URL Dimension** （特定于包的维度列表），然后选择&#x200B;**保存并启用**。

   * **在选择报表包并加载维度之前，Dimension**&#x200B;页面URL将保持禁用状态。
   * **保存并启用**&#x200B;仅在您选择页面URL维度后可用。

   ![页面URL维度并保存和启用](/help/dashboards/assets/aa-integration-06-page-url-dimension.png)

1. 保存后，配置应显示&#x200B;**已连接**&#x200B;状态。 您可以使用&#x200B;**返回引荐流量信息板**&#x200B;来返回引荐流量信息板。 在&#x200B;**业务影响**&#x200B;选项卡上的&#x200B;**引荐流量**&#x200B;中，状态应显示为&#x200B;**已连接到Adobe Analytics**。

   ![已在配置和业务影响中连接到Adobe Analytics](/help/dashboards/assets/aa-integration-07-connected.png)

### 连接后 {#after-connect}

* LLM Optimizer回填&#x200B;**过去四个完整的日历周**&#x200B;和&#x200B;**当前日历周至今**。
* 回填后，使用&#x200B;**前一天已满**&#x200B;的拉取&#x200B;**每日**&#x200B;刷新数据。

>[!NOTE]
>
>回填可能需要几个小时才能完成。

## 工作原理 {#how-it-works}

### 配置

在设置过程中，您可以定义LLM Optimizer用于Adobe Analytics摄取的报表包和页面维度。 页面维度可以是标准`variables/page`映射或自定义`eVar`，具体取决于您的报表包。

### 如何识别LLM流量

使用Adobe Analytics [反向链接类型 — 对话式AI工具](https://experienceleague.adobe.com/zh-hans/docs/analytics/components/dimensions/referrer-type#conversational-ai-tools)识别LLM产生的流量。

### 已摄取的数据 {#data-ingested}

以下数据由LLM Optimizer摄取：

**维度**

* 反向链接域
* 反向链接类型
* 国家/地区
* 设备类型
* 选定的页面维度

**个量度**

* 页面浏览量
* 访问次数
* 访客
* 进入次数
* 退出次数
* 跳出次数
* 单页访问
* 逗留时间
* 购物车
* 购物车加货
* 购物车减货
* 购物车查看
* 结账次数
* 订单数
* 收入
* 件数

### LLM Optimizer如何使用此数据

此数据集支持LLM Optimizer分析以下内容：

* 页面级别的LLM流量性能。
* 跨LLM源的反向链接性能。
* 区域和设备级趋势。
* 下游商业成果。

## 常见问题解答 {#faq}

问：Adobe Analytics集成是否在试用期间可用？

不。 该集成仅适用于付费LLM Optimizer客户。

问：收集或存储哪些数据？

请参阅上面的[摄取的数据](#data-ingested)章。 LLM Optimizer可与来自贵组织授权的Adobe Analytics API的汇总量度（而非原始点击级别数据）配合使用。

问：如何引入数据？

贵组织授权LLM Optimizer查询Adobe Analytics API。 与LLM源一致的引荐流量通过这些API使用。

问：多久刷新一次数据？

数据在&#x200B;**每日**&#x200B;刷新（回填完成后的前一天已满）。

问：原始点击级别的数据是否存储在LLM Optimizer中？

不。 只有&#x200B;**个汇总的**&#x200B;量度用于了解流量模式和趋势。

问：是否存储了完整的URL、查询字符串或页面内容？

可以摄取用于所选页面维度的完整URL；不会为此集成摄取查询字符串和页面内容。

问：是否存储了用户标识符（ECID、IP地址、Cookie ID）？

不。

问：数据会保留多长时间？

请记住，保留策略可能会发生演变。 目前，数据会无限期地存储。

问：数据在传输和静止时是否进行了加密？

目前，数据在传输中不是静止时进行加密。 这在未来更新中可能会发生变化。

问：历史数据是否已回填？

是。 成功设置后，将回填最近四个完整日历周和当前日历周。 另请参阅[连接后](#after-connect)。
