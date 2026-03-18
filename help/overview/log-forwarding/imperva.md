---
title: 日志转发 — Imperva
description: 了解如何将CDN日志从Imperva转发到Adobe的S3存储段，以便在LLM Optimizer中收集代理流量数据。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '352'
ht-degree: 4%

---


# 日志转发： Imperva {#log-forwarding-imperva}

本指南介绍如何将CDN日志从Imperva转发到Adobe的S3存储段，以进行代理流量数据收集。 您将使用LLM Optimizer CDN配置页面载入LLM Optimizer。 完成载入流程后，按照本页面上提供的步骤从Imperva Web控制台配置日志转发。

## 步骤1：在LLM Optimizer中载入 {#step-1}

在LLM Optimizer页面[https://llmo.now/](https://llmo.now/)上：

1. 转到&#x200B;**配置**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**CDN配置**&#x200B;选项卡。

   ![CDN配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)
1. 单击&#x200B;**开始**。
1. 在&#x200B;**激活AI流量分析**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)
1. 选择&#x200B;**Imperva (BYOCDN)**。

   ![选择Imperva](/help/overview/assets/log-forwarding/imperva/imperva-select.png)
1. 单击&#x200B;**板载**。

## 步骤2：在Imperva中配置日志转发 {#step-2}

在[Imperva控制台](https://my.imperva.com)上：

>[!NOTE]
>
>日志应每天发送。

1. 在[https://my.imperva.com](https://my.imperva.com)登录到您的Imperva帐户。

2. 在侧栏中，转到&#x200B;**日志** > **日志设置**（或&#x200B;**日志集成**）。

3. 选择&#x200B;**Amazon S3 ARN**&#x200B;作为连接类型（日志目标）。

4. 输入以下内容：

   | 字段 | 描述 | 注意 |
   |---|---|---|
   | **连接名称** | 连接的描述性名称（例如，生产S3日志）。 您可以重命名缺省值。 | |
   | **路径** | 将存储日志文件的文件夹的位置。 使用格式`<Amazon S3 bucket name>/<log folder>`。 例如：`MyBucket/MyImpervaLogFolder`。 | `Amazon S3 bucket name`是LLM Optimizer配置页面中的&#x200B;**存储段名称**。![存储段名称](/help/overview/assets/log-forwarding/imperva/imperva-bucket-name.png) LLM Optimizer配置页面中的日志文件夹为&#x200B;**Path**。![路径](/help/overview/assets/log-forwarding/imperva/imperva-path.png) |

5. 单击&#x200B;**测试连接**。 Imperva运行一个完整的测试，其中测试文件（没有实际数据）被发送到指定的文件夹，然后在传输完成时删除。

   - **可用** — 存储详细信息有效；您可以将日志配置为使用此连接。
   - **未定义** — 缺少所需的详细信息或测试失败。

6. 单击&#x200B;**保存**&#x200B;以存储配置。

7. 配置日志选项（日志类型、日志级别、格式、压缩）和&#x200B;**日志级别**。 您可以从LLM Optimizer配置页面获取值。

   | 字段 | 注意 |
   |---|---|
   | 日志集成模式 | ![日志集成模式](/help/overview/assets/log-forwarding/imperva/imperva-log-integration-mode.png) |
   | 投放方法 | ![传递方法](/help/overview/assets/log-forwarding/imperva/imperva-delivery-method.png) |
   | 日志类型 | ![日志类型](/help/overview/assets/log-forwarding/imperva/imperva-log-types.png) |
   | 日志级别 | ![日志级别](/help/overview/assets/log-forwarding/imperva/imperva-log-level.png) |
   | 格式化 | ![格式](/help/overview/assets/log-forwarding/imperva/imperva-format.png) |
   | 压缩日志 | ![压缩日志](/help/overview/assets/log-forwarding/imperva/imperva-compress-logs.png) |
