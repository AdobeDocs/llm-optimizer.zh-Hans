---
title: 日志转发 - Fastly
description: 了解如何将 Fastly 的内容传递网络日志转发到 Adobe 的 S3 存储桶，以便在 LLM Optimizer 中收集代理式流量数据。
feature: Agentic Traffic
source-git-commit: d1f98770b39f550c36d93ece9b89933c0e90f189
workflow-type: ht
source-wordcount: '381'
ht-degree: 100%

---


# 日志转发：Fastly {#log-forwarding-fastly}

本页介绍如何将 Fastly 的内容传递网络日志转发到 Adobe 的 S3 存储桶，以便收集代理式流量数据。您将使用 LLM Optimizer 的内容传递网络配置页面完成加入。完成加入流程后，请按照本页提供的步骤，在 Fastly web 控制台中配置日志转发。

## 第 1 步：在 LLM Optimizer 中完成加入 {#step-1}

在 LLM Optimizer 页面 [https://llmo.now/](https://llmo.now/) 上：

1. 转到&#x200B;**配置**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**内容传递网络配置**&#x200B;选项卡。

   ![内容传递网络配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 单击&#x200B;**开始使用**。
1. 在&#x200B;**激活 AI 流量洞察**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)
1. 选择 **Fastly (BYOCDN)**。

   ![选择 Fastly](/help/overview/assets/log-forwarding/fastly/fastly-select.png)
1. 单击&#x200B;**加入**。

## 第 2 步：在 Fastly 中创建 S3 端点 {#step-2}

要创建 S3 端点，请在 **Fastly 控制面板**&#x200B;中执行以下操作：

1. 在 Fastly 控制台中，进入&#x200B;**内容传递网络服务**（而非计算服务）。
1. 在 **Amazon Web Services S3** 区域中，单击&#x200B;**创建端点**。
1. 填写&#x200B;**创建 Amazon S3 端点**&#x200B;字段：

| 字段 | 描述 |
| --- | --- |
| **名称** | 为端点输入一个易于识别的名称。 |
| **位置** | 默认 |
| **日志格式** | 使用下方&#x200B;**日志格式字符串**&#x200B;部分提供的格式。 |
| **时间戳格式** | `%Y-%m-%dT%H:%M:%S.000` |
| **存储桶名称** | 从 LLM Optimizer 配置页面复制&#x200B;**存储桶名称**。![存储桶名称](/help/overview/assets/log-forwarding/fastly/fastly-bucket-name.png) |
| **域** | 从 LLM Optimizer 配置页面复制&#x200B;**域名**。![域名](/help/overview/assets/log-forwarding/fastly/fastly-domain-name.png) |
| **访问方法** | **用户凭据** |
| **用户凭据** | 从 LLM Optimizer 配置页面复制&#x200B;**访问密钥**&#x200B;和&#x200B;**秘密密钥**。![访问密钥](/help/overview/assets/log-forwarding/common/access-keys.png) |
| **句点** | `300` |

**日志格式字符串：**

```json
{ "timestamp": "%{strftime(\{"%Y-%m-%dT%H:%M:%S%z"\}, time.start)}V", "host": "%{if(req.http.Fastly-Orig-Host, req.http.Fastly-Orig-Host, req.http.Host)}V", "url": "%{json.escape(req.url)}V", "request_method": "%{json.escape(req.method)}V", "request_referer": "%{json.escape(req.http.referer)}V", "request_user_agent": "%{json.escape(req.http.User-Agent)}V", "response_status": %{resp.status}V, "response_content_type": "%{json.escape(resp.http.Content-Type)}V", "client_country_code": "%{client.geo.country_name}V", "time_to_first_byte": "%{time.to_first_byte}V" }
```

>[!WARNING]
>
>密码管理工具可能会自动将您的 Fastly 密码填入&#x200B;**秘密密钥**&#x200B;字段。如果 AWS 集成失败，请手动输入秘密密钥。

完成上述步骤后，单击&#x200B;**高级选项**&#x200B;并进行以下设置：

| 字段 | 描述 |
| --- | --- |
| **路径** | 从 LLM Optimizer 配置页面复制&#x200B;**路径**。![路径](/help/overview/assets/log-forwarding/fastly/fastly-path.png) |
| **选择日志行格式** | 空白 |
| **压缩** | Gzip |
| **冗余级别** | 标准 |
| **ACL** | 无 |
| **服务器端加密** | 无 |
| **最大字节数** | 0 |

完成高级选项设置后：

1. 单击&#x200B;**创建**&#x200B;以创建端点。
1. 从&#x200B;**激活**&#x200B;菜单中，选择&#x200B;**在生产环境激活**&#x200B;进行部署。

>[!NOTE]
>
>Fastly 会持续将日志流式传递到 S3，但 S3 网站和 API 仅在上传完成后才会提供文件访问。

### 示例日志条目 {#example}

以下为发送数据到 Amazon S3 的示例格式字符串：

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
