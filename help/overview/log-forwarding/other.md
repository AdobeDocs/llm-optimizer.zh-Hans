---
title: 日志转发 - 其他（手动上传）
description: 了解在使用不受支持的内容传递网络提供商时，如何手动将内容传递网络日志上传到 Adobe 的 S3 存储桶，以便在 LLM Optimizer 中收集代理式流量数据。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: ht
source-wordcount: '670'
ht-degree: 100%

---


# 日志转发：其他（手动上传） {#log-forwarding-other}

**其他 BYOCDN** 配置方式是一种通用选项，适用于以下场景中希望向 LLM Optimizer 提供内容传递网络日志的客户：

- 优先采用&#x200B;**手动上传** — 例如，运维团队定期导出日志并上传。
- **使用临时自动化流程** — 如一次性脚本、计划导出或无服务器任务。
- 客户使用的是内置日志转发集成&#x200B;**尚不支持的内容传递网络**。

该方法模拟“持续转发”模式：日志生成后上传至指定的 S3 位置，并最终由数据接入管道自动处理。

## 第 1 步：在 LLM Optimizer 中完成加入 {#step-1}

在 [LLM Optimizer](https://llmo.now/) 上：

1. 转到&#x200B;**配置**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**内容传递网络配置**&#x200B;选项卡。

   ![内容传递网络配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 单击&#x200B;**开始使用**。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在&#x200B;**激活 AI 流量洞察**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)

1. 选择&#x200B;**其他**。

   ![选择其他](/help/overview/assets/log-forwarding/other/other-select.png)。

1. 单击&#x200B;**加入**。

<!--   ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 第 2 步：准备并上传日志 {#step-2}

### 所需日志格式（JSON Lines） {#log-format}

日志必须以换行分隔的 JSON 格式上传（**每行一个 JSON 对象**）。每一行日志必须包含以下字段，且字段名称必须&#x200B;**与下方完全一致**。

#### 字段逐项说明 {#schema}

| 字段 | 类型 | 描述 | 示例 |
|---|---|---|---|
| **时间戳** | 字符串 | 请求的时间戳，需符合 **ISO 8601** 格式。 | `"2025-02-01T23:00:05Z"` |
| **主机** | 字符串 | 客户端请求的 Web 域名。 | `"www.example.com"` |
| **url** | 字符串 | 必须包含&#x200B;**路径**&#x200B;和&#x200B;**查询参数**，但&#x200B;**不**&#x200B;应包含域名。 | `"/home?utm_source=google"` |
| **request_method** | 字符串 | HTTP 请求方法，也称为 HTTP 动词。 | `"GET"` |
| **request_user_agent** | 字符串 | HTTP 请求标头中的 User-Agent 字段。 | `"Mozilla/5.0 (compatible; GPTBot/1.0"` |
| **request_referer** | 字符串 | HTTP 请求标头中的反向链接字段（可为空）。 | `"https://chatgpt.com"` |
| **response_status** | 整数 | HTTP 响应状态代码。 | `200` |
| **response_content_type** | 字符串 | HTTP 响应标头中的 Content-Type 字段。 | `"text/html; charset=utf-8"` |
| **time_to_first_byte** | 整数 | 从建立与服务器连接到开始下载网页内容之间的时间，单位为&#x200B;**毫秒**。如果未知或不可用，请设置为 0。 | `42` |

#### 示例日志行 {#example}

以下示例展示三条日志记录：

```json
{"timestamp":"2025-02-01T23:06:14Z","host":"www.example.com","url":"/products/llm-optimizer?utm_source=google","request_method":"GET","request_user_agent":"Mozilla/5.0 (compatible; GPTBot/1.0; +https://openai.com/gptbot)","response_status":200,"request_referer":"","response_content_type":"text/html; charset=utf-8","time_to_first_byte":198}
{"timestamp":"2025-02-01T23:19:32Z","host":"www.example.com","url":"/services/ai-consulting/overview","request_method":"GET","request_user_agent":"PerplexityBot/1.0 (+https://www.perplexity.ai/perplexitybot)","response_status":200,"request_referer":"","response_content_type":"text/html; charset=utf-8","time_to_first_byte":255}
{"timestamp":"2025-02-01T23:44:05Z","host":"www.example.com","url":"/products/pricing/enterprise?utm_medium=social","request_method":"GET","request_user_agent":"ClaudeBot/1.0 (+https://www.anthropic.com)","response_status":200,"request_referer":"","response_content_type":"application/pdf","time_to_first_byte":312}
```

### 重要说明（字段拼写与类型） {#disclaimer}

数据接入与聚合管道对&#x200B;**字段名称和数据类型**&#x200B;要求非常严格。

- 字段名称必须&#x200B;**完全**&#x200B;匹配（包括大小写和拼写）。
- 数据类型必须正确，具体如下：
   - **时间戳**&#x200B;必须为符合 **ISO 8601** 格式的字符串。类 UNIX 时间戳可能无法正常解析。
   - **response_status** 必须为整数。
   - **time_to_first_byte** 必须为整数，单位为毫秒。
   - 字符串必须为有效的 JSON 字符串。
- 格式错误的 JSON 或缺失/错误字段可能导致日志被跳过或解析失败，从而造成报告数据缺失。

### 上传位置与处理周期 {#upload-location}

#### 路径规则 {#path-rule}

请按照以下格式将日志上传到对应的文件夹路径：**`yyyy/mm/dd/`**（使用斜杠分隔）。

示例（2025 年 2 月 1 日 UTC 的日志路径）：`ABC123AdobeOrg/raw/byocdn-other/2025/02/01/`

#### 处理规则 {#processing-rule}

- 在某一 **UTC 日期**&#x200B;内上传的日志，会在&#x200B;**该 UTC 日期接近结束时**&#x200B;由处理管道进行处理（每日运行）。
- 上传到&#x200B;**历史日期文件夹中**&#x200B;的日志（补录数据）将在 24 小时内&#x200B;**检测并处理**。

## 使用场景 {#scenarios}

### 场景 1：日志存储在 Splunk / Elasticsearch —— 导出并上传至 S3 {#scenario-splunk}

**目标**：从现有观测平台获取日志，并将其传递到 S3 指定位置。

- 状态代码
- 将每条事件转换为符合上述架构的 JSON 对象（JSON Lines 格式）。
- 将生成的文件上传到指定的 S3 存储桶及&#x200B;**当前 UTC 日期**&#x200B;路径：`…/byocdn-other/yyyy/mm/dd/`
- 日志将在该 UTC 日期结束前自动处理。

### 场景 2：使用 Lambda / Azure Function —— 格式化并上传至 S3 {#scenario-serverless}

**目标**：使用无服务器计算获取或接收内容传递网络日志，对其进行标准化处理，并传递到 S3 指定位置。

- 该函数从客户的数据源（日志存储、消息队列、Blob 存储等）获取日志。
- 函数将字段映射为指定结构，并输出为 **JSON Lines** 格式。
- 函数将输出上传至：`…/byocdn-other/yyyy/mm/dd/`
- 日志将在该 UTC 日期结束前自动处理。

## 快速检查清单 {#checklist}

- **每行一个 JSON 对象**（JSON Lines）
- **字段拼写必须完全一致**
- 数据类型必须正确
- **time_to_first_byte** 必须为毫秒单位的整数
- 上传至正确的 UTC 文件夹：**byocdn-other** 下的 **yyyy/mm/dd/**
