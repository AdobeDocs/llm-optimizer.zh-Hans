---
title: 在Edge中优化 — AEM Cloud Service托管的CDN (Fastly)
description: 了解如何在LLM Optimizer中为Edge优化配置AEM Cloud Service Managed CDN (Fastly)。
feature: Opportunities
source-git-commit: 8cdd15413555057165f69ea4d5a15b243ab9098d
workflow-type: tm+mt
source-wordcount: '329'
ht-degree: 16%

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

{{verify-setup-adobe-aem-cs-cdn}}

{{return-to-overview}}
