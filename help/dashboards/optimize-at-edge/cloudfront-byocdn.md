---
title: 在Edge中优化 — CloudFront (BYOCDN)
description: 了解如何在LLM Optimizer中为Edge中的优化配置CloudFront BYOCDN。
feature: Opportunities
source-git-commit: 1253d0f0a58f6523699c52fbfab23028dc469c82
workflow-type: tm+mt
source-wordcount: '2228'
ht-degree: 1%

---


# CloudFront (BYOCDN)

此配置将代理流量（来自AI机器人和LLM用户代理的请求）路由到Edge优化后端服务(`live.edgeoptimize.net`)。 与往常一样，我们仍将从您的源头为人类访客和SEO机器人提供服务。 要测试配置，请在设置完成后查找响应中的标头`x-edgeoptimize-request-id`。

**先决条件**

在设置CloudFront配置之前，请确保您具有：

* 为网站提供服务的现有CloudFront分发。
* 用于创建Lambda函数、IAM角色、CloudFront分配和缓存策略的AWS IAM权限。
* 已完成LLM Optimizer载入流程。
* 已完成到LLM Optimizer的CDN日志转发。
* 从Edge UI检索到LLM Optimizer优化API密钥。

{{retrieve-byocdn-api-key}}

**步骤1：创建Edge优化源**

**导航：** AWS Console > CloudFront > Distributions > [您的分配] > Originates选项卡

1. 单击&#x200B;**创建来源**。

2. 配置源：
   * **原始域：** `live.edgeoptimize.net`
   * **名称：** `EdgeOptimize_Origin`

3. 将所有其他字段保留为其默认值。

4. 添加自定义标头：

   | 页眉 | 值 |
   |--------|-------|
   | `x-edgeoptimize-api-key` | 您的API密钥 |
   | `x-forwarded-host` | `www.example.com` |

   将`www.example.com`替换为实际的网站域，将`Your API key`替换为您的Edge代表提供的Adobe优化API密钥。

5. 单击&#x200B;**创建来源**。

![Cloudfront源创建](/help/assets/optimize-at-edge/cloudfront-origin-creation.png)

**步骤2：创建查看器请求函数**

**导航：** AWS Console > CloudFront >函数

1. 单击&#x200B;**创建函数**。

2. 配置：
   * **名称：** `edgeoptimize-routing`
   * **运行时：** `cloudfront-js-2.0`

3. 将默认代码替换为[viewer-request.js](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/cloudfront-function/viewer-request.js)中的代码。

   在发布之前，请自定义代码中的以下值：

   * `YOUR_DEFAULT_ORIGIN` — 将替换为您现有的默认源的名称（可在CloudFront > Distributions > [Your Distribution] > Origines选项卡中找到）。
   * `TARGETED_PATHS` — 设置为`null`以定位所有HTML页面，或设置为特定路径的数组，例如`['/', '/products', '/about']`。

4. 单击&#x200B;**保存更改** > **发布函数**。

![Cloudfront函数创建](/help/assets/optimize-at-edge/cloudfront-function-creation.png)


**步骤3：配置缓存策略**

**导航：** AWS Console > CloudFront >分发> [您的分发] >行为

检查当前附加到您行为的缓存策略。 单击行为上的&#x200B;**编辑**，查看&#x200B;**缓存密钥和源请求**&#x200B;部分以识别您的方案：

* **方案A （旧版）：**&#x200B;您看到一个单选按钮，标记为&#x200B;**已选择旧版缓存设置**。 没有策略名称下拉列表，而是会看到内联TTL和标头设置。
* **方案B（自定义策略）：**&#x200B;您看到使用您或您的团队创建的策略名称(不是AWS提供的策略)选择的&#x200B;**缓存策略**。
* **方案C （托管策略）：**&#x200B;您看到使用AWS提供的名称（如`CachingOptimized`、`CachingDisabled`或`CachingOptimizedForUncompressedObjects`）选择的&#x200B;**缓存策略** — 这些无法编辑。

**方案A：旧版缓存设置**

如果您的行为使用旧版缓存设置：

