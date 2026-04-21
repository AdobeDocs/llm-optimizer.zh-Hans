---
title: 检索API密钥
description: 如何从LLM Optimizer检索生产和暂存的Edge优化API密钥。
feature: Opportunities
source-git-commit: 3b6dc163f4488a22937916beb6778de4abc5a20c
workflow-type: tm+mt
source-wordcount: '337'
ht-degree: 18%

---


# 检索API密钥

在配置CDN之前，请从LLM Optimizer UI检索Edge优化API密钥。 实时流量需要&#x200B;**生产** API密钥。 或者，您也可以检索&#x200B;**staging** API密钥，以首先在暂存主机名上测试路由。

## 生产API密钥

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

如果您需要上述步骤的任何帮助，请联系您的Adobe客户团队或`llmo-at-edge@adobe.com`。

## 暂存API密钥（可选）

要在启用生产工艺路线之前在较低环境中验证工艺路线，您可以配置分段主机名。

**要求**

* 暂存主机名必须位于与生产主机名相同的&#x200B;**可注册域**&#x200B;上（例如，当生产主机名为`https://www.example.com`时，`https://staging.example.com`）。
* 每个站点仅&#x200B;**一个**&#x200B;暂存域。 保存后，如果不联系Adobe，则无法对其进行更改。

**步骤**

1. 在 LLM Optimizer 中，打开&#x200B;**客户配置**，然后选择&#x200B;**内容传递网络配置**&#x200B;选项卡。
2. 在&#x200B;**将优化部署到AI代理**&#x200B;下，选择&#x200B;**添加暂存域**（如果已配置暂存域，则选择&#x200B;**暂存域**）。
3. 输入包括`https://`在内的完整暂存URL并选择&#x200B;**设置域**。
4. 从确认对话框中复制&#x200B;**暂存** API密钥。

   ![暂存域API密钥](/help/assets/optimize-at-edge/byocdn-staging-domain-api-key.png)

使用测试API密钥在测试环境中部署相同的路由规则。

如果您需要帮助，请联系`llmo-at-edge@adobe.com`。

## 后续步骤

检索API密钥后，返回到[CDN安装指南](/help/dashboards/optimize-at-edge/overview.md#cdn-configuration-guides)以配置路由。
