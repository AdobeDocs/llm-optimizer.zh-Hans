---
title: 在Edge进行优化
description: 了解如何在CDN边缘的LLM Optimizer中提供优化，而无需任何所需的创作更改。
feature: Opportunities
source-git-commit: c1040edc78480f0df9ea3c29cc15009d0596941f
workflow-type: tm+mt
source-wordcount: '2149'
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

* 生成API密钥。
* 在CDN中添加Optimize at Edge路由规则。
* 允许列表用户定义的路径或整个域。
* 添加要定位的用户定义的LLM用户代理列表。
* 确保`robots.txt`不会阻止任何要定位的用户代理。
* 在LLM Optimizer界面中，确认在Edge上优化路由。

为指导设置过程，下面显示了许多CDN设置的配置示例。 请记住，这些示例应该根据您的实际实时配置进行调整。 我们建议先在较低层环境中应用更改。

>[!BEGINTABS]

>[!TAB Adobe托管的CDN]

**Adobe托管的CDN**

此配置的目的是使用代理用户代理配置请求，这些代理用户代理将被路由到优化程序服务（`live.edgeoptimize.net`后端）。 要测试配置，请在设置完成后查找响应中的标头`x-edge-optimize-request-id`。

```
curl -svo page.html https://frescopa.coffee/about-us --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edge-optimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

路由配置是使用[originSelector CDN规则](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/implementing/content-delivery/cdn-configuring-traffic#origin-selectors)完成的。 先决条件如下所示：

* 决定要路由的域
* 决定要路由的路径
* 决定要路由的用户代理（推荐的正则表达式）

要部署规则，您需要：

* 创建[配置管道](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/operations/config-pipeline)
* 提交存储库中的`cdn.yaml`配置文件
* 运行配置管道


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
            - reqHeader: x-edge-optimize-request
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

要测试设置，请运行curl并期待以下各项：

```
curl -svo page.html https://www.example.com/page.html --header "user-agent: chatgpt-user"
< HTTP/2 200
< x-edge-optimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

<!-- >>[!TAB Akamai (BYOCDN)]

**Tokowaka BYOCDN - Akamai**

