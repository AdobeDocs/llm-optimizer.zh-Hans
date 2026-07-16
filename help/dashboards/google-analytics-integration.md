---
title: Google Analytics集成
description: 了解如何将Google Analytics 4与LLM Optimizer连接起来，以在引荐流量仪表板中衡量人工智能驱动的发现、网站参与和业务成果。
feature: Referral Traffic
autotag-review: '2026-07-15T17:51:53.586Z'
TQID: 'https://experienceleague.adobe.com/SvWn3W6hpVsWNzfWdJFvPs94lwlKX4ufjjcXKM-6xIc'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
subfeature_v2:
  - id: f5a6cbd1-8a9a-4c79-a6db-ba46537f516e
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
  - id: b69b2659-1057-424e-8fc5-ed9e016dc554
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 1169
ht-degree: 17%

---


# Google Analytics集成

Google Analytics 4 (GA4)集成将LLM Optimizer与您组织的GA4数据连接在一起，以便您能够衡量ChatGPT、Gemini、Copilot、Claude和Perplexity等平台上的AI驱动型发现如何转化为实际的网站参与度和业务成果。 连接GA4属性后，LLM Optimizer会将GA4所属性的引荐流量、参与和转化指标提取到这些源，并将其显示在&#x200B;**业务影响**&#x200B;选项卡下的&#x200B;**引荐流量**&#x200B;仪表板中。

>[!IMPORTANT]
>
>GA4集成包含在付费LLM Optimizer选件中。 在升级到付费选件之前，使用免费试用版的客户将无法连接GA4。

## 开始之前 {#before-you-begin}

要完成连接，您需要：

* 对要连接的GA4属性具有至少&#x200B;**查看器**&#x200B;访问权限的Google帐户。 在Google Analytics中的&#x200B;**管理员>属性访问管理**&#x200B;下管理属性级访问。
* 在LLM Optimizer中管理配置的权限（否则，“连接”按钮可见但处于禁用状态）。
* 允许从LLM Optimizer源（ Google登录步骤）弹出窗口的浏览器将在新选项卡中打开。

您&#x200B;**不**&#x200B;需要创建Google云项目、生成服务帐户、上传JSON密钥或输入属性ID。 LLM Optimizer通过Google的标准OAuth同意屏幕代理连接。

## 将GA4连接到引荐流量仪表板 {#connect}

连接流程从[引荐流量](/help/dashboards/referral-traffic.md)仪表板开始，具体如下：

1. 在LLM Optimizer中打开&#x200B;**引荐流量**。

1. 打开&#x200B;**业务影响**&#x200B;选项卡。

   ![引荐流量仪表板，“业务影响”选项卡](/help/dashboards/assets/ga4-integration-01-business-impact-tab.png)

1. 选择&#x200B;**连接到 Analytics**。 LLM Optimizer会将您路由到&#x200B;**客户配置> Analytics**。 在Analytics提供程序选取器中，选择&#x200B;**连接Google Analytics 4**。

   ![已选择GA4的Customer Configuration， Analytics选项卡](/help/dashboards/assets/ga4-integration-02-analytics-ga4-picker.png)

1. 选择&#x200B;**连接帐户**。 此时将打开一个新的浏览器选项卡，显示Google的登录屏幕。

   用于GA4连接的![Google登录](/help/dashboards/assets/ga4-integration-03-google-sign-in.png)

1. 使用有权访问要连接的GA4资产的Google帐户登录。 在Google提示您时批准`See and download your Google Analytics data`权限（`analytics.readonly`范围）。

1. Google会将您返回到LLM Optimizer，其中列出了您的帐户可访问的GA4资产。 选择要连接和提交的属性。

1. 返回到LLM Optimizer选项卡。 Analytics选项卡自动检测已完成的连接，并且GA4卡显示&#x200B;**已连接**&#x200B;状态。

### 连接完成后 {#after-connect}

将GA4连接到LLM Optimizer后，会发生以下情况：

* LLM Optimizer 会回填&#x200B;**最近四个完整自然周**&#x200B;以及&#x200B;**当前自然周截至目前的数据**。
* 回填完成后，数据将按&#x200B;**每日**&#x200B;更新，每次拉取&#x200B;**前一整天**&#x200B;的数据。

>[!NOTE]
>
>回填过程可能需要数小时完成。 业务影响仪表板将在数据登陆后逐步填充；回填运行时，您无需执行任何操作。

如果重新连接（例如，切换Google帐户或GA4属性），则只再次回填当前日历周，保留已加载的前几周。

## 工作原理 {#how-it-works}

### 连接模型

