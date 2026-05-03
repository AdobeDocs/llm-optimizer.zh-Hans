---
title: 简化复杂内容
description: 了解LLM Optimizer如何通过人工智能代理难以解释的密集副本识别高流量页面，以及如何使用Edge中的优化查看和部署简化文本。
feature: Opportunities
source-git-commit: 7f0729839d761ca57236da935c8c7638dd92f32a
workflow-type: tm+mt
source-wordcount: '785'
ht-degree: 1%

---


# 简化复杂内容

简化复杂内容机会可识别高流量页面，其中密集的或复杂的文本会使AI代理更难解释关键信息。 它引入了更清晰、更易于扫描的现有副本版本，同时保留了原始含义。 这有助于代理更可靠地解析、总结和提取重要信息。

对于每个受影响的URL，您可以查看&#x200B;**改进文本**&#x200B;建议，将其与&#x200B;**预览**&#x200B;进行比较，然后使用[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)来部署这些建议，这样代理流量就可以接收更清晰的HTML，而无需更改内容管理系统(CMS)。

## 它如何修复问题

使用Edge[&#128279;](/help/dashboards/optimize-at-edge/overview.md)中的优化来应用修复，其中：

- 向AI代理提供预渲染的HTML快照。
- 更新代理可见页面，以便用&#x200B;**已改进文本**&#x200B;替换或增加复杂段落，使其与实时页面对齐。
- 在CDN层工作（CMS不会发生更改）。
- 是仅限AI的 — 不会对人类访客或SEO机器人造成影响。
- 几分钟即可部署，并且可从LLM Optimizer界面&#x200B;**完全还原**。

## 工作原理

LLM Optimizer会识别接收高代理流量以及内容分数低于可读性阈值的页面，然后建议重写副本。 对于每个页面，您拥有：

**改进的文本** — 简化的内容以页面上已有的内容为基础。
**预览** — 代理流量的比较前和比较后。

受影响的URL显示在&#x200B;**当前建议**&#x200B;选项卡上带有建议&#x200B;**的** URL表中，您可以在该表中展开一行来检查每个建议。

![URL包含有关当前建议的建议，扩展行包含改进的文本和预览](/help/dashboards/opportunities/assets/simplify-complex-content-expand.png)

带有建议&#x200B;**表的** URL列出了简化内容有助于进行智能理解的页面。 建议被整理为&#x200B;**当前建议**、**固定建议**&#x200B;和&#x200B;**忽略的建议**。 对于每个URL，您可以：

- **展开行**&#x200B;以查看该页面的&#x200B;**改进文本**&#x200B;建议。
- **预览**&#x200B;代理流量的比较前和比较后。
- 如果您在LLM Optimizer外部处理了商机，请&#x200B;**标记为已修复**。
- **忽略不相关的**&#x200B;建议。

**视图**&#x200B;包括&#x200B;**当前建议**、**固定建议**（部署时状态&#x200B;**已优化**）和&#x200B;**忽略的建议**。 您可以使用&#x200B;**固定建议**&#x200B;上的&#x200B;**查看实时**&#x200B;来验证实时部署，并随时回滚。

使用复选框选择要装运的URL或带有&#x200B;**改进文本**&#x200B;的行项目，然后在&#x200B;**Opportunity计划**&#x200B;标题中使用&#x200B;**标记为已修复**、**忽略建议**&#x200B;或&#x200B;**部署优化**。 演示UI还会显示选择计数以及与列表相关的操作。

![计划标题中的Opportunity计划、当前建议、展开行和部署优化](/help/dashboards/opportunities/assets/simplify-complex-content-select.png)

### 部署优化

准备在边缘发布时，单击&#x200B;**部署优化**。 **部署到Edge**&#x200B;对话框列出了选定的URL和优化详细信息。 查看列表，然后选择&#x200B;**部署**&#x200B;或&#x200B;**取消**。

![部署到Edge对话框](/help/dashboards/opportunities/assets/simplify-complex-content-deploy-dialog.png)

成功部署后，**部署完成**&#x200B;将确认有多少优化已上线，并指出AI代理可能需要一些时间为更新编制索引。 关闭对话框并打开&#x200B;**修复建议**&#x200B;以验证状态。

![部署完成确认](/help/dashboards/opportunities/assets/simplify-complex-content-deploy-confirm.png)

>[!NOTE]
>
>部署优化需要完成Optimize at Edge载入流程。 如果您尚未载入，单击&#x200B;**部署优化**&#x200B;会将您引导至载入流程。 有关在Edge中优化的工作方式、支持的CDN提供商以及载入流程的完整详细信息，请参阅[在Edge中优化](/help/dashboards/optimize-at-edge/overview.md)页面。

### 修复了建议并实时查看

在&#x200B;**已修复建议**&#x200B;上，部署的URL在状态列中显示&#x200B;**已优化**。 展开一行以查看已部署的&#x200B;**改进文本**&#x200B;和说明。

![修复了“建议”选项卡，其状态为“已优化”、已扩展的简化副本、查看实时信息和详细信息](/help/dashboards/opportunities/assets/simplify-complex-content-fixed.png)

单击行上的&#x200B;**实时查看**&#x200B;以打开作为验证依据的&#x200B;**当前页面内容**&#x200B;的只读视图（包括应用的简化段落）。 将&#x200B;**详细信息**&#x200B;用于分析。

![实时查看 — 当前页面内容，包括代理的简化文本](/help/dashboards/opportunities/assets/simplify-complex-content-view-live.png)

当需要批量还原边缘更改时，请使用复选框选择优化的行，然后在标题中使用&#x200B;**Rollback**。

![修复了标头中展开已部署行、优化状态和回滚的建议](/help/dashboards/opportunities/assets/simplify-complex-content-rollback.png)

## 回滚

如果您改变主意，可以回退任何已部署的优化。 从&#x200B;**修复建议**&#x200B;视图中，选择要还原的优化行，然后单击标题中的&#x200B;**回滚**。

**Rollback**&#x200B;对话框列出了将回滚的建议，并简短地警告将还原已部署的优化。 确认列表，然后单击&#x200B;**回滚**&#x200B;或&#x200B;**取消**。

![回滚对话框列出了还原建议](/help/dashboards/opportunities/assets/simplify-complex-content-rollback-dialog.png)

操作完成后，将显示&#x200B;**已成功回滚**&#x200B;摘要；关闭它以返回到仪表板。

![回滚完成 — 已成功回滚](/help/dashboards/opportunities/assets/simplify-complex-content-rollback-confirm.png)

## 在演示中尝试

在[Frescopa演示](https://play.llmo.now/org/demo-org)中探索简化复杂内容工作流程。
