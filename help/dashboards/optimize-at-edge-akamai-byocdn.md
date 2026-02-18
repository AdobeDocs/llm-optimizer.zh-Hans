---
title: 在Edge中优化 — Akamai (BYOCDN)
description: 了解如何在LLM Optimizer中为Edge中的优化配置Akamai BYOCDN。
feature: Opportunities
source-git-commit: 23752b30294c3d467ca477b085aa521cad0f72ca
workflow-type: tm+mt
source-wordcount: '302'
ht-degree: 26%

---


# Akamai (BYOCDN)

此配置将代理流量（来自AI机器人和LLM用户代理的请求）路由到Edge优化后端服务(`live.edgeoptimize.net`)。 与往常一样，我们仍将从您的源头为人类访客和SEO机器人提供服务。 要测试配置，请在设置完成后查找响应中的标头`x-edgeoptimize-request-id`。

**先决条件**

在设置Akamai属性管理器规则之前，请确保您具有：

* 访问域的Akamai资产管理器。
* 已完成LLM Optimizer载入流程。
* 已完成到LLM Optimizer的CDN日志转发。
* 从Edge UI检索到LLM Optimizer优化API密钥。

{{retrieve-byocdn-api-key}}

**配置**

以下Akamai属性管理器规则将LLM用户代理路由到Edge Optimize。 该配置包括以下步骤：

**1. 设置路由条件（用户-代理匹配）**

为以下用户代理设置路由:image.png

```
 *AdobeEdgeOptimize-AI*,
 *ChatGPT-User*,
 *GPTBot*,
 *OAI-SearchBot*,
 *PerplexityBot*,
 *Perplexity-User*
```

![设置路由条件](/help/assets/optimize-at-edge/akamai-step1-routing.png)

**2. 设置源站和 SSL 行为**

将源设置为`live.edgeoptimize.net`并将SAN与`*.edgeoptimize.net`匹配

![设置源站和 SSL 行为](/help/assets/optimize-at-edge/akamai-step2-origin.png)

**3. 设置缓存键变量**

将缓存键变量`PMUSER_EDGE_OPTIMIZE_CACHE_KEY`设置为`LLMCLIENT=TRUE;X_FORWARDED_HOST={{builtin.AK_HOST}}`

![设置缓存键变量](/help/assets/optimize-at-edge/akamai-step3-cachekey.png)

**4. 缓存规则**

![缓存规则](/help/assets/optimize-at-edge/akamai-step4-rules.png)

**5. 修改传入请求标头**

设置以下传入请求标头：
`x-edgeoptimize-api-key`到从LLMO检索到的API密钥
`x-edgeoptimize-config`到`LLMCLIENT=TRUE;`
`x-edgeoptimize-url`至`{{builtin.AK_URL}}`

![修改传入请求标头](/help/assets/optimize-at-edge/akamai-step5-request.png)

**6. 修改传入响应标头**

![修改传入响应标头](/help/assets/optimize-at-edge/akamai-step6-response.png)

**7. 缓存 ID 修改**

![缓存 ID 修改](/help/assets/optimize-at-edge/akamai-step7-cacheid.png)

**8. 修改传出请求标头**

将`x-forwarded-host`标头设置为`{{builtin.AK_HOST}}`

![修改传出请求标头](/help/assets/optimize-at-edge/akamai-step8-outgoing-request.png)

**9. 网站故障转移**

![网站故障转移](/help/assets/optimize-at-edge/akamai-step9-failover.png)

![故障转移行为](/help/assets/optimize-at-edge/akamai-step9-failover-behaviors.png)

![故障转移规则](/help/assets/optimize-at-edge/akamai-step9-failover-rules.png)

站点故障转移可确保，如果Edge Optimize返回`4XX`或`5XX`错误，请求将自动路由回默认来源，以便最终用户仍会收到响应。

| 场景 | 行为 |
| --- | --- |
| Edge优化返回`2XX` | 优化的响应会提供给客户端。 |
| Edge优化返回`4XX`或`5XX` | 请求被路由回默认来源。 |

{{verify-setup-byocdn}}

{{return-to-overview}}
