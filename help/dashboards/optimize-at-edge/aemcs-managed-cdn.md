---
title: Optimize at Edge - AEM 云服务托管的内容传递网络 (Fastly)
description: 了解如何在 LLM Optimizer 中为 Optimize at Edge 配置 AEM 云服务托管的内容传递网络 (Fastly)。
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: ht
source-wordcount: '481'
ht-degree: 100%

---


# AEM 云服务托管的内容传递网络 (Fastly)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。人类访客和 SEO 机器人仍将照常从您的源站获得响应。完成设置后，可在响应中查找头部 `x-edgeoptimize-request-id` 以测试配置是否成功。

**先决条件**

要开始将代理式流量路由到 Edge Optimize，请执行以下操作：

1. 导航至&#x200B;**客户配置**，然后选择&#x200B;**内容传递网络配置**&#x200B;选项卡。

   ![导航至客户配置](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 在 **用于部署优化的 AI 流量路由**&#x200B;中勾选复选框&#x200B;**将优化部署到 AI 代理**。Adobe 团队将代表您进行路由配置。

   ![勾选将优化部署到 AI 代理](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. 启用这个复选框后，状态会显示正在进行设置。Adobe 团队将为您完成路由配置。

   ![正在进行 AI 流量路由设置](/help/assets/optimize-at-edge/prereq-traffic-routing-progress.png)

   路由配置结束并激活以后，状态就会更新显示为一个绿色复选标记，表示已成功启用路由。您无需执行任何其他操作。

此外，如果您在上述步骤中需要任何帮助，请联系您的 Adobe 帐户团队或 `llmo-at-edge@adobe.com`。

**通过 Cloud Manager 管道进行自助路由**

如果您想通过 Cloud Manager 管道自行配置路由，请执行以下步骤。路由配置通过使用 [originSelector 内容传递网络规则](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors)实现。 前提条件如下：

* 确定需要进行路由的域。
* 确定需要进行路由的路径。
* 确定需要进行路由的用户代理（建议使用正则表达式）。

要部署该规则，您需要：

* 创建一个[配置管道](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/operations/config-pipeline)。
* 在您的存储库中提交 `cdn.yaml` 配置文件。
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

完成设置后，验证机器人流量是否被路由到 Edge Optimize，以及人类流量是否不受影响。

**1. 测试机器人流量（应被优化）**

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

**2. 测试人类流量（不应受影响）**

模拟一个常规的人类浏览器请求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
```

响应&#x200B;**不**&#x200B;应包含 `x-edgeoptimize-request-id` 头部。页面内容和响应时间应保持与启用 Optimize at Edge 之前时完全相同。

**3. 如何区分这两种场景**

| 页眉 | 机器人流量（已优化） | 人类流量（不受影响） |
|---|---|---|
| `x-edgeoptimize-request-id` | 存在——包含唯一的请求 ID | 不存在 |
| `x-edgeoptimize-fo` | 仅在发生故障转移的情况下存在（值：`1`） | 不存在 |

也可以在 LLM Optimizer UI 中查看流量路由的状态。导航至&#x200B;**客户配置**，然后选择&#x200B;**内容传递网络配置**&#x200B;选项卡。

![启用了路由情况下的 AI 流量路由状态](/help/assets/optimize-at-edge/adobe-CDN-traffic-routed-tick.png)

{{return-to-overview}}
