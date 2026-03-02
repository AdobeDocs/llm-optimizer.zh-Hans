---
title: Optimize at Edge - Fastly (BYOCDN)
description: 了解如何在 LLM Optimizer 中为 Optimize at Edge 配置 Fastly BYOCDN。
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: ht
source-wordcount: '369'
ht-degree: 100%

---


# Fastly (BYOCDN)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。人类访客和 SEO 机器人仍将照常从您的源站获得响应。完成设置后，可在响应中查找头部 `x-edgeoptimize-request-id` 以测试配置是否成功。

**先决条件**

设置 Fastly VCL 规则之前，请确保您：

* 可以为您的域访问 Fastly。
* 完成了 LLM Optimizer 的加入过程。
* 已将内容传递网络日志转发到 LLM Optimizer。
* 具有从 LLM Optimizer UI 检索到的 Edge Optimize API 密钥。

{{retrieve-byocdn-api-key}}

**配置**

将以下三个 VCL 代码片段添加到您的 Fastly 服务。这些代码片段用于处理将代理式请求路由到 Edge Optimize、缓存键分离以及故障转移到您的默认源站。

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

`vcl_deliver` 代码片段会自动处理故障转移。如果 Edge Optimize 返回 `4XX` 或 `5XX` 错误，请求就会重新启动并路由回到您的默认源站，以确保最终用户仍能收到响应。故障转移响应包含 `x-edgeoptimize-fo: 1` 头部。

| 场景 | 行为 |
| --- | --- |
| Edge Optimize 返回 `2XX` | 优化后的响应返回给客户端。 |
| Edge Optimize 返回 `4XX` 或 `5XX` | 请求会重新启动，并从默认源站提供响应。 |
| 故障转移响应 | 包含头部 `x-edgeoptimize-fo: 1`。 |

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

![启用了路由情况下的 AI 流量路由状态](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

{{return-to-overview}}
