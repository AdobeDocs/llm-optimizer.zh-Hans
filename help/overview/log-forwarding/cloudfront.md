---
title: 日志转发 — CloudFront
description: 了解如何将CDN日志从CloudFront转发到Adobe的S3存储段，以便在LLM Optimizer中收集代理流量数据。
feature: Agentic Traffic
source-git-commit: d1f98770b39f550c36d93ece9b89933c0e90f189
workflow-type: tm+mt
source-wordcount: '466'
ht-degree: 0%

---


# 日志转发： CloudFront {#log-forwarding-cloudfront}

本页介绍如何将CDN日志从CloudFront转发到Adobe的S3存储段，以进行代理流量数据收集。 您将使用LLM Optimizer CDN配置页面载入LLM Optimizer。 完成载入流程后，按照本页面上提供的步骤在CloudFront仪表板控制台中配置日志转发。

## 步骤1：在LLM Optimizer中载入 {#step-1}

在LLM Optimizer页面[https://llmo.now/](https://llmo.now/)上：

1. 转到&#x200B;**客户配置仪表板**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**CDN配置**&#x200B;选项卡。

   ![CDN配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 单击&#x200B;**开始**。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在&#x200B;**激活AI流量分析**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)

1. 输入您的&#x200B;**AWS帐户** ID。

   ![AWS帐户ID](/help/overview/assets/log-forwarding/cloudfront/cloudfront-aws-account.png)

1. 选择&#x200B;**CloudFront (BYOCDN)**。

   ![选择CloudFront](/help/overview/assets/log-forwarding/cloudfront/cloudfront-select.png)

1. 单击&#x200B;**板载**。

   ![板载按钮](/help/overview/assets/log-forwarding/common/onboard-button.png)

## 步骤2：启用标准日志记录（CloudFront控制台） {#step-2}

要启用标准日志记录，请从[AWS管理控制台](https://aws.amazon.com/console/)：

1. 访问[CloudFront控制台](https://console.aws.amazon.com/cloudfront/v4/home)并[更新现有分配](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HowToUpdateDistribution.html#HowToUpdateDistributionProcedure)。

1. 打开&#x200B;**日志记录**&#x200B;选项卡。

1. 选择&#x200B;**添加**，然后选择要接收日志的服务，在本例中为&#x200B;**Amazon S3**。

1. 对于&#x200B;**目标**，请选择或创建资源。 输入&#x200B;**存储段名称**，您可以从LLM Optimizer CDN配置页面复制该值。

   ![CloudFront存储段名称](/help/overview/assets/log-forwarding/cloudfront/cloudfront-bucket-name.png)

1. 配置&#x200B;**其他设置**：

   - **字段选择** — 选择日志文件字段。 请参阅LLM Optimizer CDN配置页面上的必填字段。

     ![CloudFront字段选择](/help/overview/assets/log-forwarding/cloudfront/cloudfront-field-selection.png)

   - **分区** — 从LLM Optimizer配置页面复制&#x200B;**路径后缀**。

     ![CloudFront分区](/help/overview/assets/log-forwarding/cloudfront/cloudfront-partitioning.png)

   - **输出格式** — 格式应为JSON。

     ![CloudFront输出格式](/help/overview/assets/log-forwarding/cloudfront/cloudfront-output-format.png)

1. 完成更新或创建分发的步骤。

1. 在&#x200B;**日志**&#x200B;页面上，确认&#x200B;**已启用**&#x200B;显示在分发旁边。

## 为跨帐户投放启用标准日志记录 {#cross-account}

**源帐户**（具有CloudFront分发）将访问日志发送到&#x200B;**目标帐户**（ LLM Optimizer CDN配置页面中显示的S3存储段）。 两个帐户必须具有正确的权限。

例如：源帐户`111111111111`将日志发送到目标帐户`222222222222`中的S3存储段。 您可以使用[AWS命令行界面](https://aws.amazon.com/cli/)。

>[!NOTE]
>
>在以下命令中，将投放目标ARN值(`arn:aws:logs:us-east-1:222222222222:delivery-destination:cloudfront-delivery-destination`)替换为LLM Optimizer配置页面中的&#x200B;**投放目标ARN**&#x200B;的值。

![传递目标ARN](/help/overview/assets/log-forwarding/cloudfront/cloudfront-delivery-destination-arn.png)

### 配置源帐户 {#source-account}

接下来，您需要配置源帐户：

1. **创建投放源** — 替换名称和分发ARN：

   ```bash
   aws logs put-delivery-source --name s3-cf-delivery \
     --resource-arn arn:aws:cloudfront::111111111111:distribution/E1TR1RHV123ABC \
     --log-type ACCESS_LOGS
   ```

1. **创建投放** — 将源链接到目标；使用“配置目标帐户”步骤中的目标ARN：

   ```bash
   aws logs create-delivery --delivery-source-name s3-cf-delivery \
     --delivery-destination-arn arn:aws:logs:us-east-1:222222222222:delivery-destination:cloudfront-delivery-destination
   ```

1. **验证：**

   - 在&#x200B;**源**&#x200B;帐户中： CloudFront控制台>您的分发> **日志记录**&#x200B;选项卡。 在&#x200B;**类型**&#x200B;下，您应该会看到S3跨帐户日志投放。
   - 在&#x200B;**目标**&#x200B;帐户中： S3控制台>存储段。 您应该会看到该文件夹中的前缀（例如`MyLogPrefix`）和日志。
