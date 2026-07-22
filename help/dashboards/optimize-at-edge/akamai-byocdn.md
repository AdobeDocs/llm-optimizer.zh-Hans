---
title: Optimize at Edge - Akamai (BYOCDN)
description: 了解如何在 LLM Optimizer 中为 Optimize at Edge 配置 Akamai BYOCDN。
feature: Opportunities
autotag-review: '2026-07-15T17:40:02.356Z'
TQID: 'https://experienceleague.adobe.com/XlHpXbtxqPl-XQQKWeQc3rbsizCT7U0TF1bQkyv0iM8'
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
source-git-commit: 4f0c6d398e2aab337485b7e26cf6f2aba56375fd
workflow-type: tm+mt
source-wordcount: 795
ht-degree: 70%

---


# Akamai (BYOCDN)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。 人类访客和 SEO 机器人仍将照常从您的源站获得响应。 完成设置后，可在响应中查找头部 `x-edgeoptimize-request-id` 以测试配置是否成功。

**先决条件**

在设置Akamai属性管理器规则之前，请确保您具有：

* 可以为您的域访问 Akamai 属性管理器。
* 具有从 LLM Optimizer UI 检索到的 Edge Optimize API 密钥。 有关步骤，请参阅[检索您的 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#production-api-key)。
* （可选）要测试暂存路由，请参阅[暂存 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#staging-api-key-optional)。

**配置**

以下 Akamai 属性管理器规则可将代理式 HTML 页面流量路由至 Edge Optimize。 该配置包括以下步骤：

## &#x200B;1. 设置路由条件（User-Agent和HTML流量匹配）

为以下用户代理设置路由：

```
 *AdobeEdgeOptimize-AI*
 *ChatGPT-User*
 *GPTBot*
 *OAI-SearchBot*
 *PerplexityBot*
 *Perplexity-User*
 *ClaudeBot*
 *Claude-User*
 *Claude-SearchBot*
```

>[!NOTE]
>
>仅将 Optimize at Edge 路由规则应用于代理式 HTML 页面流量。 常见的设置是使用请求端条件（如&#x200B;**文件扩展名**）来匹配无扩展名页面 URL 的 `html` 和 `EMPTY_STRING`。 如果您的网站通过其他 URL 模式提供 HTML，或包含无扩展名的非页面路由（如 API 端点），请通过添加基于路径的条件来优化该规则。

![设置路由条件](/help/assets/optimize-at-edge/akamai-step1-routing.png)

## &#x200B;2. 设置来源和SSL行为

将源站设置为 `live.edgeoptimize.net`，将 SAN 与 `*.edgeoptimize.net` 匹配

>[!NOTE]
>
>如果添加 Optimize at Edge 规则后属性激活失败，请检查该规则是否使用了与默认规则不同的源服务器 SSL 验证模式。 如果是这样，请更新 Optimize at Edge 规则，以匹配默认规则。 例如，如果默认规则使用&#x200B;**平台设置**，那么这个规则中也应使用&#x200B;**平台设置**。 如果您无法使用所需的设置，请联系 Akamai 支持部门。

![设置源站和 SSL 行为](/help/assets/optimize-at-edge/akamai-step2-origin.png)

## &#x200B;3. 设置缓存键变量

将缓存键变量 `PMUSER_EDGE_OPTIMIZE_CACHE_KEY` 设置为 `LLMCLIENT=TRUE;X_FORWARDED_HOST={{builtin.AK_HOST}}`

![设置缓存键变量](/help/assets/optimize-at-edge/akamai-step3-cachekey.png)

## &#x200B;4. 缓存规则

![缓存规则](/help/assets/optimize-at-edge/akamai-step4-rules.png)

## &#x200B;5. 修改传入请求标头

将以下传入请求标头：
`x-edgeoptimize-api-key` 设置为从 LLMO 检索到的 API 密钥
`x-edgeoptimize-config` 设置为 `LLMCLIENT=TRUE;`
`x-edgeoptimize-url` 设置为 `{{builtin.AK_URL}}`

![修改传入请求标头](/help/assets/optimize-at-edge/akamai-step5-request.png)

## 允许通过防火墙规则在Edge中优化（可选）

{{waf-allowlist-setup}}

![在属性管理器中设置 x-edgeoptimize-fetcher-key 标头](/help/assets/optimize-at-edge/akamai-step10-fetcher-key.png)

>[!NOTE]
>
>另外，在 Akamai Bot Manager 中将`*AdobeEdgeOptimize/1.0*`用户代理和 `x-edgeoptimize-fetcher-key` 标头列入允许列表。

## &#x200B;6. 修改传入响应标头

![修改传入响应标头](/help/assets/optimize-at-edge/akamai-step6-response.png)

## &#x200B;7. 缓存标识修改

![缓存 ID 修改](/help/assets/optimize-at-edge/akamai-step7-cacheid.png)

## &#x200B;8. 修改传出请求标头

将 `x-forwarded-host` 头部设置为 `{{builtin.AK_HOST}}`

![更改传出请求头](/help/assets/optimize-at-edge/akamai-step8-outgoing-request.png)

## &#x200B;9. 站点故障切换

“站点故障转移”配置包含两个部分：主“在Edge中优化”路由规则内的故障转移行为以及在发生回退时添加响应标头的同级规则。

### 9a. 配置站点故障转移行为

在主Optimize at Edge路由规则内，创建一个名为&#x200B;**站点故障转移行为**&#x200B;的子规则。 将其设置为&#x200B;**匹配任意**&#x200B;并添加这些条件：

* **响应状态代码**&#x200B;在`400`到`599`的范围内。
* **源超时**&#x200B;为`Yes`。

![网站故障转移](/help/assets/optimize-at-edge/akamai-step9-failover.png)

![配置站点故障转移行为](/help/assets/optimize-at-edge/akamai-step9-failover-settings.png)

### 9b. 配置故障转移响应标头规则

>[!IMPORTANT]
>
>将 **EdgeOptimize 故障转移测试头**&#x200B;规则作为路由规则的&#x200B;**同级**（在同一级别）创建——**而不是**&#x200B;嵌套在路由规则中。 在 Akamai 属性管理器规则树中，层级结构应如下所示：
>
>```
>▼ Optimize at Edge                         ← parent rule group
>   ▼ Optimize at Edge Routing               ← routing child
>       Site Failover Behavior                 ← nested child
>   EdgeOptimize Failover - Test Header      ← sibling of routing child
>```
>
>当Akamai为原始主机名重新创建失败的请求时，将评估同级规则。 路由规则的API密钥条件可防止将该请求再次发送到Edge Optimize。
>
>此外，应确保 **Optimize at Edge 路由**&#x200B;规则不会被任何后来匹配的，会更改源站、缓存行为或相同请求的缓存 ID 的规则所覆盖。 如果其他匹配规则重置了这些行为，Optimize at Edge 路由或缓存可能就无法按预期工作。

![配置故障转移响应标头规则](/help/assets/optimize-at-edge/akamai-step9-failover-header.png)

站点故障转移可确保当Edge Optimize返回错误或超时时，Akamai会为原始主机名重新创建请求，以便访客仍可收到站点的正常响应。

| 场景 | 行为 |
| --- | --- |
| Edge Optimize 返回 `2XX` 或 `3XX` | 提供优化的响应。 `x-edgeoptimize-request-id`存在。 |
| Edge Optimize返回`4XX`-`5XX`，或源超时 | 将为原始主机名重新创建请求。 响应包括`x-edgeoptimize-fo: true`。 |

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
| `x-edgeoptimize-fo` | 仅在发生故障转移的情况下存在（值：`true`） | 不存在 |

{{verify-routing-status-in-ui}}

{{return-to-overview}}
