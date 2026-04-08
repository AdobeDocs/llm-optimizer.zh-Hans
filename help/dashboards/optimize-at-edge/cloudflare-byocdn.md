---
title: Optimize at Edge - Cloudflare (BYOCDN)
description: 了解如何在 LLM Optimizer 中为 Optimize at Edge 配置 Cloudflare BYOCDN。
feature: Opportunities
source-git-commit: da789100d814004687de2f46e18a295671dec4b8
workflow-type: tm+mt
source-wordcount: '1439'
ht-degree: 95%

---


# Cloudflare (BYOCDN)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。 人类访客和 SEO 机器人仍将照常从您的源站获得响应。 完成设置后，可在响应中查找头部 `x-edgeoptimize-request-id` 以测试配置是否成功。

**先决条件**

设置 Cloudflare Worker 路由规则之前，请确保您：

* 具有 Cloudflare 帐户，并且在您的域中启用了 Workers。
* 可以在 Cloudflare 中访问您的域的 DNS 设置。
* 完成了 LLM Optimizer 的加入过程。
* 已将内容传递网络日志转发到 LLM Optimizer。
* 具有从 LLM Optimizer UI 检索到的 Edge Optimize API 密钥。
* （可选）如果首先在暂存主机名上测试路由，则使用暂存Edge优化API密钥。

{{retrieve-byocdn-api-key}}

{{retrieve-staging-edge-optimize-api-key}}

**如何进行路由**

正确配置后，Cloudflare Worker 会截获代理式用户代理对您的域（例如 `www.example.com/page.html`）的请求，并将其路由到 Edge Optimize 后端。 后端请求包含必需的头部。

**测试后端请求**

您可以直接向 Edge Optimize 后端发出请求来验证这个路由。

```
curl -svo /dev/null https://live.edgeoptimize.net/page.html \
  -H 'x-forwarded-host: www.example.com' \
  -H 'x-edgeoptimize-url: /page.html' \
  -H 'x-edgeoptimize-api-key: $EDGE_OPTIMIZE_API_KEY' \
  -H 'x-edgeoptimize-config: LLMCLIENT=TRUE;'
```

**必需的头部**

对 Edge Optimize 后端发出的请求中必须设置以下头部：

| 页眉 | 描述 | 示例 |
|--------|-------------|---------|
| `x-forwarded-host` | 请求的原始主机。 用于识别网站域。 | `www.example.com` |
| `x-edgeoptimize-url` | 请求的原始 URL 路径和查询字符串。 | `/page.html` 或 `/products?id=123` |
| `x-edgeoptimize-api-key` | Adobe 为您的域提供的 API 密钥。 | `your-api-key-here` |
| `x-edgeoptimize-config` | 用于区分缓存键的配置字符串。 | `LLMCLIENT=TRUE;` |

**步骤 1：创建 Cloudflare Worker**

1. 登录您的 Cloudflare 仪表板。
2. 在侧边栏中导航到 **Workers 和页面**。
3. 点击&#x200B;**创建应用程序**，然后点击&#x200B;**创建 Worker**。
4. 为您的 Worker 命名（例如 `edge-optimize-router`）。
5. 点击&#x200B;**部署**，用默认代码创建 Worker。

![Cloudflare Workers 仪表板](/help/assets/optimize-at-edge/cloudflare-workers-dashboard.png)

**步骤 2：添加 Worker 代码**

创建 Worker 后，点击&#x200B;**编辑代码**，将默认代码替换为以下内容：

