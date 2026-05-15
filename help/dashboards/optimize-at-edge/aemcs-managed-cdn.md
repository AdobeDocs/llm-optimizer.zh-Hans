---
title: Optimize at Edge - AEM 云服务托管的内容传递网络 (Fastly)
description: 了解如何在 LLM Optimizer 中为 Optimize at Edge 配置 AEM 云服务托管的内容传递网络 (Fastly)。
feature: Opportunities
autotag-review: '2026-05-15T17:31:38.650Z'
TQID: 'https://experienceleague.adobe.com/qrCODY3Qg6dDd9QRcaYGdN4YVCgKAsl56UTAVZpsIwE'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: d1956731-2adb-4bb7-8301-2b239254ac72id: e0828736-236a-487b-a478-5a635455eadc
subfeature_v2: id: d23587d6-14d6-4e3f-9ee1-cc18623832e1id: e06fae5f-830b-4222-a469-b5e148d36465
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 7a92587197cf6a9eec6b01bd4eaeeaf1194d3088
workflow-type: tm+mt
source-wordcount: 836
ht-degree: 100%

---


# AEM 云服务托管的内容传递网络 (Fastly)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。 人类访客和 SEO 机器人仍将照常从您的源站获得响应。 完成设置后，可在响应中检查标头 `x-edgeoptimize-request-id`，测试配置是否正确。

## 先决条件

要访问此功能：

- 付费客户必须有权访问 **Adobe LLM Optimizer 用户** IMS 产品配置文件。 请联系您组织的管理员，请求获取访问权限。
  ![将用户添加到一个产品配置文件](/help/assets/optimize-at-edge/cs-fastly-user-product-profiles.png)
- 试用版客户必须属于 **LLMO 管理员** IMS 组。 如果这个组不存在，您组织的管理员可以创建该组，然后将您添加进去。
  ![创建 LLMO 管理员 IMS 组](/help/assets/optimize-at-edge/cs-fastly-create-ims-group.png)

>[!NOTE]
> Safari 或隐身/私人浏览模式不支持此功能。

## 启用路由的步骤

要开始将代理式流量路由到 Edge Optimize，请执行以下操作：

1. 在 LLM Optimizer 中，打开&#x200B;**客户配置**，然后选择&#x200B;**内容传递网络配置**&#x200B;选项卡。

   ![导航至客户配置](/help/assets/optimize-at-edge/cs-fastly-prereq-customer-config-nav.png)

2. 找到&#x200B;**将优化部署到 AI 代理**&#x200B;部分。 点击&#x200B;**启用**&#x200B;按钮。

   ![将优化部署到 AI 代理——待处理](/help/assets/optimize-at-edge/cs-fastly-enable-button.png)

3. 在确认对话框中选择&#x200B;**启用**，确认您要启用路由。 如果出现错误，请参阅[疑难解答](#troubleshooting)部分，解决问题。

   ![启用优化引擎确认对话框](/help/assets/optimize-at-edge/cs-fastly-enable-dialog.png)

4. 确认后，需要几分钟才能完成路由。

   ![路由正在进行中](/help/assets/optimize-at-edge/cs-fastly-enable-button-clicked-routing-in-progress.png)

   5 分钟后重新加载页面，验证路由是否完成。 路由配置并激活后，状态将更新为&#x200B;**已完成**，并显示绿色复选标记，确认路由已启用。 您无需执行任何其他操作。

   ![将优化部署到 AI 代理——已完成](/help/assets/optimize-at-edge/cs-fastly-disable-button.png)

   要随时禁用路由，请返回到&#x200B;**内容传递网络配置**&#x200B;选项卡，在&#x200B;**将优化部署到 AI 代理**&#x200B;部分中点击&#x200B;**禁用**。

此外，如果您在上述步骤中需要任何帮助，请联系您的 Adobe 帐户团队或 `llmo-at-edge@adobe.com`。

## 故障排除

如果启用或禁用路由时出现错误，可能类似这样：

![确认对话框错误](/help/assets/optimize-at-edge/cs-fastly-confirmation-dialog-error.png)