该集成使用Google的标准OAuth 2.0用户委派流程。 LLM Optimizer存储一个范围设定到您选定的GA4资产的刷新令牌，该令牌允许LLM Optimizer代表您调用GA4数据API（具有只读访问权限），直到您从Google帐户撤销该令牌为止。

### LLM 流量的识别方式

LLM Optimizer只要求GA4处理GA4本身归因于LLM平台的会话。 今天，这些会话的`sessionSourceMedium`与`chatgpt`、`gemini.google.com`、`copilot.microsoft.com`、`claude`或`perplexity`之一匹配。 支持的LLM源列表由Adobe维护，并且可能会随着时间的推移而扩展。

### 摄取的数据 {#data-ingested}

每个每日拉取都会检索一个汇总报表，其中包含以下内容：

**维度**

| GA4尺寸 | 它表示的内容 |
| --- | --- |
| `date` | 会话发生的日期。 |
| `landingPage` | 访客在您的网站上看到的第一个页面。 |
| `countryId` | 访客的国家/地区，由GA4的IP地理位置确定。 |
| `deviceCategory` | 移动设备/台式机/平板电脑。 |
| `sessionSourceMedium` | 由GA4确定的LLM源/介质。 |

**量度**

| GA4量度 | 它表示的内容 |
| --- | --- |
| `sessions` | 存储桶中的会话数。 |
| `screenPageViews` | 存储桶中的页面查看次数。 |
| `bounceRate` | 跳出的会话的分数。 |
| `totalPurchasers` | 不同的购买者（如果在GA4中配置了电子商务）。 |
| `transactions` | 交易计数（如果配置了电子商务）。 |
| `purchaseRevenue` | 采购收入（美元）。 |
| `totalRevenue` | 总收入（美元）。 |

### LLM Optimizer 如何使用这些数据

LLM Optimizer使用此数据来填充“业务影响”功能板的页面级性能、源细分、国家/地区和设备拆分以及时间趋势。 没有数据用于训练模型或在租户外部共享。

### 未摄取的内容

无用户标识符（Google客户端ID、IP地址、设备ID）、无会话级别行、无事件级别行、除以上所列维度或量度之外没有自定义维度或量度，并且没有GA4受众或区段定义。

## 常见问题解答 {#faq}

问：GA4集成是否在试用期间可用？

不。 该集成功能仅对付费版 LLM Optimizer 客户开放。

问：我是否需要创建Google Cloud项目或服务帐户？

不。 连接是标准的Google登录。 LLM Optimizer管理Adobe端的Google OAuth客户端；您只需要一个对GA4资产具有查看器访问权限的Google帐户。

问：会收集或存储哪些数据？

LLM Optimizer可与来自贵组织授权的GA4数据API的汇总量度（而非原始事件级数据）配合使用。

问：数据是如何摄取的？

贵组织授权LLM Optimizer查询GA4数据API以查找所选资产。 与LLM源一致的引荐流量通过该API使用。

问：数据多久更新一次？

数据按&#x200B;**每日**&#x200B;更新（回填完成后，每次更新前一整天的数据）。

问：原始事件级数据是否存储在LLM Optimizer中？

不。 仅使用&#x200B;**聚合**&#x200B;量度来分析流量模式和趋势。

问：是否会存储完整 URL、查询字符串或页面内容？

登陆页面路径作为标准报表的一部分引入；此集成不会引入查询字符串和页面内容。

问：是否存储了用户标识符（Google客户端ID、IP地址、设备ID）？

不。

问：数据会保留多长时间？

目前数据将会无限期存储。

问：数据在传输和静态存储过程中是否加密？

目前，数据是在传输过程中而非静止状态下加密的。 该情况可能会在未来更新中发生变化。

问：是否会回填历史数据？

是。 设置完成后，将回填最近四个完整自然周及当前自然周的数据。 另请参阅[完成连接后](#after-connect)。

问：我可以断开或撤销访问权限吗？

可以，随时可以。 您可以通过LLM Optimizer中的GA4卡重新连接到其他帐户或属性，或完全撤销对位于[https://myaccount.google.com/permissions](https://myaccount.google.com/permissions)的LLM Optimizer帐户Google的访问权限。 撤销访问会停止提取新数据；之前摄取的聚合数据仍保留在LLM Optimizer中。

问：我的GA4资产已连接，但“业务影响”是空的 — 为什么？

LLM Optimizer仅提取GA4本身归因于受支持LLM源/媒体的会话（当前为：ChatGPT、Gemini、Copilot、Claude、Perplexity）。 如果GA4资产尚未在显示的时间范围内记录来自上述任何来源的引用会话，则仪表板将为空，即使连接状况良好也是如此。
