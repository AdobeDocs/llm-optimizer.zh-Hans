---
title: 添加多媒体文字记录摘要
description: 了解 LLM Optimizer 如何识别那些视频中嵌入了关键信息，但没有机器可读的文本的页面，以及如何通过 Optimize at Edge 审阅和部署 AI 生成的文字记录摘要。
feature: Opportunities
autotag-review: '2026-05-15T17:28:28.569Z'
TQID: 'https://experienceleague.adobe.com/LiXMsMq6D08ciXR85aQBNDpmR5Csiv-5b9kv3lfTpDc'
product_v2: id: d830747e-f8f3-4fce-8eff-d53b333b1639
feature_v2: id: c0713b97-4af8-4c41-b742-5afcc6ced468
subfeature_v2: id: e1b649f0-0a61-46e4-9082-64d5cb2576c6
topic_v2: id: cdd65e7e-8839-44a2-bc21-0e03623b5dd1
source-git-commit: 7a92587197cf6a9eec6b01bd4eaeeaf1194d3088
workflow-type: tm+mt
source-wordcount: 775
ht-degree: 100%

---


# 添加多媒体文字记录摘要

>[!NOTE]
>
> **早期访问**：“添加多媒体文字记录摘要”在早期访问中提供。 随着此功能逐步成熟，工作流的可用性、资格和某些部分可能会发生变化。 如果您对访问权限有任何疑问，请联系您的 Adobe 帐户团队。

“添加多媒体文字记录摘要”机会可识别那些视频或其他媒体中包含重要信息，但没有可供 AI 代理读取的文字记录或简短文本摘要的页面。 它会引入基于媒体和周围页面上下文的 **AI 生成的文字记录摘要**。 它能帮助恢复那些原本会被遗漏的关键品牌信息，使 AI 代理能够理解多媒体内容。

对于每一个受影响的 URL，您可以审阅所建议的&#x200B;**内容补丁**、**实施**&#x200B;详细信息（例如目标 CSS 选择器和操作）和&#x200B;**原因**，然后通过 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 进行部署，使代理式流量可以获得扩充后的 HTML，而无需更改内容管理系统 (CMS)。

## 它如何修复问题

通过 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 修复问题，它能够：

- 为 AI 代理提供一个预渲染的 HTML 快照。
- 扩充页面，在它们检索的 HTML 中添加文字记录摘要文本（例如在相关的嵌入视频的旁边）。
- 在内容传递网络层上工作（无需更改 CMS）。
- 仅限 AI，对人类访客或 SEO 机器人没有任何影响。
- 几分钟即可部署，并可从 LLM Optimizer 界面将其&#x200B;**完全撤销**。

## 工作原理

LLM Optimizer 会根据您的配置和页面结构，标记那些使用嵌入媒体但缺少机器可读文本的高流量页面。 **当前建议**&#x200B;选项卡的&#x200B;**包含建议的 URL** 表格中会列出受影响的 URL，您可以展开表中的一行，检查每个&#x200B;**内容补丁**、如何应用以及为什么建议使用这个补丁。

对于每个页面您都能获得：

**多媒体摘要**：从视频内容派生的结构化摘要。
**预览**：之前和之后的页面效果比较。

![“当前建议”中包含建议的 URL，展开的行中包含内容补丁、实施详细信息和原因](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-expand.png)

**包含建议的 URL** 表格中列出了那些文字记录或摘要文本将有助于代理发现这些内容的页面。 所有建议分成&#x200B;**当前建议**、**修复的建议**&#x200B;和&#x200B;**忽略的建议**&#x200B;三类。 对于每一个 URL，您可以：

- **展开此行**，查看&#x200B;**内容补丁**&#x200B;文本、**实施**&#x200B;详细信息（包括计划的 DOM 操作和 CSS 选择器）以及进行这一更改的&#x200B;**原因**。
- **预览**&#x200B;之前和之后的代理式流量的效果比较。
- 如果您在 LLM Optimizer 以外解决了这个机会，可将其&#x200B;**标记为已修复**。
- **忽略**&#x200B;那些不相关的建议。

如果支持相应功能，您可以在这一行中编辑补丁文本（铅笔控件），然后勾选行的复选框，选择要部署的建议。 页脚中会显示选定的数量，并提供&#x200B;**标记为已修复**、**忽略建议**&#x200B;和&#x200B;**部署优化**&#x200B;几个选项。

### 部署优化

如果您已准备好在边缘发布，点击&#x200B;**部署优化**。 **部署到边缘**&#x200B;对话框中列出了 URL、选择器和您想运行的操作。 审阅列表，然后选择&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![多媒体文字记录摘要内容补丁的“部署到边缘”对话框](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-deploy-dialog.png)

成功完成部署后，**部署已完成**&#x200B;会确认有多少优化已上线。 关闭此对话框，然后打开&#x200B;**修复的建议**，验证状态。

![部署完成确认](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-deploy-confirm.png)

>[!NOTE]
>
> 部署优化过程需要完成 Optimize at Edge 加入过程。 如果您尚未加入，点击&#x200B;**部署优化**&#x200B;会引导您完成加入过程。 关于 Optimize at Edge 如何工作、受支持的内容传递网络提供程序以及加入过程的完整详细信息，请参阅 [Optimize at Edge](/help/dashboards/optimize-at-edge/overview.md) 页面。

### 修复的建议和实时查看

在&#x200B;**修复的建议**&#x200B;中，已部署的 URL 会在状态列中显示&#x200B;**已优化**。 展开一行，实时审阅&#x200B;**内容补丁**、**实施**&#x200B;详细信息和&#x200B;**原因**。 此外，您还可以通过&#x200B;**详细信息**&#x200B;查看分析，或者在可用的情况下通过&#x200B;**实时查看**&#x200B;确认代理式流量获得了哪些内容。

![“修复的建议”中的“已优化”状态、展开的内容补丁和回滚](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-fixed.png)

如要批量回滚，可通过复选框选择已优化的行，然后使用标题中的&#x200B;**回滚**。

![“修复的建议”中选择了几行进行回滚](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-select-in-fixed.png)

## 回滚

如果您改变想法，可以撤消已部署的优化。 在&#x200B;**修复的建议**&#x200B;中选择要恢复的行，然后点击&#x200B;**回滚**。

**回滚**&#x200B;对话框中列出了将回滚的建议，以及关于已部署的优化将从代理式流量的实时路径中移除的警告。 确认，然后点击&#x200B;**回滚**&#x200B;或&#x200B;**取消**。

![回滚对话框中列出了要撤消的建议](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-rollback-dialog.png)

操作完成后，会显示&#x200B;**已成功回滚**&#x200B;的摘要消息；关闭消息，返回到仪表板。

![回滚已完成：已成功完成回滚](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-rollback-confirm.png)

## 在演示中尝试此操作

在 [Frescopa 演示](https://play.llmo.now/org/demo-org)中了解“添加多媒体文字记录摘要”工作流。
