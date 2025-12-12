---
title: 在Edge进行优化
description: 了解如何在CDN边缘的LLM Optimizer中提供优化，而无需任何所需的创作更改。
feature: Opportunities
source-git-commit: 3c6f287b3c3787cee95f99b7031412f26692a88b
workflow-type: tm+mt
source-wordcount: '2291'
ht-degree: 1%

---


# 在Edge进行优化

本节……

## Edge中的优化功能是什么？

Edge中的优化是LLM Optimizer中一项基于边缘的部署功能，可为LLM用户代理提供对人工智能友好的更改。 由于它在CDN边缘提供了优化，因此不需要在内容管理系统(CMS)中进行创作更改。 它还仅针对代理流量，不影响人类用户或SEO机器人。

当LLM Optimizer检测到优化页面的机会时，用户可以直接在边缘部署修补程序，而无需对平台进行更改。

此功能当前处于抢先访问状态。

## 客户为什么会有兴趣？

在Edge中优化是对需要复杂工程工作的传统修复的更快、更精简的替代方法。 客户完成一次性设置后，无需更改平台或较长的开发周期即可将更改应用于网页。 用户只需几分钟即可发布改进功能，无需开发人员参与，无需几周。 这是一种低风险、无代码的方法，可为AI代理优化您的网站。

### 主要优势和价值主张

* **仅用于AI的投放：**&#x200B;仅将优化的HTML提供给AI代理，对人类访客或SEO机器人没有影响。
* **更快的周期：**&#x200B;在几分钟内发布更改，而不是几周。 无需更改平台或延长工程周期。
* **低风险且可逆：**&#x200B;支持一键式回滚功能，可在几分钟内还原页面。
* **无性能影响：**&#x200B;基于Edge的优化和缓存不影响网站延迟。
* **CDN和CMS无关：**&#x200B;可与任何CDN配置和前端设置配合使用，无论CMS如何。

## 谁应该使用它？

Edge中的优化专为营销、SEO、内容和数字战略团队中的企业用户而设计。 它可以让商业用户完成LLM Optimizer的整个历程：识别机会、了解建议并轻松部署修复。 利用Edge中的优化，用户可以预览更改，在边缘快速部署这些更改，并验证优化是否已生效。 可以在LLM Optimizer生态系统中跟踪性能。

## Edge提供了哪些机会？

Edge中的优化功能支持改善代理Web体验的机会。 在[机会](/help/dashboards/opportunities.md)部分中了解有关每个机会的详细信息。

## 加入

在登录到LLM Optimizer并转发CDN日志后，您可以在Edge中启用优化。

需要CDN工程师完成初始设置以启用Edge中的优化。

设置要求：

* 生成API密钥。
* 在CDN中添加Optimize at Edge路由规则。
* 允许列表用户定义的路径或整个域。
* 添加要定位的用户定义的LLM用户代理列表。
* 确保robots.txt不会阻止任何要定位的用户代理。
* 确认在LLM Optimizer UI中优化Edge路由。

Adobe为大多数主要CDN提供了示例配置代码片段以指导设置过程。 我们的指南中包含的代码片段示例需要适应实际的实时配置。 Adobe建议您先在较低级别的环境中实施更改。

>[!BEGINTABS]

>[!TAB AEM Cloud Service托管的CDN (Fastly)]

**Tokowaka BYOCDN - Adobe托管的CDN**

仅使用originSelectors选择Tokowaka原点。

