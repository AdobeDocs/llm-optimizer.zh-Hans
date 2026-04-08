---
title: Optimize at Edge - CloudFront (BYOCDN)
description: 了解如何在 LLM Optimizer 中为 Optimize at Edge 配置 CloudFront BYOCDN。
feature: Opportunities
source-git-commit: da789100d814004687de2f46e18a295671dec4b8
workflow-type: tm+mt
source-wordcount: '2265'
ht-degree: 96%

---


# CloudFront (BYOCDN)

此配置将代理式流量（来自 AI 机器人和 LLM 用户代理的请求）路由到 Edge Optimize 后端服务（`live.edgeoptimize.net`）。 人类访客和 SEO 机器人仍将照常从您的源站获得响应。 完成设置后，可在响应中查找头部 `x-edgeoptimize-request-id` 以测试配置是否成功。

**先决条件**

在设置 CloudFront 配置之前，请确保您具有：

* 服务于您的网站的现有 CloudFront 分发。
* 用于创建 Lambda 函数、IAM 角色、CloudFront 分发和缓存策略的 AWS IAM 权限。
* 完成了 LLM Optimizer 的加入过程。
* 已将内容传递网络日志转发到 LLM Optimizer。
* 具有从 LLM Optimizer UI 检索到的 Edge Optimize API 密钥。
* （可选）如果首先在暂存主机名上测试路由，则使用暂存Edge优化API密钥。

{{retrieve-byocdn-api-key}}

{{retrieve-staging-edge-optimize-api-key}}

**步骤 1：创建 Edge Optimize 源站**

**导航：** AWS 控制台 > CloudFront > 分发 > [您的分发] > 源站选项卡

1. 点击&#x200B;**创建源站**。

2. 配置源站：
   * **源站域：** `live.edgeoptimize.net`
   * **名称：**`EdgeOptimize_Origin`

3. 让所有其他字段保留其默认值。

4. 添加自定义标头：

   | 页眉 | 值 |
   |--------|-------|
   | `x-edgeoptimize-api-key` | 您的 API 密钥 |
   | `x-forwarded-host` | `www.example.com` |

   将 `www.example.com` 替换为您真实的网站域，将 `Your API key` 替换为您的 Adobe 代表提供的 Edge Optimize API 密钥。

5. 点击&#x200B;**创建源站**。

![Cloudfront 源站创建](/help/assets/optimize-at-edge/cloudfront-origin-creation.png)

**步骤 2：创建查看器请求函数**

**导航：** AWS 控制台 > CloudFront > 函数

1. 点击&#x200B;**创建函数**。

2. 配置：
   * **名称：**`edgeoptimize-routing`
   * **运行时间：** `cloudfront-js-2.0`

3. 将默认代码替换为 [viewer-request.js](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/cloudfront-function/viewer-request.js) 中的代码。

   在发布之前，请自定义代码中的以下值：

   * `YOUR_DEFAULT_ORIGIN` — 替换为您现有的默认源站的名称（可在 CloudFront > 分发 > [您的分发] > 源站选项卡中找到）。
   * `TARGETED_PATHS` — 设置为 `null`，将所有 HTML 页面定为目标，或设置为特定路径的数组，例如 `['/', '/products', '/about']`。

4. 点击&#x200B;**保存更改** > **发布函数**。

![Cloudfront 函数创建](/help/assets/optimize-at-edge/cloudfront-function-creation.png)


**步骤 3：配置缓存策略**

**导航：** AWS 控制台 > CloudFront > 分发 > [您的分发] > 行为

检查当前附加到您行为的缓存策略。 点击行为上的&#x200B;**编辑**，查看&#x200B;**缓存键和源站请求**&#x200B;部分，识别您的场景：

* **场景 A（旧版）：**&#x200B;您会看到所选的单选按钮带有标记&#x200B;**旧版缓存设置**。 没有策略名称下拉列表，您会看到内联 TTL 和标头设置。
* **场景 B（自定义策略）：**&#x200B;您会看到所选的&#x200B;**缓存策略**&#x200B;带有您或您的团队创建的策略名称（不是 AWS 提供的策略）。
* **场景 C（托管策略）：**&#x200B;您会看到所选的&#x200B;**缓存策略**&#x200B;带有一个 AWS 提供的名称，如 `CachingOptimized`、`CachingDisabled` 或 `CachingOptimizedForUncompressedObjects`，这些都无法编辑。

**场景 A：旧版缓存设置**

