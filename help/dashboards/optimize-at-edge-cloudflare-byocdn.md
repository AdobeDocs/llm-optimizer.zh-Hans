---
title: 在Edge中优化 — Cloudflare (BYOCDN)
description: 了解如何在LLM Optimizer中为Edge中的优化配置Cloudflare BYOCDN。
feature: Opportunities
source-git-commit: 23752b30294c3d467ca477b085aa521cad0f72ca
workflow-type: tm+mt
source-wordcount: '1250'
ht-degree: 1%

---


# Cloudflare (BYOCDN)

此配置将代理流量（来自AI机器人和LLM用户代理的请求）路由到Edge优化后端服务(`live.edgeoptimize.net`)。 与往常一样，我们仍将从您的源头为人类访客和SEO机器人提供服务。 要测试配置，请在设置完成后查找响应中的标头`x-edgeoptimize-request-id`。

**先决条件**

在设置Cloudflare Worker路由规则之前，请确保您具有：

* 在您的域中启用了辅助进程的Cloudflare帐户。
* 访问Cloudflare中的域DNS设置。
* 已完成LLM Optimizer载入流程。
* 已完成到LLM Optimizer的CDN日志转发。
* 从Edge UI检索到LLM Optimizer优化API密钥。

{{retrieve-byocdn-api-key}}

**路由的工作方式**

正确配置后，Cloudflare Worker会截获来自代理用户代理的对您的域（例如`www.example.com/page.html`）的请求，并将其路由到Edge Optimize后端。 后端请求包含所需的标头。

**正在测试后端请求**

您可以通过直接向Edge优化后端发出请求来验证路由。

```
curl -svo /dev/null https://live.edgeoptimize.net/page.html \
  -H 'x-forwarded-host: www.example.com' \
  -H 'x-edgeoptimize-url: /page.html' \
  -H 'x-edgeoptimize-api-key: $EDGE_OPTIMIZE_API_KEY' \
  -H 'x-edgeoptimize-config: LLMCLIENT=TRUE;'
```

**必需的标头**

必须为对Edge优化后端的请求设置以下标头：

| 页眉 | 描述 | 示例 |
|--------|-------------|---------|
| `x-forwarded-host` | 请求的原始主机。 标识站点域时需要使用。 | `www.example.com` |
| `x-edgeoptimize-url` | 请求的原始URL路径和查询字符串。 | `/page.html` 或 `/products?id=123` |
| `x-edgeoptimize-api-key` | Adobe为您的域提供的API密钥。 | `your-api-key-here` |
| `x-edgeoptimize-config` | 用于缓存键区分的配置字符串。 | `LLMCLIENT=TRUE;` |

**步骤1：创建Cloudflare工作进程**

1. 登录到Cloudflare功能板。
2. 在侧栏中导航到&#x200B;**工作程序和页面**。
3. 单击&#x200B;**创建应用程序**，然后单击&#x200B;**创建辅助进程**。
4. 为您的工作人员命名（例如，`edge-optimize-router`）。
5. 单击&#x200B;**部署**&#x200B;以使用默认代码创建辅助进程。

![Cloudflare Workers仪表板](/help/assets/optimize-at-edge/cloudflare-workers-dashboard.png)

**步骤2：添加辅助进程代码**

创建辅助进程后，单击&#x200B;**编辑代码**&#x200B;并将默认代码替换为以下内容：

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

单击&#x200B;**保存并部署**&#x200B;以发布辅助进程。

![Cloudflare Worker代码编辑器](/help/assets/optimize-at-edge/cloudflare-worker-editor.png)

**步骤3：配置环境变量**

环境变量安全地存储敏感配置，如您的API密钥。

