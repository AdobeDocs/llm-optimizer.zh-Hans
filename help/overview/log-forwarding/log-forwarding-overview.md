---
title: BYOCDN日志转发概述
description: 了解如何在LLM Optimizer中将CDN日志从您的提供商转发到Adobe的S3存储段，以进行代理流量数据收集。
feature: Agentic Traffic
source-git-commit: a1ba7684ccef9baf3452cc158fc0d6a12aa7adb8
workflow-type: tm+mt
source-wordcount: '189'
ht-degree: 3%

---


# BYOCDN日志转发概述 {#cdn-log-forwarding}

客户管理的CDN (BYOCDN)的日志转发是将CDN访问日志发送到Adobe的Amazon S3存储桶以便LLM Optimizer可以收集和分析代理流量数据的过程。 如果没有CDN日志转发，[代理流量](/help/dashboards/agentic-traffic.md)仪表板无法显示量度。

下面提供的指南遵循相同的两个阶段工作流程：

1. 在LLM Optimizer中&#x200B;**载入** — 在[CDN配置](/help/dashboards/customer-configuration.md)页面上注册您的CDN以生成所需的S3凭据和路径详细信息。
2. **配置您的CDN** — 使用这些详细信息在CDN提供商的控制台中创建日志转发作业（或手动上载日志）。

## CDN提供商 {#cdn-providers}

请遵循适用于您的CDN提供商的相应指南。

| 内容传递网络提供者 | 指南 |
|---|---|
| Akamai | [查看指南](/help/overview/log-forwarding/akamai.md) |
| Cloudflare | [查看指南](/help/overview/log-forwarding/cloudflare.md) |
| CloudFront | [查看指南](/help/overview/log-forwarding/cloudfront.md) |
| 快速地 | [查看指南](/help/overview/log-forwarding/fastly.md) |
| 因佩尔瓦 | [查看指南](/help/overview/log-forwarding/imperva.md) |
| 其他（手动/不支持的CDN） | [查看指南](/help/overview/log-forwarding/other.md) |

>[!NOTE]
>
>如果上面未列出您的CDN提供商，请使用&#x200B;**其他（手动/不支持的CDN）**&#x200B;指南，该指南涵盖手动上传、临时脚本和任何本地不支持的CDN。
