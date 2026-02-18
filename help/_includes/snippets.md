---
source-git-commit: beae935e7a34f5bccbe21578fa9a928912958710
workflow-type: tm+mt
source-wordcount: '467'
ht-degree: 2%

---
# 代码片段

## 验证设置 — Adobe托管的CDN {#verify-setup-adobe-aem-cs-cdn}

**验证设置**

完成设置后，验证是否正在将机器人流量路由到Edge Optimize，以及人流量是否不受影响。

**1. 测试机器人流量（应优化）**

使用代理用户代理模拟AI机器人请求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

成功的响应包括`x-edgeoptimize-request-id`标头，用于确认请求是通过Edge优化路由的：

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

**2. 测试人员流量（不应受影响）**

模拟常规的人类浏览器请求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
```

响应应&#x200B;**不**&#x200B;包含`x-edgeoptimize-request-id`标头。 在Edge中启用优化之前，页面内容和响应时间应保持相同。

**3. 如何区分这两种方案**

| 页眉 | 机器人流量（已优化） | 人流（未受影响） |
|---|---|---|
| `x-edgeoptimize-request-id` | 存在 — 包含唯一的请求ID | 不存在 |
| `x-edgeoptimize-fo` | 仅在发生故障转移时存在（值： `1`） | 不存在 |

也可以在LLM Optimizer UI中查看流量路由的状态。 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

![启用了路由的AI流量路由状态](/help/assets/optimize-at-edge/adobe-CDN-traffic-routed-tick.png)

## 验证设置 — BYOCDN {#verify-setup-byocdn}

**验证设置**

完成设置后，验证是否正在将机器人流量路由到Edge Optimize，以及人流量是否不受影响。

**1. 测试机器人流量（应优化）**

使用代理用户代理模拟AI机器人请求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

成功的响应包括`x-edgeoptimize-request-id`标头，用于确认请求是通过Edge优化路由的：

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

**2. 测试人员流量（不应受影响）**

模拟常规的人类浏览器请求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
```

响应应&#x200B;**不**&#x200B;包含`x-edgeoptimize-request-id`标头。 在Edge中启用优化之前，页面内容和响应时间应保持相同。

**3. 如何区分这两种方案**

| 页眉 | 机器人流量（已优化） | 人流（未受影响） |
|---|---|---|
| `x-edgeoptimize-request-id` | 存在 — 包含唯一的请求ID | 不存在 |
| `x-edgeoptimize-fo` | 仅在发生故障转移时存在（值： `1`） | 不存在 |

也可以在LLM Optimizer UI中查看流量路由的状态。 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

![启用了路由的AI流量路由状态](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

## API密钥检索步骤 {#retrieve-byocdn-api-key}

**检索API密钥的步骤：**

1. 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

   ![导航到客户配置](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 在&#x200B;**AI流量路由到部署优化**&#x200B;下，勾选&#x200B;**将优化部署到AI代理**&#x200B;复选框。

   ![将部署优化勾选到AI代理](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. 复制API密钥，并继续执行下面的路由配置步骤。

   ![复制API密钥](/help/assets/optimize-at-edge/prereq-copy-api-key.png)

   >[!NOTE]
   >在此阶段，状态可能显示红叉，指示设置尚未完成。 这是正常情况 — 一旦您完成下面的路由配置并且AI机器人流量开始流动，状态将更新为绿色复选标记，确认路由已成功启用。

此外，如果您需要上述步骤的任何帮助，请联系您的Adobe客户团队或`llmo-at-edge@adobe.com`。

## 返回概述 {#return-to-overview}

要进一步了解Edge优化，包括可用的机会、自动优化工作流和常见问题，请返回[Edge优化概述](/help/dashboards/optimize-at-edge.md)。
