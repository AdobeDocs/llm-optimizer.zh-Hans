---
title: 在Edge中优化 — Akamai (BYOCDN)
description: 了解如何在LLM Optimizer中为Edge中的优化配置Akamai BYOCDN。
feature: Opportunities
source-git-commit: 9230e525340bb951fcd9f2ae1f88bad557d5b7d7
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 14%

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

站点故障转移配置包含两部分：故障转移行为（在主optimize-at-edge路由规则中配置）和单独的故障转移测试标头规则。

**9a。 站点故障转移行为（在主要边缘优化路由规则内）**

在主路由规则内，按如下方式配置“站点故障转移”行为和“高级XML”代码片段：

![网站故障转移](/help/assets/optimize-at-edge/akamai-step9-failover.png)

通过高级XML添加值为`fo`的请求标头`x-edgeoptimize-request`：

```
<forward:availability.fail-action2>
<add-header>
<status>on</status>
<name>x-edgeoptimize-request</name>
<value>fo</value>
</add-header>
</forward:availability.fail-action2>
```

![故障转移行为](/help/assets/optimize-at-edge/akamai-step9-failover-behaviors.png)

**9b。 故障转移测试标头规则（同级规则）**

>[!IMPORTANT]
>
>将&#x200B;**EdgeOptimize故障转移 — 测试标头**&#x200B;规则创建为路由规则的&#x200B;**同级** （在同一级别） — **不是**&#x200B;嵌套在其中。 在Akamai属性管理器规则树中，层次结构应如下所示：
>
>```
>▼ Parent Rule
>   ▶ Optimize at Edge Routing     ← routing rule
>       EdgeOptimize Failover - Test Header       ← sibling, same level
>```
>
>这可确保故障转移测试标头规则评估&#x200B;**所有**&#x200B;路由规则，而不仅仅评估一个。

如果请求标头`x-edgeoptimize-request`值为`fo`，则将传出响应标头`x-edgeoptimize-fo`设置为`true`。

![故障转移规则](/help/assets/optimize-at-edge/akamai-step9-failover-rules.png)

站点故障转移可确保，如果Edge Optimize返回`4XX`或`5XX`错误，请求将自动路由回默认来源，以便最终用户仍会收到响应。

| 场景 | 行为 |
| --- | --- |
| Edge优化返回`2XX` | 优化的响应会提供给客户端。 |
| Edge优化返回`4XX`或`5XX` | 请求被路由回默认来源。 |

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
