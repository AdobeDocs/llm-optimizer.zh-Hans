---
title: Optimize at Edge - Cloudflare (BYOCDN)
description: 了解如何在 LLM Optimizer 中为 Optimize at Edge 配置 Cloudflare BYOCDN。
feature: Opportunities
autotag-review: '2026-07-15T17:46:02.378Z'
TQID: 'https://experienceleague.adobe.com/ZgOX0yC8qyb13Y7YNCg3Y1A6Q3TSk9-mUQ8gthzQvLM'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: d1956731-2adb-4bb7-8301-2b239254ac72id: e0828736-236a-487b-a478-5a635455eadcid: e1b649f0-0a61-46e4-9082-64d5cb2576c6id: ef4e63f5-cb4d-462d-bf9a-1f617edf2a3a
subfeature_v2: id: d23587d6-14d6-4e3f-9ee1-cc18623832e1id: e06fae5f-830b-4222-a469-b5e148d36465
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: c1579802-ddd4-4214-8a91-97b2066abe11id: d095671a-1355-40aa-8b5f-06c33c68080bid: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: e36ee407933e2d3d56cadf1c9517f23f24d41d91
workflow-type: tm+mt
source-wordcount: 1919
ht-degree: 93%

---


# Cloudflare (BYOCDN)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。 人类访客和 SEO 机器人仍将照常从您的源站获得响应。 完成设置后，可在响应中查找头部 `x-edgeoptimize-request-id` 以测试配置是否成功。

**先决条件**

在设置Cloudflare Worker路由规则之前，请确保您具有：

