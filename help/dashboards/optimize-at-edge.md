---
title: 在Edge进行优化
description: 了解如何在CDN边缘的LLM Optimizer中提供优化，而无需任何所需的创作更改。
feature: Opportunities
source-git-commit: 1f665bd14349c15d92f8274742606abcf9b02000
workflow-type: tm+mt
source-wordcount: '4708'
ht-degree: 1%

---


# 在Edge进行优化

此页面详细概述了如何在CDN边缘交付优化而不进行任何创作更改。 它涵盖了载入流程、可用的优化机会以及如何在Edge自动优化。

>[!NOTE]
>此功能当前处于抢先访问状态。 您可以在[此处](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs)了解有关提前访问程序的更多信息。

## Edge中的优化功能是什么？

Edge中的优化是LLM Optimizer中一项基于边缘的部署功能，可为LLM用户代理提供对人工智能友好的更改。 在当前上下文中，“Edge”意味着在CDN层应用优化。 由于它在CDN层提供了优化，因此不需要在内容管理系统(CMS)中进行创作更改，因此您的原始CMS保持不变。 此分离允许您在不更改现有发布工作流程的情况下提高LLM可见性。 它仅针对代理流量，不影响人类用户或SEO机器人。 当LLM Optimizer检测到优化页面的机会时，用户可以直接在CDN边缘部署修补程序。

在Edge中优化是对需要复杂工程工作的传统修复的更快、更精简的替代方法。 如前所述，完成一次性设置后，无需更改平台或较长的开发周期即可应用更改。 您可以在几分钟内发布改进，而无需开发人员参与。 这是一种为AI代理优化网站的无代码方法。

Edge中的优化专为营销、SEO、内容和数字战略团队中的企业用户而设计。 它可以让商业用户完成LLM Optimizer的整个历程：识别机会、了解建议并轻松部署修复。 利用Edge中的优化，用户可以预览更改，在CDN边缘快速部署这些更改，并验证优化是否已生效。 可以在LLM Optimizer生态系统中跟踪性能。

### 主要优势

* **仅用于AI的投放：**&#x200B;仅向AI代理提供优化的HTML，对人类访客或SEO机器人没有影响。
* **更快的周期：**&#x200B;在几分钟而不是几周内发布更改。 无需更改平台或延长工程周期。
* **可逆：**&#x200B;支持一键式回滚功能，可在几分钟内还原页面。
* **无性能影响：**&#x200B;基于Edge的优化和缓存不影响网站延迟。
* **CDN和CMS无关：**&#x200B;可与任何CDN配置和前端设置配合使用，而不管内容管理系统如何。

### Edge中的优化支持哪些机会？

Edge中的优化功能支持改善代理Web体验的机会。 在[Opportunities Dashboard](/help/dashboards/opportunities.md)页面和当前页面的Opportunities部分中详细了解每个机会。

## 加入

您应该联系Adobe客户团队或FDE团队以开始载入流程。 您的IT或CDN团队还需要完成先决条件和设置过程。 此外，您还可以联系`llmo-at-edge@adobe.com`以获取进一步的入门帮助。

在Edge中优化的前提条件：

* 完成LLM Optimizer的新用户引导流程。
* 完成CDN日志的日志转发过程。

IT/CDN团队的要求：
* 列入允许列表将`*AdobeEdgeOptimize/1.0*`用户代理添加到您站点的robots.txt文件或bot-traffic管理规则中的。
* 确保在域或CDN级别不阻止页面。
* 在CDN中添加Optimize at Edge路由规则。
* 在LLM Optimizer界面中，确认在Edge上优化路由。

为指导设置过程，下面显示了许多CDN设置的配置示例。 请记住，这些示例应该根据您的实际实时配置进行调整。 我们建议先在较低层环境中应用更改。

>[!BEGINTABS]

>[!TAB AEM Cloud Service托管的CDN (Fastly)]

**Edge优化 — AEM Cloud Service托管的CDN (Fastly)**

此配置将代理流量（来自AI机器人和LLM用户代理的请求）路由到Edge优化后端服务(`live.edgeoptimize.net`)。 与往常一样，我们仍将从您的源头为人类访客和SEO机器人提供服务。 要测试配置，请在设置完成后查找响应中的标头`x-edgeoptimize-request-id`。

**先决条件**

要开始将代理流量路由到Edge Optimize，请执行以下操作：

