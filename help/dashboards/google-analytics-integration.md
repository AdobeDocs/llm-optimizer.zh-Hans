---
title: Google Analytics 集成
description: 了解如何将 Google Analytics 4 与 LLM Optimizer 连接，以便在引荐流量仪表板中衡量 AI 驱动的发现、网站互动以及业务成效。
feature: Referral Traffic
autotag-review: '2026-07-15T17:51:53.586Z'
TQID: 'https://experienceleague.adobe.com/SvWn3W6hpVsWNzfWdJFvPs94lwlKX4ufjjcXKM-6xIc'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: d1956731-2adb-4bb7-8301-2b239254ac72
subfeature_v2: id: f5a6cbd1-8a9a-4c79-a6db-ba46537f516e
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bdid: b69b2659-1057-424e-8fc5-ed9e016dc554
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: ht
source-wordcount: 1169
ht-degree: 100%

---


# Google Analytics 集成

Google Analytics 4（GA4）集成可将 LLM Optimizer 与您组织的 GA4 数据连接起来，从而帮助您衡量来自 ChatGPT、Gemini、Copilot、Claude 和 Perplexity 等平台的 AI 驱动发现如何转化为真实的网站互动和业务成效。连接 GA4 属性后，LLM Optimizer 会提取 GA4 归因于这些来源的&#x200B;**引荐流量**、互动和转化量度，并将其显示在引荐流量仪表板的&#x200B;**业务影响**&#x200B;选项卡中。

>[!IMPORTANT]
>
>GA4 集成功能包含在 LLM Optimizer 的付费版本中。使用免费试用版的客户需升级至付费版本后，才能连接 GA4。

## 开始之前 {#before-you-begin}

完成连接前，您需要满足以下条件：

* 一个对要连接的 GA4 属性至少拥有&#x200B;**查看者**&#x200B;权限的 Google 帐户。属性级访问权限可在 Google Analytics 的&#x200B;**管理 > 属性访问权限管理**&#x200B;中进行管理。
* 拥有在 LLM Optimizer 中管理配置的权限（否则连接按钮虽然可见，但无法使用）。
* 使用允许来自 LLM Optimizer 域弹出窗口的浏览器，因为 Google 登录步骤会在新标签页中打开。

您&#x200B;**无需**&#x200B;创建 Google Cloud 项目、生成服务帐号、上传 JSON 密钥，也无需输入属性 ID。LLM Optimizer 会通过 Google 的标准 OAuth 授权页面完成连接。

## 将 GA4 连接到引荐流量仪表板 {#connect}

连接流程从[引荐流量](/help/dashboards/referral-traffic.md)仪表板开始，具体如下：

1. 在 LLM Optimizer 中打开&#x200B;**引荐流量**。

1. 打开&#x200B;**业务影响**&#x200B;选项卡。

   ![引荐流量仪表板 — 业务影响选项卡](/help/dashboards/assets/ga4-integration-01-business-impact-tab.png)

1. 选择&#x200B;**连接到 Analytics**。 LLM Optimizer 会将您导航至&#x200B;**客户配置 > 分析**。在分析提供商选择器中，选择&#x200B;**连接 Google Analytics 4**。

   ![客户配置 — 已选择 GA4 的分析选项卡](/help/dashboards/assets/ga4-integration-02-analytics-ga4-picker.png)

1. 选择&#x200B;**连接帐户**。浏览器会打开一个新标签页，显示 Google 登录页面。

   ![用于连接 GA4 的 Google 登录页面](/help/dashboards/assets/ga4-integration-03-google-sign-in.png)

1. 使用拥有目标 GA4 属性访问权限的 Google 帐户登录。当 Google 提示时，请授权 `See and download your Google Analytics data` 权限（`analytics.readonly` 范围）。

1. Google 会将您返回到 LLM Optimizer，系统会列出您的帐户可访问的 GA4 属性。选择要连接的 GA4 属性，然后提交。

1. 返回 LLM Optimizer 标签页。分析选项卡会自动检测已完成的连接，并在 GA4 卡片上显示&#x200B;**已连接**&#x200B;状态。

### 连接完成后 {#after-connect}

GA4 连接到 LLM Optimizer 后，将发生以下情况：

* LLM Optimizer 会回填&#x200B;**最近四个完整自然周**&#x200B;以及&#x200B;**当前自然周截至目前的数据**。
* 回填完成后，数据将按&#x200B;**每日**&#x200B;更新，每次拉取&#x200B;**前一整天**&#x200B;的数据。

>[!NOTE]
>
>回填过程可能需要数小时完成。 随着数据逐步导入，业务影响仪表板会逐渐显示相关数据；在历史数据回填期间，您无需执行任何操作。

如果您重新建立连接（例如更换 Google 帐户或切换 GA4 属性），系统只会重新回填当前自然周的数据，而此前已加载的历史周数据将会保留。

## 工作原理 {#how-it-works}

### 连接模型

