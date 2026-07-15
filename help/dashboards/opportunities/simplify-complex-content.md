---
title: 简化复杂内容
description: 了解 LLM Optimizer 如何识别那些具有 AI 代理难以解读的密集文本的高流量页面，以及如何通过 Optimize at Edge 审阅和部署简化的文本。
feature: Opportunities
autotag-review: '2026-07-15T18:04:55.581Z'
TQID: 'https://experienceleague.adobe.com/uMK9qeAGMNrtvR0TYbeg8SIOKlwKf4L5NIE9ZgsJaUw'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: e1b649f0-0a61-46e4-9082-64d5cb2576c6id: ef4e63f5-cb4d-462d-bf9a-1f617edf2a3a
subfeature_v2: id: bbfc1b77-44c5-4fe8-b65f-ec160fe0d021
role_v2: id: b69b2659-1057-424e-8fc5-ed9e016dc554
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1id: e1e0219c-f879-479f-8427-888ed2a6e9c2
source-git-commit: 2705cf26faea9c09817bbdcec4b4c531552df7ba
workflow-type: tm+mt
source-wordcount: 785
ht-degree: 100%

---


# 简化复杂内容

“简化复杂内容”机会可识别出那些其中密集的或复杂的文本使 AI 代理难以解读关键信息的高流量页面。 它会引入更清晰、更易于扫描的现有文本内容，同时保留原始含义。 这有助于代理更可靠地解析、总结和提取重要信息。

对于每个受影响的 URL，您可以查看&#x200B;**改进的文本**&#x200B;建议，将其与&#x200B;**预览**&#x200B;进行比较，然后通过 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 部署这些建议，使代理式流量无需更改内容管理系统 (CMS) 就可以获得更清晰的 HTML。

## 它如何修复问题

通过 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 修复问题，它能够：

- 为 AI 代理提供一个预渲染的 HTML 快照。
- 更新代理可见的页面，用&#x200B;**改进的文本**&#x200B;替换或增强复杂的段落，使其与实时页面一致。
- 在内容传递网络层上工作（无需更改 CMS）。
- 仅限 AI，对人类访客或 SEO 机器人没有任何影响。
- 几分钟即可部署，并可从 LLM Optimizer 界面将其&#x200B;**完全撤销**。

## 工作原理

LLM Optimizer 会识别那些获得了很高的代理式流量，但其中的内容分数低于可读性阈值的页面，然后建议重写文本。 对于每个页面您都能获得：

**改进的文本**：基于页面上已有的内容进行了简化。
**预览**：之前和之后的代理式流量的效果比较。

受影响的 URL 会显示在&#x200B;**当前建议**&#x200B;选项卡中&#x200B;**包含建议的 URL** 表格中，您可以在此表中展开一行查看每一个建议。

![“当前建议”中包含建议的 URL，展开的行中显示改进的文本和预览](/help/dashboards/opportunities/assets/simplify-complex-content-expand.png)

**包含建议的 URL** 表格中列出了简化内容有助于代理式理解的页面。 所有建议分成&#x200B;**当前建议**、**修复的建议**&#x200B;和&#x200B;**忽略的建议**&#x200B;三类。 对于每一个 URL，您可以：

- **展开此行**，查看该页面的&#x200B;**改进的文本**&#x200B;建议。
- **预览**&#x200B;之前和之后的代理式流量的效果比较。
- 如果您在 LLM Optimizer 以外解决了这个机会，可将其&#x200B;**标记为已修复**。
- **忽略**&#x200B;那些不相关的建议。

**视图**&#x200B;中包括&#x200B;**当前建议**、**修复的建议**（部署后状态为&#x200B;**已优化**）和&#x200B;**忽略的建议**。 您可以使用&#x200B;**修复的建议**&#x200B;中的&#x200B;**实时查看**&#x200B;来验证实时部署，并能随时回滚。

使用复选框选择您想发送的 URL 或带有&#x200B;**改进的文本**&#x200B;的行，然后使用&#x200B;**机会计划**&#x200B;标题中的&#x200B;**标记为已修复**、**忽略建议**&#x200B;或&#x200B;**部署优化**。 演示 UI 中还会显示一个选择计数以及与列表相关的操作。

![计划标题中的机会计划、当前建议、展开行和部署优化](/help/dashboards/opportunities/assets/simplify-complex-content-select.png)

### 部署优化

如果您已准备好在边缘发布，点击&#x200B;**部署优化**。 **部署到边缘**&#x200B;对话框中列出了选定的 URL 和优化详细信息。 审阅列表，然后选择&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![”部署到边缘“对话框](/help/dashboards/opportunities/assets/simplify-complex-content-deploy-dialog.png)

部署成功完成后，**部署完成**&#x200B;会确认有多少优化已上线，并提示 AI 代理可能需要一些时间为更新内容编制索引。 关闭此对话框，然后打开&#x200B;**修复的建议**，验证状态。

![部署完成确认](/help/dashboards/opportunities/assets/simplify-complex-content-deploy-confirm.png)

>[!NOTE]
>
>部署优化过程需要完成 Optimize at Edge 加入过程。 如果您尚未加入，点击&#x200B;**部署优化**&#x200B;会引导您完成加入过程。 关于 Optimize at Edge 如何工作、受支持的内容传递网络提供程序以及加入过程的完整详细信息，请参阅 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 页面。

### 修复的建议和实时查看

在&#x200B;**修复的建议**&#x200B;中，已部署的 URL 会在状态列中显示&#x200B;**已优化**。 展开一行可查看已部署的&#x200B;**改进的文本**&#x200B;和说明。

![“修复的建议”选项卡中包括“已优化”状态、展开的简化文本、实时查看和详细信息](/help/dashboards/opportunities/assets/simplify-complex-content-fixed.png)

点击此行上的&#x200B;**实时查看**，打开&#x200B;**当前页面内容**&#x200B;的只读视图用于进行验证（包括适用的简化文字段落）。 使用&#x200B;**详细信息**&#x200B;进行分析。

![实时查看：当前页面内容，包括为代理提供的简化文本](/help/dashboards/opportunities/assets/simplify-complex-content-view-live.png)

如果您需要批量还原边缘更改，请使用复选框选择已优化的行，然后使用标题中的&#x200B;**回滚**。

![“修复的建议”中包括已部署的展开的行、已优化的状态和标题中的回滚](/help/dashboards/opportunities/assets/simplify-complex-content-rollback.png)

## 回滚

如果您改变想法，可以撤消任何已部署的优化。 在&#x200B;**修复的建议**&#x200B;视图中，选择您想撤消的已优化的行，然后点击标题中的&#x200B;**回滚**。

**回滚**&#x200B;对话框中会列出将回滚的建议，还有一条将撤消已部署优化的简单警告。 确认此列表，然后点击&#x200B;**回滚**&#x200B;或&#x200B;**取消**。

![回滚对话框中列出了要撤消的建议](/help/dashboards/opportunities/assets/simplify-complex-content-rollback-dialog.png)

操作完成后，会显示&#x200B;**已成功回滚**&#x200B;的摘要消息；关闭消息，返回到仪表板。

![回滚已完成：已成功完成回滚](/help/dashboards/opportunities/assets/simplify-complex-content-rollback-confirm.png)

## 在演示中尝试此操作

在 [Frescopa 演示](https://play.llmo.now/org/demo-org)中了解简化复杂内容的工作流。
