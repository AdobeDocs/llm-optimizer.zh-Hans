---
title: 添加 LLM 友好的摘要
description: 了解 LLM Optimizer 如何识别那些缺少为 AI 代理提供的简洁摘要和关键点的高流量页面，以及如何通过 Optimize at Edge 审阅和部署相关的建议。
feature: Opportunities
autotag-review: '2026-05-15T17:27:51.631Z'
TQID: 'https://experienceleague.adobe.com/QpBdx3B-qg41ZWtPU2R4CNq-POrSs31UIb0kms1H3GU'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2: id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 7a92587197cf6a9eec6b01bd4eaeeaf1194d3088
workflow-type: tm+mt
source-wordcount: 793
ht-degree: 100%

---


# 添加 LLM 友好的摘要

“添加 LLM 友好的摘要”机会可识别那些缺少简洁的结构化摘要的高流量页面，这使 AI 代理难以快速理解页面上的关键信息。 它会引入基于您的现有页面内容的清晰摘要和关键点。 这有助于代理能更有效地解读和获得重要的品牌声明，使您的内容能更准确地包含在 AI 回答中。

对于每一个受影响的 URL，您可以审阅 AI 生成的建议，然后通过 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 部署这些建议，使代理式流量能获得更清晰、可扫描的上下文，且无需更改内容管理系统 (CMS)。

## 它如何修复问题

通过 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 修复问题，它能够：

- 为 AI 代理提供一个预渲染的 HTML 快照。
- 在所检索的 HTML 中通过摘要和/或关键点扩充页面。
- 在内容传递网络层上工作（无需更改 CMS）。
- 仅限 AI，对人类访客或 SEO 机器人没有任何影响。
- 几分钟即可部署，并可从 LLM Optimizer 界面将其&#x200B;**完全撤销**。

## 工作原理

LLM Optimizer 会识别那些在页面或分区层面上添加&#x200B;**摘要**&#x200B;和&#x200B;**关键点**&#x200B;能帮助 AI 理解内容的高流量页面。 受影响的 URL 会显示在&#x200B;**当前建议**&#x200B;选项卡中&#x200B;**包含建议的 URL** 表格中，您可以在此表中展开一行查看每一个建议。

![“当前建议”中显示包含建议的 URL、展开的行、页面和分区摘要建议](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-expand.png)

**包含建议的 URL** 表中列出了那些摘要有助于代理发现内容的页面。 所有建议分成&#x200B;**当前建议**、**修复的建议**&#x200B;和&#x200B;**忽略的建议**&#x200B;三类。 对于每一个 URL，您可以：

- **展开此行**，查看分析和建议的摘要文本（以及包含的关键点）。
- **预览**&#x200B;之前和之后的代理式流量的效果比较。
- 如果您在 LLM Optimizer 以外解决了这个机会，可将其&#x200B;**标记为已修复**。
- **忽略**&#x200B;那些不相关的建议。

每个展开的条目中会显示页面级和分区级的摘要说明、**AI 生成的**&#x200B;文案、编辑控件以及与实时页面关联的上下文。

点击&#x200B;**操作**&#x200B;列中的&#x200B;**预览**，打开优化预览。 它会将您的页面目前的代理式流量效果与优化后的视图进行比较（例如根据所建议的放置环境插入&#x200B;**摘要**&#x200B;和&#x200B;**关键点**&#x200B;内容）。 您可以在部署之前随时打开或忽略此预览。

如果您准备好发布，请通过复选框选择摘要和关键点的行。 页脚中会显示选定的数量，并提供&#x200B;**标记为已修复**、**忽略建议**&#x200B;和&#x200B;**部署优化**&#x200B;几个选项。

![“当前建议”中选定了摘要行，页脚中显示“部署优化”](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-select-url.png)

### 部署优化

如果您已准备好在边缘发布，点击&#x200B;**部署优化**。 **部署到边缘**&#x200B;对话框中列出了选定的 URL 和优化详细信息。 审阅列表，然后选择&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![”部署到边缘“对话框](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-deploy-dialog.png)

部署成功完成后，**部署完成**&#x200B;会确认有多少优化已上线，并提示 AI 代理可能需要一些时间为更新内容编制索引。 关闭此对话框，然后打开&#x200B;**修复的建议**，验证状态。

![部署完成确认](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-deploy-confirm.png)

>[!NOTE]
>
>部署优化过程需要完成 Optimize at Edge 加入过程。 如果您尚未加入，点击&#x200B;**部署优化**&#x200B;会引导您完成加入过程。 关于 Optimize at Edge 如何工作、受支持的内容传递网络提供程序以及加入过程的完整详细信息，请参阅 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 页面。

### 修复的建议和实时查看

在&#x200B;**修复的建议**&#x200B;中，已部署的 URL 会在状态列中显示&#x200B;**已优化**。 展开一行可审阅已部署的摘要文字和说明。

![“修复的建议”选项卡中“已优化”状态、展开的已部署的摘要、实时查看和详细信息](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-fixed.png)

点击此行上的&#x200B;**实时查看**，打开&#x200B;**当前页面内容**&#x200B;的只读视图进行验证（包括在应用的位置上插入的&#x200B;**摘要**&#x200B;和&#x200B;**关键点**&#x200B;区块）。 使用&#x200B;**详细信息**&#x200B;进行分析。 如果您需要批量还原边缘更改，请使用复选框选择已优化的行，然后使用标题中的&#x200B;**回滚**。

![“修复的建议”中用于回滚前批量选择的复选框](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-select-in-fixed.png)

## 回滚

如果您改变想法，可以撤消任何已部署的优化。 在&#x200B;**修复的建议**&#x200B;视图中，选择您想撤消的已优化的行，然后点击标题中的&#x200B;**回滚**。

**回滚**&#x200B;对话框中会列出将回滚的建议，还有一条将撤消已部署优化的简单警告。 确认此列表，然后点击&#x200B;**回滚**&#x200B;或&#x200B;**取消**。

![回滚对话框中列出了要撤消的建议](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-rollback-dialog.png)

操作完成后，会显示&#x200B;**已成功回滚**&#x200B;的摘要消息；关闭消息，返回到仪表板。

![回滚已完成：已成功完成回滚](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-rollback-confirm.png)

## 在演示中尝试此操作

在 [Frescopa 演示](https://play.llmo.now/org/demo-org)中了解“添加 LLM 友好的摘要”工作流。
