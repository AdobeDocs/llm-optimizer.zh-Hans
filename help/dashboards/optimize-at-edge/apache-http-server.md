---
title: Optimize at Edge - Apache HTTP 服务器
description: 了解如何在 LLM Optimizer 中为 Optimize at Edge 配置 Apache HTTP 服务器（自托管反向代理）BYOCDN。
feature: Opportunities
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
subfeature_v2:
  - id: d23587d6-14d6-4e3f-9ee1-cc18623832e1
source-git-commit: d7e723161836027dcdde931378f5d0f776a1ecfc
workflow-type: ht
source-wordcount: 585
ht-degree: 100%

---


# Apache HTTP 服务器

此配置适用 Apache HTTP 服务器作为源站（自托管设置，**不使用** AEM Dispatcher）前的反向代理的情况。它会将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。人类访客和 SEO 机器人仍将照常从您的源站获得响应。 完成设置后，可在响应中查找头部 `x-edgeoptimize-request-id` 以测试配置是否成功。

此集成是一组 Apache 原生 `Include` 文件，没有需要部署的代码或 Worker。您下载三个文件，设置 API 密钥，然后在您的虚拟主机中添加两行 `Include`。

**先决条件**

在配置 Apache 路由规则之前，请确保您已具备以下条件：

* Apache HTTP 服务器 2.4 或更高版本，并启用了以下模块：`proxy`、`proxy_http`、`ssl`、`rewrite`、`headers`、`env` 和 `setenvif`。
* 可以访问您的 Apache 配置（站点的 `<VirtualHost>`），能够重新加载 Apache。
* 具有从 LLM Optimizer UI 检索到的 Edge Optimize API 密钥。 有关步骤，请参阅[检索您的 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#production-api-key)。
* （可选）要测试暂存路由，请参阅[暂存 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#staging-api-key-optional)。

## 配置

**1. 下载配置文件**

从 [Optimize at Edge 代码示例存储库](https://github.com/adobe/llmo-code-samples/tree/main/optimize-at-edge/apache)下载三个 Edge Optimize include 文件，然后将它们放在 Apache 服务器的一个目录中（例如，`conf/oae/`）：

| 文件 | 用途 |
|------|---------|
| `oae-routing.conf` | 检测 AI 机器人，注入 Edge Optimize 标头，将 HTML 页面请求路由到后端，并设置缓存隔离和故障转移。 |
| `oae-failover.conf` | 如果 Edge Optimize 返回错误，请根据您的源站重播原始请求。 |
| `domains.conf` | 在每个域中启用 Optimize at Edge，保留您的 API 密钥。 |

您无需修改 `oae-routing.conf` 或 `oae-failover.conf`，按原样使用它们。

**2. 启用您的域，并设置 API 密钥 (`domains.conf`)**

编辑 `domains.conf`，为您要启用的每个域添加一行。将主机替换为您的域，将 `YOUR_API_KEY` 替换为 LLM Optimizer UI 中的密钥。未列出的域会按原样路由至源站，因此您可以逐个启用域名。

```
SetEnvIfExpr "%{HTTP_HOST} =~ m#(?i)^(www\.)?example\.com(:\d+)?$#" OAE_DOMAIN_ENABLED=1 OAE_API_KEY=YOUR_API_KEY
```

**3. 在您的虚拟主机中包含这些文件**

在您现有的 `<VirtualHost *:443>` 中添加这两行 `Include`。路由文件在您的重写和 `ProxyPass` 规则&#x200B;**之前**，故障转移文件位于&#x200B;**其后**。在下面的示例中，标记为 `#NEWLINE` 的行是您为 Optimize at Edge 添加的唯一几行，其他所有行（`ServerName`、`ProxyPass` 及其他）都是您现有的未更改的配置。

```
Define OAE_CONF_DIR conf/oae                       #NEWLINE  directory holding the OAE include files

<VirtualHost *:443>
    ServerName www.example.com

    Include "${OAE_CONF_DIR}/oae-routing.conf"     #NEWLINE  OAE routing — BEFORE your Rewrite & ProxyPass rules

    # --- your existing rewrite rules and ProxyPass to origin ---
    ProxyPass        "/" "https://www.example.com/"
    ProxyPassReverse "/" "https://www.example.com/"

    Include "${OAE_CONF_DIR}/oae-failover.conf"    #NEWLINE  OAE failover — AFTER your ProxyPass rules
</VirtualHost>
```

**4. 重新加载 Apache**

验证配置并重新加载 Apache 以应用更改。

>[!NOTE]
>
>机器人优化的响应及人类响应会自动保留在单独的缓存条目中（路由文件集 `Vary: x-edgeoptimize-config`）。如果您的 Apache 已使用 `mod_cache`，请确保它具有 `CacheQuickHandler Off`，以便在设置 Edge Optimize 标头后执行缓存查找。

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
| `x-edgeoptimize-fo` | 仅在发生故障转移的情况下存在（值：`1`） | 不存在 |

{{verify-routing-status-in-ui}}

{{return-to-overview}}
