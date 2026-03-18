---
title: 日志转发 — Akamai
description: 了解如何将CDN日志从Akamai转发到Adobe的S3存储段，以便在LLM Optimizer中收集代理流量数据。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '595'
ht-degree: 0%

---


# 日志转发： Akamai {#log-forwarding-akamai}

本页介绍如何将CDN日志从Akamai转发到Adobe的S3存储段，以便进行代理流量数据收集。 您将使用LLM Optimizer CDN配置页面载入LLM Optimizer。 完成载入流程后，按照本页中提供的步骤在Akamai控制面板中配置日志转发。

## 步骤1：在LLM Optimizer中载入 {#step-1}

在LLM Optimizer页面[https://llmo.now/](https://llmo.now/)上：

1. 转到&#x200B;**客户配置仪表板**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**CDN配置**&#x200B;选项卡。

   ![CDN配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 单击&#x200B;**开始**。

   <!--![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在&#x200B;**激活AI流量分析**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)

1. 选择&#x200B;**Akamai (BYOCDN)**。

   ![选择Akamai](/help/overview/assets/log-forwarding/akamai/akamai-select.png)

1. 单击&#x200B;**板载**。

   <!--![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 步骤2：在Akamai中创建流 {#step-2}

在Akamai控制面板[https://control.akamai.com/](https://control.akamai.com/)上，按照官方Akamai文档中的步骤来[创建流](https://techdocs.akamai.com/datastream2/docs/create-stream)。

## 步骤3：选择数据参数 {#step-3}

创建流后，在Akamai控制面板上，单击“下一步”继续&#x200B;**数据集**&#x200B;选项卡。 按照官方Akamai文档中的步骤选择[数据参数](https://techdocs.akamai.com/datastream2/docs/choose-data-parameters)。 LLM Optimizer配置中的以下字段将需要：

![LLMO配置字段](/help/overview/assets/log-forwarding/akamai/akamai-llmo-config-fields.png)

映射应如下所示：

* **日志信息**
reqTimeSec ->请求时间
* **地理数据**
国家/地区 — >国家/地区
* **邮件交换数据**
reqHost ->请求主机
reqPath ->请求路径
queryStr ->查询字符串
reqMethod -> Request方法
ua ->用户代理
statusCode -> HTTP状态代码
rspContentType ->响应内容类型
* **请求标头数据**
referer -> Referer
* **网络性能数据**
timeToFirstByte ->到第一个字节的时间

Akamai数据集字段（包括ID）如下所示：

1100， # reqTimeSec ->请求时间
2012，#国家/地区 — >国家/地区
1011， # reqHost ->请求主机
1013， # reqPath ->请求路径
2009， # queryStr ->查询字符串
1012，# reqMethod -> Request method
1017， # ua -> User-Agent
1008， # statusCode -> HTTP状态代码
1032， #引用者 — >引用者
1016， # rspContentType ->响应内容类型
2025 # timeToFirstByte -> Time to first byte

## 步骤4：配置目标 {#step-4}

在创建数据流并选择配置目标所需的参数之后。 要配置目标，请执行以下步骤：

1. 在&#x200B;**目标**&#x200B;中，选择&#x200B;**S3**。
2. 在&#x200B;**名称**&#x200B;中，输入易于用户识别的目标的描述。
3. 在&#x200B;**Bucket**&#x200B;中，从LLM Optimizer配置页面复制&#x200B;**Bucket名称**。

   ![存储段名称](/help/overview/assets/log-forwarding/common/bucket-name.png)

4. 在&#x200B;**文件夹路径**&#x200B;中，从LLM Optimizer配置页面复制&#x200B;**路径**。

   ![路径配置](/help/overview/assets/log-forwarding/akamai/akamai-path-config.png)

5. 在&#x200B;**区域**&#x200B;中，从LLM Optimizer配置页面复制&#x200B;**区域**。

   <!--![Region](/help/overview/assets/log-forwarding/common/region.png)-->

6. 在&#x200B;**访问密钥ID**&#x200B;和&#x200B;**访问密钥**&#x200B;中，从LLM Optimizer配置页面复制这两个值。

   ![访问密钥](/help/overview/assets/log-forwarding/common/access-keys.png)

7. 单击&#x200B;**验证并保存**&#x200B;以验证与目标的连接，并保存您提供的详细信息。 作为此验证过程的一部分，系统使用提供的访问密钥标识符和秘密访问密钥在S3文件夹中创建验证文件，其文件名中的时间戳为`Akamai_access_verification_[TimeStamp].txt`格式。 仅当验证过程成功并且您有权访问您尝试将日志发送到的Amazon S3存储段和文件夹时，您才能查看此文件。

8. 在&#x200B;**传递选项**&#x200B;菜单中，编辑&#x200B;**文件名**&#x200B;字段，如下所示：

   答： 更改&#x200B;**前缀** 从LLM Optimizer配置页复制&#x200B;**日志文件前缀**&#x200B;下的值：

   ```
   {%Y}-{%m}-{%d}T{%H}:{%M}:{%S}.000
   ```

   b. 更改&#x200B;**后缀**。 从LLM Optimizer配置页的&#x200B;**日志文件后缀**&#x200B;下复制值。

9. 更改&#x200B;**推送频率**。 从LLM Optimizer配置页的&#x200B;**日志间隔**&#x200B;下复制值。

   ![日志间隔](/help/overview/assets/log-forwarding/akamai/akamai-log-interval.png)

10. 单击&#x200B;**下一步**&#x200B;以完成该过程。

在最终验证之前，配置应当类似于以下示例：

![配置验证](/help/overview/assets/log-forwarding/akamai/akamai-validation.png)