如果您的行为使用旧版缓存设置：

1. 在&#x200B;**缓存键和源站请求**&#x200B;中，您会看到选择了&#x200B;**旧版缓存设置**。

2. 将 `x-edgeoptimize-config` 和 `x-edgeoptimize-url` 添加到&#x200B;**标头**&#x200B;允许列表：

   * 从下拉列表中选择&#x200B;**包含以下标头**。
   * 添加 `x-edgeoptimize-config` 和 `x-edgeoptimize-url`。
     ![Cloudfront 缓存旧版](/help/assets/optimize-at-edge/cloudfront-cache-policy-legacy.png)

   如果您在“标头”下拉列表中选择了&#x200B;**所有**，请跳过此步骤，所有标头都会自动转发到源站。

3. 检查&#x200B;**对象缓存**&#x200B;设置：

   * 如果设置为&#x200B;**自定义**，建议将&#x200B;**最小 TTL** 设置为 `0`。 但是，如果当前的最小 TTL 已经非常短，可能就不需要再更改。
   * 如果设置为&#x200B;**使用源站缓存标头**，就无需更改。

4. 点击&#x200B;**保存更改**。

**场景 B：带有自定义缓存策略的非旧版**

如果您的行为已使用一个自定义缓存策略（您创建的策略，而不是 AWS 托管策略）：

**导航：** AWS 控制台 > CloudFront > 策略 > 缓存

1. 点击您的现有策略。

2. 单击&#x200B;**编辑**。

3. 建议将&#x200B;**最小 TTL** 设置为 `0`。 但是，如果当前的最小 TTL 已经非常短，可能就不需要再更改。
   ![缓存策略 TTL 设置](/help/assets/optimize-at-edge/cloudfront-cache-policy-ttl.png)

4. 在&#x200B;**缓存键设置** > **标头**&#x200B;中，除了您现有的包含项以外，再添加 `x-edgeoptimize-config` 和 `x-edgeoptimize-url`。
   ![缓存策略标头](/help/assets/optimize-at-edge/cloudfront-cache-policy-headers.png)

5. 点击&#x200B;**保存更改**。

**场景 C：带有托管 (AWS) 缓存策略的非旧版**

如果您的行为使用 AWS 托管缓存策略（例如 `CachingOptimized`），您就无法进行编辑。 您需要创建一个新的自定义缓存策略，此策略复制现有的托管策略设置，并添加在 Edge Optimize 标头的最上面。

**第 1 部分：记下您当前的托管缓存策略设置**

**导航：** AWS 控制台 > CloudFront > 策略 > 缓存

1. 找到并点击当前附加到您的行为的托管缓存策略（例如 `CachingOptimized`）。

2. 记下所有现有设置：
   * 最小 TTL、最大 TTL、默认 TTL
   * 缓存键中包含的标头
   * 缓存键中包含的 Cookie
   * 缓存键中包含的查询字符串
   * 压缩支持（Gzip、Brotli）

**第 2 部分：用相同的设置创建一个新的自定义缓存策略 + Edge Optimize 配置**

**导航：** AWS 控制台 > CloudFront > 策略 > 缓存

1. 点击&#x200B;**创建缓存策略**。

2. **名称：**`edgeoptimize-cache`

   ![缓存策略名称](/help/assets/optimize-at-edge/cloudfront-cache-policy-name.png)

3. 复制第 1 部分中记下的所有设置，然后进行以下修改：

   * 建议将&#x200B;**最小 TTL** 设置为 `0`。 但是，如果当前的最小 TTL 已经非常短，可能就不需要再更改。

   ![缓存策略 TTL 设置](/help/assets/optimize-at-edge/cloudfront-cache-policy-ttl.png)

   * 在&#x200B;**缓存键设置** > **标头**&#x200B;中，包含托管策略中的所有内容，然后添加 `x-edgeoptimize-config` 和 `x-edgeoptimize-url`。

   ![缓存策略标头](/help/assets/optimize-at-edge/cloudfront-cache-policy-headers.png)

4. 单击&#x200B;**创建**。

5. 返回到您的行为，关联新创建的策略：

   **导航：** AWS 控制台 > CloudFront > 分发 > [您的分发] > 行为

   1. 编辑您的行为。
   2. 在&#x200B;**缓存键和源站请求**&#x200B;中选择&#x200B;**缓存策略**。
   3. 从下拉列表中选择 `edgeoptimize-cache`。
   4. 点击&#x200B;**保存更改**。

