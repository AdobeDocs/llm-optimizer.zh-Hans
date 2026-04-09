---
title: 日志转发 — CloudFront (AWS CLI)
description: 使用AWS CLI将CloudFront CDN日志转发到Adobe的S3存储段，以进行投放设置和操作。
feature: Agentic Traffic
source-git-commit: 0d51bbde954c399dc6595522fa70b576461f458a
workflow-type: tm+mt
source-wordcount: '384'
ht-degree: 20%

---


# 日志转发：CloudFront (AWS CLI) {#log-forwarding-cloudfront-cli}

本页详细介绍如何将CDN日志从CloudFront转发到Adobe的S3存储段以进行代理流量数据收集。 您将使用 LLM Optimizer 的内容传递网络配置页面完成加入。 完成载入流程后，请按照本页面上提供的步骤操作，在[步骤2](#step-2-cli)中使用[AWS命令行界面](https://aws.amazon.com/cli/)配置日志转发。

>[!NOTE]
>
> 本指南介绍如何使用[AWS命令行界面](https://aws.amazon.com/cli/)配置日志转发。 如果要使用&#x200B;**CloudFront UI**&#x200B;配置日志转发，请参阅[日志转发： CloudFront](/help/overview/log-forwarding/cloudfront.md)。

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

## 步骤2：使用AWS CLI设置CDN日志转发 {#step-2-cli}

使用AWS CLI设置CDN日志转发，如下所示：

### 配置AWS CLI凭据

设置AWS CLI凭据MAC。 打开~/.aws/credentials并输入以下变量的值：

```text
[LLMO]
aws_access_key_id=<VALUE_OF_ACCESS_KEY_ID>
aws_secret_access_key=<VALUE_OF_SECRET_KEY>
aws_session_token=<ONLY_IF_USING_SECURITY_TOKEN_SERVICE> ## Optional
```

### 测试连接

运行以下命令以测试连接：

```bash
aws sts get-caller-identity --profile LLMO
```

成功输出的示例：

```bash
aws sts get-caller-identity --profile LLMO
{
    "UserId": "AxxxxxxxxxxxP:user",
    "Account": "012345678912",
    "Arn": "arn:aws:sts::012345678912:assumed-role/klam-master-role-BatlY3dnPVinQLC/user"
}
```

### 初始化变量

将`REPLACEME123@AdobeOrg`替换为您的组织Adobe IMS组织ID并运行以下命令。 此命令的输出ID将被称为`TRANSFORM_IMS_ID`。

```bash
echo "REPLACEME123@AdobeOrg" | sed 's/@AdobeOrg$//' | tr '[:upper:]' '[:lower:]'
```

按照以下准则输入`CUSTOMER`、`CDN_ID`、`ACCT1`和`TRANSFORM_IMS_ID`的值，然后从终端运行命令。

```bash
export PROFILE1=LLMO
export REGION1=us-east-1
export CUSTOMER=<CUSTOMER_NAME> ## No Space, user letters,numbers and dash
export CDN_ID=<YOUR_CLOUDFRONT_DISTRIBUTION_ID>
export ACCT1=<YOUR_AWS_ACCOUNT_NUMBER>
export DELIVERY_DEST_ARN=arn:aws:logs:us-east-1:640168421876:delivery-destination:cdn-logs-<TRANSFORM_IMS_ID>-ams  ## Replace TRANSFORM_IMS_ID with the output of the command above 
```

<!--Use the **Delivery destination ARN** and org values from the LLM Optimizer CDN configuration page if they differ from the pattern above.-->

### 创建投放源

在执行步骤3的同一终端上，运行以下命令：

```bash
aws logs put-delivery-source --name llmo-cf-${CUSTOMER}-${CDN_ID} \
  --profile $PROFILE1 --region $REGION1 \
  --resource-arn arn:aws:cloudfront::${ACCT1}:distribution/${CDN_ID} \
  --log-type ACCESS_LOGS
```

>[!IMPORTANT]
>
>如果收到以下错误，请搜索现有的投放源： *调用PutDeliverySource操作时出现错误(ConflictException)：此ResourceId已在此帐户中的另一个Delivery Source中使用。*
>
>要搜索现有的投放源，请运行此命令：
>
>```bash
>aws logs describe-delivery-sources --region us-east-1 \
>--query "deliverySources[?contains(resourceArns[0], '<CDN DistributionID>')]"
>```
>
>在下一个命令中，使用上述命令结果中的投放源名称。

### 创建投放配置

```bash
aws logs create-delivery \
  --profile "$PROFILE1" --region "$REGION1" \
  --delivery-source-name "llmo-cf-${CUSTOMER}-${CDN_ID}" \
  --delivery-destination-arn $DELIVERY_DEST_ARN \
  --s3-delivery-configuration '{"suffixPath":"/{yyyy}/{MM}/{dd}/{HH}"}' \
  --record-fields 'date' 'time' 'x-edge-location' 'cs-method' 'cs(Host)' 'cs-uri-stem' 'sc-status' 'cs(Referer)' 'cs(User-Agent)' 'time-to-first-byte' 'sc-content-type' 'x-host-header'
```

&lt;！ — 如果文档或产品值发生更改，请将`--record-fields`和`--s3-delivery-configuration`与LLM Optimizer CDN配置页面上显示的字段列表和路径后缀对齐。—>
