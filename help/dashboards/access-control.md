---
title: 访问控制
description: 了解产品分配用户和组织用户在Adobe LLM Optimizer中的差异、只读用户在UI中看到的内容以及管理员如何在Adobe Admin Console中分配访问权限。
feature: Customer Configuration
source-git-commit: 3b792a8ca7efd4b6d6764d2e83f9b0c103a56558
workflow-type: tm+mt
source-wordcount: '618'
ht-degree: 4%

---


# 访问控制

Adobe LLM Optimizer支持基于用户角色的基本访问控制。 此功能仅适用于&#x200B;**付费客户**，并根据请求启用。 试用版客户无法使用此功能。

>[!IMPORTANT]
>
>要请求访问此功能，付费客户应联系其Adobe客户经理。

## 产品分配的用户 {#product-assigned-users}

如果您被分配到产品，则除了以下权限外，您还具有与标准组织用户相同的权限：

* 在[客户配置](/help/dashboards/customer-configuration.md)中写入提示、类别、主题和相关设置的访问权限。
* 部署[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)优化并管理建议。
* 管理Google搜索控制台配置。
* 在Edge和CDN配置中管理优化。
* 载入新站点。

## 组织用户 {#organizational-users}

组织用户是&#x200B;**未**&#x200B;分配给产品的标准用户。 如果您是组织用户，则对[LLM Optimizer功能板](/help/dashboards/dashboards-overview.md)和相关视图具有&#x200B;**只读**&#x200B;访问权限。 以下限制适用。

### 客户配置 {#customer-configuration-restrictions}

* **上载提示**&#x200B;已禁用。
* 已禁用管理和编辑提示、类别、主题和区域。

  针对只读用户的![客户配置限制](/help/dashboards/assets/access-control-customer-configuration.png)

### CDN配置（客户配置） {#cdn-configuration-restrictions}

* **板载CDN**&#x200B;已禁用（只读用户无法添加CDN提供程序）。
* **删除CDN**&#x200B;已禁用（只读用户无法删除现有CDN配置）。
* CDN板载对话框中的&#x200B;**提交**&#x200B;按钮已禁用（只读用户无法完成CDN设置）。

  只读用户的![CDN配置限制](/help/dashboards/assets/access-control-cdn-configuration.png)

### 品牌存在感 — 数据分析 {#brand-presence-restrictions}

* 主题旁边的&#x200B;**删除**&#x200B;按钮是隐藏的（只读用户无法从跟踪中删除主题）。
* 提示旁边的&#x200B;**删除**&#x200B;按钮已隐藏（只读用户无法从跟踪中删除提示）。

  为只读用户隐藏![品牌存在感操作](/help/dashboards/assets/access-control-brand-presence.png)

### 代理流量机会（错误页面机会） {#agentic-opportunities}

对于404、403和503错误页面等机会：

* **部署优化**&#x200B;已隐藏。
* 信息性警报说明需要部署访问权限。

  ![隐藏在代理流量机会上的部署优化](/help/dashboards/assets/access-control-agentic-deploy.png)

### 其他机会页面 {#other-opportunities}

只读行为也适用于机会类型，例如：

* 目录
* 摘要
* 可读性
* 预渲染
* 标题
* 常见问题解答
* 缺少结构化数据
* 通用修补程序机会

对于这些页面：

* 当用户没有部署访问权限时，**部署优化**&#x200B;将隐藏。
* 内联警报说明需要部署访问权限。 该消息类似于：*需要部署访问权限 — 您没有权限部署优化或管理建议。 请联系您的管理员以请求访问权限。*
* 隐藏了包含部署操作的粘性底部栏。

  ![需要部署访问权限时发出内联警报](/help/dashboards/assets/access-control-deploy-alert.png)

  ![对只读用户隐藏的Edge部署操作进行优化](/help/dashboards/assets/access-control-optimize-at-edge.png)

### Google Search Console提示配置 {#gsc-restrictions}

* 已禁用或隐藏管理和连接操作。
* 用于添加提示的操作列已隐藏。

  ![Google Search Console配置限制](/help/dashboards/assets/access-control-gsc.png)

### 载入新站点 {#onboarding-restrictions}

* 对于没有访问权限的用户，新站点的载入操作被禁用。

  ![已禁用载入新站点](/help/dashboards/assets/access-control-onboarding.png)

## 将产品分配给用户或组 {#assign-product}

贵组织的&#x200B;**系统管理员**&#x200B;可以使用[Adobe Admin Console](https://adminconsole.adobe.com/)将Adobe LLM Optimizer分配给用户或组。

1. 使用对您的组织具有管理权限的帐户登录到[Adobe Admin Console](https://adminconsole.adobe.com/)。
1. 将Adobe LLM Optimizer产品配置文件（或您组织的等效产品权利）分配给应接收产品分配权能的用户或组。

有关详细步骤，请参阅[在Admin Console中管理产品](https://helpx.adobe.com/enterprise/using/manage-products.html)和[管理用户组](https://helpx.adobe.com/cn/enterprise/using/user-groups.html)。

>[!NOTE]
>
>Adobe Admin Console中的屏幕流在版本之间可能会发生变化。 如果上述选项与您的控制台不匹配，请使用Adobe Admin Console中的产品内帮助链接或联系您的Adobe客户团队。
