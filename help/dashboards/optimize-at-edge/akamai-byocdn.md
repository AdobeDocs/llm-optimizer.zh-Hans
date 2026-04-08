---
title: Optimize at Edge - Akamai (BYOCDN)
description: 了解如何在 LLM Optimizer 中为 Optimize at Edge 配置 Akamai BYOCDN。
feature: Opportunities
source-git-commit: f2a652761acbea7ca5b8e8740c1dbd0132e42f7f
workflow-type: tm+mt
source-wordcount: '849'
ht-degree: 79%

---


# Akamai (BYOCDN)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。 人类访客和 SEO 机器人仍将照常从您的源站获得响应。 完成设置后，可在响应中查找头部 `x-edgeoptimize-request-id` 以测试配置是否成功。

**先决条件**

在设置 Akamai 属性管理器规则之前，请确保您：

* 可以为您的域访问 Akamai 属性管理器。
* 完成了 LLM Optimizer 的加入过程。
* 已将内容传递网络日志转发到 LLM Optimizer。
* 具有从 LLM Optimizer UI 检索到的 Edge Optimize API 密钥。
* （可选）如果首先在暂存主机名上测试路由，则使用暂存Edge优化API密钥。

{{retrieve-byocdn-api-key}}

{{retrieve-staging-edge-optimize-api-key}}

**配置**

以下Akamai属性管理器规则将代理HTML页面流量路由到Edge Optimize。 该配置包括以下步骤：

**1. 设置路由条件（User-Agent和HTML流量匹配）**

为以下用户代理设置路由：

```
 *AdobeEdgeOptimize-AI*
 *ChatGPT-User*
 *GPTBot*
 *OAI-SearchBot*
 *PerplexityBot*
 *Perplexity-User*
```

>[!NOTE]
>
>将在Edge中优化路由规则仅适用于无代理的HTML页面流量。 常见的设置是使用请求端条件（如&#x200B;**文件扩展名**）来匹配无扩展名页面URL的`html`和`EMPTY_STRING`。 如果您的网站从其他URL模式提供HTML，或包含无扩展的非页面路由（如API端点），请用其他基于路径的标准来优化规则。

![设置路由条件](/help/assets/optimize-at-edge/akamai-step1-routing.png)

**2. 设置源站和 SSL 行为**

将源站设置为 `live.edgeoptimize.net`，将 SAN 与 `*.edgeoptimize.net` 匹配

>[!NOTE]
>
>如果添加 Optimize at Edge 规则后属性激活失败，请检查该规则是否使用了与默认规则不同的源服务器 SSL 验证模式。 如果是这样，请更新 Optimize at Edge 规则，以匹配默认规则。 例如，如果默认规则使用&#x200B;**平台设置**，那么这个规则中也应使用&#x200B;**平台设置**。 如果您无法使用所需的设置，请联系 Akamai 支持部门。

![设置源站和 SSL 行为](/help/assets/optimize-at-edge/akamai-step2-origin.png)

**3. 设置缓存键变量**

将缓存键变量 `PMUSER_EDGE_OPTIMIZE_CACHE_KEY` 设置为 `LLMCLIENT=TRUE;X_FORWARDED_HOST={{builtin.AK_HOST}}`

![设置缓存键变量](/help/assets/optimize-at-edge/akamai-step3-cachekey.png)

**4. 缓存规则**

![缓存规则](/help/assets/optimize-at-edge/akamai-step4-rules.png)

**5. 修改传入请求标头**

设置以下传入请求标头：
`x-edgeoptimize-api-key`到从LLMO检索到的API密钥
`x-edgeoptimize-config` 更改至 `LLMCLIENT=TRUE;`
`x-edgeoptimize-url`到`{{builtin.AK_URL}}`

![修改传入请求标头](/help/assets/optimize-at-edge/akamai-step5-request.png)

**6. 修改传入响应标头**

![修改传入响应标头](/help/assets/optimize-at-edge/akamai-step6-response.png)

**7. 缓存 ID 修改**

