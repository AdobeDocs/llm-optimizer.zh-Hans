---
title: 日志转发 — 其他（手动上传）
description: 了解在使用不受支持的CDN提供商时，如何在LLM Optimizer中手动将CDN日志上传到Adobe的S3存储段，以收集代理流量数据。
feature: Agentic Traffic
source-git-commit: b590cd14ba7d64e56a6c972fd6090e2df9de58f6
workflow-type: tm+mt
source-wordcount: '670'
ht-degree: 2%

---


# 日志转发：其他（手动上传） {#log-forwarding-other}

**其他BYOCDN**&#x200B;配置方法是一个通用选项，适用于希望在以下情况下向LLM Optimizer提供CDN日志的客户：

- **首选手动上传** — 例如，运营团队导出日志并定期上传。
- **使用了临时自动化进程** — 一次性脚本、计划导出、无服务器作业。
- 客户使用的&#x200B;**CDN不受内置日志转发集成的原生支持**。

此方法模拟“连续转发”模型：生成日志并将其上传到预期的S3位置，最后由摄取管道自动处理。

## 步骤1：在LLM Optimizer中载入 {#step-1}

在[LLM Optimizer](https://llmo.now/)：

1. 转到&#x200B;**配置**。

   ![“配置”按钮](/help/overview/assets/log-forwarding/common/config-button.png)

1. 单击&#x200B;**CDN配置**&#x200B;选项卡。

   ![CDN配置选项卡](/help/overview/assets/log-forwarding/common/cdn-config-tab.png)

1. 单击&#x200B;**开始**。

   <!-- ![Onboard CDN button](/help/overview/assets/log-forwarding/common/onboard-cdn-button.png)-->

1. 在&#x200B;**激活AI流量分析**&#x200B;旁边，单击&#x200B;**配置**。

   ![配置](/help/overview/assets/log-forwarding/common/configure.png)

1. 选择&#x200B;**其他**。

   ![选择其他](/help/overview/assets/log-forwarding/other/other-select.png)

1. 单击&#x200B;**板载**。

<!--   ![Onboard button](/help/overview/assets/log-forwarding/common/onboard-button.png)-->

## 步骤2：准备和上传日志 {#step-2}

### 必需的日志格式（JSON行） {#log-format}

日志必须以换行符分隔的JSON形式上传（每行&#x200B;**一个JSON对象**）。 每个日志行必须包括以下拼写完全低于&#x200B;**的字段**。

#### 逐字段架构 {#schema}

| 字段 | 类型 | 描述 | 示例 |
|---|---|---|---|
| **timestamp** | 字符串 | 请求的时间戳遵循&#x200B;**ISO 8601**&#x200B;格式。 | `"2025-02-01T23:00:05Z"` |
| **host** | 字符串 | 客户端请求的Web域。 | `"www.example.com"` |
| **url** | 字符串 | 需要&#x200B;**path**&#x200B;和&#x200B;**查询参数**，而域应该&#x200B;**不包括**。 | `"/home?utm_source=google"` |
| **request_method** | 字符串 | HTTP请求方法，有时称为HTTP谓词。 | `"GET"` |
| **request_user_agent** | 字符串 | HTTP User-Agent请求标头。 | `"Mozilla/5.0 (compatible; GPTBot/1.0"` |
| **request_referer** | 字符串 | HTTP Referer请求标头（不能为空）。 | `"https://chatgpt.com"` |
| **response_status** | 整数 | HTTP响应状态代码。 | `200` |
| **response_content_type** | 字符串 | HTTP内容类型响应标头。 | `"text/html; charset=utf-8"` |
| **time_to_first_byte** | 整数 | 从创建到服务器的连接到下载网页内容之间的时间，以&#x200B;**毫秒为单位**。 如果未知或不可用，则设置为零。 | `42` |

#### 示例日志行 {#example}

以下示例显示了三个日志行：

```json
{"timestamp":"2025-02-01T23:06:14Z","host":"www.example.com","url":"/products/llm-optimizer?utm_source=google","request_method":"GET","request_user_agent":"Mozilla/5.0 (compatible; GPTBot/1.0; +https://openai.com/gptbot)","response_status":200,"request_referer":"","response_content_type":"text/html; charset=utf-8","time_to_first_byte":198}
{"timestamp":"2025-02-01T23:19:32Z","host":"www.example.com","url":"/services/ai-consulting/overview","request_method":"GET","request_user_agent":"PerplexityBot/1.0 (+https://www.perplexity.ai/perplexitybot)","response_status":200,"request_referer":"","response_content_type":"text/html; charset=utf-8","time_to_first_byte":255}
{"timestamp":"2025-02-01T23:44:05Z","host":"www.example.com","url":"/products/pricing/enterprise?utm_medium=social","request_method":"GET","request_user_agent":"ClaudeBot/1.0 (+https://www.anthropic.com)","response_status":200,"request_referer":"","response_content_type":"application/pdf","time_to_first_byte":312}
```

### 关键免责声明（拼写和类型） {#disclaimer}

引入和聚合管道对&#x200B;**字段名和数据类型**&#x200B;有严格要求。

- 字段名称必须与&#x200B;**完全匹配**（大小写和拼写）。
- 数据类型应正确，如下所示：
   - **timestamp**&#x200B;必须是格式为&#x200B;**ISO 8601**&#x200B;的字符串。 类似UNIX的时间戳可能不起作用。
   - **response_status**&#x200B;必须为整数。
   - **time_to_first_byte**&#x200B;必须为整数并使用毫秒。
   - 字符串必须是有效的JSON字符串。
- JSON格式错误或字段/字段缺失或错误可能会导致日志被跳过或无法解析，从而导致报表中缺少数据。

### 上传位置和处理节奏 {#upload-location}

#### 路径规则 {#path-rule}

使用以下格式上载相应文件夹路径下的日志： **`yyyy/mm/dd/`**（带斜杠）。

2025年2月1日UTC的示例日志： `ABC123AdobeOrg/raw/byocdn-other/2025/02/01/`

#### 处理规则 {#processing-rule}

- 在给定&#x200B;**UTC日期**&#x200B;期间上载的日志将在该UTC日期&#x200B;**接近结束时由管道**&#x200B;处理（每日运行）。
- 上载到&#x200B;**前几天文件夹** （回填）的日志在24小时内被&#x200B;**检测并处理**。

## 方案 {#scenarios}

### 场景1：Splunk/Elasticsearch中的日志 — 导出并上传到S3 {#scenario-splunk}

**目标**：从现有可观察性平台检索日志，并将日志交付到S3位置。

- 从Splunk/Elastic搜索事件中提取必填字段。
- 按照上述架构（JSON线）将每个事件转换为一个JSON对象。
- 将结果文件上传到指定的S3存储段和&#x200B;**当前UTC时间**&#x200B;路径： `…/byocdn-other/yyyy/mm/dd/`
- 日志将在UTC时间结束时自动处理。

### 场景2：Lambda/Azure函数 — 设置格式并上传到S3 {#scenario-serverless}

**目标**：使用无服务器计算获取/接收CDN日志，对其进行标准化并将其交付到S3位置。

- 函数从客户的源（日志存储、队列、Blob存储等）检索日志。
- 函数将字段映射到预期的架构并发出&#x200B;**JSON行**。
- 函数将输出上传到： `…/byocdn-other/yyyy/mm/dd/`
- 日志将在UTC时间结束时自动处理。

## 快速核对清单 {#checklist}

- 每行&#x200B;**一个JSON对象** （JSON行）
- **指定的字段拼写**&#x200B;完全相同
- 正确的数据类型
- **time_to_first_byte**（以毫秒为单位，整数）
- 上载到相应的UTC文件夹： **byocdn-other**&#x200B;下的&#x200B;**yyyy/mm/dd/**