```javascript
/**
 * Edge Optimize BYOCDN - Cloudflare Worker
 *
 * This worker routes requests from agentic bots (AI/LLM user agents) to the
 * Edge Optimize backend service for optimized content delivery.
 *
 * Features:
 * - Routes agentic bot traffic to Edge Optimize backend
 * - Failover to origin on Edge Optimize errors (any 4XX or 5XX errors)
 * - Loop protection to prevent infinite redirects
 * - Human visitors and SEO bots are served from the origin as usual
 */

// List of agentic bot user agents to route to Edge Optimize
const AGENTIC_BOTS = [
  'AdobeEdgeOptimize-AI',
  'ChatGPT-User',
  'GPTBot',
  'OAI-SearchBot',
  'PerplexityBot',
  'Perplexity-User'
];

// Targeted paths for Edge Optimize routing
// Set to null to route all HTML pages, or specify an array of paths
const TARGETED_PATHS = null; // e.g., ['/', '/page.html', '/products']

// Failover configuration
// Failover on any 4XX client error or 5XX server error from Edge Optimize
const FAILOVER_ON_4XX = true; // Failover on any 4XX error (400-499)
const FAILOVER_ON_5XX = true; // Failover on any 5XX error (500-599)

export default {
  async fetch(request, env, ctx) {
    return await handleRequest(request, env, ctx);
  },
};

async function handleRequest(request, env, ctx) {
  const url = new URL(request.url);
  const userAgent = request.headers.get("user-agent")?.toLowerCase() || "";

  // Check if request is already processed (loop protection)
  const isEdgeOptimizeRequest = !!request.headers.get("x-edgeoptimize-request");

  // Construct the original path and query string
  const pathAndQuery = `${url.pathname}${url.search}`;

  // Check if the path matches HTML pages (no extension or .html extension)
  const isHtmlPage = /(?:\/[^./]+|\.html|\/)$/.test(url.pathname);

  // Check if path is in targeted paths (if specified)
  const isTargetedPath = TARGETED_PATHS === null
    ? isHtmlPage
    : (isHtmlPage && TARGETED_PATHS.includes(url.pathname));

  // Check if user agent is an agentic bot
  const isAgenticBot = AGENTIC_BOTS.some((ua) => userAgent.includes(ua.toLowerCase()));

  // Route to Edge Optimize if:
  // 1. Request is NOT already from Edge Optimize (prevents infinite loops)
  // 2. User agent matches one of the agentic bots
  // 3. Path is targeted for optimization
  if (!isEdgeOptimizeRequest && isAgenticBot && isTargetedPath) {

    // Build the Edge Optimize request URL
    const edgeOptimizeURL = `https://live.edgeoptimize.net${pathAndQuery}`;

    // Clone and modify headers for the Edge Optimize request
    const edgeOptimizeHeaders = new Headers(request.headers);

    // Remove any existing Edge Optimize headers for security
    edgeOptimizeHeaders.delete("x-edgeoptimize-api-key");
    edgeOptimizeHeaders.delete("x-edgeoptimize-url");
    edgeOptimizeHeaders.delete("x-edgeoptimize-config");

    // x-forwarded-host: The original site domain
    // Use environment variable if set, otherwise use the request host
    edgeOptimizeHeaders.set("x-forwarded-host", env.EDGE_OPTIMIZE_TARGET_HOST ?? url.host);

    // x-edgeoptimize-api-key: Your Adobe-provided API key
    edgeOptimizeHeaders.set("x-edgeoptimize-api-key", env.EDGE_OPTIMIZE_API_KEY);

    // x-edgeoptimize-url: The original request URL path and query
    edgeOptimizeHeaders.set("x-edgeoptimize-url", pathAndQuery);

    // x-edgeoptimize-config: Configuration for cache key differentiation
    edgeOptimizeHeaders.set("x-edgeoptimize-config", "LLMCLIENT=TRUE;");

    try {
      // Send request to Edge Optimize backend
      const edgeOptimizeResponse = await fetch(new Request(edgeOptimizeURL, {
        headers: edgeOptimizeHeaders,
        redirect: "manual", // Preserve redirect responses from Edge Optimize
      }), {
        cf: {
          cacheEverything: true, // Enable caching based on origin's cache-control headers
        },
      });

      // Check if we need to failover to origin
      const status = edgeOptimizeResponse.status;
      const is4xxError = FAILOVER_ON_4XX && status >= 400 && status < 500;
      const is5xxError = FAILOVER_ON_5XX && status >= 500 && status < 600;

      if (is4xxError || is5xxError) {
        console.log(`Edge Optimize returned ${status}, failing over to origin`);
        return await failoverToOrigin(request, env, url);
      }

      // Return the Edge Optimize response
      return edgeOptimizeResponse;

    } catch (error) {
      // Network error or timeout - failover to origin
      console.log(`Edge Optimize request failed: ${error.message}, failing over to origin`);
      return await failoverToOrigin(request, env, url);
    }
  }

  // For all other requests (human users, SEO bots), pass through unchanged
  return fetch(request);
}

