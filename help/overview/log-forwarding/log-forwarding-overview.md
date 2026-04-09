---
title: BYOCDN 日志转发概述
description: 了解如何将您所使用的内容传递网络提供商日志转发到 Adobe 的 S3 存储桶，以便在 LLM Optimizer 中收集代理式流量数据。
feature: Agentic Traffic
source-git-commit: b6e74e8706c4074a47cc355cb5f3a69a817f8a49
workflow-type: tm+mt
source-wordcount: '215'
ht-degree: 88%

---


# BYOCDN 日志转发概述 {#cdn-log-forwarding}

客户自管内容传递网络（BYOCDN）的日志转发，是将内容传递网络访问日志发送至 Adobe 的 Amazon S3 存储桶，以便 LLM Optimizer 收集并分析代理式流量数据的过程。 如果未进行内容传递网络日志转发，[代理式流量](/help/dashboards/agentic-traffic.md)仪表板将无法显示相关量度。

以下指南均遵循相同的两阶段流程：

1. **在 LLM Optimizer 中完成加入** — 在[内容传递网络配置](/help/dashboards/customer-configuration.md)页面注册您的内容传递网络，以生成所需的 S3 凭据和路径信息。
2. **配置您的内容传递网络** — 使用这些信息在内容传递网络提供商控制台中创建日志转发任务（或手动上传日志）。 对于CloudFront，您只能通过&#x200B;**AWS CLI**&#x200B;使用控制台或完成投放设置；请参阅[CloudFront (AWS CLI)](/help/overview/log-forwarding/cloudfront-cli.md)。

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