1. 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

   ![导航到客户配置](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 在&#x200B;**AI流量路由到部署优化**&#x200B;下，勾选&#x200B;**将优化部署到AI代理**&#x200B;复选框。 Adobe团队将代表您处理路由配置。

   ![将部署优化勾选到AI代理](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. 启用该复选框后，状态将显示设置正在进行。 Adobe团队将为您完成路由配置。

   ![AI流量路由设置正在进行中](/help/assets/optimize-at-edge/prereq-traffic-routing-progress.png)

   配置并激活路由后，状态将更新为显示绿色复选标记，指示已成功启用路由。 您无需执行其他操作。

此外，如果您需要上述步骤的任何帮助，请联系您的Adobe客户团队或`llmo-at-edge@adobe.com`。

**通过Cloud Manager Pipeline的自助路由**

如果您希望通过Cloud Manager Pipeline自行配置路由，请执行以下步骤。 路由配置是使用[originSelector CDN规则](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors)完成的。 先决条件如下所示：

* 决定要路由的域。
* 决定要路由的路径。
* 决定要路由的用户代理（推荐的正则表达式）。

要部署规则，您需要：

* 创建[配置管道](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/operations/config-pipeline)。
* 在存储库中提交`cdn.yaml`配置文件。
* 运行配置管道。

```
kind: "CDN"
version: "1"
data:
  # Origin selectors to route to Edge Optimize backend
  originSelectors:
    rules:
      - name: route-to-edge-optimize-backend
        when:
          allOf:
            - reqHeader: x-edgeoptimize-request
              exists: false # avoid loops when requests comes from Edge Optimize
            - reqHeader: user-agent
              matches: "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)" # routed user agents
            - reqProperty: domain
              equals: "example.com" # routed domain
            - reqProperty: originalPath
              matches: '(/[^./]+|\.html|/)$' # routed extensions, with .html extension or without extension
            - anyOf:
              - { reqProperty: originalPath, in: [ "/page.html" ] } # routed pages, exact path matching
              - { reqProperty: originalPath, like: "/dir/*" } # routed pages, wildcard path matching
        action:
          type: selectOrigin
          originName: edge-optimize-backend
    origins:
      - name: edge-optimize-backend
        domain: "live.edgeoptimize.net"
```

**验证设置**

要测试设置，请运行curl并期待以下各项：

```
curl -svo /dev/null https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

也可以在LLM Optimizer UI中查看流量路由的状态。 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

![启用了路由的AI流量路由状态](/help/assets/optimize-at-edge/adobe-CDN-traffic-routed-tick.png)

>[!TAB Fastly (BYOCDN)]

**Edge优化 — Fastly (BYOCDN)**

此配置将代理流量（来自AI机器人和LLM用户代理的请求）路由到Edge优化后端服务(`live.edgeoptimize.net`)。 与往常一样，我们仍将从您的源头为人类访客和SEO机器人提供服务。 要测试配置，请在设置完成后查找响应中的标头`x-edgeoptimize-request-id`。

**先决条件**

在设置Fastly VCL规则之前，请确保您具有：

* 您的域访问Fastly。
* 已完成LLM Optimizer载入流程。
* 已完成到LLM Optimizer的CDN日志转发。
* 从Edge UI检索到LLM Optimizer优化API密钥。

**检索API密钥的步骤：**

1. 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

   ![导航到客户配置](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 在&#x200B;**AI流量路由到部署优化**&#x200B;下，勾选&#x200B;**将优化部署到AI代理**&#x200B;复选框。

   ![将部署优化勾选到AI代理](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. 复制API密钥，并继续执行下面的路由配置步骤。

   ![复制API密钥](/help/assets/optimize-at-edge/prereq-copy-api-key.png)

   >[!NOTE]
   >在此阶段，状态可能显示红叉，指示设置尚未完成。 这是正常情况 — 一旦您完成下面的路由配置并且AI机器人流量开始流动，状态将更新为绿色复选标记，确认路由已成功启用。

此外，如果您需要上述步骤的任何帮助，请联系您的Adobe客户团队或`llmo-at-edge@adobe.com`。

**配置**

将以下三个VCL片段添加到您的Fastly服务。 这些代码片段处理向Edge Optimize发出的路由代理请求、缓存密钥分离以及故障转移到默认来源。

![Fastly VCL](/help/assets/optimize-at-edge/fastly-vcl.png)

![添加VCL代码片段](/help/assets/optimize-at-edge/add-vcl-snippets.png)

**vcl_recv代码片段**

```
unset req.http.x-edgeoptimize-url;
unset req.http.x-edgeoptimize-config;
unset req.http.x-edgeoptimize-api-key;

if (!req.http.x-edgeoptimize-request
    && req.http.user-agent ~ "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)") {
  set req.http.x-forwarded-host = req.http.host; # required for identifying the original host
  set req.http.x-edgeoptimize-url = req.url; # required for identifying the original url
  set req.http.x-edgeoptimize-config = "LLMCLIENT=TRUE;"; # required for cache key
  set req.http.x-edgeoptimize-api-key = "<YOUR API KEY>"; # required for identifying the client
  set req.backend = F_EDGE_OPTIMIZE;
}
```

**vcl_hash代码片段**

```
if (req.http.x-edgeoptimize-config) {
  set req.hash += "edge-optimize";
  set req.hash += req.http.x-edgeoptimize-config;
}
```

**vcl_deliver代码片段**

```
if (req.http.x-edgeoptimize-config && resp.status >= 400) {
  set req.http.x-edgeoptimize-request = "failover";
  set req.backend = F_Default_Origin;
  restart;
}

if (!req.http.x-edgeoptimize-config && req.http.x-edgeoptimize-request == "failover") {
  set resp.http.x-edgeoptimize-fo = "1";
}
```

**故障转移**

`vcl_deliver`代码片段会自动处理故障转移。 如果Edge Optimize返回`4XX`或`5XX`错误，请求将重新启动并路由回您的默认来源，这样最终用户仍会收到响应。 故障转移响应包括`x-edgeoptimize-fo: 1`标头。

| 场景 | 行为 |
| --- | --- |
| Edge优化返回`2XX` | 优化的响应会提供给客户端。 |
| Edge优化返回`4XX`或`5XX` | 请求将重新启动，并从默认源提供服务。 |
| 故障转移响应 | 包含标头`x-edgeoptimize-fo: 1`。 |

**验证设置**

要测试设置，请运行curl并期待以下各项：

```
curl -svo /dev/null https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

也可以在LLM Optimizer UI中查看流量路由的状态。 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

![启用了路由的AI流量路由状态](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

>[!TAB Akamai (BYOCDN)]

**Edge优化 — Akamai (BYOCDN)**

此配置将代理流量（来自AI机器人和LLM用户代理的请求）路由到Edge优化后端服务(`live.edgeoptimize.net`)。 与往常一样，我们仍将从您的源头为人类访客和SEO机器人提供服务。 要测试配置，请在设置完成后查找响应中的标头`x-edgeoptimize-request-id`。

**先决条件**

在设置Akamai属性管理器规则之前，请确保您具有：

* 访问域的Akamai资产管理器。
* 已完成LLM Optimizer载入流程。
* 已完成到LLM Optimizer的CDN日志转发。
* 从Edge UI检索到LLM Optimizer优化API密钥。

**检索API密钥的步骤：**

1. 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

   ![导航到客户配置](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 在&#x200B;**AI流量路由到部署优化**&#x200B;下，勾选&#x200B;**将优化部署到AI代理**&#x200B;复选框。

   ![将部署优化勾选到AI代理](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. 复制API密钥，并继续执行下面的路由配置步骤。

   ![复制API密钥](/help/assets/optimize-at-edge/prereq-copy-api-key.png)

   >[!NOTE]
   >在此阶段，状态可能显示红叉，指示设置尚未完成。 这是正常情况 — 一旦您完成下面的路由配置并且AI机器人流量开始流动，状态将更新为绿色复选标记，确认路由已成功启用。

此外，如果您需要上述步骤的任何帮助，请联系您的Adobe客户团队或`llmo-at-edge@adobe.com`。

**配置**

以下Akamai属性管理器规则将LLM用户代理路由到Edge Optimize。 该配置包括以下步骤：

**1. 设置路由条件（用户 — 代理匹配）**

为以下用户代理设置路由：

```
 *AdobeEdgeOptimize-AI*,
 *ChatGPT-User*,
 *GPTBot*,
 *OAI-SearchBot*,
 *PerplexityBot*,
 *Perplexity-User*
```

![设置路由条件](/help/assets/optimize-at-edge/akamai-step1-routing.png)

**2. 设置来源和SSL行为**

将源设置为`live.edgeoptimize.net`并将SAN与`*.edgeoptimize.net`匹配

![设置来源和SSL行为](/help/assets/optimize-at-edge/akamai-step2-origin.png)

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

**7. 缓存ID修改**

![缓存ID修改](/help/assets/optimize-at-edge/akamai-step7-cacheid.png)

**8. 修改传出请求标头**

将`x-forwarded-host`标头设置为`{{builtin.AK_HOST}}`

![修改传出请求标头](/help/assets/optimize-at-edge/akamai-step8-outgoing-request.png)

**9. 站点故障转移**

![站点故障转移](/help/assets/optimize-at-edge/akamai-step9-failover.png)

![故障转移行为](/help/assets/optimize-at-edge/akamai-step9-failover-behaviors.png)

![故障转移规则](/help/assets/optimize-at-edge/akamai-step9-failover-rules.png)

站点故障转移可确保，如果Edge Optimize返回`4XX`或`5XX`错误，请求将自动路由回默认来源，以便最终用户仍会收到响应。

| 场景 | 行为 |
| --- | --- |
| Edge优化返回`2XX` | 优化的响应会提供给客户端。 |
| Edge优化返回`4XX`或`5XX` | 请求被路由回默认来源。 |

**验证设置**

要测试设置，请运行curl并期待以下各项：

```
curl -svo /dev/null https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

也可以在LLM Optimizer UI中查看流量路由的状态。 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

![启用了路由的AI流量路由状态](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

>[!TAB Cloudflare (BYOCDN)]

**Edge优化 — Cloudflare (BYOCDN)**

此配置将代理流量（来自AI机器人和LLM用户代理的请求）路由到Edge优化后端服务(`live.edgeoptimize.net`)。 与往常一样，我们仍将从您的源头为人类访客和SEO机器人提供服务。 要测试配置，请在设置完成后查找响应中的标头`x-edgeoptimize-request-id`。

**先决条件**

在设置Cloudflare Worker路由规则之前，请确保您具有：

* 在您的域中启用了辅助进程的Cloudflare帐户。
* 访问Cloudflare中的域DNS设置。
* 已完成LLM Optimizer载入流程。
* 已完成到LLM Optimizer的CDN日志转发。
* 从Edge UI检索到LLM Optimizer优化API密钥。

**检索API密钥的步骤：**

1. 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

   ![导航到客户配置](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 在&#x200B;**AI流量路由到部署优化**&#x200B;下，勾选&#x200B;**将优化部署到AI代理**&#x200B;复选框。

   ![将部署优化勾选到AI代理](/help/assets/optimize-at-edge/prereq-deploy-checkbox.png)

3. 复制API密钥，并继续执行下面的路由配置步骤。

   ![复制API密钥](/help/assets/optimize-at-edge/prereq-copy-api-key.png)

   >[!NOTE]
   >在此阶段，状态可能显示红叉，指示设置尚未完成。 这是正常情况 — 一旦您完成下面的路由配置并且AI机器人流量开始流动，状态将更新为绿色复选标记，确认路由已成功启用。

此外，如果您需要上述步骤的任何帮助，请联系您的Adobe客户团队或`llmo-at-edge@adobe.com`。

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

**步骤5：验证设置**

部署后，通过使用代理用户代理发出请求来测试配置。

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

成功的响应包括`x-edgeoptimize-request-id`标头：

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

也可以在LLM Optimizer UI中查看流量路由的状态。 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

![启用了路由的AI流量路由状态](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

您还可以验证正常流量是否可以继续工作：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0"
```

此请求应从源网站提供，不带`x-edgeoptimize-request-id`标头。

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

>[!ENDTABS]

>[!NOTE]
>对于其他CDN提供商，请联系`llmo-at-edge@adobe.com`以协助您的IT/CDN团队进行入门。 完成设置配置后，您可以在LLM Optimizer中部署有关优化at Edge机会的建议。

## 机会

下表列出了可以改善常规Web体验的机会，Edge中的优化支持这些机会。

| 机会 | 类型 | 自动识别 | 自动建议 | 自动优化 |
|---------|----------|----------|----------|----------|
| 恢复内容可见度 | 技术性 GEO | 检测对AI代理隐藏关键内容的页面。 显示受影响的URL和可恢复的预期内容。 | 突出显示可供AI代理使用的内容，并建议启用这些页面的预呈现。 | 向会恢复先前隐藏内容的代理流量提供完全呈现的、对人工智能友好的HTML快照。 |
| 添加LLM友好的摘要 | 内容优化 | 标识缺少页面或部分级别简洁摘要的长页面或复杂页面，这会使AI更难快速扫描和理解这些页面。 | 建议在页面和节级别使用人工智能生成的简短摘要，用于捕获关键内容。 | 将摘要插入相关的HTML部分，以改进模型解释和描述页面内容的方式。 |
| 添加相关常见问题解答 | 内容优化 | 检测现有页面内容中可能受益于常见问题的意图差距。 | 根据用户意图和现有主题建议AI生成的常见问题解答内容。 | 将常见问题解答内容注入HTML，使页面在AI驱动的答案中更易于发现且更相关。 |
| 简化复杂内容 | 内容优化 | 标记包含复杂文本的页面，这些文本可能阻碍AI理解。 | 提供AI生成的复杂文本的简化版本，同时保留原始含义。 | 重写页面中的复杂部分，提高AI可读性。 |

### 其他工具

[Adobe LLM Optimizer：您的网页是否可编辑？](https://chromewebstore.google.com/detail/adobe-llm-optimizer-is-yo/jbjngahjjdgonbeinjlepfamjdmdcbcc) Chrome扩展显示LLM可以访问多少网页内容以及哪些内容保持隐藏状态。 它设计为免费、独立的诊断工具，无需产品许可证或设置。

通过单击鼠标，您可以评估任何站点的计算机可读性。 您可以查看人工智能代理所看到的内容与人类用户所看到的内容的并排比较，并估计使用LLM Optimizer可以恢复的内容量。 查看[AI是否可以读取您的网站？](https://business.adobe.com/cn/blog/introducing-the-llm-optimizer-chrome-extension) 页面以了解更多信息。

## 机会详细信息

在接下来的部分中，您可以查看Edge中的优化支持的每个商机的其他详细信息。

### 恢复内容可见度

此机会标记因客户端渲染而为AI代理隐藏关键内容的页面。 对于每个标识的页面，它都会精确显示AI代理视图中缺少哪些内容，突出显示可见性差距，并允许您直接应用更改以恢复隐藏的内容。 当您使用Edge中的优化来部署此机会时，为LLM用户代理提供预呈现的、AI优化的页面版本，以便他们无需执行Javascript即可访问整个上下文。
这可确保该页面首先对AI代理完全可见。 在该预呈现的HTML之上应用了其他增强功能。

>[!IMPORTANT]
>在Edge上使用优化部署时，此预呈现功能自动应用于下面显示的所有机会，以确保AI代理完全看到该页面。

### 添加LLM友好的摘要

此机会标识了可从简洁摘要中受益的页面，以帮助LLM快速了解页面内容的内容。 对于每个页面，机会检测最需要摘要的位置，并在页面级别或部分级别创建AI生成的摘要。 在Edge中使用优化进行部署时，这些摘要将插入到AI代理检索的HTML中，从而提高内容描述更准确的可能性。

### 添加相关常见问题解答

此机会标记页面，在这些页面中，其他问答内容可以更好地匹配人工智能驱动发现中的用户意图和提示。 对于每个页面，它会提出与页面上的用户意图和内容绑定的人工智能生成的常见问题解答块。 通过使用Edge优化，这些常见问题解答将插入到HTML中，以使您的页面更易于使用AI，并增加AI回答直接反映您指导的可能性。

### 简化复杂内容

此机会查找包含长且复杂段落的页面，这些段落可能会降低对人工智能的理解。 对于超出可读性阈值的每个页面，它会创建AI生成的内容，该内容更简单、更易于扫描，同时保留原始含义。 部署在边缘时，交付给代理流量的简化内容可帮助LLM更忠实地解读和总结您的内容。

## 在Edge中自动优化

对于每个opportunity ，您可以在边缘预览、编辑、部署、查看实时优化和回退优化。

>[!VIDEO](https://video.tv.adobe.com/v/3477983/?learn=on&enablevpops)

### 预览

**预览**&#x200B;允许您在建议上线之前查看建议的影响。 它显示当前页面与应用建议后预期的AI优化版本之间的并排差异。 此视图使用将支持实时流量的同一个Edge优化逻辑，但处于隔离的预览模式。 这不会影响实时流量，因为它是只读模拟以供审查。

![预览](/help/assets/optimize-at-edge/preview.png)

### 编辑

**编辑**&#x200B;允许您在部署自动生成的建议之前对其进行优化或完全重写。 您无需接受建议，而是通过编辑工作流来保持完全控制权。 该视图在结构化编辑器中显示提议的更改，您可以在其中修改文本以更好地匹配原始意图。 之后，编辑后的版本将提供给部署后的AI代理。

![编辑](/help/assets/optimize-at-edge/edit.png)

### 部署

**部署**&#x200B;发布所选建议，以便能够为边缘和AI代理提供优化的体验。 如果CDN完全路由，则域中的所有页面通常会在几分钟内启用新更改。 如果仅为选定路径配置了路由，则只有已列入允许列表的页面会通过优化启用。

![部署](/help/assets/optimize-at-edge/deploy.png)

### 实时查看

**查看实时**&#x200B;允许您验证优化是否处于实时状态，以及代理流量是否按预期运行，否则很难访问视图。 您可以在固定建议下查看实时页面，这会向AI代理呈现页面，如图所示。

![实时查看](/help/assets/optimize-at-edge/view-live.png)

### 回滚

回滚可安全地还原以前部署的优化。 该页面的仅限AI版本通常会在几分钟内返回到其之前的状态，这样便可以在需要时安全地尝试进行优化。

![回滚](/help/assets/optimize-at-edge/rollback.png)

## 常见问题解答

问：在Edge中，使用“优化”功能可以定位哪些LLM？

要定位的用户代理列表由您在载入过程中定义。

<!--Q. What does "Edge" in Optimize at Edge mean?

In our context, "Edge" means that the optimization is applied at the CDN layer and not inside your CMS.

Q. Why does this optimization require a CDN?

The CDN is where the optimized version of the page is assembled and delivered to AI agents. We leverage the CDN to ensure your origin CMS remains unchanged. This separation lets you improve LLM visibility without altering your existing publishing workflows.-->

问：如果我还未加入Edge优化，会发生什么？

如果在完成所需的设置之前单击&#x200B;**部署优化**，则任何内容都不会应用于您的网站。 相反，弹出对话框会提示您通过`llmo-at-edge@adobe.com`联系我们的团队以获取载入帮助。 在新用户引导完成之前，您仍然可以探索检测到的机会和建议，但一键式部署工作流将保持不活动状态。

问：在源中更新内容时会发生什么情况？

只要基础源页面未发生更改，我们就会从缓存中提供页面的优化版本。 但是，当&#x200B;**恢复内容可见度**&#x200B;的源发生更改时，我们的系统会自动刷新，以便AI代理始终接收最新的内容。 这是因为我们使用低缓存生存时间(TTL)设置（以分钟为单位），因此您网站上的任何内容更新都会触发该窗口内的新优化。 对于诸如&#x200B;**添加LLM友好型摘要**&#x200B;之类的内容机会，LLM Optimizer会监控源页面是否有更改。 如果检测到更改，我们会暂停优化并将其标记为人工审核，以防止代理可见页面和人类可见页面之间的内容漂移。
<!--As there is no universal TTL that fits every site, we can configure this TTL based on your cache invalidation rules to ensure both systems stay in sync.-->

问：Edge是否仅为使用Adobe Edge Delivery Service (EDS)的站点进行优化？

不行。 在Edge中优化与CDN无关，它可与任何前端架构配合使用，而不仅仅是部署在Adobe的EDS栈栈上的架构。

问：在Edge上优化预渲染与传统的服务器端渲染(SSR)有何不同？

两者都可以解决不同的问题，并且可以协同工作。 传统SSR渲染服务器端内容，但不包括稍后在浏览器中加载的内容。 在Edge中优化预呈现会在JavaScript和客户端数据加载后捕获页面，并在CDN边缘生成完整汇编版本。 SSR侧重于改善人工体验，而Edge中的优化可改善LLM的Web体验。

问：如果我为域中的某些URL（而非所有URL）部署了优化功能，会发生什么情况？

只修改您明确优化的URL。 对于具有已部署机会的URL，AI代理将接收优化版本。 对于没有部署机会的URL，我们的服务只是按原样代理原始页面，而不应用更改或将其存储在我们的优化缓存层中。 这可确保您能够有选择地部署优化，而不会影响网站的其余部分。