1. 在辅助进程的设置中，导航到&#x200B;**设置** > **变量**。
2. 在&#x200B;**环境变量**&#x200B;下，单击&#x200B;**添加变量**。
3. 添加以下变量：

   | 变量名称 | 描述 | 必需 |
   |---------------|-------------|----------|
   | `EDGE_OPTIMIZE_API_KEY` | Adobe提供的Edge优化API密钥。 | 是 |
   | `EDGE_OPTIMIZE_TARGET_HOST` | Edge优化请求的目标主机（作为`x-forwarded-host`标头发送）和故障转移的原始域。 必须是没有协议的域（例如，`www.example.com`，而不是`https://www.example.com`）。 | 是 |

4. 对于API密钥，请单击&#x200B;**加密**&#x200B;以安全地存储它。
5. 单击&#x200B;**保存并部署**。

![Cloudflare环境变量](/help/assets/optimize-at-edge/cloudflare-env-variables.png)

**步骤4：向域添加路由**

要在您的域中激活工作程序，请执行以下操作：

1. 转到工作人员的&#x200B;**设置** > **触发器**。
2. 在&#x200B;**路由**&#x200B;下，单击&#x200B;**添加路由**。
3. 输入您的域模式（例如，`www.example.com/*`或`example.com/*`）。
4. 从下拉列表中选择您的区域。
5. 单击&#x200B;**保存**。

或者，您可以在区域级别配置路由：

1. 在Cloudflare中导航到您的域。
2. 转到&#x200B;**工作人员路由**。
3. 单击&#x200B;**添加路由**&#x200B;并指定模式和辅助进程。

![Cloudflare辅助进程路由](/help/assets/optimize-at-edge/cloudflare-worker-routes.png)

**验证故障转移行为**

如果Edge Optimize不可用或返回错误，工作人员将自动故障转移到您的来源。 故障转移响应包括`x-edgeoptimize-fo`标头：

```
< HTTP/2 200
< x-edgeoptimize-fo: 1
```

您可以在Cloudflare Worker日志中监视故障转移事件以解决问题。

**了解辅助进程逻辑**

Cloudflare Worker将实施以下逻辑：

1. **用户代理检测：**&#x200B;检查传入请求的用户代理是否与定义的任何代理机器人匹配（不区分大小写）。

2. **路径定位：**&#x200B;可以根据目标路径筛选请求。 默认情况下，所有HTML页面（以`/`、无扩展名或`.html`结尾的URL）都会被路由。 您可以使用`TARGETED_PATHS`数组指定特定路径。

3. **循环保护：** `x-edgeoptimize-request`标头阻止无限循环。 当Edge Optimize将请求发送回您的源时，此标头设置为`"1"`，并且Worker传递请求时没有将其路由回Edge Optimize。

4. **标头安全性：**&#x200B;在设置Edge Optimize标头之前，工作进程将从传入请求中删除任何现有的`x-edgeoptimize-*`标头，以防止标头注入攻击。

5. **标头映射：**&#x200B;辅助进程设置Edge优化所需的标头：
   * `x-forwarded-host` — 标识原始站点域。
   * `x-edgeoptimize-url` — 保留原始请求路径和查询字符串。
   * `x-edgeoptimize-api-key` — 通过Edge Optimize验证请求。
   * `x-edgeoptimize-config` — 提供缓存键配置。

6. **故障转移逻辑：**&#x200B;如果Edge Optimize返回任何错误状态代码（4XX客户端错误或5XX服务器错误），或者请求由于网络错误而失败，则工作进程将使用`EDGE_OPTIMIZE_TARGET_HOST`自动故障转移到您的源。 故障转移响应包含`x-edgeoptimize-fo: 1`标头以指示已发生故障转移。

7. **重定向处理：** `redirect: "manual"`选项可确保将来自Edge Optimize的重定向响应传递到客户端，而无需辅助进程对其进行跟进。

**自定义配置**

您可以通过修改代码顶部的配置常量来自定义辅助进程行为：

**代理机器人列表**

修改`AGENTIC_BOTS`阵列以添加或删除用户代理：

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

默认情况下，所有HTML页面都会被路由到Edge Optimize。 要将路由限制到特定路径，请修改`TARGETED_PATHS`数组：