/**
 * Failover to origin server when Edge Optimize returns an error
 * @param {Request} request - The original request
 * @param {Object} env - Environment variables
 * @param {URL} url - Parsed URL object
 */
async function failoverToOrigin(request, env, url) {
  // Build origin URL
  const originURL = `${url.protocol}//${env.EDGE_OPTIMIZE_TARGET_HOST}${url.pathname}${url.search}`;

  // Prepare headers - clean Edge Optimize headers and add loop protection
  const originHeaders = new Headers(request.headers);
  originHeaders.set("Host", env.EDGE_OPTIMIZE_TARGET_HOST);
  originHeaders.delete("x-edgeoptimize-api-key");
  originHeaders.delete("x-edgeoptimize-url");
  originHeaders.delete("x-edgeoptimize-config");
  originHeaders.delete("x-forwarded-host");
  originHeaders.set("x-edgeoptimize-request", "fo");

  // Create and send origin request
  const originRequest = new Request(originURL, {
    method: request.method,
    headers: originHeaders,
    body: request.body,
    redirect: "manual",
  });

  const originResponse = await fetch(originRequest);

  // Add failover marker header to response
  const modifiedResponse = new Response(originResponse.body, {
    status: originResponse.status,
    statusText: originResponse.statusText,
    headers: originResponse.headers,
  });
  modifiedResponse.headers.set("x-edgeoptimize-fo", "1");
  return modifiedResponse;
}
```

点击&#x200B;**保存并部署**，发布 Worker。

![Cloudflare Worker 代码编辑器](/help/assets/optimize-at-edge/cloudflare-worker-editor.png)

**步骤 3：配置环境变量**

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

**步骤 4：添加指向您的域名的路由**

要在您的域中激活 Worker，请执行以下操作：

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

**验证故障转移行为**

如果 Edge Optimize 不可用或返回错误，Worker 就会自动将故障转移到您的源站。 故障转移响应包含 `x-edgeoptimize-fo` 头部：

```
< HTTP/2 200
< x-edgeoptimize-fo: 1
```

您可以在 Cloudflare Worker 日志中监控故障转移事件，以解决问题。

**了解 Worker 逻辑**

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

**自定义配置**

您可以通过更改代码顶部的配置常量来自定义 Worker 行为：

**代理式机器人列表**

更改 `AGENTIC_BOTS` 数组，以添加或移除用户代理：

```javascript
const AGENTIC_BOTS = [
  'AdobeEdgeOptimize-AI',
  'ChatGPT-User',
  'GPTBot',
  'OAI-SearchBot',
  'PerplexityBot',
  'Perplexity-User',
  // Add additional user agents as needed
  'ClaudeBot',
  'Anthropic-AI'
];
```

**目标路径**

默认情况下，所有 HTML 页面都会被路由到 Edge Optimize。 要将路由限制到特定路径，请更改 `TARGETED_PATHS` 数组：

```javascript
// Route all HTML pages (default)
const TARGETED_PATHS = null;

