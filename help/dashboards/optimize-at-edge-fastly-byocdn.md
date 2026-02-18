---
title: 在Edge中优化 — Fastly (BYOCDN)
description: 了解如何在LLM Optimizer中为Edge中的优化配置Fastly BYOCDN。
feature: Opportunities
source-git-commit: 8cdd15413555057165f69ea4d5a15b243ab9098d
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 6%

---


# Fastly (BYOCDN)

此配置将代理流量（来自AI机器人和LLM用户代理的请求）路由到Edge优化后端服务(`live.edgeoptimize.net`)。 与往常一样，我们仍将从您的源头为人类访客和SEO机器人提供服务。 要测试配置，请在设置完成后查找响应中的标头`x-edgeoptimize-request-id`。

**先决条件**

在设置Fastly VCL规则之前，请确保您具有：

* 您的域访问Fastly。
* 已完成LLM Optimizer载入流程。
* 已完成到LLM Optimizer的CDN日志转发。
* 从Edge UI检索到LLM Optimizer优化API密钥。

{{retrieve-byocdn-api-key}}

**配置**

将以下三个VCL片段添加到您的Fastly服务。 这些代码片段处理向Edge Optimize发出的路由代理请求、缓存密钥分离以及故障转移到默认来源。

![Fastly VCL](/help/assets/optimize-at-edge/fastly-vcl.png)

![添加 VCL 代码片段](/help/assets/optimize-at-edge/add-vcl-snippets.png)

**vcl_recv 代码片段**

```
unset req.http.x-edgeoptimize-url;
unset req.http.x-edgeoptimize-config;
unset req.http.x-edgeoptimize-api-key;

if (!req.http.x-edgeoptimize-request
    && req.http.user-agent ~ "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)") {
  set req.http.x-forwarded-host = req.http.host; # required for identifying the original host
  set req.http.x-edgeoptimize-url = req.url; # required for identifying the original url
  set req.http.x-edgeoptimize-config = "LLMCLIENT=TRUE;"; # required for cache key
  set req.http.x-edgeoptimize-api-key = "<YOUR API KEY>"; # required for identifying the client
  set req.backend = F_EDGE_OPTIMIZE;
}
```

**vcl_hash 代码片段**

```
if (req.http.x-edgeoptimize-config) {
  set req.hash += "edge-optimize";
  set req.hash += req.http.x-edgeoptimize-config;
}
```

**vcl_deliver 代码片段**

```
if (req.http.x-edgeoptimize-config && resp.status >= 400) {
  set req.http.x-edgeoptimize-request = "failover";
  set req.backend = F_Default_Origin;
  restart;
}

if (!req.http.x-edgeoptimize-config && req.http.x-edgeoptimize-request == "failover") {
  set resp.http.x-edgeoptimize-fo = "1";
}
```

**故障转移**

`vcl_deliver`代码片段会自动处理故障转移。 如果Edge Optimize返回`4XX`或`5XX`错误，请求将重新启动并路由回您的默认来源，这样最终用户仍会收到响应。 故障转移响应包括`x-edgeoptimize-fo: 1`标头。

| 场景 | 行为 |
| --- | --- |
| Edge优化返回`2XX` | 优化的响应会提供给客户端。 |
| Edge优化返回`4XX`或`5XX` | 请求将重新启动，并从默认源提供服务。 |
| 故障转移响应 | 包含标头`x-edgeoptimize-fo: 1`。 |

{{verify-setup-byocdn}}

{{return-to-overview}}
