---
title: 日志转发 - CloudFront (AWS CLI)
description: 使用 AWS CLI 将 CloudFront 内容传递网络日志转发到 Adobe 的 S3 存储桶，以进行传递设置和操作。
feature: Agentic Traffic
autotag-review: '2026-05-15T17:42:44.992Z'
TQID: 'https://experienceleague.adobe.com/NoVv3qv1RbtqAWGMPYC1Rz4wO-5Au1yL2e8tRKd9Hao'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: d1956731-2adb-4bb7-8301-2b239254ac72
subfeature_v2:
  - id: d23587d6-14d6-4e3f-9ee1-cc18623832e1
  - id: e69d5a42-0217-4ca5-9396-a9a826a170da
topic_v2:
  - id: d3cdead0-685a-4489-9250-4bb709942f66
  - id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: a68e70baa717d392a03249cada83337d3f51341c
workflow-type: tm+mt
source-wordcount: 379
ht-degree: 100%

---


# 日志转发：CloudFront (AWS CLI) {#log-forwarding-cloudfront-cli}

本页详细说明了如何将 CloudFront 的内容传递网络日志转发到 Adobe 的 S3 存储桶，以收集代理式流量数据。 您将使用 LLM Optimizer 的内容传递网络配置页面完成加入。 完成加入过程后，请按照此页面上提供的步骤，使用[步骤 2](#step-2-cli) 中的 [AWS 命令&#x200B;&#x200B;行界面](https://aws.amazon.com/cli/)配置日志转发。

>[!NOTE]
>
> 本指南介绍了如何使用 [AWS 命令行界面](https://aws.amazon.com/cli/)配置日志转发。 如果要使用 **CloudFront UI** 配置日志转发，请参阅[日志转发：CloudFront](/help/overview/log-forwarding/cloudfront.md)。

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

<!--  ![AWS Account ID](/help/overview/assets/log-forwarding/cloudfront/cloudfront-aws-account.png)-->

1. 选择 **CloudFront (BYOCDN)**。

   ![选择 CloudFront](/help/overview/assets/log-forwarding/cloudfront/cloudfront-select.png)

1. 单击&#x200B;**加入**。

<!-- ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 步骤 2：通过 AWS CLI 设置内容传递网络日志转发 {#step-2-cli}

按照下述方法通过 AWS CLI 设置内容传递网络日志转发：

### 配置 AWS CLI 凭据

设置 AWS CLI 凭据 MAC。 打开 ~/.aws/credentials，然后输入以下变量的值：

```text
[LLMO]
aws_access_key_id=<VALUE_OF_ACCESS_KEY_ID>
aws_secret_access_key=<VALUE_OF_SECRET_KEY>
aws_session_token=<ONLY_IF_USING_SECURITY_TOKEN_SERVICE> ## Optional
```

### 测试连接

运行以下命令，测试连接：

```bash
aws sts get-caller-identity --profile LLMO
```

成功输出示例：

```bash
aws sts get-caller-identity --profile LLMO
{
    "UserId": "AxxxxxxxxxxxP:user",
    "Account": "012345678912",
    "Arn": "arn:aws:sts::012345678912:assumed-role/klam-master-role-BatlY3dnPVinQLC/user"
}
```

### 初始化变量

将 `REPLACEME123@AdobeOrg` 替换为您组织的 Adobe IMS 组织 ID，然后执行以下命令。 此命令的输出 ID 将被称为 `TRANSFORM_IMS_ID`。

```bash
echo "REPLACEME123@AdobeOrg" | sed 's/@AdobeOrg$//' | tr '[:upper:]' '[:lower:]'
```

按照下述说明输入 `CUSTOMER`、`CDN_ID`、`ACCT1` 和 `TRANSFORM_IMS_ID` 的值，然后在您的终端上执行命令。

```bash
export PROFILE1=LLMO
export REGION1=us-east-1
export CUSTOMER=<CUSTOMER_NAME> ## No Space, user letters,numbers and dash
export CDN_ID=<YOUR_CLOUDFRONT_DISTRIBUTION_ID>
export ACCT1=<YOUR_AWS_ACCOUNT_NUMBER>
export DELIVERY_DEST_ARN=arn:aws:logs:us-east-1:640168421876:delivery-destination:cdn-logs-<TRANSFORM_IMS_ID>  ## Replace TRANSFORM_IMS_ID with the output of the command above
```

<!--Use the **Delivery destination ARN** and org values from the LLM Optimizer CDN configuration page if they differ from the pattern above.-->

### 创建传递来源

在执行步骤 3 的同一个终端上，运行以下命令：

```bash
aws logs put-delivery-source --name llmo-cf-${CUSTOMER}-${CDN_ID} \
  --profile $PROFILE1 --region $REGION1 \
  --resource-arn arn:aws:cloudfront::${ACCT1}:distribution/${CDN_ID} \
  --log-type ACCESS_LOGS
```

>[!IMPORTANT]
>
>如果收到以下错误，请搜索现有的传递来源：*调用 PutDeliverySource 操作时出现错误 (ConflictException)：这个资源 Id 已在此帐户中的另一个传递来源中使用。*
>
>要搜索现有的传递来源，执行此命令：
>
>```bash
>aws logs describe-delivery-sources --region us-east-1 \
>--query "deliverySources[?contains(resourceArns[0], '<CDN DistributionID>')]"
>```
>
>在下一个命令中，使用上述命令执行结果中的传递来源名称。

### 创建传递配置

```bash
aws logs create-delivery \
  --profile "$PROFILE1" --region "$REGION1" \
  --delivery-source-name "llmo-cf-${CUSTOMER}-${CDN_ID}" \
  --delivery-destination-arn $DELIVERY_DEST_ARN \
  --s3-delivery-configuration '{"suffixPath":"/{yyyy}/{MM}/{dd}/{HH}"}' \
  --record-fields 'date' 'time' 'x-edge-location' 'cs-method' 'cs(Host)' 'cs-uri-stem' 'sc-status' 'cs(Referer)' 'cs(User-Agent)' 'time-to-first-byte' 'sc-content-type' 'x-host-header'
```

&lt;!--如果文档或产品值发生变化，请将 `--record-fields` 和 `--s3-delivery-configuration` 符合 LLM Optimizer 内容传递网络配置页面上显示的字段列表和路径后缀。-->
