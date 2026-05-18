---
title: 添加相关常见问题（FAQ）
description: 了解 LLM Optimizer 如何识别那些缺少为 AI 代理提供的结构化问答内容的高流量页面，以及如何通过 Optimize at Edge 审阅和部署 AI 生成的常见问题内容。
feature: Opportunities
autotag-review: '2026-05-15T17:28:53.611Z'
TQID: 'https://experienceleague.adobe.com/491jK6SRnc2yJ4Uw9UzK71W3nsTWDhxt3lW0Sy8-3NQ'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2: id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 7a92587197cf6a9eec6b01bd4eaeeaf1194d3088
workflow-type: tm+mt
source-wordcount: 742
ht-degree: 100%

---


# 添加相关常见问题（FAQ）

“添加相关常见问题”机会可识别那些缺少 AI 代理在生成回答时通常依赖的结构化问答内容的高流量页面。 它以您现有的页面材料作为基础，引入相关的&#x200B;**与意图一致的常见问题**&#x200B;内容。 这有助于代理能够更直接地将用户问题与您的内容相匹配。

对于每一个受影响的 URL，您都可以审阅 AI 生成的常见问题建议，然后通过 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 部署这些建议，这样代理式流量就可以获得更清晰的问答上下文，而无需更改内容管理系统 (CMS)。

## 它如何修复问题

通过 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 修复问题，它能够：

- 为 AI 代理提供一个预渲染的 HTML 快照。
- 在所检索的 HTML 中通过常见问题内容扩充页面。
- 在内容传递网络层上工作（无需更改 CMS）。
- 仅限 AI，对人类访客或 SEO 机器人没有任何影响。
- 几分钟即可部署，并可从 LLM Optimizer 界面将其&#x200B;**完全撤销**。

## 工作原理

LLM Optimizer 会根据您品牌的提示词集识别那些问答内容完全缺失或数量很少的高流量页面。 受影响的 URL 会显示在&#x200B;**当前建议**&#x200B;选项卡中&#x200B;**包含建议的 URL** 表格中，您可以在此表中展开一行查看每一个建议。

![”当前建议“中的“包含建议的 URL”，展开的一行中包含常见问题提示词和 AI 生成的回答](/help/dashboards/opportunities/assets/add-relevant-faqs-expand.png)

**包含建议的 URL** 表格中列出了那些常见问题有助于 AI 驱动的内容发现的页面。 所有建议分成&#x200B;**当前建议**、**修复的建议**&#x200B;和&#x200B;**忽略的建议**&#x200B;三类。 对于每一个 URL，您可以：

- **展开此行**，查看为这个页面建议的常见问题内容。
- **预览**&#x200B;之前和之后的代理式流量的效果比较。
- 如果您在 LLM Optimizer 以外解决了这个机会，可将其&#x200B;**标记为已修复**。
- **忽略**&#x200B;那些不相关的建议。

每一个展开的条目都会列出与此页面关联的&#x200B;**常见问题提示词**、**AI 生成的**&#x200B;建议回答、简短的&#x200B;**推理**&#x200B;以及&#x200B;**来源**。 表格中还会显示为每个 URL 建议的常见问题数量和&#x200B;**代理式流量（4 周）**，帮助您确定优先级。

在某一行上点击&#x200B;**预览**，打开优化预览。 它会将您的页面目前的代理式流量效果与优化后的视图进行比较（例如新的&#x200B;**常见问题**&#x200B;区域）。

![预览优化将当前的代理式视图与优化后带有常见问题的视图进行比较](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-01.png)

使用行复选框选择您想发送的常见问题建议。 页脚中会显示选定的数量，并提供&#x200B;**标记为已修复**、**忽略建议**&#x200B;和&#x200B;**部署优化**&#x200B;几个选项。

![在”当前建议“中选定的常见问题建议和“部署优化”](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-02.png)

### 部署优化

如果您已准备好在边缘发布，点击&#x200B;**部署优化**。 **部署到边缘**&#x200B;对话框中会列出您将要推送的 URL、问题和解答。 审阅列表，然后选择&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![”部署到边缘“对话框](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-03.png)

成功完成部署后，**部署已完成**&#x200B;会确认有多少优化已上线。 关闭此对话框，然后打开&#x200B;**修复的建议**，验证状态。

![部署完成确认](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-04.png)

>[!NOTE]
>
>部署优化过程需要完成 Optimize at Edge 加入过程。 如果您尚未加入，点击&#x200B;**部署优化**&#x200B;会引导您完成加入过程。 关于 Optimize at Edge 如何工作、受支持的内容传递网络提供程序以及加入过程的完整详细信息，请参阅 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 页面。

### 修复的建议和实时查看

在&#x200B;**修复的建议**&#x200B;中，已部署的 URL 会在状态列中显示&#x200B;**已优化**。 展开某一行，审阅已上线的常见问题内容，使用&#x200B;**详细信息**&#x200B;进行分析，或点击&#x200B;**实时查看**&#x200B;打开&#x200B;**当前页面内容**&#x200B;的只读视图进行验证（包括插入的&#x200B;**常见问题**&#x200B;分区）。

![“修复的建议”中的“已优化”状态、实时查看和回滚](/help/dashboards/opportunities/assets/add-relevant-faqs-fixed.png)

**实时查看**&#x200B;窗口中会显示在这个检查中显示的页面结构和常见问题副本。

![实时查看：当前页面内容，包括常见问题](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-05.png)

## 回滚

如果您改变想法，可以撤消任何已部署的优化。 在&#x200B;**修复的建议**&#x200B;视图中，您可以选择要撤消的已优化的行，然后点击标题中的&#x200B;**回滚**。

**回滚**&#x200B;对话框中会列出将回滚的建议，还有一条将撤消已部署优化的简单警告。 确认此列表，然后点击&#x200B;**回滚**&#x200B;或&#x200B;**取消**。

![回滚对话框中列出了要撤消的建议](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-07.png)

操作完成后，会显示&#x200B;**已成功回滚**&#x200B;的摘要消息；关闭消息，返回到仪表板。

![回滚已完成：已成功完成回滚](/help/dashboards/opportunities/assets/add-relevant-faqs-ui-08.png)

## 在演示中尝试此操作

在 [Frescopa 演示](https://play.llmo.now/org/demo-org)中浏览“添加相关的常见问题”工作流。