使用下面的列表确定错误，按照说明进行操作。

1. **用户没有 LLMO 产品的访问权限**

   **原因：**&#x200B;在您的 Adobe IMS 配置文件中，用户帐户没有 LLM Optimizer 产品上下文。 付费客户配置内容传递网络路由时需要这个。

   **推荐：**&#x200B;验证您的组织管理员是否已在 Adobe Admin Console 中为您分配了 **Adobe LLM Optimizer 用户**&#x200B;产品配置文件。

2. **只有 LLMO 管理员组的成员可以配置内容传递网络路由**

   **原因：**&#x200B;您的帐户不是 **LLMO 管理员** IMS 组的成员。 试用版客户配置内容传递网络路由时需要这个。

   **推荐：**&#x200B;验证您的组织管理员已在 Adobe Admin Console 中将您添加到 **LLMO 管理员** IMS 组。

3. **所请求的内容传递网络类型 aem-cs-fastly 与此域检测到的内容传递网络不匹配**

   **原因：**&#x200B;这表示为您的网站检测到的内容传递网络类型不是 *AEM 云服务托管的内容传递网络 (Fastly)*。

   **推荐：**&#x200B;验证您的网站是否通过 AEM 云服务托管的内容传递网络 (Fastly) 提供服务。

4. **探查网站时出错**

   **原因：**&#x200B;路由设置过程中 LLM Optimizer 无法到达您的网站。 如果网站已关闭、不可到达或请求超时，就可能会发生这种情况。

   **推荐：**&#x200B;验证您的网站可公开访问并能返回有效响应，然后重试。

5. **路由探查时网站不返回有效响应**

   **原因：**&#x200B;在设置过程中探查时，网站返回了意外的 HTTP 状态（不是 2xx 或 301）。

   **推荐：**&#x200B;验证您的网站对 LLM Optimizer 中注册的基本 URL 是否返回成功响应 (2xx)，然后重试。

6. **与上游 IMS 服务的身份验证失败**

   **原因：**&#x200B;会话可能已过期，或在路由请求过程中与 Adobe IMS 身份验证时出现问题。

   **推荐：**&#x200B;注销 LLM Optimizer 后重新登录，然后重新尝试启用路由。

如果问题仍然存在，请联系您的 Adobe 帐户团队或 `llmo-at-edge@adobe.com`。

## （可选）验证设置

路由配置完成后，您可以选择验证 AI 机器人流量是否路由到 Edge Optimize，以及人类流量是否不受影响。

1. **测试机器人流量（应优化）**

   用代理式用户代理模拟 AI 机器人请求：

   ```
   curl -svo /dev/null https://www.example.com/page.html \
     --header "user-agent: chatgpt-user"
   ```

   成功的响应包括 `x-edgeoptimize-request-id` 头部，用于确认请求是通过 Edge Optimize 路由的：

   ```
   < HTTP/2 200
   < x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
   ```

2. **测试人类流量（不应受影响）**

   模拟一个常规的人类浏览器请求：

   ```
   curl -svo /dev/null https://www.example.com/page.html \
     --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
   ```

   响应不应包含 `x-edgeoptimize-request-id` 标头。 页面内容和响应时间应保持与启用 Optimize at Edge 之前时完全相同。

3. **如何区分这两种场景**

   | 页眉 | 机器人流量（已优化） | 人类流量（不受影响） |
   |---|---|---|
   | `x-edgeoptimize-request-id` | 存在——包含唯一的请求 ID | 不存在 |
   | `x-edgeoptimize-fo` | 仅在发生故障转移的情况下存在（值：`1`） | 不存在 |

4. **检查 LLM Optimizer 中的路由状态**

   您还可以在 LLM Optimizer UI 中确认路由。 打开&#x200B;**客户配置**，然后选择&#x200B;**内容传递网络配置**&#x200B;选项卡。 当路由激活时，**将优化部署到 AI 代理**&#x200B;部分显示&#x200B;**已完成**。

   ![将优化部署到 AI 代理——已完成](/help/assets/optimize-at-edge/cs-fastly-disable-button.png)

{{return-to-overview}}