1. 在&#x200B;**缓存密钥和原始请求**&#x200B;下，您将看到已选择&#x200B;**旧版缓存设置**。

2. 将`x-edgeoptimize-config`和`x-edgeoptimize-url`添加到&#x200B;**标头**&#x200B;允许列表：

   * 从下拉列表中选择&#x200B;**Include the headers**。
   * 添加`x-edgeoptimize-config`和`x-edgeoptimize-url`。
     ![旧版Cloudfront缓存](/help/assets/optimize-at-edge/cloudfront-cache-policy-legacy.png)

   如果您已在“标头”下拉列表中选择&#x200B;**所有**，请跳过此步骤 — 所有标头都会自动转发到来源。

3. 检查&#x200B;**对象缓存**&#x200B;设置：

   * 如果设置为&#x200B;**自定义** — 建议将&#x200B;**最小TTL**&#x200B;设置为`0`。 但是，如果当前的最低TTL已经非常短，则可能不需要对其进行更改。
   * 如果设置为&#x200B;**使用原始缓存标头** — 无需更改。

4. 单击&#x200B;**保存更改**。

**方案B：具有自定义缓存策略的非旧版本**

如果您的行为已使用自定义缓存策略(您创建的策略，而不是AWS托管策略)：

**导航：** AWS Console > CloudFront >策略>缓存

1. 单击现有策略。

2. 单击&#x200B;**编辑**。

3. 建议将&#x200B;**最小TTL**&#x200B;设置为`0`。 但是，如果当前的最低TTL已经非常短，则可能不需要对其进行更改。
   ![缓存策略TTL设置](/help/assets/optimize-at-edge/cloudfront-cache-policy-ttl.png)

4. 在&#x200B;**缓存键设置** > **标头**&#x200B;以及您现有的包含项下，添加`x-edgeoptimize-config`和`x-edgeoptimize-url`。
   ![缓存策略标头](/help/assets/optimize-at-edge/cloudfront-cache-policy-headers.png)

5. 单击&#x200B;**保存更改**。

**方案C：具有托管(AWS)缓存策略的非旧版**

如果您的行为使用AWS托管缓存策略（例如，`CachingOptimized`），则无法对其进行编辑。 您需要创建新的自定义缓存策略，以复制现有的托管策略设置并在顶部添加Edge优化标头。

**第1部分：记下当前托管缓存策略设置**

**导航：** AWS Console > CloudFront >策略>缓存

1. 查找并单击当前附加到您的行为的托管缓存策略（例如，`CachingOptimized`）。

2. 记下所有现有设置：
   * 最小TTL、最大TTL、默认TTL
   * 缓存键中包含的标头
   * 缓存键中包含的Cookie
   * 缓存键中包含的查询字符串
   * 压缩支持(Gzip、Brotli)

**第2部分：使用相同的设置创建新的自定义缓存策略+ Edge优化配置**

**导航：** AWS Console > CloudFront >策略>缓存

1. 单击&#x200B;**创建缓存策略**。

2. **名称：** `edgeoptimize-cache`

   ![缓存策略名称](/help/assets/optimize-at-edge/cloudfront-cache-policy-name.png)

3. 复制第1部分中记录的所有设置，并进行以下修改：

   * 建议将&#x200B;**最小TTL**&#x200B;设置为`0`。 但是，如果当前的最低TTL已经非常短，则可能不需要对其进行更改。

   ![缓存策略TTL设置](/help/assets/optimize-at-edge/cloudfront-cache-policy-ttl.png)

   * 在&#x200B;**缓存键设置** > **标头**&#x200B;下，包含托管策略所具有的所有内容，并添加`x-edgeoptimize-config`和`x-edgeoptimize-url`。

   ![缓存策略标头](/help/assets/optimize-at-edge/cloudfront-cache-policy-headers.png)

4. 单击&#x200B;**创建**。

5. 返回您的行为并关联新创建的策略：

   **导航：** AWS Console > CloudFront >分发> [您的分发] >行为

   1. 编辑您的行为。
   2. 在&#x200B;**缓存密钥和原始请求**&#x200B;下，选择&#x200B;**缓存策略**。
   3. 从下拉列表中选择`edgeoptimize-cache`。
   4. 单击&#x200B;**保存更改**。

