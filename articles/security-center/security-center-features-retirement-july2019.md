---
title: 停用安全中心功能（2019年7月） |Microsoft Docs
description: 本文介绍 2019 年 7 月 31 日安全中心内停用的功能。
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: article
ms.date: 09/10/2019
ms.author: memildin
ms.openlocfilehash: 4a760fe1e5df7cf614a68f8fa8a05926326d3edf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "80583222"
---
# <a name="retirement-of-security-center-features-july-2019"></a>停用的安全中心功能（2019 年 7 月）

> [!NOTE]
> 本文档详细介绍了 2019 年 7 月 31 日从 Azure 安全中心停用的功能列表。
>
>

在截至 2019 年 7 月的六个月内，我们对 Azure 安全中心进行了多项[改进](https://azure.microsoft.com/updates/?product=security-center)。
伴随着这些改进的功能，我们于 2019 年 7 月 31 日从安全中心内删除了一些冗余功能和相关 API。

大多数停用的功能都可以由 Azure 安全中心或 Azure Log Analytics 中的其他功能所替代。 其他功能可以使用[Azure Sentinel （预览版）](https://azure.microsoft.com/services/azure-sentinel/)来实现。

停用的安全中心功能包括：

- [事件仪表板](#menu_events)
- [搜索菜单项](#menu_search)
- [在“标识和访问”上查看经典标识和访问链接（预览）](#menu_classicidentity)
- [安全警报映射上的安全事件映射按钮（预览版）](#menu_securityeventsmap)
- [自定义警报规则（预览）](#menu_customalerts)
- [威胁防护安全警报中的调查按钮](#menu_investigate)
- [安全解决方案的一个子集](#menu_solutions)
- [编辑安全策略的安全配置](#menu_securityconfigurations)
- [Log Analytics 工作区的安全和审核仪表板（最初在 OMS 门户中使用）](#menu_securityomsdashboard)

本文提供了每个停用功能的详细信息，以及实现替换功能可以采取的步骤。

## <a name="events-dashboard"></a>事件仪表板<a name="menu_events"></a>

安全中心使用 Log Analytics 代理收集计算机中各种与安全有关的配置和事件。 它将这些事件存储在工作区中。 [事件仪表板](https://docs.microsoft.com/azure/security-center/security-center-events-dashboard)让你能够查看这些数据，并为你提供 Log Analytics 的入口点。

我们停用了选择工作区时显示的事件仪表板：

![事件仪表板][2]

### <a name="events-dashboard---the-new-experience"></a>事件仪表板 - 新体验

我们鼓励你使用 Azure Log Analytics 的本机功能来查看工作区中的重要事件。

如果已在安全中心中创建了自定义的重要事件，则可以访问这些事件。 在 Log Analytics 中，转到“选择工作区” > “保存的搜索” 。 你的数据不会丢失或被修改。 Log Analytics 中的同一屏幕也提供了本机的重要事件。

![工作区 - 保存的搜索][3]

## <a name="search-menu-entry"></a>搜索菜单项<a name="menu_search"></a>

Azure 安全中心目前使用 Azure Monitor 日志搜索来检索和分析安全数据。 此屏幕用作 Log Analytics 搜索页的窗口，使用户能够在选定的工作区上运行搜索查询。 有关详细信息，请参阅 [Azure 安全中心搜索](https://docs.microsoft.com/azure/security-center/security-center-search)。 我们停用了此搜索窗口：

![“搜索”页][4]

### <a name="search-menu-entry---the-new-experience"></a>搜索菜单项 - 新体验

我们鼓励你使用 Azure Log Analytics 本机功能在工作区上执行搜索查询。 转到 Azure Log Analytics 并选择“日志”。

![Log Analytics 日志页][5]

## <a name="classic-identity--access-preview"></a>经典标识和访问（预览）<a name="menu_classicidentity"></a>

安全中心中的经典标识和访问体验当前在 Log Analytics 中显示标识和访问信息的仪表板。 若要查看此仪表板：

1. 选择“查看经典标识和访问”。

   ![标识页][6]

1. 查看“标识和访问仪表板”。

    ![标识页 - 工作区选择][7]

1. 选择工作区，在 Log Analytics 中打开“标识和访问”仪表板以查看工作区上的标识和访问信息。

   ![标识页面 - 仪表板][8]

我们停用了上述步骤中显示的三个屏幕。 你的数据在 Log Analytics 安全解决方案中仍然可用，并且没有被修改或删除。

### <a name="classic-identity--access-preview---the-new-experience"></a>经典标识和访问（预览）- 新体验

Log Analytics 仪表板在单个工作区中显示了见解。 但是，本机安全中心功能提供对所有订阅及其关联的所有工作区的可见性。 你可以访问易于使用的视图，该视图根据安全功能分数对建议进行排序，让你专注于重要事项。

可以通过在安全中心选择“标识和访问(预览)”来访问 Log Analytics 中的“标识和访问”仪表板的所有功能 。

![标识页 - 经典体验停用][9]

## <a name="security-events-map"></a>安全事件映射<a name="menu_securityeventsmap"></a>

安全中心提供安全警报映射来帮助识别安全威胁。 该图中的 "**中转到安全事件映射**" 按钮将打开一个仪表板，可在其中查看所选工作区的原始安全事件。

我们删除了 "**中转到安全事件" 映射**按钮和每个工作区的仪表板。

![安全警报映射-按钮][10]

如果选择 "**中转到安全事件" 映射**按钮，则会打开 "（现已停用）" "威胁智能" 仪表板。

![“威胁智能”仪表板][11]

选择工作区以查看其威胁智能仪表板时，将在 Log Analytics 中打开 "（现已停用）安全警报映射（预览版）" 屏幕。

![安全警报映射 Log Analytics][12]

你的现有数据将在 Log Analytics 安全解决方案中保持可用，并且未进行修改或删除。

### <a name="security-events-map---the-new-experience"></a>安全事件映射-新体验

我们鼓励你使用安全中心内内置的警报映射功能：**安全警报映射（预览版）**。 此功能提供优化的体验，并在所有订阅和关联的工作区中工作。 它为你提供了跨环境的高级视图，并不侧重于单个工作区。

## <a name="custom-alert-rules-preview"></a>自定义警报规则（预览）<a name="menu_customalerts"></a>

我们在2019年6月30日[停用了自定义警报体验](https://docs.microsoft.com/azure/security-center/security-center-custom-alert)，因为其基础结构已停用。 停用日期后，将不再生成自定义安全警报。
建议启用[Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/) ，并在其中重新创建自定义警报。 或者，你可以通过 Azure Monitor 日志警报创建警报。

用 Azure Sentinel 创建自定义警报：

1. [打开 Azure Sentinel](https://portal.azure.com/#create/Microsoft.ASI/preview)并选择要在其中存储自定义警报的工作区
1. 从菜单中选择 "**分析**"
1. 按照以下[教程](https://docs.microsoft.com/azure/sentinel/tutorial-detect-threats)中的说明，了解如何在 Azure Sentinel 中创建自定义警报

如果你不想使用 Azure Sentinel，可以使用 Azure Monitor 日志警报创建警报。 有关说明，请参阅[在 Azure Monitor 中](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-unified-log)使用 Azure Monitor 和日志警报 [创建、查看和管理日志警报](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log)。

![自定义警报][13]

有关自定义警报停用的详细信息，请参阅[Azure 安全中心（预览版）中的自定义警报规则](https://docs.microsoft.com/azure/security-center/security-center-custom-alert)。

## <a name="security-alerts-investigation"></a>安全警报调查<a name="menu_investigate"></a>

安全中心的调查功能有助于诊断潜在的安全事件。 利用此功能，您可以了解事件的范围并追踪其根本原因。 我们从安全中心删除了此功能，因为它已被替换为[Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/)中改进的体验。

![安全事件][14]

在 "**安全事件**" 屏幕中选择 "**调查**" 按钮时，将在 Log Analytics 中打开调查仪表板（预览）。 我们停用了调查仪表板。

你的现有数据将在 Log Analytics 安全解决方案中保持可用，并且未进行修改或删除。

![Log Analytics 中的调查仪表板][15]

### <a name="investigation---the-new-experience"></a>调查-新体验

我们鼓励你过渡到[Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/)以获得丰富的调查体验。 Azure Sentinel 提供强大的搜索和查询工具，可在组织的数据源中寻找安全威胁。

## <a name="subset-of-security-solutions"></a>安全解决方案子集<a name="menu_solutions"></a>

安全中心可以[在 Azure 中启用集成的安全解决方案](https://docs.microsoft.com/azure/security-center/security-center-partner-integration)。 我们从安全中心停用了以下合作伙伴解决方案。 这些解决方案在[Azure Sentinel](https://azure.microsoft.com/services/azure-sentinel/)中启用，还有许多其他数据源。

- [下一代防火墙和 web 应用程序防火墙解决方案](https://docs.microsoft.com/azure/sentinel/connect-data-sources)
- [集成支持通用事件格式（CEF）的安全解决方案](https://docs.microsoft.com/azure/sentinel/connect-common-event-format)
- [Microsoft 高级威胁分析](https://docs.microsoft.com/azure/sentinel/connect-azure-atp)
- [Azure AD 标识保护](https://docs.microsoft.com/azure/sentinel/connect-azure-ad-identity-protection)

停用后，不能添加或修改前面列表中提到的任何解决方案类型，无论是通过 UI 还是 API。 Azure 安全中心将不再发现这些合作伙伴解决方案的所有新实例。

如果你有现成的连接解决方案，我们建议你转到 Azure Sentinel。

![安全中心解决方案][16]

## <a name="edit-security-configurations-for-security-policies"></a>编辑安全策略的安全配置<a name="menu_securityconfigurations"></a>

Azure 安全中心应用 [150 多种建议的规则](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335)来监视安全配置，以便强化 OS。 这些规则适用于防火墙、审核、密码策略等。 如果发现计算机中的某项配置有漏洞，则安全中心会生成安全建议。 [编辑安全配置屏幕](https://docs.microsoft.com/azure/security-center/security-center-customize-os-security-config)使客户能够在安全中心内自定义默认的 OS 安全配置。

我们停用了此预览功能。 若要在停用日期后将安全配置重置回默认值，请使用[以下说明](https://github.com/Azure/Azure-Security-Center/tree/master/Powershell%20scripts/Reset%20security%20configurations%20customization)通过 API 和 Powershell 执行此操作。

![编辑安全配置][17]

### <a name="edit-security-configurations---the-new-experience"></a>编辑安全配置 - 新体验

我们打算让安全中心支持[来宾配置代理](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration)。 这样的更新将实现更丰富的功能集，包括支持更多操作系统，以及为来宾配置集成 Azure 来宾策略。 启用这些更改后，你还可以大规模控制配置并自动将它们应用于新资源。

## <a name="security-and-audit-dashboard-for-log-analytics-workspaces"></a>Log Analytics 工作区的安全和审核仪表板<a name="menu_securityomsdashboard"></a>

安全和审核仪表板最初在 OMS 门户中使用。 在 Log Analytics 中，仪表板提供每个工作区重要安全事件和威胁的概述、威胁情报映射以及保存在工作区中的安全事件的标识和访问评估。 我们删除了该仪表板。 正如仪表板 UI 中提供的建议，我们建议你转到 Azure 安全中心。

![Log Analytics 安全仪表板][18]

### <a name="security-and-audit-dashboard---the-new-experience"></a>安全和审核仪表板 - 新体验

建议切换到 Azure 安全中心。 它跨多个订阅及关联的工作区，提供相同的安全概况以及更丰富的功能集。

你可以在 [GitHub 存储库](https://github.com/Azure/Azure-Security-Center/tree/master/Legacy%20Log%20Analytics%20dashboards)中获取填充安全和审核仪表板的原始 Log Analytics 查询，并将这些查询用于安全中心。

## <a name="next-steps"></a>后续步骤

- 了解有关 [Azure 安全中心](https://docs.microsoft.com/azure/security-center/)的详细信息。
- 了解有关[Azure Sentinel](https://docs.microsoft.com/azure/sentinel)的详细信息。

<!--Image references - events-->
[1]: ./media/security-center-features-retirement-july2019/asc_events_dashboard.png
[2]: ./media/security-center-features-retirement-july2019/asc_events_dashboard_inner.png
[3]: ./media/security-center-features-retirement-july2019/workspace_saved_searches.png
<!--Image references - search-->
[4]: ./media/security-center-features-retirement-july2019/asc_search.png
[5]: ./media/security-center-features-retirement-july2019/workspace_logs.png
<!--Image references - classic identity and access-->
[6]: ./media/security-center-features-retirement-july2019/asc_identity.png
[7]: ./media/security-center-features-retirement-july2019/asc_identity_workspace_selection.png
[8]: ./media/security-center-features-retirement-july2019/loganalytics_dashboard_identity.png
[9]: ./media/security-center-features-retirement-july2019/asc_identity_nobuttonhighlight.png
<!--Image references - alerts map-->
[10]: ./media/security-center-features-retirement-july2019/asc_security_alerts_map.png
[11]: ./media/security-center-features-retirement-july2019/asc_threat_intellignece_dashboard.png
[12]: ./media/security-center-features-retirement-july2019/loganalytics_security_alerts_map.png
<!--Image references - custom alerts-->
[13]: ./media/security-center-features-retirement-july2019/asc_custom_alerts.png
<!--Image references - Investigation-->
[14]: ./media/security-center-features-retirement-july2019/asc-security-incident.png
[15]: ./media/security-center-features-retirement-july2019/loganalytics_investigation_dashboard.png
<!--Image references - Solutions-->
[16]: ./media/security-center-features-retirement-july2019/asc_security_solutions.png
<!--Image references - Edit security configurations-->
[17]: ./media/security-center-features-retirement-july2019/asc_edit_security_configurations.png
<!--Image references - Security dashboard in log analytics-->
[18]: ./media/security-center-features-retirement-july2019/loganalytics_security_dashboard.png
