---
title: 添加LLM友好的摘要
description: 了解LLM Optimizer如何识别缺少AI代理简洁摘要和关键点的高流量页面，以及如何使用Edge中的优化功能查看和部署这些页面。
feature: Opportunities
source-git-commit: 7f0729839d761ca57236da935c8c7638dd92f32a
workflow-type: tm+mt
source-wordcount: '793'
ht-degree: 0%

---


# 添加LLM友好的摘要

添加LLM友好型摘要机会可识别缺少简洁结构化摘要的高流量页面，这会使AI代理更难快速了解页面上的关键信息。 它引入以现有页面内容为基础的清晰摘要和要点。 这有助于代理更高效地解释和捕获重要的品牌声明，并提高将您的内容准确包含在AI响应中的可能性。

对于每个受影响的URL，您可以查看人工智能生成的建议，然后通过[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)来部署这些建议，这样代理流量会变得更清晰、可扫描的上下文，而无需更改内容管理系统(CMS)。

## 它如何修复问题

使用Edge](/help/dashboards/optimize-at-edge/overview.md)中的[优化来应用修复，其中：

- 向AI代理提供预渲染的HTML快照。
- 在HTML中通过所检索到的摘要和/或关键点丰富页面。
- 在CDN层工作（CMS不会发生更改）。
- 是仅限AI的 — 不会对人类访客或SEO机器人造成影响。
- 几分钟即可部署，并且可从LLM Optimizer界面&#x200B;**完全还原**。

## 工作原理

LLM Optimizer标识高流量页面，其中页面或区域级别&#x200B;**摘要**&#x200B;和&#x200B;**关键点**&#x200B;将帮助AI理解。 受影响的URL显示在&#x200B;**当前建议**&#x200B;选项卡上带有建议&#x200B;**的** URL表中，您可以在该表中展开一行来检查每个建议。

![URL包含有关当前建议的建议，展开的行包含页面和部分摘要建议](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-expand.png)

带有建议&#x200B;**表的** URL列出了摘要将有助于代理发现的页面。 建议被整理为&#x200B;**当前建议**、**固定建议**&#x200B;和&#x200B;**忽略的建议**。 对于每个URL，您可以：

- **展开行**&#x200B;以查看分析和提议的摘要文本（以及包含的关键点）。
- **预览**&#x200B;代理流量的比较前和比较后。
- 如果您在LLM Optimizer外部处理了商机，请&#x200B;**标记为已修复**。
- **忽略不相关的**&#x200B;建议。

每个扩展条目显示页面级和节级摘要说明、**AI生成的**&#x200B;副本、编辑控件以及与实时页面关联的上下文。

单击&#x200B;**操作**&#x200B;列中的&#x200B;**预览**&#x200B;以打开优化预览。 它将比较您的页面现在查找代理流量的方式与优化后的视图（例如，插入与建议投放位置对齐的&#x200B;**摘要**&#x200B;和&#x200B;**关键点**&#x200B;内容）。 您可以在部署之前随时打开或取消该预览。

准备发布时，使用复选框选择摘要和关键点行项目。 页脚显示选择的数量，并提供&#x200B;**标记为已修复**、**忽略建议**&#x200B;和&#x200B;**部署优化**。

![当前建议，已选择摘要行项目，并在页脚中部署优化](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-select-url.png)

### 部署优化

准备在边缘发布时，单击&#x200B;**部署优化**。 **部署到Edge**&#x200B;对话框列出了选定的URL和优化详细信息。 查看列表，然后选择&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![部署到Edge对话框](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-deploy-dialog.png)

成功部署后，**部署完成**&#x200B;将确认有多少优化已上线，并指出AI代理可能需要一些时间为更新编制索引。 关闭对话框并打开&#x200B;**修复建议**&#x200B;以验证状态。

![部署完成确认](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-deploy-confirm.png)

>[!NOTE]
>
>部署优化需要完成Optimize at Edge载入流程。 如果您尚未载入，单击&#x200B;**部署优化**&#x200B;会将您引导至载入流程。 有关在Edge中优化的工作方式、支持的CDN提供商以及载入流程的完整详细信息，请参阅[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)页面。

### 修复了建议并实时查看

在&#x200B;**已修复建议**&#x200B;上，部署的URL在状态列中显示&#x200B;**已优化**。 展开一行可查看已部署的摘要副本和说明。

![修复了“建议”选项卡，其状态为“已优化”、已部署的摘要已展开、正在查看、详细信息已上线](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-fixed.png)

单击行上的&#x200B;**查看实时内容**&#x200B;以打开作为验证服务的&#x200B;**当前页面内容**&#x200B;的只读视图（包括插入的&#x200B;**摘要**&#x200B;和已应用的&#x200B;**关键点**&#x200B;块）。 将&#x200B;**详细信息**&#x200B;用于分析。 当需要批量还原边缘更改时，请使用复选框选择优化的行，然后在标题中使用&#x200B;**Rollback**。

![修复了在回滚之前带有批量选择复选框的建议](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-select-in-fixed.png)

## 回滚

如果您改变主意，可以回退任何已部署的优化。 从&#x200B;**修复建议**&#x200B;视图中，选择要还原的优化行，然后单击标题中的&#x200B;**回滚**。

**Rollback**&#x200B;对话框列出了将回滚的建议，并简短地警告将还原已部署的优化。 确认列表，然后单击&#x200B;**回滚**&#x200B;或&#x200B;**取消**。

![回滚对话框列出了还原建议](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-rollback-dialog.png)

操作完成后，将显示&#x200B;**已成功回滚**&#x200B;摘要；关闭它以返回到仪表板。

![回滚完成 — 已成功回滚](/help/dashboards/opportunities/assets/add-llm-friendly-summaries-rollback-confirm.png)

## 在演示中尝试

在[Frescopa演示](https://play.llmo.now/org/demo-org)中探索添加LLM友好的摘要工作流。
