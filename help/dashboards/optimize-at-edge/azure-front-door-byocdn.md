---
title: Optimize at Edge - Azure Front Door (BYOCDN)
description: 了解如何在 LLM Optimizer 中为 Azure Front Door BYOCDN 配置 Optimize at Edge。
feature: Opportunities
autotag-review: '2026-07-15T17:40:54.797Z'
TQID: 'https://experienceleague.adobe.com/fe-kultqzWQdRdcUjzfNs21UpL6m5zcoAmaQyMMv5kk'
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
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: ht
source-wordcount: 768
ht-degree: 100%

---


# Azure Front Door (BYOCDN)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。 人类访客和 SEO 机器人仍将照常从您的源站获得响应。 完成设置后，可在响应中查找头部 `x-edgeoptimize-request-id` 以测试配置是否成功。

Azure Front Door 不支持在边缘节点运行自定义代码。路由通过&#x200B;**规则集**&#x200B;和专用于 Edge Optimize 的&#x200B;**源站组**&#x200B;共同进行配置。故障切换由 Azure Front Door 基于优先级的源站组健康探测机制负责处理。

**先决条件**

在配置 Azure Front Door 路由规则之前，请确保您已具备以下条件：

* 拥有 Azure Front Door 配置文件的访问权限。
* 具有从 LLM Optimizer UI 检索到的 Edge Optimize API 密钥。 有关步骤，请参阅[检索您的 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#production-api-key)。
* （可选）要测试暂存路由，请参阅[暂存 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#staging-api-key-optional)。

## 步骤 1：为 Edge Optimize 创建源站组

您的 Azure Front Door 配置文件中已包含一个指向源站的默认源站组。请为 Edge Optimize 创建一个&#x200B;**新**&#x200B;的源站组：

* **名称：**`edge-optimize-origin-group`
* **源站（基于优先级的故障切换）：**
  * **优先级 1** — `live.edgeoptimize.net`（源站主机标头：`live.edgeoptimize.net`）
  * **优先级 2** — 您的域名终结点（例如 `www.example.com`）。 此配置用于故障切换：如果 Edge Optimize 不可用，请求将路由到您的域名，然后重新进入 Azure Front Door，并由默认源站提供服务。
* **健康探测：****已启用**
  * 路径：`/health/<your-domain>`（例如，`/health/www.example.com`）
  * 协议：HTTPS
  * 间隔：225 秒
* **会话关联：****已禁用**
* **证书主题名称验证：****已启用**

![包含两个基于优先级的源站及健康探测的 Edge Optimize 源站组](/help/assets/optimize-at-edge/azure-front-door-origin-group.png)

>[!NOTE]
>
>在 Azure 门户中，`edge-optimize-origin-group` 源站组会显示 **&quot;Unassociated&quot;**（未关联）警告。这是预期行为，因为该源站组是通过规则集的路由覆盖进行引用，而不是直接由路由引用。

## 步骤 2：配置路由

通常，在创建 Azure Front Door 配置文件时会自动创建一条默认路由。规则集（步骤 3）会针对代理式流量覆盖源站组配置，因此无需为 Edge Optimize 单独创建路由。

## 步骤 3：创建规则集

依次进入&#x200B;**规则集** > **添加规则集**，并将其命名为 `EORouting`。 按照以下顺序添加三个规则。

![显示请求头清理和机器人路由规则的 EORouting 规则集](/help/assets/optimize-at-edge/azure-front-door-ruleset-routing.png)

### 规则 1：StripIncomingEOHeaders01

删除传入请求中的 Edge Optimize 请求头，以防止伪造请求。无需设置条件，适用于所有请求。停止继续评估：**关**。

**操作**，删除以下每个请求头：

* `x-edgeoptimize-url`
* `x-edgeoptimize-config`
* `x-edgeoptimize-api-key`
* `x-edgeoptimize-fetcher-key`

### 规则 2：EOGPTBotRootGET03

将访问 HTML 页面路径的机器人请求路由至 Edge Optimize。停止继续评估：**开启**。

**条件**（必须全部满足）：

* 请求方法：**等于** `GET`
* 请求路径：**正则表达式**`(^$|^.*/$|(^|.*/)[^./]+$|^.*\.html$)`（匹配网站根路径、以 `/` 结尾的路径、无扩展名的页面路径以及 `.html` 路径）
* User-Agent：**包含以下任意值：** `chatgpt-user`、`gptbot`、`oai-searchbot`、`adobeedgeoptimize-ai`、`perplexitybot`、`perplexity-user`、`claudebot`、`claude-user`、`claude-searchbot`。 将字符串转换设置为&#x200B;**转换为小写**。
* `x-edgeoptimize-monitor`：**不包含** `1`
* `x-edgeoptimize-request`：**不包含** `failover`或 `1`

**操作**：

* 覆盖请求头 `x-edgeoptimize-url` = `/{url_path}?{query_string}`
* 覆盖请求头 `x-edgeoptimize-config` = `LLMCLIENT=TRUE;`
* 覆盖请求头 `x-edgeoptimize-api-key` = `YOUR_API_KEY`
* 覆盖请求头 `x-edgeoptimize-monitor` = `1`
* 覆盖路由配置：源站组 → `edge-optimize-origin-group`，转发协议 → 与传入请求保持一致，缓存 → **禁用**

### 规则 3：HealthProbeRewrite03

重写 Azure Front Door 的健康探测请求，使其访问您的源站时使用 `/`，而不是 `/health/<domain>`。这样，Azure Front Door 无需在您的源站配置专用健康检查端点，即可监控 Edge Optimize 的可用性。停止继续评估：**开启**。

![健康探测重写规则](/help/assets/optimize-at-edge/azure-front-door-ruleset-healthprobe.png)

**条件**（必须全部满足）：

* 请求 URL 路径：**以** `/health/`开头
* `x-fd-healthprobe`：**包含** `1`

**操作**：

* URL 重写，源路径：`/health/`；目标路径：`/`
* 覆盖响应头 `custom-origin-health` = `routed`（用于诊断，验证完成后可移除）
* 向请求头 `user-agent` 追加 ` AdobeEdgeOptimize/1.0`（注意前面保留一个空格，Azure Front Door 会按原样追加该值）
* 覆盖路由配置：源站组 → `default-origin-group`，转发协议 → 与传入请求保持一致，缓存 → **禁用**

## 步骤 4：将规则集关联到路由

打开您的路由，滚动到页面底部的&#x200B;**规则**&#x200B;部分，然后从下拉列表中选择 `EORouting` 规则集。如果您已有其他规则集，请使用&#x200B;**移至顶部**&#x200B;将 `EORouting` 调整到 **#1** 的位置。Optimize at Edge 规则仅会拦截代理式流量和 Edge Optimize 回环请求，其他所有流量都会不受影响地继续经过您的其他规则进行处理。 保存配置，并等待其生效（约需 20 分钟）。

## 允许 Optimize at Edge 通过防火墙规则（可选）

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
