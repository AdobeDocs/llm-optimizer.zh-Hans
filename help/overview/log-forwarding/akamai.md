---
title: 日志转发 - Akamai
description: 了解如何将 Akamai 的内容传递网络日志转发到 Adobe 的 S3 存储桶，以便在 LLM Optimizer 中收集代理式流量数据。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: ht
source-wordcount: '595'
ht-degree: 100%

---


# 日志转发：Akamai {#log-forwarding-akamai}

本页介绍如何将 Akamai 的内容传递网络日志转发到 Adobe 的 S3 存储桶，以便收集代理式流量数据。您将使用 LLM Optimizer 的内容传递网络配置页面完成加入。完成加入流程后，请按照本页提供的步骤，在 Akamai 控制面板中配置日志转发。

## 第 1 步：在 LLM Optimizer 中完成加入 {#step-1}

在 LLM Optimizer 页面 [https://llmo.now/](https://llmo.now/) 上：

1. 转到&#x200B;**客户配置仪表板**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**内容传递网络配置**&#x200B;选项卡。

   ![内容传递网络配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 单击&#x200B;**开始使用**。

   <!--![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在&#x200B;**激活 AI 流量洞察**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)

1. 选择 **Akamai (BYOCDN)**。

   ![选择 Akamai](/help/overview/assets/log-forwarding/akamai/akamai-select.png)

1. 单击&#x200B;**加入**。

   <!--![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 第 2 步：在 Akamai 中创建数据流 {#step-2}

在 Akamai 控制面板 [https://control.akamai.com/](https://control.akamai.com/) 中，按照官方文档步骤[创建数据流](https://techdocs.akamai.com/datastream2/docs/create-stream)。

## 第 3 步：选择数据参数 {#step-3}

创建数据流后，在 Akamai 控制面板中点击“下一步”，进入&#x200B;**数据集**&#x200B;选项卡。按照 Akamai 官方文档步骤选择[数据参数](https://techdocs.akamai.com/datastream2/docs/choose-data-parameters)。需要使用以下来自 LLM Optimizer 配置页面的字段：

![LLMO 配置字段](/help/overview/assets/log-forwarding/akamai/akamai-llmo-config-fields.png)

字段映射应如下：

* **日志信息**
reqTimeSec -> 请求时间
* **地理数据**
country -> 国家/地区
* **消息交互数据**
reqHost -> 请求主机
reqPath -> 请求路径
queryStr -> 查询字符串
reqMethod -> 请求方法
ua ->用户代理
statusCode -> HTTP 状态代码
rspContentType -> 响应内容类型
* **请求标头数据**
referer -> 反向链接
* **网络性能数据**
timeToFirstByte -> 首字节时间

Akamai 数据集字段（包含 ID）如下：

1100, # reqTimeSec -> 请求时间
2012, # country -> 国家/地区
1011, # reqHost -> 请求主机
1013, # reqPath -> 请求路径
2009, # queryStr -> 查询字符串
1012, # reqMethod -> 请求方法
1017, # ua -> 用户代理
1008, # statusCode -> HTTP 状态代码
1032, # referer -> 反向链接
1016, # rspContentType ->响应内容类型
2025 # timeToFirstByte -> 首字节时间

## 第 4 步：配置目标 {#step-4}

创建数据流并选择所需参数后，需要配置目标。请按以下步骤配置目标：

1. 在&#x200B;**目标**&#x200B;中，选择 **S3**。
2. 在&#x200B;**名称**&#x200B;中输入便于识别的描述。
3. 在&#x200B;**存储桶**&#x200B;中，复制 LLM Optimizer 配置页面中的&#x200B;**存储桶名称**。

   ![存储桶名称](/help/overview/assets/log-forwarding/common/bucket-name.png)

4. 在&#x200B;**文件夹路径**&#x200B;中，复制 LLM Optimizer 配置页面中的&#x200B;**路径**。

   ![路径配置](/help/overview/assets/log-forwarding/akamai/akamai-path-config.png)

5. 在&#x200B;**区域**&#x200B;中，复制 LLM Optimizer 配置页面中的&#x200B;**区域**。

   <!--![Region](/help/overview/assets/log-forwarding/common/region.png)-->

6. 在&#x200B;**访问密钥 ID**&#x200B;和&#x200B;**秘密访问密钥**&#x200B;中，复制 LLM Optimizer 配置页面中的对应值。

   ![访问密钥](/help/overview/assets/log-forwarding/common/access-keys.png)

7. 单击&#x200B;**验证并保存**，以验证目标连接并保存配置。在验证过程中，系统会使用提供的访问密钥标识符和秘密访问密钥，在您的 S3 文件夹中创建一个验证文件，文件名格式为 `Akamai_access_verification_[TimeStamp].txt`（包含时间戳）。只有在验证成功且您具备目标 Amazon S3 存储桶及文件夹访问权限时，才能看到该文件。

8. 在&#x200B;**传递选项**&#x200B;菜单中，按如下方式编辑&#x200B;**文件名**&#x200B;字段：

   a. 更改&#x200B;**前缀**。从 LLM Optimizer 配置页面中的&#x200B;**日志文件前缀**&#x200B;复制该值：

   ```
   {%Y}-{%m}-{%d}T{%H}:{%M}:{%S}.000
   ```

   b. 更改&#x200B;**后缀**。从 LLM Optimizer 配置页面中的&#x200B;**日志文件后缀**&#x200B;复制该值：

9. 更改&#x200B;**推送频率**。从 LLM Optimizer 配置页面中的&#x200B;**日志间隔**&#x200B;复制该值：

   ![日志间隔](/help/overview/assets/log-forwarding/akamai/akamai-log-interval.png)

10. 单击&#x200B;**下一步**&#x200B;完成流程。

在最终验证之前，配置应类似于以下示例：

![配置验证](/help/overview/assets/log-forwarding/akamai/akamai-validation.png)