![缓存 ID 修改](/help/assets/optimize-at-edge/akamai-step7-cacheid.png)

**8. 更改传出请求头**

将 `x-forwarded-host` 头部设置为 `{{builtin.AK_HOST}}`

![更改传出请求头](/help/assets/optimize-at-edge/akamai-step8-outgoing-request.png)

**9. 网站故障转移**

网站故障转移配置包含两个部分：故障转移行为（在 optimize-at-edge 主路由规则中配置）和一个单独的故障转移测试头规则。

**9a。 网站故障转移行为（在 optimize-at-edge 主路由规则中）**

在主路由规则中，按如下方式配置网站故障转移行为和高级 XML 代码片段：

>[!IMPORTANT]
>
>此步骤中的 XML 代码片段需要&#x200B;**高级**&#x200B;行为。 在某些 Akamai 环境中，此行为不可用于自助编辑。 如果您看不到&#x200B;**高级**&#x200B;选项，请联系您的 Akamai 帐户团队或 Akamai 支持部门，以启用所需的配置。

![网站故障转移](/help/assets/optimize-at-edge/akamai-step9-failover.png)

通过高级 XML 添加值为 `fo` 的请求头 `x-edgeoptimize-request`：

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

**9b。 故障转移测试头规则（同级规则）**

>[!IMPORTANT]
>
>将 **EdgeOptimize 故障转移测试头**&#x200B;规则作为路由规则的&#x200B;**同级**（在同一级别）创建——**而不是**&#x200B;嵌套在路由规则中。 在 Akamai 属性管理器规则树中，层级结构应如下所示：
>
>```
>▼ Parent Rule
>   ▶ Optimize at Edge Routing     ← routing rule
>       EdgeOptimize Failover - Test Header       ← sibling, same level
>```
>
>这可确保故障转移测试头规则会评估&#x200B;**所有**&#x200B;路由规则，而不仅仅评估一个。
>
>此外，应确保 **Optimize at Edge 路由**&#x200B;规则不会被任何后来匹配的，会更改源站、缓存行为或相同请求的缓存 ID 的规则所覆盖。 如果其他匹配规则重置了这些行为，Optimize at Edge 路由或缓存可能就无法按预期工作。

如果请求头的 `x-edgeoptimize-request` 值为 `fo`，传出响应头 `x-edgeoptimize-fo` 就应设置为 `true`。

![故障转移规则](/help/assets/optimize-at-edge/akamai-step9-failover-rules.png)

网站故障转移可确保在 Edge Optimize 返回 `4XX` 或 `5XX` 错误的情况下，请求会自动路由回到您的默认源站，从而确保最终用户仍会收到响应。

| 场景 | 行为 |
| --- | --- |
| Edge Optimize 返回 `2XX` | 优化后的响应返回给客户端。 |
| Edge Optimize 返回 `4XX` 或 `5XX` | 请求被路由回到默认源站。 |

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

响应&#x200B;**不**&#x200B;应包含 `x-edgeoptimize-request-id` 头部。 页面内容和响应时间应保持与启用 Optimize at Edge 之前时完全相同。

**3. 如何区分这两种场景**

| 页眉 | 机器人流量（已优化） | 人类流量（不受影响） |
|---|---|---|
| `x-edgeoptimize-request-id` | 存在——包含唯一的请求 ID | 不存在 |
| `x-edgeoptimize-fo` | 仅在发生故障转移的情况下存在（值：`1`） | 不存在 |

**4. 暂存域（可选）**

如果您使用LLM Optimizer中的暂存主机名和暂存API密钥，请在规则中使用&#x200B;**staging**&#x200B;密钥在&#x200B;**staging** Akamai资产上部署相同的路由模式。 然后，验证暂存主机上的机器人流量：

```
curl -svo /dev/null https://staging.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

将`https://staging.example.com/page.html`替换为您的实际暂存URL和路径。 成功的响应包括`x-edgeoptimize-request-id`标头。

{{verify-routing-status-in-ui}}

{{return-to-overview}}
