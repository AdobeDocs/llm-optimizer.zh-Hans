---
title: 添加多媒体成绩单摘要
description: 了解LLM Optimizer如何识别关键信息嵌入到不含机器可读文本的视频中的页面，以及如何在Edge上使用优化查看和部署人工智能生成的转录摘要。
feature: Opportunities
source-git-commit: 36a6836f86b6d31cc4bf4682e881bd127edf66e4
workflow-type: tm+mt
source-wordcount: '775'
ht-degree: 0%

---


# 添加多媒体成绩单摘要

>[!NOTE]
>
> **提前访问** — 添加多媒体成绩单摘要可供提前访问。 随着功能的成熟，工作流的可用性、资格和部分可能会发生变化。 如果您对访问权限存有任何疑问，请联系您的Adobe客户团队。

“添加多媒体转录摘要”机会可识别重要信息存在于视频或其他媒体中的页面，这些页面没有转录或简短文本摘要，AI代理可以阅读。 它引入了&#x200B;**AI生成的转录摘要**，该摘要以媒体和周围的页面上下文为基础。 它通过使AI代理能够理解多媒体内容，帮助恢复原本会遗漏的关键品牌信息。

对于每个受影响的URL，您可以查看建议的&#x200B;**内容修补程序**、**实现**&#x200B;详细信息（例如，目标CSS选择器和操作）以及&#x200B;**基本原则**，然后使用[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)进行部署，这样代理流量就可以接收扩充的HTML，而无需更改内容管理系统(CMS)。

## 它如何修复问题

使用Edge](/help/dashboards/optimize-at-edge/overview.md)中的[优化来应用修复，其中：

- 向AI代理提供预渲染的HTML快照。
- 在HTML中用转录摘要文本丰富页面（例如，在相关的内联视频附近）。
- 在CDN层工作（CMS不会发生更改）。
- 是仅限AI的 — 不会对人类访客或SEO机器人造成影响。
- 几分钟即可部署，并且可从LLM Optimizer界面&#x200B;**完全还原**。

## 工作原理

LLM Optimizer会根据您的配置和页面结构，标记嵌入媒体缺少机器可读文本的高流量页面。 受影响的URL显示在&#x200B;**当前建议**&#x200B;选项卡上带有建议&#x200B;**的** URL表中，您可以在该表中展开一行以检查每个&#x200B;**内容修补程序**、其应用方式以及建议使用的原因。

对于每个页面，您具有：

**多媒体摘要** — 从视频内容派生的结构化摘要。
**预览** — 页面比较之前和之后。

![URL包含有关当前建议的建议、包含内容修补程序的扩展行、实施详细信息和基本信息](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-expand.png)

带有建议&#x200B;**表的** URL列出了转录文本或摘要文本将有助于代理发现的页面。 建议被整理为&#x200B;**当前建议**、**固定建议**&#x200B;和&#x200B;**忽略的建议**。 对于每个URL，您可以：

- **展开行**&#x200B;以查看&#x200B;**内容修补程序**&#x200B;文本、**实现**&#x200B;详细信息（包括计划的DOM操作和CSS选择器）以及更改的&#x200B;**理由**。
- **预览**&#x200B;代理流量的比较前和比较后。
- 如果您在LLM Optimizer外部处理了商机，请&#x200B;**标记为已修复**。
- **忽略不相关的**&#x200B;建议。

如果支持（铅笔控件），则可以从行中编辑修补程序文本，然后使用行复选框选择要部署的内容。 页脚显示选择的数量，并提供&#x200B;**标记为已修复**、**忽略建议**&#x200B;和&#x200B;**部署优化**。

### 部署优化

准备在边缘发布时，单击&#x200B;**部署优化**。 **部署到Edge**&#x200B;对话框列出了要运行的URL、选择器和操作。 查看列表，然后选择&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![部署到Edge对话框的多媒体转录摘要内容修补程序](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-deploy-dialog.png)

成功部署后，**部署完成**&#x200B;将确认有多少优化已上线。 关闭对话框并打开&#x200B;**修复建议**&#x200B;以验证状态。

![部署完成确认](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-deploy-confirm.png)

>[!NOTE]
>
> 部署优化需要完成Optimize at Edge载入流程。 如果您尚未载入，单击&#x200B;**部署优化**&#x200B;会将您引导至载入流程。 有关在Edge中优化的工作方式、支持的CDN提供商以及载入流程的完整详细信息，请参阅[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)页面。

### 修复了建议并实时查看

在&#x200B;**已修复建议**&#x200B;上，部署的URL在状态列中显示&#x200B;**已优化**。 展开一行以查看实时&#x200B;**内容修补程序**、**实施**&#x200B;详细信息和&#x200B;**基本信息** 。 此外，您还可以将&#x200B;**详细信息**&#x200B;用于Analytics，或者在可用处使用&#x200B;**实时查看**&#x200B;以确认代理流量将接收到什么内容。

![修复了具有优化状态、扩展的内容修补程序和回滚的建议](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-fixed.png)

要批量回滚，请使用复选框选择优化的行，然后在标题中使用&#x200B;**Rollback**。

![修复了在回滚](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-select-in-fixed.png)之前选择的行的建议

## 回滚

如果您改变主意，可以回退已部署的优化。 从&#x200B;**修复建议**&#x200B;中，选择要还原的行，然后单击&#x200B;**回滚**。

**Rollback**&#x200B;对话框列出了将回滚的建议，并警告已部署的优化将从代理流量的实时路径中删除。 确认，然后单击&#x200B;**回滚**&#x200B;或&#x200B;**取消**。

![回滚对话框列出了还原建议](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-rollback-dialog.png)

操作完成后，将显示&#x200B;**已成功回滚**&#x200B;摘要；关闭它以返回到仪表板。

![回滚完成 — 已成功回滚](/help/dashboards/opportunities/assets/add-multimedia-transcript-summaries-rollback-confirm.png)

## 在演示中尝试

在[Frescopa演示](https://play.llmo.now/org/demo-org)中探索“添加多媒体成绩单摘要”工作流。
