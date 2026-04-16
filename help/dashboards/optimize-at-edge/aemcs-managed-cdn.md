---
title: Optimize at Edge - AEM 云服务托管的内容传递网络 (Fastly)
description: 了解如何在 LLM Optimizer 中为 Optimize at Edge 配置 AEM 云服务托管的内容传递网络 (Fastly)。
feature: Opportunities
source-git-commit: 184d6008c2579014c6ff453e8bfff4bb898f4b82
workflow-type: tm+mt
source-wordcount: '836'
ht-degree: 32%

---


# AEM 云服务托管的内容传递网络 (Fastly)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。 人类访客和 SEO 机器人仍将照常从您的源站获得响应。 要测试配置，请在设置完成后，检查响应中的标头`x-edgeoptimize-request-id`。

## 先决条件

要访问此功能：

- 付费客户必须有权访问&#x200B;**Adobe LLM Optimizer用户** IMS产品配置文件。 请联系贵组织的管理员以请求获取访问权限。
  ![将用户添加到产品配置文件](/help/assets/optimize-at-edge/cs-fastly-user-product-profiles.png)
- 试用客户必须属于&#x200B;**LLMO管理员** IMS组。 如果该组不存在，则贵组织的管理员可以创建该组并添加您。
  ![创建LLMO管理员IMS组](/help/assets/optimize-at-edge/cs-fastly-create-ims-group.png)

>[!NOTE]
> Safari或无痕浏览/专用浏览模式不支持此功能。

## 启用路由的步骤

要开始将代理式流量路由到 Edge Optimize，请执行以下操作：

1. 在 LLM Optimizer 中，打开&#x200B;**客户配置**，然后选择&#x200B;**内容传递网络配置**&#x200B;选项卡。

   ![导航至客户配置](/help/assets/optimize-at-edge/cs-fastly-prereq-customer-config-nav.png)

2. 找到&#x200B;**将优化部署到 AI 代理**&#x200B;部分。 单击&#x200B;**启用**&#x200B;按钮。

   ![将优化部署到 AI 代理——待处理](/help/assets/optimize-at-edge/cs-fastly-enable-button.png)

3. 在确认对话框中，选择&#x200B;**启用**&#x200B;以确认您要启用路由。 如果出现错误，请参阅[疑难解答](#troubleshooting)部分以解决该问题。

   ![启用优化引擎确认对话框](/help/assets/optimize-at-edge/cs-fastly-enable-dialog.png)

4. 确认后，路由需要几分钟才能完成。

   ![路由正在进行中](/help/assets/optimize-at-edge/cs-fastly-enable-button-clicked-routing-in-progress.png)

   5分钟后重新加载页面，以验证路由是否完成。 路由配置并激活后，状态将更新为&#x200B;**已完成**，并显示绿色复选标记，确认路由已启用。 您无需执行任何其他操作。

   ![将优化部署到 AI 代理——已完成](/help/assets/optimize-at-edge/cs-fastly-disable-button.png)

   要随时禁用路由，请返回&#x200B;**CDN配置**&#x200B;选项卡中的&#x200B;**将优化部署到AI代理**&#x200B;部分，然后单击&#x200B;**禁用**。

此外，如果您在上述步骤中需要任何帮助，请联系您的 Adobe 帐户团队或 `llmo-at-edge@adobe.com`。

## 故障排除

如果启用或禁用路由时出现错误，则它类似于：

![确认对话框错误](/help/assets/optimize-at-edge/cs-fastly-confirmation-dialog-error.png)

使用下面的列表确定错误并按照说明进行操作。

1. **用户没有LLMO产品访问权限**

   **原因：**&#x200B;用户帐户的Adobe IMS配置文件中没有LLM Optimizer产品上下文。 付费客户配置CDN路由时需要此项。

   **推荐：**&#x200B;验证您的组织管理员是否已在Adobe Admin Console中为您分配了&#x200B;**Adobe LLM Optimizer用户**&#x200B;产品配置文件。

2. **只有LLMO管理员组成员可以配置CDN路由**

   **原因：**&#x200B;您的帐户不是&#x200B;**LLMO管理员** IMS组的成员。 试用客户需要此项才能配置CDN路由。

   **推荐：**&#x200B;确认您的组织管理员已将您添加到Adobe Admin Console中的&#x200B;**LLMO管理员** IMS组。

3. **请求的CDN类型aem-cs-fastly与此域检测到的CDN不匹配**

   **原因：**&#x200B;这表示检测到的网站CDN类型不是&#x200B;*AEM Cloud Service托管的CDN (Fastly)*。

   **推荐：**&#x200B;验证您的网站是否通过AEM Cloud Service Managed CDN (Fastly)提供。

4. **探测站点时出错**

   **原因：** LLM Optimizer在路由设置期间无法访问您的网站。 如果站点已关闭、无法访问或请求超时，则可能会发生这种情况。

   **推荐：**&#x200B;验证您的网站可公开访问并返回有效响应，然后重试。

5. **站点未返回路由探测**&#x200B;的有效响应

   **原因：**&#x200B;在安装过程中探测时，站点返回了意外的HTTP状态（不是2xx或301）。

   **建议：**&#x200B;验证您的网站是否针对LLM Optimizer中注册的基本URL返回了成功的响应(2xx)，然后重试。

6. **对上游IMS服务的身份验证失败**

   **原因：**&#x200B;会话可能已过期，或在路由请求期间与Adobe IMS进行身份验证时出现问题。

   **建议：**&#x200B;注销LLM Optimizer并重新登录，然后再次尝试启用路由。

如果问题仍然存在，请联系您的Adobe客户团队或`llmo-at-edge@adobe.com`。

## （可选）验证设置

完成路由配置后，您可以选择验证是否正在将AI机器人流量路由到Edge Optimize，以及人工流量是否不受影响。

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

2. **测试人员流量（不应受影响）**

   模拟一个常规的人类浏览器请求：

   ```
   curl -svo /dev/null https://www.example.com/page.html \
     --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
   ```

   响应不应包含`x-edgeoptimize-request-id`标头。 页面内容和响应时间应保持与启用 Optimize at Edge 之前时完全相同。

3. **如何区分这两种方案**

   | 页眉 | 机器人流量（已优化） | 人类流量（不受影响） |
   |---|---|---|
   | `x-edgeoptimize-request-id` | 存在——包含唯一的请求 ID | 不存在 |
   | `x-edgeoptimize-fo` | 仅在发生故障转移的情况下存在（值：`1`） | 不存在 |

4. **检查LLM Optimizer中的路由状态**

   您还可以在 LLM Optimizer UI 中确认路由。 打开&#x200B;**客户配置**，然后选择&#x200B;**内容传递网络配置**&#x200B;选项卡。 当路由激活时，**将优化部署到 AI 代理**&#x200B;部分显示&#x200B;**已完成**。

   ![将优化部署到 AI 代理——已完成](/help/assets/optimize-at-edge/cs-fastly-disable-button.png)

{{return-to-overview}}
