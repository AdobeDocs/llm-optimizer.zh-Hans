---
title: 日志转发 - Imperva
description: 了解如何将 Imperva 的内容传递网络日志转发到 Adobe 的 S3 存储桶，以便在 LLM Optimizer 中收集代理式流量数据。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: ht
source-wordcount: '352'
ht-degree: 100%

---


# 日志转发：Imperva {#log-forwarding-imperva}

本指南介绍如何将 Imperva 的内容传递网络日志转发到 Adobe 的 S3 存储桶，以便收集代理式流量数据。您将使用 LLM Optimizer 的内容传递网络配置页面完成加入。完成加入流程后，请按照本页提供的步骤，在 Imperva web 控制台中配置日志转发。

## 第 1 步：在 LLM Optimizer 中完成加入 {#step-1}

在 LLM Optimizer 页面 [https://llmo.now/](https://llmo.now/) 上：

1. 转到&#x200B;**配置**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**内容传递网络配置**&#x200B;选项卡。

   ![内容传递网络配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)
1. 单击&#x200B;**开始使用**。
1. 在&#x200B;**激活 AI 流量洞察**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)
1. 选择 **Imperva (BYOCDN)**。

   ![选择 Imperva](/help/overview/assets/log-forwarding/imperva/imperva-select.png)
1. 单击&#x200B;**加入**。

## 第 2 步：在 Imperva 中配置日志转发 {#step-2}

在 [Imperva 控制台](https://my.imperva.com)中：

>[!NOTE]
>
>日志应按每日发送。

1. 在 [https://my.imperva.com](https://my.imperva.com) 登录您的 Imperva 帐户。

2. 在侧边栏中，转到&#x200B;**日志** > **日志设置**（或&#x200B;**日志集成**）。

3. 选择 **Amazon S3 ARN** 作为连接类型（日志目标）。

4. 输入以下内容：

   | 字段 | 描述 | 注意 |
   |---|---|---|
   | **连接名称** | 为连接设置一个描述性名称（例如：生产 S3 日志）。您可以重命名默认名称。 | |
   | **路径** | 日志文件存储的文件夹位置。使用格式 `<Amazon S3 bucket name>/<log folder>`。例如：`MyBucket/MyImpervaLogFolder`。 | `Amazon S3 bucket name` 为 LLM Optimizer 配置页面中的&#x200B;**存储桶名称**。![存储桶名称](/help/overview/assets/log-forwarding/imperva/imperva-bucket-name.png) 日志文件夹为 LLM Optimizer 配置页面中的&#x200B;**路径**。![路径](/help/overview/assets/log-forwarding/imperva/imperva-path.png) |

5. 单击&#x200B;**测试连接**。Imperva 会执行完整测试：向指定文件夹发送一个测试文件（不包含真实数据），并在传递完成后将其删除。

   - **可用** — 存储配置有效，可以使用该连接配置日志。
   - **未定义** — 可能缺少必要信息或测试失败。

6. 单击&#x200B;**保存**&#x200B;以保存配置。

7. 配置日志选项（日志类型、日志级别、格式、压缩）以及&#x200B;**日志级别**。您可以从 LLM Optimizer 配置页面获取相应的值。

   | 字段 | 注意 |
   |---|---|
   | 日志集成模式 | ![日志集成模式](/help/overview/assets/log-forwarding/imperva/imperva-log-integration-mode.png) |
   | 投放方法 | ![投放方法](/help/overview/assets/log-forwarding/imperva/imperva-delivery-method.png) |
   | 日志类型 | ![日志类型](/help/overview/assets/log-forwarding/imperva/imperva-log-types.png) |
   | 日志级别 | ![日志级别](/help/overview/assets/log-forwarding/imperva/imperva-log-level.png) |
   | 格式 | ![格式](/help/overview/assets/log-forwarding/imperva/imperva-format.png) |
   | 压缩日志 | ![压缩日志](/help/overview/assets/log-forwarding/imperva/imperva-compress-logs.png) |
