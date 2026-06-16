---
title: 在Edge中优化 — Azure前门(BYOCDN)
description: 了解如何在LLM Optimizer中为Edge中的优化配置Azure前门BYOCDN。
feature: Opportunities
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
subfeature_v2:
  - id: d23587d6-14d6-4e3f-9ee1-cc18623832e1
source-git-commit: 1d07ce1820e2688a130e410ee88ab329d628356a
workflow-type: tm+mt
source-wordcount: 768
ht-degree: 24%

---


# Azure前门(BYOCDN)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。 人类访客和 SEO 机器人仍将照常从您的源站获得响应。 完成设置后，可在响应中查找头部 `x-edgeoptimize-request-id` 以测试配置是否成功。

Azure Front Door不会在边缘运行自定义代码。 路由是使用&#x200B;**规则集**&#x200B;以及专用的&#x200B;**原始组**&#x200B;为Edge优化配置的。 故障转移由Azure Front Door基于优先级的原始组运行状况探测器处理。

**先决条件**

在设置Azure前门路由规则之前，请确保您具有：

* 访问您的Azure前门配置文件。
* 具有从 LLM Optimizer UI 检索到的 Edge Optimize API 密钥。 有关步骤，请参阅[检索您的 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#production-api-key)。
* （可选）要测试暂存路由，请参阅[暂存 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#staging-api-key-optional)。

## 步骤1：为Edge优化创建源组

您的Azure前门配置文件已有一个指向您的源的默认源组。 为Edge优化创建一个&#x200B;**新**&#x200B;原始组：

* **名称：**`edge-optimize-origin-group`
* **源（基于优先级的故障转移）：**
   * **优先级1** — `live.edgeoptimize.net` （原始主机标头： `live.edgeoptimize.net`）
   * **优先级2** — 您的域终结点（例如，`www.example.com`）。 这是用于故障转移：如果Edge Optimize不正常，则请求路由到您的域，该域将重新进入Azure前门并从您的默认来源提供服务。
* **运行状况探测器：** **已启用**
   * 路径： `/health/<your-domain>` （例如，`/health/www.example.com`）
   * 协议：HTTPS
   * 间隔：225秒
* **会话关联性：** **已禁用**
* **证书使用者名称验证：** **已启用**

![Edge优化具有两个基于优先级的源和运行状况探测的原始组](/help/assets/optimize-at-edge/azure-front-door-origin-group.png)

>[!NOTE]
>
>`edge-optimize-origin-group`源组在门户中显示&#x200B;**“未关联”**&#x200B;警告。 这是预期的 — 它通过规则集路由覆盖引用，而不是直接通过路由引用。

## 步骤2：配置路由

默认路由通常使用您的Azure前门配置文件创建。 规则集（步骤3）会覆盖代理流量的源组，因此Edge Optimize不需要单独的路由。

## 第3步：创建规则集

转到&#x200B;**规则集** > **添加规则集**&#x200B;并将其命名为`EORouting`。 按此顺序添加三个规则。

![EORouting规则集显示标头剥离和机器人路由规则](/help/assets/optimize-at-edge/azure-front-door-ruleset-routing.png)

### 规则1： StripIncomingEOHeaders01

剥离传入的Edge Optimize标头以防止欺骗。 无条件 — 适用于所有请求。 停止评估： **关**。

**操作** — 删除每个的请求标头：

* `x-edgeoptimize-url`
* `x-edgeoptimize-config`
* `x-edgeoptimize-api-key`
* `x-edgeoptimize-fetcher-key`

### 规则2：EOGPTBotRootGET03

在HTML页面路径上将机器人请求路由到Edge Optimize。 停止评估：**于**。

**条件** （所有条件都必须匹配）：

* 请求方法： **等于** `GET`
* 请求路径： **RegEx** `(^$|^.*/$|(^|.*/)[^./]+$|^.*\.html$)` （与站点根、以`/`结尾的路径、无扩展名的页面路径和`.html`个路径匹配）
* 用户代理： **包含任何** `chatgpt-user`、`gptbot`、`oai-searchbot`、`adobeedgeoptimize-ai`、`perplexitybot`、`perplexity-user`、`claudebot`、`claude-user`、`claude-searchbot`。 将字符串转换设置为&#x200B;**小写**。
* `x-edgeoptimize-monitor`： **不包含** `1`
* `x-edgeoptimize-request`： **不包含任何** `failover`，`1`

**操作**：

* 请求标头覆盖`x-edgeoptimize-url` = `/{url_path}?{query_string}`
* 请求标头覆盖`x-edgeoptimize-config` = `LLMCLIENT=TRUE;`
* 请求标头覆盖`x-edgeoptimize-api-key` = `YOUR_API_KEY`
* 请求标头覆盖`x-edgeoptimize-monitor` = `1`
* 路由配置覆盖：源组→ `edge-optimize-origin-group`，转发协议→匹配传入请求，缓存→ **已禁用**

### 规则3： HealthProbeRewrite03

重写了Azure Front Door运行状况探测请求，以便它们以`/`而不是`/health/<domain>`的形式到达您的来源。 这使得Azure前门能够监控Edge优化可用性，而无需您的来源具有专门的运行状况端点。 停止评估：**于**。

![运行状况探测重写规则](/help/assets/optimize-at-edge/azure-front-door-ruleset-healthprobe.png)

**条件** （所有条件都必须匹配）：

* 请求URL路径： **以**&#x200B;开头`/health/`
* `x-fd-healthprobe`： **包含** `1`

**操作**：

* URL重写 — Source模式： `/health/`，目标： `/`
* 覆盖了`custom-origin-health` = `routed`的响应标头（诊断 — 可在验证后删除）
* 请求标头附加`user-agent` = ` AdobeEdgeOptimize/1.0`（添加前导空格 — Azure Front Door按原样附加值）
* 路由配置覆盖：源组→ `default-origin-group`，转发协议→匹配传入请求，缓存→ **已禁用**

## 步骤4：将规则集与路由关联

打开您的路由，滚动到底部的&#x200B;**规则**&#x200B;部分，然后从下拉列表中选择`EORouting`规则集。 如果您有现有的规则集，请使用&#x200B;**移至顶部**&#x200B;以在&#x200B;**#1**&#x200B;处放置`EORouting`。 在Edge中优化规则仅拦截代理流量，Edge优化环回请求 — 所有其他流量在不影响您的其他规则的情况下通过。 保存并等待传播（大约20分钟）。

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

{{verify-routing-status-in-ui}}

{{return-to-overview}}