**步骤 4：创建 Lambda@Edge 函数（源站请求和响应）**

>[!IMPORTANT]
>**必须在`us-east-1`（弗吉尼亚北部）区域**&#x200B;中创建 Lambda@Edge 函数。 这是 AWS 的要求。 即使函数是在 `us-east-1` 中创建，AWS 仍会自动将其复制到全球所有 CloudFront 边缘位置，因此它会在距离查看器最近的边缘位置执行。 在继续操作之前，请确保您位于 AWS 控制台的 `us-east-1` 区域中。

**创建 Lambda 函数**

**导航：** AWS 控制台 > Lambda

1. 点击&#x200B;**创建函数**。

2. 选择&#x200B;**从头开始创作**。

3. 配置：
   * **函数名称：** `edgeoptimize-origin`
   * 让所有其他字段保留其默认值。

4. 点击&#x200B;**创建函数**。

5. 在代码编辑器中，将默认代码替换为 [origin-request-response.js](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/origin-request-response.js) 中的代码。

6. 点击&#x200B;**部署**，保存代码。

7. 请注意&#x200B;**配置** > **权限**&#x200B;中显示的&#x200B;**执行角色名称**（例如 `edgeoptimize-origin-role-xxxxx`）。 在以下步骤中您需要此名称。

**更新执行角色的信任策略**

自动创建的角色只信任 `lambda.amazonaws.com`。 您还必须为 Lambda@Edge 添加 `edgelambda.amazonaws.com`。

**导航：** AWS 控制台 > IAM > 角色 > [上一步中的角色] > 信任关系选项卡

1. 点击&#x200B;**编辑信任策略**。

2. 将策略替换为 [trust-policy.json](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/trust-policy.json) 中的内容。

3. 点击&#x200B;**更新策略**。

>[!WARNING]
>Lambda@Edge 的`edgelambda.amazonaws.com`服务主体是&#x200B;**必填项**。 否则，CloudFront 就无法在边缘位置调用您的函数。

**修复 CloudWatch 日志权限策略**

自动创建的角色带有为常规 Lambda 配置的 `AWSLambdaBasicExecutionRole` 策略，具有错误的 Lambda@Edge 区域和日志组名称。 您需要将其更新。

**导航：** AWS 控制台 > IAM >角色 > [您的角色] > 权限选项卡 > 点击所附加的策略名称（例如 `AWSLambdaBasicExecutionRole-xxxx`）

1. 单击&#x200B;**编辑**。

