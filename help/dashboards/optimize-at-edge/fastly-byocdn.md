---
title: Optimize at Edge - Fastly (BYOCDN)
description: 了解如何在 LLM Optimizer 中为 Optimize at Edge 配置 Fastly BYOCDN。
feature: Opportunities
autotag-review: '2026-07-15T17:50:43.991Z'
TQID: 'https://experienceleague.adobe.com/Ueis3UcuGZz19FUJavq44dF3q3Irz2Ri4s7JTCB-H80'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
  - id: ef4e63f5-cb4d-462d-bf9a-1f617edf2a3a
  - id: e0828736-236a-487b-a478-5a635455eadc
subfeature_v2:
  - id: d23587d6-14d6-4e3f-9ee1-cc18623832e1
  - id: e06fae5f-830b-4222-a469-b5e148d36465
role_v2:
  - id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2:
  - id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: e36ee407933e2d3d56cadf1c9517f23f24d41d91
workflow-type: tm+mt
source-wordcount: 350
ht-degree: 92%

---


# Fastly (BYOCDN)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。 人类访客和 SEO 机器人仍将照常从您的源站获得响应。 完成设置后，可在响应中查找头部 `x-edgeoptimize-request-id` 以测试配置是否成功。

**先决条件**

在设置Fastly VCL规则之前，请确保您具有：

* 可以为您的域访问 Fastly。
* 具有从 LLM Optimizer UI 检索到的 Edge Optimize API 密钥。 有关步骤，请参阅[检索您的 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#production-api-key)。
* （可选）要测试暂存路由，请参阅[暂存 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#staging-api-key-optional)。

## 配置

将以下三个 VCL 代码片段添加到您的 Fastly 服务。 这些代码片段用于处理将代理式请求路由到 Edge Optimize、缓存键分离以及故障转移到您的默认源站。

![Fastly后端配置](/help/assets/optimize-at-edge/fastly-backend-config.png)

![添加 VCL 代码片段](/help/assets/optimize-at-edge/add-vcl-snippets.png)

### vcl_recv代码片段

```
unset req.http.x-edgeoptimize-url;
unset req.http.x-edgeoptimize-config;
unset req.http.x-edgeoptimize-api-key;
unset req.http.x-edgeoptimize-fetcher-key; # Optional (required only in case of WAF)

if (!req.http.x-edgeoptimize-request
    && req.http.user-agent ~ "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User|ClaudeBot|Claude-User|Claude-SearchBot)") {
  set req.http.x-forwarded-host = req.http.host; # required for identifying the original host
  set req.http.x-edgeoptimize-url = req.url; # required for identifying the original url
  set req.http.x-edgeoptimize-config = "LLMCLIENT=TRUE;"; # required for cache key
  set req.http.x-edgeoptimize-api-key = "<YOUR API KEY>"; # required for identifying the client
  set req.http.x-edgeoptimize-fetcher-key = "<YOUR FETCHER KEY>"; # Optional (required only in case of WAF)
  set req.backend = F_EDGE_OPTIMIZE;
  return(lookup);
}
```

### vcl_hash代码片段

```
if (req.http.x-edgeoptimize-config) {
  set req.hash += "edge-optimize";
  set req.hash += req.http.x-edgeoptimize-config;
}
```

### vcl_deliver代码片段

```
if (req.http.x-edgeoptimize-config && resp.status >= 400) {
  set req.http.x-edgeoptimize-request = "failover";
  restart;
}

if (req.http.x-edgeoptimize-config) {
  return(deliver);
}

if (!req.http.x-edgeoptimize-config && req.http.x-edgeoptimize-request == "failover") {
  set resp.http.x-edgeoptimize-fo = "1";
}
```

### 故障转移

`vcl_deliver` 代码片段会自动处理故障转移。 如果 Edge Optimize 返回 `4XX` 或 `5XX` 错误，请求就会重新启动并路由回到您的默认源站，以确保最终用户仍能收到响应。 故障转移响应包含 `x-edgeoptimize-fo: 1` 头部。

| 场景 | 行为 |
| --- | --- |
| Edge Optimize 返回 `2XX` | 优化后的响应返回给客户端。 |
| Edge Optimize 返回 `4XX` 或 `5XX` | 请求会重新启动，并从默认源站提供响应。 |
| 故障转移响应 | 包含头部 `x-edgeoptimize-fo: 1`。 |

## 允许通过防火墙规则在Edge中优化（可选）

{{waf-allowlist-setup}}

## 验证设置

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

响应&#x200B;**不**&#x200B;应包含 `x-edgeoptimize-request-id` 头部。 页面内容和响应时间应保持与启用 Optimize at Edge 之前时完全相同。

**3. 如何区分这两种场景**

| 页眉 | 机器人流量（已优化） | 人类流量（不受影响） |
|---|---|---|
| `x-edgeoptimize-request-id` | 存在——包含唯一的请求 ID | 不存在 |
| `x-edgeoptimize-fo` | 仅在发生故障转移的情况下存在（值：`1`） | 不存在 |

{{verify-routing-status-in-ui}}

{{return-to-overview}}
