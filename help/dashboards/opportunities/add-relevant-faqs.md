---
title: 添加相关常见问题（FAQ）
description: 了解LLM Optimizer如何识别缺少人工智能代理的结构化问答内容的高流量页面，以及如何在Edge中使用优化查看和部署人工智能生成的常见问题解答。
feature: Opportunities
autotag-review: '2026-05-15T17:28:53.611Z'
TQID: 'https://experienceleague.adobe.com/491jK6SRnc2yJ4Uw9UzK71W3nsTWDhxt3lW0Sy8-3NQ'
product_v2:
  - id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2:
  - id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2:
  - id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2:
  - id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 7a92587197cf6a9eec6b01bd4eaeeaf1194d3088
workflow-type: tm+mt
source-wordcount: 742
ht-degree: 1%

---


# 添加相关常见问题（FAQ）

添加相关常见问题解答机会识别缺少结构化问答内容的高流量页面，人工智能代理在生成响应时通常依赖这些内容。 它以您现有的页面资料为基础，介绍了相关的&#x200B;**意图一致的常见问题解答**&#x200B;内容。 这有助于代理更直接地将用户问题与您的内容匹配。

对于每个受影响的URL，您可以查看人工智能生成的常见问题解答建议，然后通过[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)来部署这些建议，这样代理流量就可以接收更清晰的问答上下文，而无需更改内容管理系统(CMS)。

## 它如何修复问题

使用Edge[&#128279;](/help/dashboards/optimize-at-edge/overview.md)中的优化来应用修复，其中：

- 向AI代理提供预渲染的HTML快照。
- 在HTML中通过所检索的常见问题解答内容丰富页面。
- 在CDN层工作（CMS不会发生更改）。
- 是仅限AI的 — 对人类访客或SEO机器人没有影响。
- 几分钟即可部署，并且可从LLM Optimizer界面&#x200B;**完全还原**。

## 工作原理

LLM Optimizer会根据您品牌的提示集识别缺少或缺少问答内容的高流量页面。 受影响的URL显示在&#x200B;**当前建议**&#x200B;选项卡上带有建议&#x200B;**的** URL表中，您可以在该表中展开一行来检查每个建议。

![包含有关当前建议的建议的URL，包含常见问题提示和AI生成答案的展开行](/help/dashboards/opportunities/assets/add-relevant-faqs-expand.png)

带有建议&#x200B;**表的** URL列出了常见问题解答将帮助AI驱动发现的页面。 建议被整理为&#x200B;**当前建议**、**固定建议**&#x200B;和&#x200B;**忽略的建议**。 对于每个URL，您可以：

- **展开行**&#x200B;以查看该页面的建议常见问题解答内容。
- **预览**&#x200B;代理流量的比较前和比较后。
- 如果您在LLM Optimizer外部处理了商机，请&#x200B;**标记为已修复**。
- **忽略不相关的**&#x200B;建议。

每个扩展条目都列出了与页面关联的常见问题解答&#x200B;**提示**、**AI生成**&#x200B;建议答案、简短&#x200B;**推理**&#x200B;和&#x200B;**源**。 该表还显示了每个URL和&#x200B;**代理流量（4周）**&#x200B;建议的常见问题解答数量，以帮助您排定优先级。

在行上单击&#x200B;**预览**&#x200B;以打开优化预览。 它会比较您的页面现在查找代理流量的方式与优化后的视图（例如，新的&#x200B;**常见问题解答**&#x200B;块）。

![预览优化将当前代理视图与带有常见问题的优化后代理视图进行比较](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-01.png)

使用行复选框选择要发运的常见问题解答。 页脚显示选择的数量，并提供&#x200B;**标记为已修复**、**忽略建议**&#x200B;和&#x200B;**部署优化**。

![有关当前建议的选定常见问题解答建议及部署优化](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-02.png)

### 部署优化

准备在边缘发布时，单击&#x200B;**部署优化**。 **部署到Edge**&#x200B;对话框列出了要推送的URL、问题和答案。 查看列表，然后选择&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![部署到Edge对话框](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-03.png)

成功部署后，**部署完成**&#x200B;将确认有多少优化已上线。 关闭对话框并打开&#x200B;**修复建议**&#x200B;以验证状态。

![部署完成确认](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-04.png)

>[!NOTE]
>
>部署优化需要完成Optimize at Edge载入流程。 如果您尚未载入，单击&#x200B;**部署优化**&#x200B;会将您引导至载入流程。 有关在Edge中优化的工作方式、支持的CDN提供商以及载入流程的完整详细信息，请参阅[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)页面。

### 修复了建议并实时查看

在&#x200B;**已修复建议**&#x200B;上，部署的URL在状态列中显示&#x200B;**已优化**。 展开一行以查看实时常见问题解答内容，使用&#x200B;**详细信息**&#x200B;进行分析，或单击&#x200B;**查看实时**&#x200B;打开作为验证服务的&#x200B;**当前页面内容**&#x200B;的只读视图（包括插入的&#x200B;**常见问题解答**&#x200B;部分）。

![修复了具有优化状态、查看实时和回滚的建议](/help/dashboards/opportunities/assets/add-relevant-faqs-fixed.png)

**查看实时**&#x200B;窗口显示该检查中显示的页面结构和常见问题解答。

![查看实时内容 — 当前页面内容，包括常见问题解答](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-05.png)

## 回滚

如果您改变主意，可以回退任何已部署的优化。 从&#x200B;**Fixed Suggestions**&#x200B;视图中，您可以选择要还原的优化行，然后单击标题中的&#x200B;**Rollback**。

**Rollback**&#x200B;对话框列出了将回滚的建议，并简短地警告将还原已部署的优化。 确认列表，然后单击&#x200B;**回滚**&#x200B;或&#x200B;**取消**。

![回滚对话框列出了还原建议](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-07.png)

操作完成后，将显示&#x200B;**已成功回滚**&#x200B;摘要；关闭它以返回到仪表板。

![回滚完成 — 已成功回滚](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-08.png)

## 在演示中尝试

浏览[Frescopa演示](https://play.llmo.now/org/demo-org)中的“添加相关常见问题解答”工作流。