2. 将策略替换为 [cloudwatch-policy.json](https://github.com/adobe-rnd/llmo-edge-optimize-samples/blob/main/cloudfront/lambda/cloudwatch-policy.json) 中的内容。

   在 JSON 中，将 `ACCOUNT_ID` 替换为您的真实 AWS 帐户 ID（可在 AWS 控制台的右上角找到），将 `FUNCTION_NAME` 替换为 Lambda 函数的名称（例如 `edgeoptimize-origin`）。

3. 点击&#x200B;**保存更改**。

>[!WARNING]
>ARN 中的区域必须为 `*` — Lambda@Edge 在距离查看器最近的边缘位置执行，因此日志将写入边缘位置区域的 CloudWatch（例如 `ap-south-1`，`eu-west-1`），不一定是 `us-east-1`。 日志组使用一个区域前缀名：`/aws/lambda/us-east-1.FUNCTION_NAME`，其中 `us-east-1` 总是函数的主区域。

**发布版本**

1. 在函数页面上点击&#x200B;**操作**（右上方）> **发布新版本**。

2. 添加描述。

3. 单击&#x200B;**发布**。
   ![Lambda 发布](/help/assets/optimize-at-edge/cloudfront-lambda-publish.png)

4. 复制或记下&#x200B;**函数 ARN** — 您在下一步中需要它。
   ![Lambda ARN](/help/assets/optimize-at-edge/cloudfront-lambda-arn.png)

**步骤 5：将函数和缓存策略与行为关联起来**

**导航：** AWS 控制台 > CloudFront > 分发 > [您的分发] > 行为

1. 编辑您的行为。

2. 如果您在步骤 3（场景 C）中创建了新的缓存策略，应将&#x200B;**缓存策略**&#x200B;设置为 `edgeoptimize-cache`。
   ![缓存策略](/help/assets/optimize-at-edge/cloudfront-behaviour-cache.png)

3. 在&#x200B;**函数关联**&#x200B;中配置：

   * **查看器请求：** `edgeoptimize-routing`
   * **源站请求：**&#x200B;步骤 4 中带版本的函数 ARN（在&#x200B;**发布版本**&#x200B;中）
   * **源站响应：**&#x200B;步骤 4 中带版本的函数 ARN（在&#x200B;**发布版本**&#x200B;中）

   ![函数关联配置](/help/assets/optimize-at-edge/cloudfront-function-association.png)

4. 点击&#x200B;**保存更改**。

**步骤 6：测试配置**

**1. 测试机器人流量（应被优化）**

通过代理式机器人用户代理发送请求。 在&#x200B;**第一个请求**&#x200B;上，Edge Optimize 在处理和缓存页面时可能会返回一个代理（非优化的）响应。 您可以通过响应中的 `x-edgeoptimize-proxy: 1` 标头识别这一点。

用代理式用户代理模拟 AI 机器人请求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

成功的响应包括 `x-edgeoptimize-request-id` 头部，用于确认请求是通过 Edge Optimize 路由的：

```
< HTTP/2 200
< x-edgeoptimize-request-id: 50fce12d-0519-4fc6-af78-d928785c1b85
```

**2. 测试人类流量（不应受影响）**

模拟一个常规的人类浏览器请求：

```
curl -svo /dev/null https://www.example.com/page.html \
  --header "user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
```

响应&#x200B;**不**&#x200B;应包含 `x-edgeoptimize-request-id` 头部。 页面内容和响应时间应保持与启用 Optimize at Edge 之前时完全相同。

**3. 如何区分这两种场景**

| 页眉 | 机器人流量（已优化） | 人类流量（不受影响） |
|---|---|---|
| `x-edgeoptimize-request-id` | 存在——包含唯一的请求 ID | 不存在 |
| `x-edgeoptimize-fo` | 仅在发生故障转移的情况下存在（值：`1`） | 不存在 |

**4. 暂存域（可选）**

如果您使用LLM Optimizer中的暂存主机名和暂存API密钥，请使用&#x200B;**暂存** API密钥在&#x200B;**暂存**&#x200B;分发上部署相同的CloudFront配置。 然后，验证暂存主机上的机器人流量：

```
curl -svo /dev/null https://staging.example.com/page.html \
  --header "user-agent: chatgpt-user"
```

将`https://staging.example.com/page.html`替换为您的实际暂存URL和路径。 成功的响应包括`x-edgeoptimize-request-id`标头。

{{verify-routing-status-in-ui}}

**5. 验证日志是否正确流动**

运行上述测试请求后，验证是否为 CloudFront 函数和 Lambda@Edge 函数写入了日志。

*CloudFront 函数日志 (`edgeoptimize-routing`)*

**导航：** AWS 控制台 > CloudWatch > 日志组（在 `us-east-1` 中或配置了您的 CloudFront 分发的区域）

1. 查找名为 `/aws/cloudfront/function/edgeoptimize-routing` 的日志组。
2. 打开最近的日志流。
3. 对于代理式请求，您应能看到如下条目：
   * `Adding origin group for userAgent: chatgpt-user`
   * `Routing to Edge Optimize origin for userAgent: chatgpt-user`
4. 对于非代理式请求，您应看到：
   * `Routing to Default origin for userAgent: ...`

您还可以在 **AWS 控制台 > CloudFront > 函数 > edgeoptimize 路由** 中检查&#x200B;**量度**&#x200B;选项卡，查看调用次数和错误率。

*Lambda@Edge 日志 (`edgeoptimize-origin`)*

>[!IMPORTANT]
>Lambda@Edge 日志被写入处理请求的&#x200B;**边缘位置区域**&#x200B;中的 CloudWatch，而不是 `us-east-1`。 检查距离运行 curl 命令的位置最近的 AWS 区域中的 CloudWatch。

**导航：** AWS 控制台 > CloudWatch > 日志组（确保您位于正确的区域）

1. 查找名为 `/aws/lambda/us-east-1.edgeoptimize-origin` 的日志组。
2. 打开最近的日志流。
3. 对于代理式请求，您应能看到如下条目：
   * `Calling Edge Optimize Origin for agentic requests` — 主要路径
   * `Calling Default Origin in case of failover for agentic requests` — 故障转移路径
   * `Failover Triggered for agentic requests` — 源站响应故障转移检测

如果日志组不存在，请验证在步骤 4 中正确更新了 IAM 权限。 另请检查其他 AWS 附近区域，处理请求的边缘位置可能与您期望的位置不同。

**疑难解答**

| 问题 | 可能的原因 | 解决方案 |
|-------|----------------|----------|
| 对代理式请求的响应没有 `x-edgeoptimize-request-id` | 源站组未路由到 Edge Optimize | 验证是否在 CloudFront 函数代码中正确替换了 `YOUR_DEFAULT_ORIGIN`（步骤 2）。 |
| 代理式请求出现 403 错误 | API 密钥无效或缺失 | 检查 Edge Optimize 源站自定义标头中的 `x-edgeoptimize-api-key` 标头值（步骤 1）。 |
| 找不到 Lambda@Edge 的 CloudWatch 日志 | IAM 权限错误 | 验证是否更新了 CloudWatch 日志权限策略（步骤 4）。 注意：Lambda@Edge 日志会显示在处理请求的边缘区域中，不一定是 `us-east-1`。 |
| 缓存未履行 `cache-control: no-store` | 最小 TTL 可能过高 | 将缓存策略中的最小 TTL 设置为 `0`（步骤 3）。 如果最小 TTL 已经很短，这可能不是问题。 |
| 设置后常规（非代理式）流量中断 | 缓存策略配置错误 | 如果创建了一个新的缓存策略（场景 C），请确保您复制了原始托管策略的所有设置。 |

**禁用并重新启用 Optimize at Edge**

Lambda@Edge 函数 (`edgeoptimize-origin`) 与您的 CloudFront 行为的源站请求和源站响应事件相关联。 由于它会在每个通过此行为的请求上内联运行——无论是人类还是代理式请求，因此 Lambda@Edge 中断会影响所有实时流量，而不仅仅是代理式请求。 如果您检测到 Lambda@Edge 中断，请立即移除函数关联，将正常通信流恢复到您的默认源站。

**如何检测 Lambda@Edge 中断**

* **AWS 服务运行状况仪表板**——检查 [AWS 服务运行状况仪表板](https://health.aws.amazon.com/health/status)，了解会影响 **Amazon CloudFront** 或 **AWS Lambda** 的任何活跃事件。 此处报告的全球或区域性中断是确认问题出现在 AWS 基础设施端而不是在您的配置中的最快方法。
* **Lambda@Edge 错误**——导航到 **AWS 控制台 > CloudFront > 监控 > [您的分发]**。 打开 **Lambda@Edge 错误**&#x200B;选项卡，检查&#x200B;**执行错误**&#x200B;图形中的执行错误。 如果错误很多，Lambda@Edge 可能不可用。

**分离 Lambda@Edge 函数**

**导航：** AWS 控制台 > CloudFront > 分发 > [您的分发] > 行为

1. 点击您的行为上的&#x200B;**编辑**。

2. 向下滚动到&#x200B;**函数关联**&#x200B;部分。

3. 将以下关联设置为&#x200B;**无关联**：

   | 事件 | 更改为 |
   |---|---|
   | 查看器请求 | 无关联 |
   | 源站请求 | 无关联 |
   | 源站响应 | 无关联 |

   ![函数关联配置](/help/assets/optimize-at-edge/cloudfront-no-function-association.png)

4. 点击&#x200B;**保存更改**。

5. 等待 CloudFront 分发完成部署。 状态从&#x200B;**部署**&#x200B;改为上次更改日期，通常几分钟就完成。

部署后，所有流量都会直接路由到您的默认源站。 不删除任何配置；可以随时恢复 Lambda 函数及其关联。

**重新附加 Lambda@Edge 函数**

**导航：** AWS 控制台 > CloudFront > 分发 > [您的分发] > 行为

1. 点击您的行为上的&#x200B;**编辑**。

2. 向下滚动到&#x200B;**函数关联**&#x200B;部分。

3. 恢复关联：

   | 事件 | 设置为 |
   |---|---|
   | 查看器请求 | `edgeoptimize-routing`（CloudFront 函数） |
   | 源站请求 | 步骤 4 中带版本的 Lambda ARN |
   | 源站响应 | 步骤 4 中带版本的 Lambda ARN |

   使用您在步骤 4（**发布版本**）中记下的带版本的&#x200B;**函数 ARN**。

   ![函数关联配置](/help/assets/optimize-at-edge/cloudfront-function-association.png)

4. 点击&#x200B;**保存更改**。

5. 等待分发完成部署，然后验证代理式请求是否如步骤 6 中所述返回 `x-edgeoptimize-request-id` 标头。

{{return-to-overview}}