以下示例在匹配模式“/es/*”或确切路径（仅路由html页面）的特定域上路由LLM代理请求。 此示例应该提供一个起点，如果您的配置中有多个originSelectors，建议先放置此起点。

重要说明：

* 在路由到Tokowaka后端之前，需要检查x-tokowaka-request。 只有没有此标头的请求才应路由到Tokowaka后端。
* 如果有多个规则，则第一个列出路由到中胁后端的originSelector规则。
* 部署cdn.yaml之前需要部署TOKOWAKA_API_KEY密钥

```
kind: "CDN"
version: "1"
data:
  # Origin selectors to route to Tokowaka backend
  originSelectors:
    rules:
      - name: route-to-tokowaka-backend
        when:
          allOf:
            - reqHeader: x-tokowaka-request
              exists: false # avoid loops when requests comes from Tokowaka
            - reqHeader: user-agent
              matches: "(?i)(Tokowaka-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)" # routed user agents
            - reqProperty: domain
              equals: "example.com" # routed domain
            - reqProperty: originalPath
              matches: '(/[^./]+|\.html|/)$' # routed extensions, with .html extension or without extension
            - anyOf:
              - { reqProperty: originalPath, in: [ "/page.html" ] } # routed pages, exact path matching
              - { reqProperty: originalPath, like: "/dir/*" } # routed pages, wildcard path matching
        action:
          type: selectOrigin
          originName: tokowaka-backend
          headers:
            x-tokowaka-api-key: "${{TOKOWAKA_API_KEY}}"
    origins:
      - name: tokowaka-backend
        domain: "edge.tokowaka.now"
```

>[!TAB Akamai (BYOCDN)]

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

重要注意事项：

* Tokowaka规则将基于User-Agent + Path + x-tokowaka-request（如果不存在） + TOKOWAKA_DISABLE变量（允许使用单个变量切换关闭）
* 设置规则以&#x200B;**修改传入请求标头**&#x200B;规则以设置自定义标头
* 通过Cache-ID修改机制，使用用户定义的变量在Akamai中设置cache-key。 只允许一个用户定义的变量，因此请为cache_key创建一个单独的变量并相应地对其进行设置。
* Lang：使用“regex”从Accept-Language标头提取： &quot;^([a-zA-Z]{2})。*&quot;
* 在用户代理上的匹配项中进行缓存ID修改后，无法通过URL清除内容（仅供参考）
* 站点故障转移机制：使用User-Agent规则上的匹配项，Akamai不允许基于运行状况检查进行故障转移，而仅允许基于每个请求的原始响应/连接。 设置故障转移响应时的&#x200B;**x-tokowaka-fo:true**&#x200B;响应标头。
* Akamai不支持SWR。 因此，只存在基于TTL的缓存。 因此，请在Akamai中配置一条规则，以从源响应中剥离Age标头，否则基于TTL的缓存将无法正常运行。
* 确保Tokowaka规则是规则层次结构中最低的规则（以便其覆盖所有其他规则）。

>[!TAB Fastly (BYOCDN)]

**Tokowaka BYOCDN - Fastly - VCL**

![Fastly VCL](/help/assets/optimize-at-edge/fastly-vcl.png)

![添加VCL代码片段](/help/assets/optimize-at-edge/add-vcl-snippets.png)

**vcl_recv代码片段**

```
unset req.http.x-tokowaka-url;
unset req.http.x-tokowaka-config;
unset req.http.x-tokowaka-api-key;

if (!req.http.x-tokowaka-request
    && req.http.user-agent ~ "(?i)(Tokowaka-AI|ChatGPT-User|GPTBot|OAI-SearchBot|PerplexityBot|Perplexity-User)") {
  set req.http.x-fowarded-host = req.http.host; # required for identifying the original host
  set req.http.x-tokowaka-url = req.url; # required for identifying the original url
  set req.http.x-tokowaka-config = "LLMCLIENT=true"; # required for cache key
  set req.http.x-tokowaka-api-key = "<YOUR API KEY>"; # required for identifying the client
  set req.backend = F_Tokowaka;
}
```

**vcl_hash代码片段**

```
if (req.http.x-tokowaka-config) {
  set req.hash += "tokowaka";
  set req.hash += req.http.x-tokowaka-config;
}
```

**vcl_deliver代码片段**

```
if (req.http.x-tokowaka-config && resp.status >= 400) {
  set req.http.x-tokowaka-request = "failover";
  set req.backend = F_Default_Origin;
  restart;
}

if (!req.http.x-tokowaka-config && req.http.x-tokowaka-request == "failover") {
  set resp.http.x-tokowaka-fo = "1";
}
```

>[!ENDTABS]


对于其他CDN提供商，请联系llmo-at-edge@adobe.com以协助您的IT/CDN团队进行入门。

<!--This should probably be included Opportunities dashboard content. Content also needs serious editing - lots of "customer needs"and business user" etc.-->

配置完成后，企业用户可以在LLM Optimizer中部署有关优化at Edge商机的建议。

## 机会

| 机会 | 类型 | 自动识别 | 自动建议 | 自动优化 |
|---------|----------|----------|----------|----------|
| 恢复内容可见性 | 技术地理位置 | 检测对AI代理隐藏关键内容的页面。 显示受影响的URL和可恢复的预期内容。 | 突出显示可供AI代理使用的内容，并建议启用这些页面的预呈现。 | 向会恢复先前隐藏内容的代理流量提供完全呈现的、对人工智能友好的HTML快照。 |
| 优化AI标题 | 内容优化 | 扫描标题以检测空的、重复的、缺失的或模糊的标题，这会降低机器可读性。 | 建议更简洁的标题层次结构和改进的标签，并显示每个页面的更新结构的预览。 | 为AI代理注入改进的标题结构，保留您的视觉设计，同时使页面对LLM更易于理解。 |
| 添加AI友好的摘要 | 内容优化 | 标识缺少页面或部分级别简洁摘要的长页面或复杂页面，这会使AI更难快速扫描和理解这些页面。 | 建议在页面级别和节级别使用人工智能生成的简短摘要，用于捕获关键内容。 | 将摘要插入相关的HTML部分，以改进模型解释和描述页面内容的方式。 |
| 添加相关常见问题解答 | 内容优化 | 检测现有页面内容中可能受益于常见问题的意图差距。 | 根据用户意图和现有主题建议AI生成的常见问题解答内容。 | 将常见问题解答内容注入HTML，使页面在AI驱动的答案中更易于发现且更相关。 |
| 简化复杂内容 | 内容优化 | 标记包含复杂文本的页面，这些文本可能阻碍AI理解。 | 提供复杂测试的AI生成的简化版本，同时保留原始含义。 | 重写页面中的复杂部分，提高AI可读性。 |

### 恢复内容可见性

此机会标记因客户端渲染而为AI代理隐藏关键内容的页面。 对于每个标识的页面，它都会精确显示AI代理视图中缺少哪些内容，突出显示可见性差距，并允许您直接应用更改以恢复隐藏的内容。 当您使用Edge中的优化来部署此机会时，为LLM用户代理提供预呈现的、AI优化的页面版本，以便他们无需执行Javascript即可访问整个上下文。

**此预呈现功能自动适用于在Edge上使用优化部署后出现的所有机会。**&#x200B;这将确保首先该页面对AI代理完全可见。 在该预呈现的HTML之上应用了其他增强功能。

#### 其他工具

您的网页是否可编辑？ [Adobe LLM Optimizer：您的网页是否可编辑？](https://chromewebstore.google.com/detail/adobe-llm-optimizer-is-yo/jbjngahjjdgonbeinjlepfamjdmdcbcc) Chrome扩展允许您确切查看LLM可以访问多少网页内容以及哪些内容保持隐藏。 它设计为免费、独立的诊断工具，无需产品许可证或设置。

通过单击，您可以评估任何站点的计算机可读性，查看人工智能代理所看到的内容与人类用户所看到的内容的并排比较，并估计使用LLM Optimizer可以恢复的内容量。 请参阅[AI是否可以读取您的网站？](https://business.adobe.com/blog/introducing-the-llm-optimizer-chrome-extension)以了解更多信息。

### 优化LLM的标题

此机会检测标题结构导致AI代理由于标题为空、重复、缺失或模糊难以理解的页面。 对于每个受影响的页面，机会会显示次优标题并建议更清晰的层次结构。 在Edge中使用Optimize进行部署时，改进的标题将应用于为代理流量提供服务的HTML，这有助于提高计算机可读性，同时保持您面向人物的布局不变。

### 添加LLM友好的摘要

此机会标识了可从简洁摘要中受益的页面，以帮助LLM快速了解该页面的内容。 对于每个页面，机会检测最需要摘要的位置，并在页面级别和/或区域级别创建AI生成的摘要。 在Edge中使用优化进行部署时，这些摘要将插入到AI代理检索的HTML中，从而提高内容描述更准确的可能性。

### 添加相关常见问题解答

此机会标记页面，在这些页面中，其他问答内容可以更好地匹配人工智能驱动发现中的用户意图和提示。 对于每个页面，它会提出与页面上的用户意图和内容绑定的人工智能生成的常见问题解答块。 通过使用Edge优化，这些常见问题解答将插入到HTML中，以使您的页面更易于使用AI，并增加AI回答直接反映您指导的可能性。

### 简化复杂内容

此机会查找包含长且复杂段落的页面，这些段落可能会降低对人工智能的理解。 对于超出可读性阈值的每个页面，它都会创建AI生成的内容，这些内容更简单、更可浏览，同时保留原始含义。 部署在边缘时，交付给代理流量的简化内容可帮助LLM更忠实地解读和总结您的内容。

## 建议

对于每个机会，您都可以预览、编辑、部署、实时预览和回滚边缘位置处的优化情况。

### 预览

通过预览，用户可在任何内容上线之前查看建议对页面的影响。 它显示当前页面与应用建议后预期的AI优化版本之间的并排差异。 此视图使用将为Edge实时流量提供支持的相同优化，但处于安全、隔离的预览模式。 这不会影响实时流量，因为它是只读模拟以供审查。

![预览](/help/assets/optimize-at-edge/preview.png)

### 编辑

编辑允许用户在部署自动生成的建议之前对其进行优化或完全重写。 用户不会被动接受建议，而是通过这种人在环工作流保持完全控制。 该视图在结构化编辑器中显示提议的更改，用户可以在其中修改文本以更好地匹配其意图。 之后，编辑后的版本将提供给部署后的AI代理。

![编辑](/help/assets/optimize-at-edge/edit.png)

### 部署

部署将发布所选建议，以便能够为边缘和AI代理提供优化的体验。 如果CDN完全路由，则域中的所有页面都会在进行新更改时上线，通常在几分钟内。 如果仅为选定路径配置了路由，则只有已列入允许列表的页面会通过优化启用。

![部署](/help/assets/optimize-at-edge/deploy.png)

### 实时查看

“查看实时”功能可让用户验证优化是否处于实时状态，以及代理流量是否按预期运行，否则很难访问代理流量。 用户可以查看固定建议下的实时页面，该页面会呈现给AI代理显示的内容。

![实时查看](/help/assets/optimize-at-edge/view-live.png)

### 回滚

回滚可安全地还原以前部署的优化。 该页面的仅限AI的版本通常会在数分钟内返回到其之前的状态，使用户在需要时可以安全地尝试进行优化。

![回滚](/help/assets/optimize-at-edge/rollback.png)

## 常见问题解答

问：在Edge中，使用“优化”功能可以定位哪些LLM？

要定位的用户代理列表完全由客户在新用户引导时定义。

问：在Edge中优化的“Edge”表示什么？

在我们的上下文中，“Edge”意味着优化应用于CDN层，而不应用于CMS内部。

问：为什么此优化需要CDN？

在CDN中，将组装优化版本的页面并将其交付给AI代理。 我们利用CDN来确保您的源CMS保持不变。 此分离允许您在不更改现有发布工作流程的情况下提高LLM可见性。

问：如果我还未加入Edge优化，会发生什么？

如果在完成所需的设置之前单击&#x200B;**部署优化**，则任何内容都不会应用于您的网站。 相反，弹出对话框会提示您通过llmo-at-edge@adobe.com联系我们的团队以获取载入帮助。 在新用户引导完成之前，您仍然可以探索检测到的机会和建议，但一键式部署工作流将保持不活动状态。

问：在源中更新内容时会发生什么情况？

只要底层源页面未发生更改，我们就会从缓存中提供该页面的优化版本。 但是，当源发生更改时，我们的系统会自动刷新，以便AI代理始终接收最新的内容。 这是因为我们在几分钟内使用了低缓存TTL，因此网站上的任何内容更新都会触发该窗口内的新优化。 由于没有适用于每个站点的通用TTL，因此我们可以根据您的缓存失效规则配置此TTL，以确保两个系统保持同步。

问：Edge是否仅为使用Adobe Edge Delivery Service (EDS)的站点进行优化？

不行。在Edge中优化与CDN无关，它可与任何前端架构配合使用，而不仅仅是部署在Adobe的EDS栈栈上的架构。

问：在Edge上优化预渲染与传统的服务器端渲染(SSR)有何不同？

二者解决不同问题并协同工作。 传统SSR渲染服务器端内容，但不包括稍后在浏览器中加载的内容。 在Edge中优化预呈现会在JavaScript和客户端数据加载后捕获页面，在CDN边缘生成完整汇编版本。 SSR侧重于改善人工体验，而Edge中的优化可改善LLM的Web体验。


















