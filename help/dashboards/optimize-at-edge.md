---
title: Optimize at Edge
description: 了解如何在无需进行任何内容创作更改的情况下，通过内容传递网络边缘在 LLM Optimizer 中交付优化。
feature: Opportunities
source-git-commit: 23752b30294c3d467ca477b085aa521cad0f72ca
workflow-type: tm+mt
source-wordcount: '2172'
ht-degree: 85%

---


# Optimize at Edge

本页面详细介绍了如何在无需进行内容创作更改的情况下，在内容传递网络边缘交付优化。 内容涵盖加入流程、可用的优化机会，以及如何在边缘实现自动优化。

>[!NOTE]
>此功能目前处于抢先体验阶段。 您可以在[此处](https://experienceleague.adobe.com/zh-hans/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs)了解有关抢先体验计划的更多信息。

## 什么是 Optimize at Edge？

Optimize at Edge 是 LLM Optimizer 中的一项基于边缘的部署功能，用于向 LLM 用户代理提供对 AI 友好的更改。 在当前语境中，“Edge”指的是在内容传递网络层应用优化。 由于优化是在内容传递网络层交付，因此无需在内容管理系统（CMS）中进行任何创作更改，您的来源 CMS 保持不变。 这种分离方式使您能够在不更改现有发布工作流程的情况下提升 LLM 可见度。 该功能仅针对代理式流量，不会影响真人用户或 SEO 机器人。 当 LLM Optimizer 检测到页面存在可优化机会时，用户可以直接在内容传递网络边缘部署修复。

相比需要复杂工程投入的传统修复方式，Optimize at Edge 是一种更快速、更精简的替代方案。 如前所述，在完成一次性设置后，应用更改无需进行平台改动或经历漫长的开发周期。 您可以在数分钟内发布改进，而无需开发人员参与。 这是一种无需编写代码即可为 AI 代理优化网站的方式。

Optimize at Edge 专为营销、SEO、内容和数字战略团队中的业务用户设计。 它支持业务用户在 LLM Optimizer 中完成完整流程：识别优化机会、理解优化建议，并轻松部署修复。 借助 Optimize at Edge 功能，用户可以预览更改，在内容传递网络边缘快速部署，并验证优化是否已生效。 相关表现可在 LLM Optimizer 生态系统中进行跟踪。

### 主要优势

* **仅面向 AI 的交付：**&#x200B;仅向 AI 代理提供优化后的 HTML，不会影响真人访客或 SEO 机器人。
* **更快的发布周期：**&#x200B;以分钟为单位发布更改，而非数周。 无需进行平台改动或经历漫长的工程周期。
* **可回滚：**&#x200B;支持一键回滚功能，可在数分钟内将页面恢复至之前状态。
* **无性能影响：**&#x200B;基于边缘的优化与缓存机制确保网站延迟不受影响。
* **兼容任意内容传递网络和 CMS：**&#x200B;无论使用何种内容管理系统，均可适配任意内容传递网络配置和前端设置。

### Optimize at Edge 支持哪些优化机会？

所有可提升代理式 Web 体验的优化机会均可通过 Optimize at Edge 功能实现。 您可在[机会仪表板](/help/dashboards/opportunities.md)页面和当前页面的机会部分中详细了解每个机会。

## 加入

如需启动加入流程，请联系您的 Adobe 客户团队或 FDE 团队。 同时，您的 IT 或内容传递网络团队需完成相关前提条件和设置流程。 此外，您也可发送邮件至 `llmo-at-edge@adobe.com` 获取进一步的加入支持。

加入 Optimize at Edge 的前提条件：

* 完成 LLM Optimizer 的加入流程。
* 完成内容传递网络日志的转发流程。

对您的 IT/内容传递网络团队的要求：
* 列入允许列表将`*AdobeEdgeOptimize/1.0*`用户代理添加到您站点的robots.txt文件或bot-traffic管理规则中的。
* 确保页面在域名或内容传递网络层级未设置访问限制。
* 在内容传递网络中添加 Optimize at Edge 路由规则。
* 在 LLM Optimizer 界面中确认 Optimize at Edge 路由已生效。

要指导设置过程，请在下面选择您的CDN提供商，然后遵循相应的配置指南。 请注意，这些示例需根据您的实际生产环境配置进行调整。 建议您先在低级环境中应用更改。

### CDN配置指南

| 内容传递网络提供者 | 类型 | 指南 |
|---|---|---|
| AEM Cloud Service托管的CDN (Fastly) | 由 Adobe 管理 | [查看安装指南](/help/dashboards/optimize-at-edge-aem-managed-cdn.md) |
| Fastly (BYOCDN) | 自带CDN | [查看安装指南](/help/dashboards/optimize-at-edge-fastly-byocdn.md) |
| Akamai (BYOCDN) | 自带CDN | [查看安装指南](/help/dashboards/optimize-at-edge-akamai-byocdn.md) |
| Cloudflare (BYOCDN) | 自带CDN | [查看安装指南](/help/dashboards/optimize-at-edge-cloudflare-byocdn.md) |

>[!NOTE]
>如果您的CDN提供商未在上面列出，或者您在LLM Optimizer UI中未找到您的域或电子邮件，请联系`llmo-at-edge@adobe.com`以获取载入帮助。 完成设置配置后，您即可在 LLM Optimizer 中部署 Optimize at Edge 相关优化建议。

上述每个CDN设置指南在结尾都包含详细的验证步骤，以确认代理流量是否正确路由，以及人工流量是否不受影响。

## 机会

下表列出了可提升代理式 Web 体验且受 Optimize at Edge 支持的优化机会。

| 机会 | 类型 | 自动识别 | 自动建议 | 自动优化 |
|---------|----------|----------|----------|----------|
| 恢复内容可见度 | 技术性 GEO | 检测关键内容对 AI 代理不可见的页面。 显示受影响的 URL 以及可恢复的预期内容。 | 突出显示可向 AI 代理开放的内容，并建议为这些页面启用预渲染。 | 向代理式流量提供完全渲染的、对 AI 友好的 HTML 快照，从而恢复此前隐藏的内容。 |
| 添加对 LLM 友好的摘要 | 内容优化 | 识别缺少页面级或章节级简要摘要的长篇或复杂页面，这类页面通常不利于 AI 快速扫描和理解。 | 推荐在页面级或章节级添加简短的 AI 生成摘要，以概括关键信息。 | 将摘要插入至相关 HTML 区块，提升模型对页面内容的理解与描述能力。 |
| 添加相关常见问题（FAQ） | 内容优化 | 检测现有页面内容中的意图空白，这些空白可通过添加 FAQ 得到补充。 | 建议与用户意图及现有主题相匹配的 AI 生成的 FAQ 内容。 | 将 FAQ 内容注入 HTML，使页面在 AI 驱动的答案中更易被发现且更具相关性。 |
| 简化复杂内容 | 内容优化 | 标记包含复杂文本、可能影响 AI 理解的页面。 | 在保留原始含义的前提下，提供 AI 生成的简化版本文本。 | 重写页面中的复杂内容区块，提升 AI 可读性。 |

### 其他工具

[Adobe LLM Optimizer：您的网页是否可引用？](https://chromewebstore.google.com/detail/adobe-llm-optimizer-is-yo/jbjngahjjdgonbeinjlepfamjdmdcbcc) 该 Chrome 扩展程序可显示 LLM 能访问您网页内容的比例，以及哪些内容仍处于隐藏状态。 该工具为免费、独立的诊断工具，无需产品许可证或额外设置。

只需单击一次，即可评估任意网站的机器可读性。 您可以并排查看 AI 代理与真人用户所见内容的差异，并估算通过使用 LLM Optimizer 可恢复的内容量。 请参阅 [AI 能读取您的网站吗？](https://business.adobe.com/cn/blog/introducing-the-llm-optimizer-chrome-extension) 页面以了解更多信息。

## 优化机会详解

在下文各章节中，您可查看每项受 Optimize at Edge 支持的优化机会的更多详细信息。

### 恢复内容可见度

此优化机会会标记因客户端渲染导致关键内容对 AI 代理不可见的页面。 对于每个识别出的页面，它会明确指出 AI 代理视图中缺失的内容，突出显示可见性缺口，并支持您直接应用更改以恢复隐藏内容。 当您通过 Optimize at Edge 部署该优化机会时，将向 LLM 用户代理提供预渲染、针对 AI 优化的页面版本，使其无需执行 JavaScript 即可访问完整上下文。
这可确保页面对 AI 代理实现完整可见。 在该预渲染 HTML 的基础上，还会叠加其他增强优化。

>[!IMPORTANT]
>当通过 Optimize at Edge 部署下述所有优化机会时，预渲染功能将自动生效，以确保页面对 AI 代理完全可见。

### 添加对 LLM 友好的摘要

此优化机会会识别适合添加简要摘要的页面，以帮助 LLM 快速理解页面内容的核心主题。 对于每个页面，该功能会识别最需要添加摘要的位置，并在页面级或章节级生成 AI 摘要。 通过 Optimize at Edge 部署后，这些摘要将插入至 AI 代理获取的 HTML 中，从而提升内容被更准确描述的可能性。

### 添加相关常见问题（FAQ）

此优化机会会标记可通过补充问答内容更好匹配用户意图和 AI 驱动发现提示的页面。 对于每个页面，该功能会生成与用户意图及页面内容相关的 AI FAQ 模块建议。 通过 Optimize at Edge，这些 FAQ 将被注入至 HTML，使页面更符合 AI 使用需求，并提高 AI 答案直接反映您内容指导的可能性。

### 简化复杂内容

此优化机会会识别包含长篇复杂段落、可能降低 AI 理解能力的页面。 对于超出可读性阈值的页面，该功能会生成更简洁、便于快速浏览的 AI 内容版本，同时保留原始含义。 在边缘层部署后，提供给代理式流量的简化内容将有助于 LLM 更准确地理解和概括您的内容。

## 在边缘层自动优化

针对每个优化机会，您可以在边缘层进行预览、编辑、部署、实时查看和回滚操作。

>[!VIDEO](https://video.tv.adobe.com/v/3477994/?captions=chi_hans&learn=on&enablevpops)

### 预览

**预览**&#x200B;可让您在建议上线前查看其影响。 它会并排展示当前页面与应用建议后预期生成的 AI 优化版本之间的差异。 该视图采用与线上流量相同的 Optimize at Edge 逻辑，但运行于独立的预览模式。 由于这是用于审查的只读模拟，因此不会影响线上流量。

![预览](/help/assets/optimize-at-edge/preview.png)

### 编辑

**编辑**&#x200B;允许您在部署前优化或完全重写自动生成的建议。 您无需直接接受建议，而是可通过编辑流程保持完全控制。 该视图会在结构化编辑器中显示建议更改，您可以修改文本，使其更符合您的原始意图。 部署后，编辑后的版本将提供给 AI 代理。

![编辑](/help/assets/optimize-at-edge/edit.png)

### 部署

**部署**&#x200B;会发布所选建议，使优化后的体验可从边缘层提供给 AI 代理。 如果内容传递网络已完成全量路由，域名下的所有页面通常会在数分钟内上线新更改。 如果仅为部分路径配置了路由，则只有加入允许列表的页面会应用优化并上线。

![部署](/help/assets/optimize-at-edge/deploy.png)

### 实时查看

**查看实时效果**&#x200B;允许您验证优化是否已上线，并确认其在代理式流量中的表现是否符合预期。这种视图通常较难直接访问。 您可以在“已修复建议”下查看实时页面，该页面以 AI 代理所见方式进行渲染。

![查看实时效果](/help/assets/optimize-at-edge/view-live.png)

### 回滚

回滚可安全撤销之前已部署的优化。 仅面向 AI 的页面版本通常会在数分钟内恢复至先前状态，因此在需要时可安全进行优化试验。

![回滚](/help/assets/optimize-at-edge/rollback.png)

## 常见问题解答

问：Optimize at Edge 针对哪些类型的 LLM？

目标用户代理列表由您在加入流程中自行定义。

<!--Q. What does "Edge" in Optimize at Edge mean?

In our context, "Edge" means that the optimization is applied at the CDN layer and not inside your CMS.

Q. Why does this optimization require a CDN?

The CDN is where the optimized version of the page is assembled and delivered to AI agents. We leverage the CDN to ensure your origin CMS remains unchanged. This separation lets you improve LLM visibility without altering your existing publishing workflows.-->

问：如果我尚未完成 Optimize at Edge 加入，会发生什么？

如果您在完成必要设置之前点击&#x200B;**部署优化**，您的网站不会应用任何更改。 系统会弹出提示框，引导您联系 `llmo-at-edge@adobe.com` 获取加入支持。 在完成加入之前，您仍可查看检测到的优化机会和建议，但一键部署流程将保持不可用状态。

问：当源站内容更新时会发生什么？

只要基础源页面未发生更改，我们就会从缓存中提供页面的优化版本。 但是，当&#x200B;**恢复内容可见度**&#x200B;的源发生更改时，我们的系统会自动刷新，以便AI代理始终接收最新的内容。 这是因为我们使用低缓存生存时间(TTL)设置（以分钟为单位），因此您网站上的任何内容更新都会触发该窗口内的新优化。 对于诸如&#x200B;**添加LLM友好型摘要**&#x200B;之类的内容机会，LLM Optimizer会监控源页面是否有更改。 如果检测到更改，我们会暂停优化并将其标记为人工审核，以防止代理可见页面和人类可见页面之间的内容漂移。
<!--As there is no universal TTL that fits every site, we can configure this TTL based on your cache invalidation rules to ensure both systems stay in sync.-->

问：Optimize at Edge 是否仅适用于使用 Adobe Edge Delivery Service（EDS）的网站？

不。 Optimize at Edge 与内容传递网络无关，可兼容任意前端架构，而不仅限于部署在 Adobe EDS 技术栈上的网站。

问：Optimize at Edge 的预渲染与传统服务器端渲染（SSR）有何不同？

两者解决的问题不同，且可协同使用。 传统 SSR 渲染服务器端内容，但不包含随后在浏览器中加载的内容。 Optimize at Edge 预渲染会在 JavaScript 和客户端数据加载完成后捕获页面，在 CDN 边缘层生成完整组装版本。 SSR 侧重于提升真人用户体验，而 Optimize at Edge 则专注于提升面向 LLM 的 Web 体验。

问：如果我为域中的某些URL（而非所有URL）部署了优化功能，会发生什么情况？

只修改您明确优化的URL。 对于具有已部署机会的URL，AI代理将接收优化版本。 对于没有部署机会的URL，我们的服务只是按原样代理原始页面，而不应用更改或将其存储在我们的优化缓存层中。 这可确保您能够有选择地部署优化，而不会影响网站的其余部分。
