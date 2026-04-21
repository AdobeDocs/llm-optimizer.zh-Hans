---
source-git-commit: b13f91d144d4899198891c4dcd841de8cfbb2355
workflow-type: tm+mt
source-wordcount: '157'
ht-degree: 14%

---
# 代码片段

## 在LLM Optimizer中检查路由状态 {#verify-routing-status-in-ui}

也可以在 LLM Optimizer UI 中查看流量路由的状态。 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

![将优化部署到 AI 代理——已完成](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

## 允许通过防火墙规则在Edge中优化（可选） {#waf-allowlist-setup}

如果您的CDN使用WAF或机器人管理器：

* 在WAF或机器人管理器中允许列表`*AdobeEdgeOptimize/1.0*`用户代理，以便Edge上的优化服务可以获取您的源内容。
* 如果您的防火墙需要用户代理以外的其他验证，请生成密钥（例如，`openssl rand -hex 32`）并：
   * 将带有密码的`x-edgeoptimize-fetcher-key`添加到您的路由规则中其他`x-edgeoptimize-*`标头旁边。
   * 添加WAF或机器人管理器规则，以允许请求`x-edgeoptimize-fetcher-key`与同一密码匹配。
* 在Edge中优化会按原样转发此标头 — 您拥有完整的密钥生命周期。

## 返回概述 {#return-to-overview}

要进一步了解Edge优化，包括可用的机会、自动优化工作流和常见问题，请返回[Edge优化概述](/help/dashboards/optimize-at-edge/overview.md)。