**步骤4：创建Lambda@Edge函数（原始请求和响应）**

>[!IMPORTANT]
>必须在`us-east-1` （弗吉尼亚北部）区域&#x200B;**中创建Lambda@Edge函数**。 这是AWS的要求。 即使函数是在`us-east-1`中创建的，AWS仍会自动将其复制到全球所有CloudFront边缘位置，因此它会在距离查看器最近的边缘位置执行。 在继续之前，请确保您位于AWS控制台的`us-east-1`区域中。

**创建Lambda函数**

**导航：** AWS控制台> Lambda

1. 单击&#x200B;**创建函数**。

2. 从头开始选择&#x200B;**创作**。

3. 配置：
   * **函数名称：** `edgeoptimize-origin`
   * 将所有其他字段保留为其默认值。

4. 单击&#x200B;**创建函数**。

5. 在代码编辑器中，将默认代码替换为[origin-request-response.js](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/origin-request-response.js)中的代码。

6. 单击&#x200B;**部署**&#x200B;以保存代码。

7. 请注意&#x200B;**配置** > **权限**&#x200B;下显示的&#x200B;**执行角色名称**（例如，`edgeoptimize-origin-role-xxxxx`）。 您需要在以下步骤中完成此操作。

**更新执行角色的信任策略**

自动创建的角色仅信任`lambda.amazonaws.com`。 对于Lambda@Edge，您还必须添加`edgelambda.amazonaws.com`。

**导航：** AWS Console > IAM >角色> [您在上一步中的角色] >信任关系选项卡

1. 单击&#x200B;**编辑信任策略**。

2. 将策略替换为[trust-policy.json](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/trust-policy.json)中的内容。

3. 单击&#x200B;**更新策略**。

>[!WARNING]
>Lambda@Edge的`edgelambda.amazonaws.com`服务主体是&#x200B;**必需的**。 没有它，CloudFront无法在边缘位置调用您的函数。

**修复CloudWatch日志权限策略**

自动创建的角色带有为常规Lambda配置的`AWSLambdaBasicExecutionRole`策略，该策略具有错误的Lambda@Edge区域和日志组名称。 你需要更新它。

**导航：** AWS Console > IAM >角色> [您的角色] >权限选项卡>单击附加的策略名称（例如，`AWSLambdaBasicExecutionRole-xxxx`）

1. 单击&#x200B;**编辑**。

2. 将策略替换为[cloudwatch-policy.json](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/cloudwatch-policy.json)中的内容。

   在JSON中，将`ACCOUNT_ID`替换为您的实际AWS帐户ID(可在AWS控制台的右上角找到)，将`FUNCTION_NAME`替换为Lambda函数的名称（例如，`edgeoptimize-origin`）。

3. 单击&#x200B;**保存更改**。

>[!WARNING]
>ARN中的区域必须为`*` — Lambda@Edge在距离查看器最近的边缘位置执行，因此日志将写入边缘位置区域的CloudWatch（例如，`ap-south-1`， `eu-west-1`），不一定是`us-east-1`。 日志组使用区域前缀名称： `/aws/lambda/us-east-1.FUNCTION_NAME`，其中`us-east-1`始终是函数的主区域。

**发布版本**

1. 在函数页面上，单击&#x200B;**操作** （右上方） > **发布新版本**。

2. 添加描述。

3. 单击&#x200B;**发布**。
   ![Lambda发布](/help/assets/optimize-at-edge/cloudfront-lambda-publish.png)

4. 复制或记下&#x200B;**函数ARN** — 在下一步中需要此项。
   ![Lambda ARN](/help/assets/optimize-at-edge/cloudfront-lambda-arn.png)

**步骤5：将函数和缓存策略与行为关联**

**导航：** AWS Console > CloudFront >分发> [您的分发] >行为

1. 编辑您的行为。

2. 如果您在步骤3（场景C）中创建新缓存策略，请将&#x200B;**缓存策略**&#x200B;设置为`edgeoptimize-cache`。
   ![缓存策略](/help/assets/optimize-at-edge/cloudfront-behaviour-cache.png)

