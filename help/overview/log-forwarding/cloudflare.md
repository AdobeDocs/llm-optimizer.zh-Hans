---
title: 日志转发 - Cloudflare
description: 了解如何将 Cloudflare 的内容传递网络日志转发到 Adobe 的 S3 存储桶，以便在 LLM Optimizer 中收集代理式流量数据。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: ht
source-wordcount: '381'
ht-degree: 100%

---


# 日志转发：Cloudflare {#log-forwarding-cloudflare}

本页详细说明如何将 Cloudflare 的内容传递网络日志转发到 Adobe 的 S3 存储桶，以便收集代理式流量数据。您将使用 LLM Optimizer 的内容传递网络配置页面完成加入。完成加入流程后，请按照本页提供的步骤，在 Cloudflare 仪表板控制台中配置日志转发。

## 第 1 步：在 LLM Optimizer 中完成加入 {#step-1}

在 LLM Optimizer 页面 [https://llmo.now/](https://llmo.now/) 上：

1. 转到&#x200B;**客户配置仪表板**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**内容传递网络配置**&#x200B;选项卡。

   ![内容传递网络配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 单击&#x200B;**开始使用**。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png) -->

1. 在&#x200B;**激活 AI 流量洞察**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)

1. 选择 **Cloudflare (BYOCDN)**。

   ![选择 Cloudflare](/help/overview/assets/log-forwarding/cloudflare/cloudflare-select.png)

1. 单击&#x200B;**加入**。

   <!-- ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 第 2 步：在 Cloudflare 中创建 Logpush 任务 {#step-2}

在 [Cloudflare 仪表板](https://dash.cloudflare.com/login)上，执行以下步骤：

1. 在&#x200B;**域（区域）**&#x200B;级别进入 **Logpush** 页面。
1. 选择&#x200B;**创建 Logpush 任务**。
1. 在&#x200B;**选择目标**&#x200B;中，选择 **Amazon S3**。
1. 输入以下目标信息：

   - **存储桶** — S3 存储桶名称。从 LLM Optimizer 内容传递网络配置页面复制该值。

     ![存储桶名称](/help/overview/assets/log-forwarding/common/bucket-name.png)

   - **路径** — 存储桶中的路径位置。从 LLM Optimizer 内容传递网络配置页面复制该值。

     ![Cloudflare 路径](/help/overview/assets/log-forwarding/cloudflare/cloudflare-path.png)

   - 建议启用&#x200B;**按每日子文件夹组织日志**。

     ![每日子文件夹](/help/overview/assets/log-forwarding/cloudflare/cloudflare-daily-subfolders.png)

   - **存储桶区域** — 从 LLM Optimizer 内容传递网络配置页面复制该值。

     <!-- ![Region](/help/overview/assets/log-forwarding/cloudflare/cloudflare-region.png)-->

   - 如果不需要服务器端加密，可保持未选中。

   完成以上步骤后，选择&#x200B;**继续**。

1. 为验证所有权，Cloudflare 会向指定目标发送一个文件。要获取令牌，请在所有权验证文件的&#x200B;**概述**&#x200B;选项卡中点击&#x200B;**打开**&#x200B;按钮。从 LLM Optimizer 内容传递网络配置页面复制所有权令牌，并粘贴到 Cloudflare 仪表板中，以验证您对该存储桶的访问权限。输入所有权令牌，然后选择&#x200B;**继续**。

   <!--![Ownership token](/help/overview/assets/log-forwarding/cloudflare/cloudflare-ownership-token.png)-->

1. 选择 **HTTP 请求**&#x200B;数据集并推送至存储服务。

1. 配置 Logpush 任务：

   - 输入&#x200B;**任务名称**。

   - 在&#x200B;**发送以下字段**&#x200B;中，请参考 LLM Optimizer 配置页面中的字段值。

     ![Logpush 字段](/help/overview/assets/log-forwarding/cloudflare/cloudflare-logpush-fields.png)

   - **日志格式**：JSON。

     <!--![JSON format](/help/overview/assets/log-forwarding/cloudflare/cloudflare-json-format.png)-->

1. 在&#x200B;**高级选项**&#x200B;中：

   - 选择日志中时间戳字段的格式：`RFC3339`。

     ![时间戳格式](/help/overview/assets/log-forwarding/cloudflare/cloudflare-timestamp-format.png)

   - 在采样率设置中，选择&#x200B;**所有日志**。

     ![采样率](/help/overview/assets/log-forwarding/cloudflare/cloudflare-sampling-rate.png)

1. 完成 Logpush 任务配置后，选择&#x200B;**提交**。
