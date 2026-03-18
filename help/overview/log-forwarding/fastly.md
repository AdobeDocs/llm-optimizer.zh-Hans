---
title: 日志转发 — Fastly
description: 了解如何将CDN日志从Fastly转发到Adobe的S3存储段，以便在LLM Optimizer中收集代理流量数据。
feature: Agentic Traffic
source-git-commit: d1f98770b39f550c36d93ece9b89933c0e90f189
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 4%

---


# 日志转发： Fastly {#log-forwarding-fastly}

本页介绍如何将CDN日志从Fastly转发到Adobe的S3存储段，以进行代理流量数据收集。 您将使用LLM Optimizer CDN配置页面载入LLM Optimizer。 完成载入流程后，按照本页面上提供的步骤在Fastly Web控制台中配置日志转发。

## 步骤1：在LLM Optimizer中载入 {#step-1}

在LLM Optimizer页面[https://llmo.now/](https://llmo.now/)上：

1. 转到&#x200B;**配置**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**CDN配置**&#x200B;选项卡。

   ![CDN配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 单击&#x200B;**开始**。
1. 在&#x200B;**激活AI流量分析**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)
1. 选择&#x200B;**Fastly (BYOCDN)**。

   ![选择Fastly](/help/overview/assets/log-forwarding/fastly/fastly-select.png)
1. 单击&#x200B;**板载**。

## 步骤2：在Fastly中创建S3端点 {#step-2}

要创建S3终结点，请在&#x200B;**Fastly控制面板**&#x200B;上：

1. 在Fastly仪表板中，转到&#x200B;**CDN服务**（不是计算服务）。
1. 在&#x200B;**Amazon Web Services S3**&#x200B;区域中，单击&#x200B;**创建终结点**。
1. 填写&#x200B;**创建Amazon S3终结点**&#x200B;字段：

| 字段 | 描述 |
| --- | --- |
| **名称** | 易于用户识别的端点名称。 |
| **投放位置** | 默认 |
| **日志格式** | 使用下面的&#x200B;**日志格式字符串**&#x200B;部分中显示的日志格式字符串。 |
| **时间戳格式** | `%Y-%m-%dT%H:%M:%S.000` |
| **存储段名称** | 从LLM Optimizer配置页面复制&#x200B;**存储段名称**。![存储段名称](/help/overview/assets/log-forwarding/fastly/fastly-bucket-name.png) |
| **域** | 从LLM Optimizer配置页面复制&#x200B;**域名**。![域名](/help/overview/assets/log-forwarding/fastly/fastly-domain-name.png) |
| **访问方法** | **用户凭据** |
| **用户凭据** | 从LLM Optimizer配置页面复制&#x200B;**访问密钥**&#x200B;和&#x200B;**密钥**。![访问密钥](/help/overview/assets/log-forwarding/common/access-keys.png) |
| **周期** | `300` |

**日志格式字符串：**

```json
{ "timestamp": "%{strftime(\{"%Y-%m-%dT%H:%M:%S%z"\}, time.start)}V", "host": "%{if(req.http.Fastly-Orig-Host, req.http.Fastly-Orig-Host, req.http.Host)}V", "url": "%{json.escape(req.url)}V", "request_method": "%{json.escape(req.method)}V", "request_referer": "%{json.escape(req.http.referer)}V", "request_user_agent": "%{json.escape(req.http.User-Agent)}V", "response_status": %{resp.status}V, "response_content_type": "%{json.escape(resp.http.Content-Type)}V", "client_country_code": "%{client.geo.country_name}V", "time_to_first_byte": "%{time.to_first_byte}V" }
```

>[!WARNING]
>
>密码管理员可以使用您的Fastly密码自动填充&#x200B;**密钥**&#x200B;字段。 如果AWS集成失败，请手动输入密钥。

完成上述步骤后，单击&#x200B;**高级选项**&#x200B;并设置：

| 字段 | 描述 |
| --- | --- |
| **路径** | 从LLM Optimizer配置页面复制&#x200B;**路径**。![路径](/help/overview/assets/log-forwarding/fastly/fastly-path.png) |
| **选择日志行格式** | 空白 |
| **压缩** | Gzip |
| **冗余级别** | 标准 |
| **ACL** | 无 |
| **服务器端加密** | 无 |
| **最大字节** | 0 |

设置高级选项后：

1. 单击&#x200B;**创建**&#x200B;以创建终结点。
1. 从&#x200B;**激活**&#x200B;菜单中，选择要部署的&#x200B;**在生产上激活**。

>[!NOTE]
>
>Fastly将日志连续流式传输到S3，S3网站和API仅在上传完成后才使文件可用。

### 示例日志条目 {#example}

以下是向Amazon S3发送数据的格式字符串示例：

```json
{
  "timestamp": "2026-02-10T05:05:36+0000",
  "host": "example.com",
  "url": "/my/path",
  "request_method": "GET",
  "request_referer": "https://example.com/my/other/path",
  "request_user_agent": "Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1",
  "response_status": 200,
  "response_content_type": "text/css; charset=utf-8",
  "client_country_code": "argentina",
  "time_to_first_byte": "0.138"
}
```
