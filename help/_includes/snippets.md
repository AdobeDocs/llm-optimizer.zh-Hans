---
source-git-commit: da789100d814004687de2f46e18a295671dec4b8
workflow-type: tm+mt
source-wordcount: '363'
ht-degree: 10%

---
# 代码片段

## API密钥检索步骤 {#retrieve-byocdn-api-key}

**检索生产Edge的步骤优化API密钥：**

1. 在LLM Optimizer中，打开&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

   ![导航至客户配置](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 找到&#x200B;**将优化部署到AI代理**&#x200B;部分。 勾选&#x200B;**启用优化引擎**&#x200B;复选框。

   ![将优化部署到AI代理 — 挂起](/help/assets/optimize-at-edge/byocdn-deploy-optimizations-pending.png)

3. 在确认对话框中，选择&#x200B;**启用**。

   ![启用优化引擎确认对话框](/help/assets/optimize-at-edge/byocdn-enable-optimization-engine-dialog.png)

4. 选择&#x200B;**查看详细信息**。 在&#x200B;**部署优化详细信息**&#x200B;对话框中，复制&#x200B;**生产API密钥**（使用字段旁边的&#x200B;**复制**）。

   部署优化详细信息中的![生产API密钥](/help/assets/optimize-at-edge/byocdn-production-api-key-details.png)

   >[!NOTE]
   >该对话框可能显示设置未完成。 在验证路由之前，这是正常情况 — 您仍然可以复制API密钥，以便您的IT或CDN团队可以完成配置。

此外，如果您在上述步骤中需要任何帮助，请联系您的 Adobe 帐户团队或 `llmo-at-edge@adobe.com`。

## 暂存域API密钥（可选） {#retrieve-staging-edge-optimize-api-key}

当要在生产流量使用路由规则之前，在较低级别环境中测试Edge中的优化时，请使用暂存主机名。

**先决条件**

* 暂存主机名必须属于与生产站点相同的&#x200B;**可注册域**（例如，当生产站点为`https://www.example.com`时，`https://staging.example.com`）。
* 只能为该站点配置&#x200B;**一个**&#x200B;暂存域。 保存后，如果没有帮助，将无法对其进行更改。

**步骤**

1. 在LLM Optimizer中，打开&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

2. 在&#x200B;**将优化部署到AI代理**&#x200B;部分中，选择&#x200B;**添加暂存域**（如果已配置暂存域，则选择&#x200B;**暂存域**）。

3. 在&#x200B;**暂存域**&#x200B;对话框中，输入包括`https://`在内的完整暂存URL并选择&#x200B;**设置域**。

   ![暂存域输入对话框](/help/assets/optimize-at-edge/byocdn-staging-domain-input.png)

4. 在下一个提示中确认域。 工作流完成时，**暂存域**&#x200B;对话框显示配置的域及其&#x200B;**API密钥**。 选择&#x200B;**复制**&#x200B;以复制暂存API密钥。

   ![暂存域API密钥](/help/assets/optimize-at-edge/byocdn-staging-domain-api-key.png)

如果您需要帮助，请联系`llmo-at-edge@adobe.com`。

## 在LLM Optimizer中检查路由状态 {#verify-routing-status-in-ui}

也可以在 LLM Optimizer UI 中查看流量路由的状态。 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

![将优化部署到AI代理 — 已完成](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

## 返回概述 {#return-to-overview}

要进一步了解Edge优化，包括可用的机会、自动优化工作流和常见问题，请返回[Edge优化概述](/help/dashboards/optimize-at-edge/overview.md)。
