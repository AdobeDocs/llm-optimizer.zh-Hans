---
source-git-commit: e9309dc8f8d1d81b953483f17dcb424e46d5cd3b
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 20%

---
# 代码片段

## API密钥检索步骤 {#retrieve-byocdn-api-key}

**检索生产Edge的步骤优化API密钥：**

1. 在 LLM Optimizer 中，打开&#x200B;**客户配置**，然后选择&#x200B;**内容传递网络配置**&#x200B;选项卡。

   ![导航至客户配置](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 找到&#x200B;**将优化部署到 AI 代理**&#x200B;部分。 勾选&#x200B;**启用优化引擎**&#x200B;复选框。

   ![将优化部署到 AI 代理——待处理](/help/assets/optimize-at-edge/byocdn-deploy-optimizations-pending.png)

3. 在确认对话框中选择&#x200B;**启用**。

   ![启用优化引擎确认对话框](/help/assets/optimize-at-edge/byocdn-enable-optimization-engine-dialog.png)

4. 选择&#x200B;**查看详细信息**。 在&#x200B;**部署优化详细信息**&#x200B;对话框中，复制&#x200B;**生产API密钥**（使用字段旁边的&#x200B;**复制**）。

   部署优化详细信息中的![生产API密钥](/help/assets/optimize-at-edge/byocdn-production-api-key-details.png)

   >[!NOTE]
   >该对话框可能显示设置未完成。 在验证路由之前，这是正常情况 — 您仍然可以复制API密钥，以便您的IT或CDN团队可以完成配置。

此外，如果您在上述步骤中需要任何帮助，请联系您的 Adobe 帐户团队或 `llmo-at-edge@adobe.com`。

## 可选：在暂存主机名上测试路由 {#retrieve-staging-edge-optimize-api-key}

**可选：测试临时主机名上的路由**

如果要在启用生产工艺路线之前在较低环境中验证工艺路线，则可以配置分段主机名。

**要求**

* 暂存主机名必须位于与生产主机名相同的&#x200B;**可注册域**&#x200B;上（例如，当生产主机名为`https://www.example.com`时，`https://staging.example.com`）。
* 每个站点仅&#x200B;**一个**&#x200B;暂存域。 保存后，如果不联系Adobe，则无法对其进行更改。

**获取您的暂存API密钥**

1. 打开&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**。
2. 在&#x200B;**将优化部署到AI代理**&#x200B;下，选择&#x200B;**添加暂存域**（如果已配置暂存域，则选择&#x200B;**暂存域**）。
3. 输入包括`https://`在内的完整暂存URL并选择&#x200B;**设置域**。
4. 从确认对话框中复制&#x200B;**暂存** API密钥。

![暂存域API密钥](/help/assets/optimize-at-edge/byocdn-staging-domain-api-key.png)

使用测试API密钥在测试环境中部署相同的路由规则。

**测试暂存机器人流量**

将 `https://staging.example.com/page.html` 替换为您的实际暂存 URL 和路径。 **成功：**&#x200B;响应包含`x-edgeoptimize-request-id`标头。

如果您需要帮助，请联系`llmo-at-edge@adobe.com`。

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