3. 在&#x200B;**函数关联**&#x200B;下，配置：

   * **查看器请求：** `edgeoptimize-routing`
   * **源请求：**&#x200B;来自步骤4的版本化函数ARN（在&#x200B;**发布版本**&#x200B;中）
   * **源响应：**&#x200B;来自步骤4的版本化函数ARN（在&#x200B;**发布版本**&#x200B;中）

   ![函数关联配置](/help/assets/optimize-at-edge/cloudfront-function-association.png)

4. 单击&#x200B;**保存更改**。

**步骤6：测试配置**

**1. 测试机器人流量（应优化）**

使用代理机器人用户代理发送请求。 在&#x200B;**第一个请求**&#x200B;上，Edge Optimize在处理并缓存页面时可能会返回代理（非优化）响应。 您可以通过响应中的`x-edgeoptimize-proxy: 1`标头标识此内容。

使用代理用户代理模拟AI机器人请求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

成功的响应包括`x-edgeoptimize-request-id`标头，用于确认请求是通过Edge优化路由的：

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

**2. 测试人员流量（不应受影响）**

模拟常规的人类浏览器请求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
```

响应应&#x200B;**不**&#x200B;包含`x-edgeoptimize-request-id`标头。 在Edge中启用优化之前，页面内容和响应时间应保持相同。

**3. 如何区分这两种方案**

| 页眉 | 机器人流量（已优化） | 人流（未受影响） |
|---|---|---|
| `x-edgeoptimize-request-id` | 存在 — 包含唯一的请求ID | 不存在 |
| `x-edgeoptimize-fo` | 仅在发生故障转移时存在（值： `1`） | 不存在 |

也可以在LLM Optimizer UI中查看流量路由的状态。 导航到&#x200B;**客户配置**&#x200B;并选择&#x200B;**CDN配置**&#x200B;选项卡。

![启用了路由的AI流量路由状态](/help/assets/optimize-at-edge/byocdn-CDN-traffic-routed-tick.png)

**4. 验证日志是否正确运行**

运行上述测试请求后，验证是否为CloudFront函数和Lambda@Edge函数编写了日志。

*CloudFront函数日志(`edgeoptimize-routing`)*

**导航：** AWS Console > CloudWatch >日志组（在`us-east-1`中或配置了CloudFront分发的地区）

1. 查找名为`/aws/cloudfront/function/edgeoptimize-routing`的日志组。
2. 打开最新的日志流。
3. 对于代理请求，您应会看到如下条目：
   * `Adding origin group for userAgent: chatgpt-user`
   * `Routing to Edge Optimize origin for userAgent: chatgpt-user`
4. 对于非代理请求，您应会看到：
   * `Routing to Default origin for userAgent: ...`

您还可以检查&#x200B;**AWS Console > CloudFront > Functions > edgeoptimize-routing**&#x200B;下的&#x200B;**Metrics**&#x200B;选项卡，以查看调用数和错误率。

*Lambda@Edge日志(`edgeoptimize-origin`)*

>[!IMPORTANT]
>Lambda@Edge日志将写入为请求提供服务的边缘位置&#x200B;**的**&#x200B;区域中的CloudWatch，而不是`us-east-1`。 在距离运行curl命令的位置最近的AWS区域中检查CloudWatch。

**导航：** AWS Console > CloudWatch >日志组（确保您在正确的区域）

1. 查找名为`/aws/lambda/us-east-1.edgeoptimize-origin`的日志组。
2. 打开最新的日志流。
3. 对于代理请求，您应会看到如下条目：
   * `Calling Edge Optimize Origin for agentic requests` — 主路径
   * `Calling Default Origin in case of failover for agentic requests` — 故障转移路径
   * `Failover Triggered for agentic requests` — 原点响应故障转移检测

如果日志组不存在，请确认已在步骤4中正确更新IAM权限。 另请检查其他AWS附近区域 — 提供请求的边缘位置可能与您期望的位置不同。

**疑难解答**

| 问题 | 可能的原因 | 解决方案 |
|-------|----------------|----------|
| 没有`x-edgeoptimize-request-id`响应代理请求 | 源组未路由到Edge Optimize | 验证是否已在CloudFront函数代码中正确替换`YOUR_DEFAULT_ORIGIN`（步骤2）。 |
| 代理请求出现403错误 | API密钥无效或缺失 | 检查Edge优化原始自定义标头中的`x-edgeoptimize-api-key`标头值（步骤1）。 |
| 找不到Lambda@Edge的CloudWatch日志 | 错误的IAM权限 | 验证是否已更新CloudWatch日志权限策略（步骤4）。 注意： Lambda@Edge日志显示在为请求提供服务的边缘区域中，不一定是`us-east-1`。 |
| 缓存未接受`cache-control: no-store` | 最小TTL可能过高 | 将缓存策略中的最小TTL设置为`0`（步骤3）。 如果最低TTL已经很短，这可能不是问题。 |
| 设置后中断的常规（非代理）流量 | 缓存策略配置错误 | 如果创建了新缓存策略（场景C），请确保复制了原始托管策略的所有设置。 |

**在Edge中禁用并重新启用优化**

Lambda@Edge函数(`edgeoptimize-origin`)与CloudFront行为的源请求和源响应事件相关联。 由于它会在每个通过此行为的请求（包括人类和代理）上内联运行，因此Lambda@Edge中断将影响所有实时流量，而不仅仅是代理请求。 如果检测到Lambda@Edge中断，请立即删除函数关联以将正常通信流恢复到默认来源。

**如何检测Lambda@Edge中断**

* **AWS服务运行状况仪表板** — 检查[AWS服务运行状况仪表板](https://health.aws.amazon.com/health/status)，了解影响&#x200B;**Amazon CloudFront**&#x200B;或&#x200B;**AWS Lambda**&#x200B;的任何活动事件。 此处报告的全球或地区性中断是确认问题出现在AWS基础设施方而不是您的配置中的最快方法。
* **Lambda@Edge错误** — 导航到&#x200B;**AWS Console > CloudFront >监控> [您的分发]**。 打开&#x200B;**Lambda@Edge错误**&#x200B;选项卡，并检查&#x200B;**执行错误**&#x200B;图形中的执行错误。 如果这些值很高，则Lambda@Edge可能会不可用。

**分离Lambda@Edge函数**

**导航：** AWS Console > CloudFront >分发> [您的分发] >行为

1. 单击&#x200B;**编辑**&#x200B;您的行为。

2. 向下滚动到&#x200B;**函数关联**&#x200B;部分。

3. 将以下关联设置为&#x200B;**无关联**：

   | 事件 | 更改为 |
   |---|---|
   | 查看器请求 | 无关联 |
   | 源请求 | 无关联 |
   | 源响应 | 无关联 |

   ![函数关联配置](/help/assets/optimize-at-edge/cloudfront-no-function-association.png)

4. 单击&#x200B;**保存更改**。

5. 等待CloudFront分发完成部署。 状态从&#x200B;**部署**&#x200B;更改为上次修改日期，通常在几分钟内。

部署后，所有流量都会直接路由到您的默认来源。 不删除任何配置；可以随时恢复Lambda函数及其关联。

**重新附加Lambda@Edge函数**

**导航：** AWS Console > CloudFront >分发> [您的分发] >行为

1. 单击&#x200B;**编辑**&#x200B;您的行为。

2. 向下滚动到&#x200B;**函数关联**&#x200B;部分。

3. 恢复关联：

   | 事件 | 设置为 |
   |---|---|
   | 查看器请求 | `edgeoptimize-routing` （CloudFront函数） |
   | 源请求 | 步骤4中的版本控制Lambda ARN |
   | 源响应 | 步骤4中的版本控制Lambda ARN |

   使用您在步骤4 （**发布版本**）中注意到的版本化&#x200B;**函数ARN**。

   ![函数关联配置](/help/assets/optimize-at-edge/cloudfront-function-association.png)

4. 单击&#x200B;**保存更改**。

5. 等待分发完成部署，然后验证代理请求是否按步骤6所述返回`x-edgeoptimize-request-id`标头。

{{return-to-overview}}
