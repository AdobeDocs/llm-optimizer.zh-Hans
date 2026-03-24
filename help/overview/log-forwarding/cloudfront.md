---
title: 日志转发 - CloudFront
description: 了解如何将 CloudFront 的内容传递网络日志转发到 Adobe 的 S3 存储桶，以便在 LLM Optimizer 中收集代理式流量数据。
feature: Agentic Traffic
source-git-commit: d1f98770b39f550c36d93ece9b89933c0e90f189
workflow-type: ht
source-wordcount: '466'
ht-degree: 100%

---


# 日志转发：CloudFront {#log-forwarding-cloudfront}

本页介绍如何将 CloudFront 的内容传递网络日志转发到 Adobe 的 S3 存储桶，以便收集代理式流量数据。您将使用 LLM Optimizer 的内容传递网络配置页面完成加入。完成加入流程后，请按照本页提供的步骤，在 CloudFront 仪表板控制台中配置日志转发。

## 第 1 步：在 LLM Optimizer 中完成加入 {#step-1}

在 LLM Optimizer 页面 [https://llmo.now/](https://llmo.now/) 上：

1. 转到&#x200B;**客户配置仪表板**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**内容传递网络配置**&#x200B;选项卡。

   ![内容传递网络配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 单击&#x200B;**开始使用**。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在&#x200B;**激活 AI 流量洞察**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)

1. 输入您的 **AWS 帐户** ID。

   ![AWS 帐户 ID](/help/overview/assets/log-forwarding/cloudfront/cloudfront-aws-account.png)

1. 选择 **CloudFront (BYOCDN)**。

   ![选择 CloudFront](/help/overview/assets/log-forwarding/cloudfront/cloudfront-select.png)

1. 单击&#x200B;**加入**。

   ![“加入”按钮](/help/overview/assets/log-forwarding/common/onboard-button.png)

## 第 2 步：启用标准日志记录（CloudFront 控制台） {#step-2}

要启用标准日志记录，请在 [AWS 管理控制台](https://aws.amazon.com/console/)中执行以下操作：

1. 访问 [CloudFront 控制台](https://console.aws.amazon.com/cloudfront/v4/home)，并[更新现有分配](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/HowToUpdateDistribution.html#HowToUpdateDistributionProcedure)。

1. 打开&#x200B;**记录**&#x200B;选项卡。

1. 选择&#x200B;**添加**，然后选择接收日志的服务，此处选择 **Amazon S3**。

1. 在&#x200B;**目标**&#x200B;中，选择或创建资源。输入&#x200B;**存储桶名称**，您可以从 LLM Optimizer 的内容传递网络配置页面复制该值。

   ![CloudFront 存储桶名称](/help/overview/assets/log-forwarding/cloudfront/cloudfront-bucket-name.png)

1. 配置&#x200B;**其他设置**：

   - **字段选择** — 选择日志文件字段。所需字段可在 LLM Optimizer 的内容传递网络配置页面查看。

     ![CloudFront 字段选择](/help/overview/assets/log-forwarding/cloudfront/cloudfront-field-selection.png)

   - **分区** — 从 LLM Optimizer 配置页面复制&#x200B;**路径后缀**。

     ![CloudFront 分区](/help/overview/assets/log-forwarding/cloudfront/cloudfront-partitioning.png)

   - **输出格式**— 格式应为 JSON。

     ![CloudFront 输出格式](/help/overview/assets/log-forwarding/cloudfront/cloudfront-output-format.png)

1. 完成相关步骤以更新或创建分配。

1. 在&#x200B;**日志**&#x200B;页面中，确认分配旁显示为&#x200B;**已启用**。

## 启用跨帐户传递的标准日志记录 {#cross-account}

**源帐户**（包含 CloudFront 分配）会将访问日志发送到&#x200B;**目标帐户**（即 LLM Optimizer 内容传递网络配置页面中显示的 S3 存储桶）。两个帐户都必须具备相应权限。

例如：源帐户 `111111111111` 将日志发送到目标帐户 `222222222222` 中的 S3 存储桶。您可以使用 [AWS 命令行界面](https://aws.amazon.com/cli/)。

>[!NOTE]
>
>在以下命令中，请将传递目标 ARN 值（`arn:aws:logs:us-east-1:222222222222:delivery-destination:cloudfront-delivery-destination`）替换为 LLM Optimizer 配置页面中的&#x200B;**传递目标 ARN**&#x200B;值。

![传递目标 ARN](/help/overview/assets/log-forwarding/cloudfront/cloudfront-delivery-destination-arn.png)

### 配置源帐户 {#source-account}

接下来，您需要配置源帐户：

1. **创建传递源** - 替换名称和分配 ARN：

   ```bash
   aws logs put-delivery-source --name s3-cf-delivery \
     --resource-arn arn:aws:cloudfront::111111111111:distribution/E1TR1RHV123ABC \
     --log-type ACCESS_LOGS
   ```

1. **创建传递** - 将源连接到目标；使用“配置目标帐户”步骤中的目标 ARN：

   ```bash
   aws logs create-delivery --delivery-source-name s3-cf-delivery \
     --delivery-destination-arn arn:aws:logs:us-east-1:222222222222:delivery-destination:cloudfront-delivery-destination
   ```

1. **验证：**

   - 在&#x200B;**源**&#x200B;帐户中：CloudFront 控制台 > 您的分配 > **记录**&#x200B;选项卡。在&#x200B;**类型**&#x200B;下，您应看到 S3 跨帐户日志传递。
   - 在&#x200B;**目标**&#x200B;帐户中：S3 控制台 > 存储桶。您应能看到前缀（例如 `MyLogPrefix`）以及该文件夹中的日志。