* 具有 Cloudflare 帐户，并且在您的域中启用了 Workers。
* 可以在 Cloudflare 中访问您的域的 DNS 设置。
* 具有从 LLM Optimizer UI 检索到的 Edge Optimize API 密钥。 有关步骤，请参阅[检索您的 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#production-api-key)。
* （可选）要测试暂存路由，请参阅[暂存 API 密钥](/help/dashboards/optimize-at-edge/retrieve-api-keys.md#staging-api-key-optional)。

## 路由的工作方式

正确配置后，Cloudflare Worker 会截获代理式用户代理对您的域（例如 `www.example.com/page.html`）的请求，并将其路由到 Edge Optimize 后端。 后端请求包含必需的头部。

### 测试后端请求

您可以直接向 Edge Optimize 后端发出请求来验证这个路由。

```
curl -svo /dev/null https://live.edgeoptimize.net/page.html \
  -H 'x-forwarded-host: www.example.com' \
  -H 'x-edgeoptimize-url: /page.html' \
  -H 'x-edgeoptimize-api-key: $EDGE_OPTIMIZE_API_KEY' \
  -H 'x-edgeoptimize-config: LLMCLIENT=TRUE;'
```

### 必需标题

对 Edge Optimize 后端发出的请求中必须设置以下头部：

| 页眉 | 描述 | 示例 |
|--------|-------------|---------|
| `x-forwarded-host` | 请求的原始主机。 用于识别网站域。 | `www.example.com` |
| `x-edgeoptimize-url` | 请求的原始 URL 路径和查询字符串。 | `/page.html` 或 `/products?id=123` |
| `x-edgeoptimize-api-key` | Adobe 为您的域提供的 API 密钥。 | `your-api-key-here` |
| `x-edgeoptimize-config` | 用于区分缓存键的配置字符串。 | `LLMCLIENT=TRUE;` |

## 设置选项

有两种方法可以为 Edge Optimize 设置 Cloudflare Worker：

* [**选项 1：部署到 Cloudflare（推荐）**](#option-1-deploy-to-cloudflare)——自动创建新的 Worker，并提示您输入必需的环境变量和密码。 如果您当前没有此域的 Cloudflare Worker，请使用此选项。
* [**选项 2：手动设置**](#option-2-manual-setup)——分步说明了如何自行创建和配置 Worker。 如果您的域当前已配置了一个 Cloudflare Worker，您需要将 Edge Optimize 代码合并到您现有的 Worker 中（请参阅[步骤 2：添加 Worker 代码](#option-2-manual-setup)），或者如果您希望完全控制部署过程，请使用此选项。

无论您选择哪个选项，都必须手动将 Worker 关联到您的域——请参阅[步骤：给您的域添加路由](#add-a-route-to-your-domain)。

## 选项 1：部署到 Cloudflare

此选项使用&#x200B;**部署到 Cloudflare** 按钮自动创建 Worker，并在您的 Cloudflare 帐户中配置必需的环境变量和密码。 如果您要设置新的 Worker，这个方法可以让您快速开始使用。

>[!IMPORTANT]
>
>只有在您的域中当前&#x200B;**没有** Cloudflare Worker 的情况下才使用此选项。 如果您已经有一个 Worker，请使用[选项 2：手动设置](#option-2-manual-setup)将 Edge Optimize 路由逻辑添加到您现有的 Worker。

### 步骤1：部署工作程序

点击下面的按钮，将 Edge Optimize worker 部署到您的 Cloudflare 帐户：

[![部署到 Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/adobe/llmo-code-samples/tree/main/optimize-at-edge/cloudflare/automation)

### 第2步：填写部署表单

点击按钮，打开 Worker 设置页面。 按照下述方法填写表单：

![Cloudflare Workers 设置页面](/help/assets/optimize-at-edge/cloudflare-deploy-form.png)

1. **Git 帐户**——从下拉菜单中选择您的 GitHub 或 GitLab 帐户。 Cloudflare 会将 Worker 代码分支到您帐户中的存储库中。 如果未列出任何帐户，您可以直接从下拉列表中选择 **+ 新 GitHub 连接**&#x200B;或 **+ 新 GitLab 连接**，添加一个新连接。 有关详细信息，请参阅 [Cloudflare Git 集成指南](https://developers.cloudflare.com/workers/ci-cd/builds/git-integration/github-integration/)。

   ![Git 帐户下拉列表中显示了新的 GitHub 连接和新的 GitLab 连接选项](/help/assets/optimize-at-edge/cloudflare-git-connection.png)
2. **创建专用 Git 存储库**——勾选此项（默认）。
3. **项目名称**——就用 `edge-optimize-router` 或输入一个您自选的名称。
4. **EDGE_OPTIMIZE_API_KEY**——粘贴 Adobe 为您提供的 Edge Optimize API 密钥。 这个值存储为一个加密密码。
5. **EDGE_OPTIMIZE_TARGET_HOST**——输入您网站的域，不含协议（例如 `www.example.com`）。
6. **构建命令**——留空。
7. **部署命令**——就用 `npm run deploy`（已预填充）。
8. **非生产分支的版本**——不勾选。 这是开发人员工作流功能，这个部署中不需要。
9. 点击&#x200B;**创建并部署**。

Worker 部署完成后，继续[添加指向您的域名的路由](#add-a-route-to-your-domain)，将 Worker 与您的域相关联。 路由未自动配置，必须手动完成。

## 选项 2：手动设置

按照这些步骤手动创建和配置 Worker。

### 步骤1：创建Cloudflare工作器

1. 登录您的 Cloudflare 仪表板。
2. 在侧边栏中导航到 **Workers 和页面**。
3. 点击&#x200B;**创建应用程序**，然后点击&#x200B;**创建 Worker**。
4. 为您的 Worker 命名（例如 `edge-optimize-router`）。
5. 点击&#x200B;**部署**，用默认代码创建 Worker。

![Cloudflare Workers 仪表板](/help/assets/optimize-at-edge/cloudflare-workers-dashboard.png)

### 第2步：添加工作人员代码

创建辅助进程后，单击&#x200B;**编辑代码**，并将默认代码替换为[worker.js](https://github.com/adobe/llmo-code-samples/blob/main/optimize-at-edge/cloudflare/automation/src/worker.js)中的代码。 如果您已经拥有现有的Cloudflare Worker，请将该代码与现有工作程序代码合并，而不是完全替换。

点击&#x200B;**保存并部署**，发布 Worker。

### 步骤3：配置环境变量和密钥

环境变量可以安全地存储敏感配置，例如您的 API 密钥。

1. 在 Worker 的设置中，导航前往&#x200B;**设置** > **变量**。
2. 在&#x200B;**环境变量**&#x200B;中点击&#x200B;**添加变量**。
3. 添加以下变量：

   | 变量名称 | 描述 | 必需 |
   |---------------|-------------|----------|
   | `EDGE_OPTIMIZE_API_KEY` | Adobe 提供的 Edge Optimize API 密钥。 | 是 |
   | `EDGE_OPTIMIZE_TARGET_HOST` | Edge Optimize 请求的目标主机（作为 `x-forwarded-host` 头部发送）和故障转移的源域。 域名不能包含协议头（例如 `www.example.com`，不能是 `https://www.example.com`）。 | 是 |

4. 对于 API 密钥，点击&#x200B;**加密**，以安全存储。
5. 点击&#x200B;**保存并部署**。

![Cloudflare 环境变量](/help/assets/optimize-at-edge/cloudflare-env-variables.png)

## 添加指向您的域名的路由 {#add-a-route-to-your-domain}

无论您使用哪个设置选项，都必须手动将 Worker 与您的域相关联。 此步骤会激活您流量中的 Worker。

1. 前往 Worker 的&#x200B;**设置** > **触发器**。
2. 在&#x200B;**路由**&#x200B;中点击&#x200B;**添加路由**。
3. 输入您的域模式（例如 `www.example.com/*` 或 `example.com/*`）。
4. 从下拉列表中选择您的区域。
5. 单击&#x200B;**保存**。

或者，您也可以在区域层面配置路由：

1. 在 Cloudflare 中导航到您的域。
2. 前往 **Workers 路由**。
3. 点击&#x200B;**添加路由**，指定模式和 Worker。

![Cloudflare Worker 路由](/help/assets/optimize-at-edge/cloudflare-worker-routes.png)

### 验证故障转移行为

如果 Edge Optimize 不可用或返回错误，Worker 就会自动将故障转移到您的源站。 故障转移响应包含 `x-edgeoptimize-fo` 头部：

```
< HTTP/2 200
< x-edgeoptimize-fo: 1
```

您可以在 Cloudflare Worker 日志中监控故障转移事件，以解决问题。

### 了解工作程序逻辑

Cloudflare Worker 会实施以下逻辑：

1. **用户代理检测：**&#x200B;检查传入请求的用户代理是否与任何已定义的代理式机器人匹配（不区分大小写）。

2. **路径目标：**&#x200B;可选择根据目标路径筛选请求。 默认情况下，会将所有 HTML 页面（以 `/`、无扩展名或以 `.html` 结尾的URL）路由。 您可以使用 `TARGETED_PATHS` 数组指定特定路径。

3. **循环保护：** `x-edgeoptimize-request` 头部可阻止无限循环。 如果 Edge Optimize 将请求送回到您的源站，这个头部设置为 `"1"`，并且 Worker 传递请求时未将其路由回到 Edge Optimize。

4. **头部安全性：**&#x200B;在设置 Edge Optimize 头部之前，Worker 会从传入请求中移除任何现有的 `x-edgeoptimize-*` 头部，以防止头部注入攻击。

5. **头部映射：** Worker 设置 Edge Optimize 所需的头部：
   * `x-forwarded-host`——识别原始站点域。
   * `x-edgeoptimize-url`——保留原始请求路径和查询字符串。
   * `x-edgeoptimize-api-key`——通过 Edge Optimize 对请求进行身份验证。
   * `x-edgeoptimize-config`——提供缓存键配置。

6. **故障转移逻辑：**&#x200B;如果 Edge Optimize 返回任何错误状态代码（4XX 客户端错误或 5XX 服务器错误），或者由于网络出错而请求失败，Worker 就会使用 `EDGE_OPTIMIZE_TARGET_HOST` 自动将故障转移到您的源站。 故障转移响应包含 `x-edgeoptimize-fo: 1` 头部，表示已进行了故障转移。

7. **重定向处理：**`redirect: "manual"` 选项可确保来自 Edge Optimize 的重定向响应被传递到客户端，而无需 Worker 跟随这些响应。

## 自定义配置

您可以通过更改代码顶部的配置常量来自定义 Worker 行为：

### 代理机器人列表

更改 `AGENTIC_BOTS` 数组，以添加或移除用户代理：

```javascript
const AGENTIC_BOTS = [
  'AdobeEdgeOptimize-AI',
  'ChatGPT-User',
  'GPTBot',
  'OAI-SearchBot',
  'PerplexityBot',
  'Perplexity-User',
  'ClaudeBot',
  'Claude-User',
  'Claude-SearchBot',
  // Add additional user agents as needed
];
```

### 目标路径

默认情况下，所有 HTML 页面都会被路由到 Edge Optimize。 要将路由限制到特定路径，请更改 `TARGETED_PATHS` 数组：

```javascript
// Route all HTML pages (default)
const TARGETED_PATHS = null;

// Or specify exact paths to route
const TARGETED_PATHS = ['/', '/page.html', '/products', '/about-us'];
```

### 故障转移配置

默认情况下，如果 Edge Optimize 返回任何 4XX 或 5XX 错误，Worker 都会触发故障转移。 自定义这个行为：

```javascript
// Default: failover on any 4XX or 5XX error
const FAILOVER_ON_4XX = true;
const FAILOVER_ON_5XX = true;

// Failover only on 5XX server errors (not 4XX client errors)
const FAILOVER_ON_4XX = false;
const FAILOVER_ON_5XX = true;

// Disable automatic failover (not recommended)
const FAILOVER_ON_4XX = false;
const FAILOVER_ON_5XX = false;
```

### 重要注意事项

* **故障转移行为：**&#x200B;如果 Edge Optimize 返回任何错误（4XX 或 5XX 状态代码），或者由于网络出错而请求失败，Worker 就会自动将故障转移到您的源站。 故障转移使用 `EDGE_OPTIMIZE_TARGET_HOST` 作为源域（类似于 Fastly 的 `F_Default_Origin` 或 CloudFront 的 `Default_Origin`）。 故障转移响应包含 `x-edgeoptimize-fo: 1` 头部，可用于监控和调试。

* **缓存：**&#x200B;默认情况下，Cloudflare 会根据 URL 缓存响应。 由于代理式流量获得的内容与人工流量不同，因此请确保您的缓存配置考虑到这一点。 请考虑使用缓存 API 或缓存头来区分缓存的内容。 `x-edgeoptimize-config` 头部应包含在您的缓存键中。

* **速率限制：**&#x200B;监控您的 Edge Optimize 使用情况，需要时考虑为代理式流量实施速率限制。

* **测试：**&#x200B;将配置部署到生产环境之前，始终在暂存环境中进行测试。 验证代理式流量和人类流量都按预期运行。 通过模拟 Edge Optimize 错误来测试故障转移行为。

* **日志记录：**&#x200B;启用 Cloudflare Workers 日志记录，以监控请求，解决问题。 导航到 **Workers** > **您的 Worker** > **日志**，查看实时日志。 Worker 会记录故障转移事件，用于进行调试。

## 故障排除

| 问题 | 可能的原因 | 解决方案 |
|-------|----------------|----------|
| 响应中没有 `x-edgeoptimize-request-id` 头部 | Worker 路由不匹配，或用户代理不在代理式机器人列表中。 | 验证您的路由模式是否与请求 URL 匹配。 检查用户代理是否在 `AGENTIC_BOTS` 数组中。 |
| 来自 Edge Optimize 的 401 或 403 错误 | API 密钥无效或缺失。 | 验证是否在环境变量和密码中正确设置了 `EDGE_OPTIMIZE_API_KEY`。 请联系 Adobe 确认您的 API 密钥为活跃状态。 |
| 无限重定向或循环 | 循环保护头未经过正确设置或检查。 | 确保已设置了 `x-edgeoptimize-request` 头部检查。 |
| 人类流量受到影响 | Worker 路由逻辑过于宽泛。 | 验证用户代理匹配逻辑正确且不区分大小写。 检查 `TARGETED_PATHS` 已正确配置。 |
| 响应时间慢 | Edge Optimize 后端的网络延迟。 | 第一个请求会发生这个现象；后续请求会缓存在 Edge Optimize 中。 |
| 响应中的 `x-edgeoptimize-fo: 1` 头部 | Edge Optimize 返回了错误，故障转移到了源站。 | 查看 Cloudflare Workers 日志，了解特定的错误代码。 通过 Adobe 验证 Edge Optimize 服务状态。 |
| 故障转移不起作用 | 故障转移标志已禁用，或故障转移逻辑中出错。 | 验证 `FAILOVER_ON_4XX` 和 `FAILOVER_ON_5XX` 都已设置为 `true`。 检查 Worker 日志中的错误消息。 |
| 某些路径未优化 | 路径与目标路径或 HTML 页面模式不匹配。 | 验证路径位于 `TARGETED_PATHS`（如果已指定）并与 HTML 页面正则表达式模式匹配。 |
| 因主机无效，请求失败 | `EDGE_OPTIMIZE_TARGET_HOST` 包含协议（例如 `https://`）。 | 仅使用不带协议的域名（例如 `example.com`，而不是 `https://example.com`）。 |
| 故障转移过程中出现 530 错误 | Cloudflare 无法连接到源站，或者故障转移请求的头部无效。 | 确保故障转移功能会移除 Edge Optimize 头部。 验证您的源站可访问并且 DNS 配置正确。 |

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