```
{
    "name": "Project Tokowaka CDN Rule",
    "children": [
        {
            "name": "Connection settings",
            "children": [],
            "behaviors": [
                {
                    "name": "advanced",
                    "options": {
                        "description": "",
                        "xml": "<forward:availability.health-detect.status>off</forward:availability.health-detect.status>\n<forward:availability>\n<max-reforwards>1</max-reforwards>\n<max-reconnects>1</max-reconnects>\n</forward:availability>\n<match:forward.server-type value=\"CUSTOMER_ORIGIN\">\n<network:http.read>%(PMUSER_HTTP_READ)</network:http.read>\n<network:http.first-byte-timeout>%(PMUSER_FIRST_BYTE_TIMEOUT)</network:http.first-byte-timeout>\n<network:http.connect-timeout>%(PMUSER_HTTP_CONNECT_TIMEOUT)</network:http.connect-timeout> \n</match:forward.server-type>"
                    },
                    "uuid": "4a8c027b-1b23-44a7-8e12-f8d07e453679",
                    "templateUuid": "41c77091-419f-43f2-9a84-0b406b050cc8"
                }
            ],
            "uuid": "4759571b-8036-4c16-9b60-d3aeb84958f1",
            "criteria": [],
            "criteriaMustSatisfy": "all"
        },
        {
            "name": "Site Failover Behavior",
            "children": [],
            "behaviors": [
                {
                    "name": "failAction",
                    "options": {
                        "actionType": "RECREATED_CO",
                        "contentCustomPath": false,
                        "contentHostname": "www.adobe.com",
                        "enabled": true
                    }
                },
                {
                    "name": "advanced",
                    "options": {
                        "description": "",
                        "xml": "<forward:availability.fail-action2>\n<add-header>\n<status>on</status>\n<name>x-tokowaka-request</name>\n<value>fo</value>\n</add-header>\n</forward:availability.fail-action2>"
                    }
                }
            ],
            "uuid": "b3000c12-1ab8-49b1-a5d0-75e58bb18c9c",
            "criteria": [
                {
                    "name": "matchResponseCode",
                    "options": {
                        "lowerBound": 400,
                        "matchOperator": "IS_BETWEEN",
                        "upperBound": 599
                    }
                },
                {
                    "name": "originTimeout",
                    "options": {
                        "matchOperator": "ORIGIN_TIMED_OUT"
                    }
                }
            ],
            "criteriaMustSatisfy": "any",
            "comments": "If Tokowaka origin returns a 4xx or 5xx error (or times out), failover condition is to send the request back to Akamai and set the x-tokowaka-request header so we don't re-send the request to Tokowaka"
        }
    ],
    "behaviors": [
        {
            "name": "origin",
            "options": {
                "cacheKeyHostname": "ORIGIN_HOSTNAME",
                "compress": true,
                "customValidCnValues": [
                    "{{Origin Hostname}}",
                    "{{Forward Host Header}}",
                    "*.tokowaka.now"
                ],
                "enableTrueClientIp": true,
                "forwardHostHeader": "ORIGIN_HOSTNAME",
                "hostname": "edge.tokowaka.now",
                "httpPort": 80,
                "httpsPort": 443,
                "ipVersion": "IPV4",
                "minTlsVersion": "DYNAMIC",
                "originCertificate": "",
                "originCertsToHonor": "STANDARD_CERTIFICATE_AUTHORITIES",
                "originSni": true,
                "originType": "CUSTOMER",
                "ports": "",
                "standardCertificateAuthorities": [
                    "akamai-permissive",
                    "THIRD_PARTY_AMAZON"
                ],
                "tlsVersionTitle": "",
                "trueClientIpClientSetting": true,
                "trueClientIpHeader": "True-Client-IP",
                "verificationMode": "CUSTOM"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "transform": "NONE",
                "valueSource": "EXPRESSION",
                "variableName": "PMUSER_LLMCLIENT",
                "variableValue": "TRUE"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "caseSensitive": false,
                "extractLocation": "CLIENT_REQUEST_HEADER",
                "globalSubstitution": false,
                "headerName": "Accept-Language ",
                "regex": "^([a-zA-Z]{2}).*",
                "replacement": "$1",
                "transform": "SUBSTITUTE",
                "valueSource": "EXTRACT",
                "variableName": "PMUSER_LANG"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "transform": "NONE",
                "valueSource": "EXPRESSION",
                "variableName": "PMUSER_X_FORWARDED_HOST",
                "variableValue": "{{builtin.AK_HOST}}"
            }
        },
        {
            "name": "setVariable",
            "options": {
                "transform": "NONE",
                "valueSource": "EXPRESSION",
                "variableName": "PMUSER_TOKOWAKA_CACHE_KEY",
                "variableValue": "LLMCLIENT={{user.PMUSER_LLMCLIENT}};LANG={{user.PMUSER_LANG}};X_FORWARDED_HOST={{user.PMUSER_X_FORWARDED_HOST}}"
            }
        },
        {
            "name": "caching",
            "options": {
                "behavior": "CACHE_CONTROL_AND_EXPIRES",
                "cacheControlDirectives": "",
                "defaultTtl": "1d",
                "enhancedRfcSupport": false,
                "honorMustRevalidate": false,
                "honorPrivate": false,
                "mustRevalidate": false
            }
        },
        {
            "name": "modifyIncomingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "X-tokowaka-api-key",
                "newHeaderValue": "<your api-key here>",
                "standardModifyHeaderName": "OTHER"
            }
        },
        {
            "name": "modifyIncomingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "x-tokowaka-config",
                "newHeaderValue": "LLMCLIENT={{user.PMUSER_LLMCLIENT}};LANG={{user.PMUSER_LANG}}",
                "standardModifyHeaderName": "OTHER"
            }
        },
        {
            "name": "modifyIncomingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "x-tokowaka-url",
                "newHeaderValue": "{{builtin.AK_URL}}",
                "standardModifyHeaderName": "OTHER"
            }
        },
        {
            "name": "cacheId",
            "options": {
                "rule": "INCLUDE_VARIABLE",
                "variableName": "PMUSER_TOKOWAKA_CACHE_KEY"
            }
        },
        {
            "name": "modifyIncomingResponseHeader",
            "options": {
                "action": "DELETE",
                "customHeaderName": "Age",
                "standardDeleteHeaderName": "OTHER"
            }
        },
        {
            "name": "prefreshCache",
            "options": {
                "enabled": true,
                "prefreshval": 90
            }
        },
        {
            "name": "modifyOutgoingRequestHeader",
            "options": {
                "action": "MODIFY",
                "avoidDuplicateHeaders": true,
                "customHeaderName": "X-Forwarded-Host",
                "newHeaderValue": "{{builtin.AK_HOST}}",
                "standardModifyHeaderName": "OTHER"
            }
        }
    ],
    "criteria": [
        {
            "name": "userAgent",
            "options": {
                "matchCaseSensitive": false,
                "matchOperator": "IS_ONE_OF",
                "matchWildcard": true,
                "values": [
                    "*Tokowaka-AI*",
                    "*ChatGPT-User*",
                    "*GPTBot*",
                    "*OAI-SearchBot*"
                ]
            }
        },
        {
            "name": "path",
            "options": {
                "matchCaseSensitive": false,
                "matchOperator": "MATCHES_ONE_OF",
                "normalize": false,
                "values": [
                ]
            }
        },
        {
            "name": "requestHeader",
            "options": {
                "headerName": "x-tokowaka-request",
                "matchOperator": "DOES_NOT_EXIST",
                "matchWildcardName": false
            }
        },
        {
            "name": "matchVariable",
            "options": {
                "matchCaseSensitive": true,
                "matchOperator": "IS",
                "matchWildcard": false,
                "variableExpression": "FALSE",
                "variableName": "PMUSER_TOKOWAKA_DISABLE"
            }
        }
    ],
    "criteriaMustSatisfy": "all"
}
```

