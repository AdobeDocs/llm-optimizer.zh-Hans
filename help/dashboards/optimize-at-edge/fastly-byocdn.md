---
title: 在Edge中优化 — Fastly (BYOCDN)
description: 了解如何在LLM Optimizer中为Edge中的优化配置Fastly BYOCDN。
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: tm+mt
source-wordcount: '369'
ht-degree: 5%

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

{{return-to-overview}}
