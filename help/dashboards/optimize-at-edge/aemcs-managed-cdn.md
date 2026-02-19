---
title: 在Edge中优化 — AEM Cloud Service托管的CDN (Fastly)
description: 了解如何在LLM Optimizer中为Edge优化配置AEM Cloud Service Managed CDN (Fastly)。
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 12%

---


# AEM Cloud Service托管的CDN (Fastly)

此配置将代理流量（来自AI机器人和LLM用户代理的请求）路由到Edge优化后端服务(`live.edgeoptimize.net`)。 与往常一样，我们仍将从您的源头为人类访客和SEO机器人提供服务。 要测试配置，请在设置完成后查找响应中的标头`x-edgeoptimize-request-id`。

**先决条件**

要开始将代理流量路由到Edge Optimize，请执行以下操作：

1. 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

   ![导航到客户配置](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 在&#x200B;**AI流量路由到部署优化**&#x200B;下，勾选&#x200B;**将优化部署到AI代理**&#x200B;复选框。 Adobe团队将代表您处理路由配置。

   ![将部署优化勾选到AI代理](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. 启用该复选框后，状态将显示设置正在进行。 Adobe团队将为您完成路由配置。

   ![AI流量路由设置正在进行中](/help/assets/optimize-at-edge/prereq-traffic-routing-progress.png)

   配置并激活路由后，状态将更新为显示绿色复选标记，指示已成功启用路由。 您无需执行其他操作。

此外，如果您需要上述步骤的任何帮助，请联系您的Adobe客户团队或`llmo-at-edge@adobe.com`。

**通过Cloud Manager Pipeline的自助路由**

如果您希望通过Cloud Manager Pipeline自行配置路由，请执行以下步骤。 路由配置通过使用 [originSelector 内容传递网络规则](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors)实现。 前提条件如下：

* 决定要路由的域。
* 决定要路由的路径。
* 决定要路由的用户代理（推荐的正则表达式）。

要部署该规则，您需要：

* 创建[配置管道](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/operations/config-pipeline)。
* 在存储库中提交`cdn.yaml`配置文件。
* 运行配置管道。

```
kind: "CDN"
version: "1"
data:
  # Origin selectors to route to Edge Optimize backend
  originSelectors:
    rules:
      - name: route-to-edge-optimize-backend
        when:
          allOf:
            - reqHeader: x-edgeoptimize-request
              exists: false # avoid loops when requests comes from Edge Optimize
            - reqHeader: user-agent
              matches: "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)" # routed user agents
            - reqProperty: domain
              equals: "example.com" # routed domain
            - reqProperty: originalPath
              matches: '(/[^./]+|\.html|/)$' # routed extensions, with .html extension or without extension
            - anyOf:
              - { reqProperty: originalPath, in: [ "/page.html" ] } # routed pages, exact path matching
              - { reqProperty: originalPath, like: "/dir/*" } # routed pages, wildcard path matching
        action:
          type: selectOrigin
          originName: edge-optimize-backend
    origins:
      - name: edge-optimize-backend
        domain: "live.edgeoptimize.net"
```

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

{{return-to-overview}}
