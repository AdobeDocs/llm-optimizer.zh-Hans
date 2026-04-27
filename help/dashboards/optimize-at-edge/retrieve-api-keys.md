---
title: 检索您的 API 密钥
description: 如何从 LLM Optimizer 检索您的生产和暂存 Edge Optimize API 密钥。
feature: Opportunities
source-git-commit: 3b6dc163f4488a22937916beb6778de4abc5a20c
workflow-type: ht
source-wordcount: '337'
ht-degree: 100%

---


# 检索您的 API 密钥

在配置您的内容传递网络之前，请先从 LLM Optimizer UI 检索您的 Edge Optimize API 密钥。实时流量需要&#x200B;**生产** API 密钥。或者，您也可以先检索&#x200B;**暂存** API 密钥，先在一个暂存主机名上测试路由。

## 生产 API 密钥

1. 在 LLM Optimizer 中，打开&#x200B;**客户配置**，然后选择&#x200B;**内容传递网络配置**&#x200B;选项卡。

   ![导航至客户配置](/help/assets/optimize-at-edge/prereq-customer-config-nav.png)

2. 找到&#x200B;**将优化部署到 AI 代理**&#x200B;部分。 勾选&#x200B;**启用优化引擎**&#x200B;复选框。

   ![将优化部署到 AI 代理——待处理](/help/assets/optimize-at-edge/byocdn-deploy-optimizations-pending.png)

3. 在确认对话框中选择&#x200B;**启用**。

   ![启用优化引擎确认对话框](/help/assets/optimize-at-edge/byocdn-enable-optimization-engine-dialog.png)

4. 选择&#x200B;**查看详细信息**。在&#x200B;**部署优化详细信息**&#x200B;对话框中，复制&#x200B;**生产 API 密钥**（使用字段旁边的&#x200B;**复制**）。

   ![部署优化详细信息中的生产 API 密钥](/help/assets/optimize-at-edge/byocdn-production-api-key-details.png)

   >[!NOTE]
   >对话框可能会显示设置未完成。在路由验证完成之前，这是正常的。您仍然可以复制 API 密钥，使您的 IT 或内容传递网络团队可以完成配置。

如果您在上述步骤中需要任何帮助，请联系您的 Adobe 帐户团队或 `llmo-at-edge@adobe.com`。

## 暂存 API 密钥（可选）

要在启用生产路由之前在一个更低的环境中验证路由，您可以配置一个暂存主机名。

**要求**

* 暂存主机名必须位于与生产环境&#x200B;**相同的可注册域**&#x200B;上（例如在生产是 `https://www.example.com` 的情况下：`https://staging.example.com`）。
* 每个网站只能有&#x200B;**一个**&#x200B;暂存域。保存后，只有联系 Adobe 才能对其进行更改。

**步骤**

1. 在 LLM Optimizer 中，打开&#x200B;**客户配置**，然后选择&#x200B;**内容传递网络配置**&#x200B;选项卡。
2. 在&#x200B;**将优化部署到 AI 代理**&#x200B;中选择&#x200B;**添加暂存域**（如果已配置了暂存域，就选择&#x200B;**暂存域**）。
3. 输入包含 `https://` 在内的完整暂存 URL，然后选择&#x200B;**设置域**。
4. 从确认对话框中复制&#x200B;**暂存** API 密钥。

   ![暂存域 API 密钥](/help/assets/optimize-at-edge/byocdn-staging-domain-api-key.png)

使用暂存 API 密钥在您的暂存环境中部署相同的路由规则。

如果您需要帮助，请联系 `llmo-at-edge@adobe.com`。

## 后续步骤

检索您的 API 密钥后，返回到[内容传递网络设置指南](/help/dashboards/optimize-at-edge/overview.md#cdn-configuration-guides)，配置路由。