该集成采用 Google 标准的 OAuth 2.0 用户授权流程。LLM Optimizer 会保存一个仅限于您所选 GA4 属性的刷新令牌。该令牌允许 LLM Optimizer 在您撤销授权之前，以只读权限代表您调用 GA4 Data API。

### LLM 流量的识别方式

LLM Optimizer 仅会向 GA4 请求由 GA4 自身归因于 LLM 平台的会话数据。目前，这些会话是指 `sessionSourceMedium` 匹配以下任一来源的会话：`chatgpt`、`gemini.google.com`、`copilot.microsoft.com`、`claude` 或 `perplexity`。 支持的 LLM 来源列表由 Adobe 维护，并可能会随着时间推移不断扩展。

### 摄取的数据 {#data-ingested}

每日数据拉取会获取一份聚合报告，其中包含以下内容：

**维度**

| GA4 维度 | 表示的内容 |
| --- | --- |
| `date` | 会话发生的日期。 |
| `landingPage` | 访客进入您网站后看到的第一个页面。 |
| `countryId` | 根据 GA4 的 IP 地理位置确定的访客所在国家/地区。 |
| `deviceCategory` | 移动设备 / 桌面设备 / 平板设备。 |
| `sessionSourceMedium` | GA4 归因的 LLM 来源/媒介。 |

**量度**

| GA4 量度 | 表示的内容 |
| --- | --- |
| `sessions` | 该统计维度下的会话数。 |
| `screenPageViews` | 该统计维度下的页面浏览量。 |
| `bounceRate` | 跳出会话占比（跳出率）。 |
| `totalPurchasers` | 独立购买用户数（如果 GA4 已配置电子商务功能）。 |
| `transactions` | 交易次数（如果已配置电子商务功能）。 |
| `purchaseRevenue` | 购买收入（USD）。 |
| `totalRevenue` | 总收入（USD）。 |

### LLM Optimizer 如何使用这些数据

LLM Optimizer 使用这些数据填充业务影响仪表板中的页面级表现、来源细分、国家/地区和设备分布以及时间趋势等数据。这些数据不会用于训练模型，也不会共享到您的租户之外。

### 不会采集的数据

不会采集任何用户标识符（如 Google Client ID、IP 地址、设备 ID），也不会采集会话级数据、事件级数据、上述内容以外的任何自定义维度或量度，以及 GA4 受众或细分定义。

## 常见问题解答 {#faq}

问：试用期间可以使用 GA4 集成功能吗？

不。 该集成功能仅对付费版 LLM Optimizer 客户开放。

问：我是否需要创建 Google Cloud 项目或服务帐号？

不。 连接过程采用标准的 Google 登录方式。LLM Optimizer 会由 Adobe 负责管理 Google OAuth 客户端。您只需使用一个对目标 GA4 属性拥有（查看者）权限的 Google 帐户即可。

问：会收集或存储哪些数据？

LLM Optimizer 使用的是经您组织授权的 GA4 Data API 提供的聚合指标，而不是原始事件级数据。

问：数据是如何摄取的？

您的组织会授权 LLM Optimizer 查询所选 GA4 属性的 GA4 Data API。与 LLM 来源对应的引荐流量数据将通过该 API 获取。

问：数据多久更新一次？

数据按&#x200B;**每日**&#x200B;更新（回填完成后，每次更新前一整天的数据）。

问：LLM Optimizer 是否会存储原始事件级数据？

不。 仅使用&#x200B;**聚合**&#x200B;量度来分析流量模式和趋势。

问：是否会存储完整 URL、查询字符串或页面内容？

登录页面路径会作为标准报告的一部分被采集；但查询字符串和页面内容不会通过此集成进行采集。

问：是否会存储用户标识符（Google Client ID、IP 地址、设备 ID）？

不。

问：数据会保留多长时间？

目前数据将会无限期存储。

问：数据在传输和静态存储过程中是否加密？

目前，数据在传输过程中会加密，但静态存储时不会加密。该情况可能会在未来更新中发生变化。

问：是否会回填历史数据？

是。 设置完成后，将回填最近四个完整自然周及当前自然周的数据。 另请参阅[完成连接后](#after-connect)。

问：我可以断开连接或撤销授权吗？

可以，您可随时执行此操作。您可以在 LLM Optimizer 的 GA4 卡片中重新连接其他 Google 帐户或 GA4 属性，也可以在您的 Google 帐户中通过 [https://myaccount.google.com/permissions](https://myaccount.google.com/permissions) 完全撤销 LLM Optimizer 的访问权限。撤销授权后，系统将停止拉取新的数据，但此前已导入的聚合数据仍会保留在 LLM Optimizer 中。

问：我的 GA4 属性已连接，但业务影响为空，为什么？

LLM Optimizer 仅会拉取 GA4 自身归因于受支持 LLM 来源/媒介的会话数据（目前包括：ChatGPT、Gemini、Copilot、Claude 和 Perplexity）。 如果您的 GA4 属性在当前显示的时间范围内尚未记录来自上述任何来源的引荐会话，即使连接状态正常，仪表板也不会显示任何数据。
