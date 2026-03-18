---
title: 日志转发 — Cloudflare
description: 了解如何将CDN日志从Cloudflare转发到Adobe的S3存储段，以便在LLM Optimizer中收集代理流量数据。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 0%

---


# 日志转发： Cloudflare {#log-forwarding-cloudflare}

本页详细介绍如何将CDN日志从Cloudflare转发到Adobe的S3存储段以进行代理流量数据收集。 您将使用LLM Optimizer CDN配置页面载入LLM Optimizer。 完成载入流程后，按照本页面上提供的步骤在Cloudflare功能板控制台中配置日志转发。

## 步骤1：在LLM Optimizer中载入 {#step-1}

在LLM Optimizer页面[https://llmo.now/](https://llmo.now/)上：

1. 转到&#x200B;**客户配置仪表板**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**CDN配置**&#x200B;选项卡。

   ![CDN配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 单击&#x200B;**开始**。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png) -->

1. 在&#x200B;**激活AI流量分析**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)

1. 选择&#x200B;**Cloudflare (BYOCDN)**。

   ![选择Cloudflare](/help/overview/assets/log-forwarding/cloudflare/cloudflare-select.png)

1. 单击&#x200B;**板载**。

   <!-- ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 步骤2：在Cloudflare中创建Logpush作业 {#step-2}

在[Cloudflare功能板](https://dash.cloudflare.com/login)上，执行以下步骤：

1. 转到&#x200B;**域（区域）**&#x200B;级别的&#x200B;**Logpush**&#x200B;页面。
1. 选择&#x200B;**创建日志推送作业**。
1. 在&#x200B;**选择目标**&#x200B;中，选择&#x200B;**Amazon S3**。
1. 输入以下目标信息：

   - **存储桶** — S3存储桶名称。 从“LLM Optimizer CDN配置”页面复制值。

     ![存储段名称](/help/overview/assets/log-forwarding/common/bucket-name.png)

   - **路径** — 存储容器中的存储桶位置。 从“LLM Optimizer CDN配置”页面复制值。

     ![Cloudflare路径](/help/overview/assets/log-forwarding/cloudflare/cloudflare-path.png)

   - **将日志组织到每日子文件夹中** （推荐）。

     ![每日子文件夹](/help/overview/assets/log-forwarding/cloudflare/cloudflare-daily-subfolders.png)

   - **存储段区域** — 从LLM Optimizer CDN配置页面复制值。

     <!-- ![Region](/help/overview/assets/log-forwarding/cloudflare/cloudflare-region.png)-->

   - 如果不需要服务器端加密，请不要选中。

   完成上述步骤后，选择&#x200B;**继续**。

1. 为了证明所有权， Cloudflare将向您指定的目标发送文件。 要查找该令牌，请单击所有权质询文件&#x200B;**概述**&#x200B;选项卡中的&#x200B;**打开**&#x200B;按钮。 从LLM Optimizer CDN配置页面复制所有权令牌，然后将其粘贴到Cloudflare仪表板中，验证您对存储段的访问权限。 输入所有权令牌并选择&#x200B;**继续**。

   <!--![Ownership token](/help/overview/assets/log-forwarding/cloudflare/cloudflare-ownership-token.png)-->

1. 选择要推送到存储服务的&#x200B;**HTTP请求**&#x200B;数据集。

1. 配置Logpush作业：

   - 输入&#x200B;**作业名称**。

   - 在&#x200B;**发送以下字段**&#x200B;中，查看LLM Optimizer配置页面中的值。

     ![日志推送字段](/help/overview/assets/log-forwarding/cloudflare/cloudflare-logpush-fields.png)

   - **日志格式**： JSON。

     <!--![JSON format](/help/overview/assets/log-forwarding/cloudflare/cloudflare-json-format.png)-->

1. 在&#x200B;**高级选项**&#x200B;中：

   - 选择日志中时间戳字段的格式： `RFC3339`。

     ![时间戳格式](/help/overview/assets/log-forwarding/cloudflare/cloudflare-timestamp-format.png)

   - 对于采样速率，选择&#x200B;**所有日志**。

     ![采样速率](/help/overview/assets/log-forwarding/cloudflare/cloudflare-sampling-rate.png)

1. 配置完日志推送作业后，选择&#x200B;**提交**。