Important considerations:

* Tokowaka Rule will be ON based on User-Agent + Path + x-tokowaka-request (if not present) + TOKOWAKA_DISABLE variable (to allow switch off using a single variable toggle)
* Set up rules to **Modify Incoming Request Headers** rule to set custom headers
* Set cache-key in Akamai using user defined variable through Cache-ID modification mechanism. Only a single user defined variable is allowed, so create a separate variable for cache_key and set it accordingly.
* Lang: extracted from Accept-Language header using "regex": "^([a-zA-Z]{2}).*"
* With Cache ID Modification within a match on User Agent, the content can't be purged by URL (just FYI)
* Site failover mechanism: With the match on User-Agent rule, Akamai does not allows to failover based on health check, but only only basis of origin response/connectivity per request. Set **x-tokowaka-fo:true**  resp header in case of failover response.
* SWR is not supported by Akamai. So, only TTL based caching is there. So, configure a rule in Akamai to strip Age header from origin response else TTL based caching would not work.
* Ensure that the Tokowaka rule is the bottom most rule in the rule hierarchy (so that it overrides all other rules).-->

>[!TAB Fastly (BYOCDN)]

**Edge优化BYOCDN - Fastly - VCL**

![Fastly VCL](/help/assets/optimize-at-edge/fastly-vcl.png)

![添加VCL代码片段](/help/assets/optimize-at-edge/add-vcl-snippets.png)

**vcl_recv代码片段**

```
unset req.http.x-edge-optimize-url;
unset req.http.x-edge-optimize-config;
unset req.http.x-edge-optimize-api-key;

if (!req.http.x-edge-optimize-request
    && req.http.user-agent ~ "(?i)(AdobeEdgeOptimize-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)") {
  set req.http.x-fowarded-host = req.http.host; # required for identifying the original host
  set req.http.x-edge-optimize-url = req.url; # required for identifying the original url
  set req.http.x-edge-optimize-config = "LLMCLIENT=true"; # required for cache key
  set req.http.x-edge-optimize-api-key = "<YOUR API KEY>"; # required for identifying the client
  set req.backend = F_EDGE_OPTIMIZE;
}
```

**vcl_hash代码片段**

```
if (req.http.x-edge-optimize-config) {
  set req.hash += "edge-optimize";
  set req.hash += req.http.x-edge-optimize-config;
}
```

**vcl_deliver代码片段**

```
if (req.http.x-edge-optimize-config && resp.status >= 400) {
  set req.http.x-edge-optimize-request = "failover";
  set req.backend = F_Default_Origin;
  restart;
}

if (!req.http.x-edge-optimize-config && req.http.x-edge-optimize-request == "failover") {
  set resp.http.x-edge-optimize-fo = "1";
}
```

>[!ENDTABS]

>[!NOTE]
>对于其他CDN提供商，请联系`llmo-at-edge@adobe.com`以协助您的IT/CDN团队进行入门。 完成设置配置后，您可以在LLM Optimizer中部署有关优化at Edge机会的建议。

## 机会

下表列出了可以改善常规Web体验的机会，Edge中的优化支持这些机会。