```javascript
// Route all HTML pages (default)
const TARGETED_PATHS = null;

// Or specify exact paths to route
const TARGETED_PATHS = ['/', '/page.html', '/products', '/about-us'];
```

**故障转移配置**

默认情况下，如果出现Edge优化中的任何4XX或5XX错误，工作进程将进行故障切换。 自定义此行为：

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

* **故障转移行为：**&#x200B;如果Edge Optimize返回任何错误（4XX或5XX状态代码），或者请求由于网络错误而失败，则工作进程会自动故障转移到您的源。 故障转移使用`EDGE_OPTIMIZE_TARGET_HOST`作为原始域（类似于Fastly的`F_Default_Origin`或CloudFront的`Default_Origin`）。 故障转移响应包括`x-edgeoptimize-fo: 1`标头，可用于监视和调试。

* **缓存：**&#x200B;默认情况下，Cloudflare将根据URL缓存响应。 由于代理流量接收的内容与人工流量不同，因此请确保您的缓存配置考虑到了这一点。 请考虑使用缓存API或缓存标头来区分缓存的内容。 `x-edgeoptimize-config`标头应包含在您的缓存键中。

* **速率限制：**&#x200B;监视您的Edge优化使用情况，并在需要时考虑为代理流量实施速率限制。

* **测试：**&#x200B;在部署到生产环境之前，始终在暂存环境中测试配置。 验证代理流量和人流量是否均可按预期运行。 通过模拟Edge优化错误来测试故障转移行为。

* **日志记录：**&#x200B;启用Cloudflare Workers日志记录以监视请求并排除问题。 导航到&#x200B;**工作程序** > **您的工作程序** > **日志**&#x200B;以查看实时日志。 工作进程会记录故障转移事件以进行调试。

**疑难解答**

| 问题 | 可能的原因 | 解决方案 |
|-------|----------------|----------|
| 没有响应的`x-edgeoptimize-request-id`标头 | 工作路由不匹配或用户代理不在代理机器人列表中。 | 验证您的路由模式是否与请求URL匹配。 检查用户代理是否在`AGENTIC_BOTS`阵列中。 |
| Edge优化出现401或403错误 | API密钥无效或缺失。 | 验证是否已在环境变量中正确设置`EDGE_OPTIMIZE_API_KEY`。 请联系Adobe以确认您的API密钥处于活动状态。 |
| 无限重定向或循环 | 未正确设置或检查循环保护标头。 | 确保`x-edgeoptimize-request`标头检查已准备就绪。 |
| 受影响的人流 | 工作人员路由逻辑过于宽泛。 | 验证用户代理匹配逻辑是否正确且不区分大小写。 检查`TARGETED_PATHS`是否正确配置。 |
| 响应时间慢 | Edge优化后端的网络延迟。 | 第一个请求需要此地址；后续请求将缓存在Edge Optimize中。 |
| 响应中的`x-edgeoptimize-fo: 1`标头 | Edge优化返回错误，并发生到源的故障转移。 | 查看Cloudflare Worker日志以了解特定错误代码。 使用Adobe验证Edge优化服务状态。 |
| 故障转移不起作用 | 已禁用故障转移标志，或故障转移逻辑中出错。 | 验证`FAILOVER_ON_4XX`和`FAILOVER_ON_5XX`是否设置为`true`。 检查工作进程日志中的错误消息。 |
| 某些路径未优化 | 路径不匹配目标路径或HTML页面模式。 | 验证路径是否位于`TARGETED_PATHS` （如果已指定）中并与HTML页面正则表达式模式匹配。 |
| 请求因主机无效而失败 | `EDGE_OPTIMIZE_TARGET_HOST`包含协议（例如，`https://`）。 | 仅使用不带协议的域名（例如，`example.com`，而不是`https://example.com`）。 |
| 故障转移期间出现530错误 | Cloudflare无法连接到源，或者故障转移请求具有无效的标头。 | 确保故障转移功能删除Edge Optimize标头。 验证您的源是否可访问，以及DNS是否配置正确。 |

{{verify-setup-byocdn}}

{{return-to-overview}}
