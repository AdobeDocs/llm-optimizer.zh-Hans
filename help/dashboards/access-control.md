---
title: 访问控制
description: 了解 Adobe LLM Optimizer 中产品分配的用户和组织用户的区别、只读用户在 UI 中能看到什么以及管理员如何在 Adobe Admin Console 中分配访问权限。
feature: Customer Configuration
autotag-review: '2026-07-15T16:44:26.227Z'
TQID: 'https://experienceleague.adobe.com/km1BB-gqTl1U92LhHxbXtoH4MTA2tLXS3mPx5u9rEoQ'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: d1956731-2adb-4bb7-8301-2b239254ac72
subfeature_v2: id: d622681e-b12a-44e4-b49f-91c12f18b52b
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: e1e0219c-f879-479f-8427-888ed2a6e9c2id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 618
ht-degree: 100%

---


# 访问控制

Adobe LLM Optimizer 支持基于用户画像的基本访问控制。 此功能仅提供给&#x200B;**付费客户**，可根据请求启用。 不提供给试用版客户。

>[!IMPORTANT]
>
>要请求访问此功能，付费客户请联系其 Adobe 帐户经理。

## 产品分配的用户 {#product-assigned-users}

如果您被分配到产品，除了以下权限外，您还具有与标准组织用户相同的权限：

* [客户配置](/help/dashboards/customer-configuration.md)中提示词、类别、主题和相关设置的写入权限。
* 部署 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 优化并管理建议。
* 管理 Google Search Console 配置。
* 管理 Optimize at Edge 和内容传递网络配置。
* 加入新网站。

## 组织用户 {#organizational-users}

组织用户是&#x200B;**未**&#x200B;分配给产品的标准用户。 如果您是组织用户，您就具有对 [LLM Optimizer 仪表板](/help/dashboards/dashboards-overview.md)和相关视图的&#x200B;**只读**&#x200B;访问权限。 以下限制适用。

### 客户配置 {#customer-configuration-restrictions}

* **上传提示词**&#x200B;已禁用。
* 对提示词、类别、主题和区域的管理和编辑功能已禁用。

  ![只读用户的客户配置限制](/help/dashboards/assets/access-control-customer-configuration.png)

### 内容传递网络配置（客户配置） {#cdn-configuration-restrictions}

* **加入内容传递网络**&#x200B;已禁用（只读用户无法添加内容传递网络提供程序）。
* **删除内容传递网络**&#x200B;已禁用（只读用户无法移除现有的内容传递网络配置）。
* 内容传递网络加入对话框中的&#x200B;**提交**&#x200B;按钮已禁用（只读用户无法完成内容传递网络设置）。

  ![只读用户的内容传递网络配置限制](/help/dashboards/assets/access-control-cdn-configuration.png)

### 品牌存在感——数据洞察 {#brand-presence-restrictions}

* 主题旁边的&#x200B;**删除**&#x200B;按钮已隐藏（只读用户无法从跟踪中移除主题）。
* 提示词旁边的&#x200B;**删除**&#x200B;按钮已隐藏（只读用户无法从跟踪中移除提示词）。

  ![品牌存在感操作对只读用户隐藏](/help/dashboards/assets/access-control-brand-presence.png)

### 代理式流量机会（错误页面机会） {#agentic-opportunities}

对于 404、403 和 503 错误页面等机会：

* **部署优化**&#x200B;已隐藏。
* 一条信息性警报说明必须具有部署访问权限。

  ![代理式流量机会上隐藏的部署优化](/help/dashboards/assets/access-control-agentic-deploy.png)

### 其他机会页面 {#other-opportunities}

只读行为也适用于机会类型，例如：

* 目录
* 摘要
* 可读性
* 预渲染
* 标题
* 常见问题解答
* 缺少结构化数据
* 一般补丁机会

对于这些页面：

* 如果用户没有部署访问权限，**部署优化**&#x200B;就会隐藏。
* 一条内嵌警报说明必须具有部署访问权限。 消息类似于：*需要部署访问权限。您没有部署优化或管理建议的权限。 请联系您的管理员，请求访问权限。*
* 包含部署操作的固定底边栏已隐藏。

  ![需要部署访问权限情况下的内嵌警报](/help/dashboards/assets/access-control-deploy-alert.png)

  ![Optimize at Edge 部署操作对只读用户隐藏](/help/dashboards/assets/access-control-optimize-at-edge.png)

### Google Search Console 提示词配置 {#gsc-restrictions}

* 管理和连接操作已禁用或隐藏。
* 用于添加提示词的操作列已隐藏。

  ![Google Search Console 配置限制](/help/dashboards/assets/access-control-gsc.png)

### 加入新网站 {#onboarding-restrictions}

* 加入新网站的操作对于没有访问控制的用户已禁用。

  ![加入新网站已禁用](/help/dashboards/assets/access-control-onboarding.png)

## 将产品分配给用户或组 {#assign-product}

您的组织的&#x200B;**系统管理员**&#x200B;可以使用 [Adobe Admin Console](https://adminconsole.adobe.com/) 将 Adobe LLM Optimizer 分配给一个用户或一个组。

1. 使用具有组织的管理权限的帐户登录 [Adobe Admin Console](https://adminconsole.adobe.com/)。
1. 将 Adobe LLM Optimizer 产品配置文件（或您组织的同等产品权限）分配给用户或组，使其获得产品分配的功能。

有关详细步骤，请参阅[在 Admin Console 中管理产品](https://helpx.adobe.com/cn/enterprise/using/manage-products.html)和[管理用户组](https://helpx.adobe.com/cn/enterprise/using/user-groups.html)。

>[!NOTE]
>
>不同发行版本的 Adobe Admin Console 中的屏幕流可能会不一样。 如果上述选项与您的控制台不匹配，请使用 Adobe Admin Console 中的产品内帮助链接，或者联系您的 Adobe 帐户团队。