| 机会 | 类型 | 自动识别 | 自动建议 | 自动优化 |
|---------|----------|----------|----------|----------|
| 恢复内容可见性 | 技术地理位置 | 检测对AI代理隐藏关键内容的页面。 显示受影响的URL和可恢复的预期内容。 | 突出显示可供AI代理使用的内容，并建议启用这些页面的预呈现。 | 向会恢复先前隐藏内容的代理流量提供完全呈现的、对人工智能友好的HTML快照。 |
| 优化LLM的标题 | 内容优化 | 扫描标题以检测空的、重复的、缺失的或模棱两可的标题，这会降低计算机可读性。 | 建议更简洁的标题层次结构和改进的标签，并显示每个页面的更新结构的预览。 | 注入AI代理的改进标题结构，保留可视设计，同时使LLM的页面更易读取。 |
| 添加LLM友好的摘要 | 内容优化 | 标识缺少页面或部分级别简洁摘要的长页面或复杂页面，这会使AI更难快速扫描和理解这些页面。 | 建议在页面和节级别使用人工智能生成的简短摘要，用于捕获关键内容。 | 将摘要插入相关的HTML部分，以改进模型解释和描述页面内容的方式。 |
| 添加相关常见问题解答 | 内容优化 | 检测现有页面内容中可能受益于常见问题的意图差距。 | 根据用户意图和现有主题建议AI生成的常见问题解答内容。 | 将常见问题解答内容注入HTML，使页面在AI驱动的答案中更易于发现且更相关。 |
| 简化复杂内容 | 内容优化 | 标记包含复杂文本的页面，这些文本可能阻碍AI理解。 | 提供AI生成的复杂文本的简化版本，同时保留原始含义。 | 重写页面中的复杂部分，提高AI可读性。 |

### 其他工具

[Adobe LLM Optimizer：您的网页是否可编辑？](https://chromewebstore.google.com/detail/adobe-llm-optimizer-is-yo/jbjngahjjdgonbeinjlepfamjdmdcbcc) Chrome扩展显示LLM可以访问多少网页内容以及哪些内容保持隐藏。 它设计为免费、独立的诊断工具，无需产品许可证或设置。

通过单击鼠标，您可以评估任何站点的计算机可读性。 您可以查看人工智能代理所看到的内容与人类用户所看到的内容的并排比较，并估计使用LLM Optimizer可以恢复的内容量。 查看[AI是否可以读取您的网站？](https://business.adobe.com/blog/introducing-the-llm-optimizer-chrome-extension)页以了解更多信息。

## 机会详细信息

在接下来的部分中，您可以查看Edge中的优化支持的每个商机的其他详细信息。

### 恢复内容可见性

此机会标记因客户端渲染而为AI代理隐藏关键内容的页面。 对于每个标识的页面，它都会精确显示AI代理视图中缺少哪些内容，突出显示可见性差距，并允许您直接应用更改以恢复隐藏的内容。 当您使用Edge中的优化来部署此机会时，为LLM用户代理提供预呈现的、AI优化的页面版本，以便他们无需执行Javascript即可访问整个上下文。
这可确保该页面首先对AI代理完全可见。 在该预呈现的HTML之上应用了其他增强功能。

>[!IMPORTANT]
>在Edge上使用优化部署时，此预呈现功能自动应用于下面显示的所有机会，以确保AI代理完全看到该页面。

### 优化LLM的标题

此机会检测标题结构导致AI代理由于标题为空、重复、缺失或模糊难以理解的页面。 对于每个受影响的页面，机会会显示次优标题并建议更清晰的层次结构。 在Edge中使用Optimize进行部署时，改进的标题会在HTML中应用于代理流量。 这有助于机器可读性，同时保持面向人的布局不变。

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

只要底层源页面未发生更改，我们就会从缓存中提供该页面的优化版本。 但是，当源发生更改时，我们的系统会自动刷新，以便AI代理始终接收最新的内容。 这是因为我们使用低缓存生存时间(TTL)设置（以分钟为单位），因此您网站上的任何内容更新都会触发该窗口内的新优化。<!--As there is no universal TTL that fits every site, we can configure this TTL based on your cache invalidation rules to ensure both systems stay in sync.-->

问：Edge是否仅为使用Adobe Edge Delivery Service (EDS)的站点进行优化？

不行。在Edge中优化与CDN无关，它可与任何前端架构配合使用，而不仅仅是部署在Adobe的EDS栈栈上的架构。

问：在Edge上优化预渲染与传统的服务器端渲染(SSR)有何不同？

两者都可以解决不同的问题，并且可以协同工作。 传统SSR渲染服务器端内容，但不包括稍后在浏览器中加载的内容。 在Edge中优化预呈现会在JavaScript和客户端数据加载后捕获页面，并在CDN边缘生成完整汇编版本。 SSR侧重于改善人工体验，而Edge中的优化可改善LLM的Web体验。