// Or specify exact paths to route
const TARGETED_PATHS = ['/', '/page.html', '/products', '/about-us'];
```

**故障转移配置**

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

**重要注意事项**

* **故障转移行为：**&#x200B;如果 Edge Optimize 返回任何错误（4XX 或 5XX 状态代码），或者由于网络出错而请求失败，Worker 就会自动将故障转移到您的源站。 故障转移使用 `EDGE_OPTIMIZE_TARGET_HOST` 作为源域（类似于 Fastly 的 `F_Default_Origin` 或 CloudFront 的 `Default_Origin`）。 故障转移响应包含 `x-edgeoptimize-fo: 1` 头部，可用于监控和调试。

* **缓存：**&#x200B;默认情况下，Cloudflare 会根据 URL 缓存响应。 由于代理式流量获得的内容与人工流量不同，因此请确保您的缓存配置考虑到这一点。 请考虑使用缓存 API 或缓存头来区分缓存的内容。 `x-edgeoptimize-config` 头部应包含在您的缓存键中。

* **速率限制：**&#x200B;监控您的 Edge Optimize 使用情况，需要时考虑为代理式流量实施速率限制。

* **测试：**&#x200B;将配置部署到生产环境之前，始终在暂存环境中进行测试。 验证代理式流量和人类流量都按预期运行。 通过模拟 Edge Optimize 错误来测试故障转移行为。

* **日志记录：**&#x200B;启用 Cloudflare Workers 日志记录，以监控请求，解决问题。 导航到 **Workers** > **您的 Worker** > **日志**，查看实时日志。 Worker 会记录故障转移事件，用于进行调试。

**疑难解答**

| 问题 | 可能的原因 | 解决方案 |
|-------|----------------|----------|
| 响应中没有 `x-edgeoptimize-request-id` 头部 | Worker 路由不匹配，或用户代理不在代理式机器人列表中。 | 验证您的路由模式是否与请求 URL 匹配。 检查用户代理是否在 `AGENTIC_BOTS` 数组中。 |
| 来自 Edge Optimize 的 401 或 403 错误 | API 密钥无效或缺失。 | 验证是否在环境变量中正确设置了 `EDGE_OPTIMIZE_API_KEY`。 请联系 Adobe 确认您的 API 密钥为活跃状态。 |
| 无限重定向或循环 | 循环保护头未经过正确设置或检查。 | 确保已设置了 `x-edgeoptimize-request` 头部检查。 |
| 人类流量受到影响 | Worker 路由逻辑过于宽泛。 | 验证用户代理匹配逻辑正确且不区分大小写。 检查 `TARGETED_PATHS` 已正确配置。 |
| 响应时间慢 | Edge Optimize 后端的网络延迟。 | 第一个请求会发生这个现象；后续请求会缓存在 Edge Optimize 中。 |
| 响应中的 `x-edgeoptimize-fo: 1` 头部 | Edge Optimize 返回了错误，故障转移到了源站。 | 查看 Cloudflare Workers 日志，了解特定的错误代码。 通过 Adobe 验证 Edge Optimize 服务状态。 |
| 故障转移不起作用 | 故障转移标志已禁用，或故障转移逻辑中出错。 | 验证 `FAILOVER_ON_4XX` 和 `FAILOVER_ON_5XX` 都已设置为 `true`。 检查 Worker 日志中的错误消息。 |
| 某些路径未优化 | 路径与目标路径或 HTML 页面模式不匹配。 | 验证路径位于 `TARGETED_PATHS`（如果已指定）并与 HTML 页面正则表达式模式匹配。 |
| 因主机无效，请求失败 | `EDGE_OPTIMIZE_TARGET_HOST` 包含协议（例如 `https://`）。 | 仅使用不带协议的域名（例如 `example.com`，而不是 `https://example.com`）。 |
| 故障转移过程中出现 530 错误 | Cloudflare 无法连接到源站，或者故障转移请求的头部无效。 | 确保故障转移功能会移除 Edge Optimize 头部。 验证您的源站可访问并且 DNS 配置正确。 |

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

如果您使用LLM Optimizer中的暂存主机名和暂存API密钥，请使用&#x200B;**暂存** API密钥在&#x200B;**暂存**&#x200B;区域上部署相同的Worker逻辑。 然后，验证暂存主机上的机器人流量：

```
curl -svo /dev/null https://staging.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

将`https://staging.example.com/page.html`替换为您的实际暂存URL和路径。 成功的响应包括`x-edgeoptimize-request-id`标头。

{{verify-routing-status-in-ui}}

{{return-to-overview}}
