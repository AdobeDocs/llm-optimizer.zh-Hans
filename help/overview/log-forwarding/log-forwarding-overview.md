---
title: BYOCDN 日志转发概述
description: 了解如何将您所使用的内容传递网络提供商日志转发到 Adobe 的 S3 存储桶，以便在 LLM Optimizer 中收集代理式流量数据。
feature: Agentic Traffic
autotag-review: '2026-07-15T18:07:52.453Z'
TQID: 'https://experienceleague.adobe.com/iN1Tm-7j2FTQ1UodWvCZpOSy0FnQEyMkBMZIL9z3t38'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: d1956731-2adb-4bb7-8301-2b239254ac72id: ef4e63f5-cb4d-462d-bf9a-1f617edf2a3aid: e0828736-236a-487b-a478-5a635455eadc
subfeature_v2: id: d23587d6-14d6-4e3f-9ee1-cc18623832e1id: dd952468-5202-43af-a365-6e0d2e67a703id: e06fae5f-830b-4222-a469-b5e148d36465
role_v2: id: c66ffd68-0f65-42bb-aa23-b4020f12e0bd
topic_v2: id: d3cdead0-685a-4489-9250-4bb709942f66id: eddd9b14-83bd-4ff4-9072-54a4a484abb7
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 215
ht-degree: 100%

---


# BYOCDN 日志转发概述 {#cdn-log-forwarding}

客户自管内容传递网络（BYOCDN）的日志转发，是将内容传递网络访问日志发送至 Adobe 的 Amazon S3 存储桶，以便 LLM Optimizer 收集并分析代理式流量数据的过程。 如果未进行内容传递网络日志转发，[代理式流量](/help/dashboards/agentic-traffic.md)仪表板将无法显示相关量度。

以下指南均遵循相同的两阶段流程：

1. **在 LLM Optimizer 中完成加入** — 在[内容传递网络配置](/help/dashboards/customer-configuration.md)页面注册您的内容传递网络，以生成所需的 S3 凭据和路径信息。
2. **配置您的内容传递网络** — 使用这些信息在内容传递网络提供商控制台中创建日志转发任务（或手动上传日志）。 对于 CloudFront，您只能使用控制台或通过 **AWS CLI** 完成传递设置；请参阅 [CloudFront (AWS CLI)](/help/overview/log-forwarding/cloudfront-cli.md)。

## 内容传递网络提供商 {#cdn-providers}

请根据您所使用的内容传递网络提供商，选择对应指南。

| 内容传递网络提供商 | 指南 |
|---|---|
| Akamai | [查看指南](/help/overview/log-forwarding/akamai.md) |
| Cloudflare | [查看指南](/help/overview/log-forwarding/cloudflare.md) |
| CloudFront（控制台） | [查看指南](/help/overview/log-forwarding/cloudfront.md) |
| CloudFront (AWS CLI) | [查看指南](/help/overview/log-forwarding/cloudfront-cli.md) |
| Fastly | [查看指南](/help/overview/log-forwarding/fastly.md) |
| Imperva | [查看指南](/help/overview/log-forwarding/imperva.md) |
| 其他（手动 / 不支持的内容传递网络） | [查看指南](/help/overview/log-forwarding/other.md) |

>[!NOTE]
>
>如果您的内容传递网络提供商未在上述列表中，请使用&#x200B;**其他（手动 / 不支持的内容传递网络）**&#x200B;指南，该指南涵盖手动上传、临时脚本以及所有未原生支持的内容传递网络。
