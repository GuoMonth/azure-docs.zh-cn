---
title: 从 Azure 计划程序迁移到 Azure 逻辑应用
description: 了解如何将即将停用的 Azure 计划程序中的作业替换为 Azure 逻辑应用
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 02/29/2020
ms.openlocfilehash: dd61ac9751010d57cbf5b742a5081beb3ac560e9
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/25/2020
ms.locfileid: "83826054"
---
# <a name="migrate-azure-scheduler-jobs-to-azure-logic-apps"></a>将 Azure 计划程序作业迁移到 Azure 逻辑应用

> [!IMPORTANT]
> [Azure 逻辑应用](../logic-apps/logic-apps-overview.md)将替代[即将停用](#retire-date)的 Azure 计划程序。 若要继续使用在计划程序中设置的作业，请按照本文所述尽快迁移到 Azure 逻辑应用。 
>
> 计划程序在 Azure 门户中不再可用，但 [REST API](/rest/api/scheduler) 和 [Azure 计划程序 PowerShell cmdlet](scheduler-powershell-reference.md) 目前仍可用，以便你可以管理作业和作业集合。

本文介绍如何通过使用 Azure 逻辑应用而非 Azure 计划程序来创建自动化工作流，从而安排一次性作业和重复作业。 使用逻辑应用创建计划的作业时，将获得以下好处：

* 使用可视化设计器和来自 Azure Blob 存储、Azure 服务总线、Office 365 Outlook 和 SAP 等数百种服务的[现成连接器](../connectors/apis-list.md)来构建作业。

* 将计划的每个工作流作为一级 Azure 资源进行管理。 不必担心作业集合的概念，因为每个逻辑应用都是一个单独的 Azure 资源。

* 使用单个逻辑应用运行多个一次性作业。

* 设置支持时区并自动调整为夏令时 (DST) 的计划。

若要了解详细信息，请参阅[什么是 Azure 逻辑应用？](../logic-apps/logic-apps-overview.md)或尝试在此快速入门中创建自己的第一个逻辑应用：[创建自己的第一个逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。

## <a name="prerequisites"></a>先决条件

* Azure 订阅。 如果没有 Azure 订阅，请[注册一个免费 Azure 帐户](https://azure.microsoft.com/free/)。

* 若要通过发送 HTTP 请求来触发逻辑应用，请使用 [Postman 桌面应用](https://www.getpostman.com/apps)等工具。

## <a name="migrate-by-using-a-script"></a>使用脚本进行迁移

每种计划程序作业具有其独特性，因此不存在通用的工具可以将所有计划程序作业迁移到 Azure 逻辑应用。 不过，可以[编辑此脚本](https://github.com/Azure/logicapps/tree/master/scripts/scheduler-migration)以满足自己的需求。

## <a name="schedule-one-time-jobs"></a>安排一次性作业

只需创建一个逻辑应用即可运行多个一次性作业。

1. 在 [Azure 门户](https://portal.azure.com)的逻辑应用设计器中创建一个空白的逻辑应用。

   有关基本步骤，请遵循[快速入门：创建第一个逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。

1. 在搜索框中，输入 `when a http request` 以查找请求触发器。 从触发器列表中选择此触发器：当收到 HTTP 请求时

   ![添加“请求”触发器](./media/migrate-from-scheduler-to-logic-apps/request-trigger.png)

1. 对于请求触发器，可以选择提供 JSON 架构，这有助于逻辑应用设计器了解包含在请求触发器入站调用中的输入的结构，使你稍后在工作流中更易选择输出。

   在“请求正文 JSON 架构”框中输入架构，例如：

   ![请求架构](./media/migrate-from-scheduler-to-logic-apps/request-schema.png)

   如果你没有架构，但有一个 JSON 格式的示例有效负载，则可以基于该有效负载生成一个架构。

   1. 在请求触发器中，选择“使用示例有效负载生成架构”。

   1. 在“输入或粘贴示例 JSON 有效负载”下，提供示例有效负载并选择“完成”，例如 ：

      ![示例有效负载](./media/migrate-from-scheduler-to-logic-apps/sample-payload.png)

      ```json
      {
         "runat": "2012-08-04T00:00Z",
         "endpoint": "https://www.bing.com"
      }
      ```

1. 在触发器下，选择“下一步”。

1. 在搜索框中，输入 `delay until` 作为筛选器。 在操作列表中选择以下操作：延迟截止时间

   此操作会暂停逻辑应用工作流，直到指定的日期和时间。

   ![添加“延迟截止时间”操作](./media/migrate-from-scheduler-to-logic-apps/delay-until.png)

1. 输入要启动逻辑应用工作流的时间戳。

   在“时间戳”框内单击时，系统会显示动态内容列表，这样你可以根据需要选择触发器的输出。

   ![提供“延迟截止时间”详细信息](./media/migrate-from-scheduler-to-logic-apps/delay-until-details.png)

1. 通过从[数百个现成的连接器](../connectors/apis-list.md)中进行选择，来添加要运行的任何其他操作。

   例如，可以包含向 URL 发送请求的 HTTP 操作，或包含处理存储队列、服务总线队列或服务总线主题的操作：

   ![HTTP 操作](./media/migrate-from-scheduler-to-logic-apps/request-http-action.png)

1. 完成后，保存逻辑应用。

   ![保存逻辑应用](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

   第一次保存逻辑应用时，逻辑应用的请求触发器的终结点 URL 会显示在“HTTP POST URL”框中。 如果要调用逻辑应用并将输入发送到逻辑应用进行处理，请使用此 URL 作为调用目标。

   ![保存请求触发器终结点 URL](./media/migrate-from-scheduler-to-logic-apps/request-endpoint-url.png)

1. 复制并保存此终结点 URL，以便稍后发送触发逻辑应用的手动请求。

## <a name="start-a-one-time-job"></a>启动一次性作业

若要手动运行或触发一次性作业，请发送对逻辑应用的请求触发器终结点 URL 的调用。 在此调用中，请指定要发送的输入或有效负载，你之前可能已通过指定架构进行了描述。

例如，通过使用 Postman 应用，可以采用与此示例类似的设置创建 POST 请求，然后选择“发送”以发出请求。

| 请求方法 | 代码 | Body | 头文件 |
|----------------|-----|------|---------|
| **POST** | <endpoint-URL> | **raw** <p>**JSON(application/json)** <p>在“raw”框中，输入要在请求中发送的有效负载。 <p>**注意**：此设置会自动配置“标头”值。 | **密钥**：Content-Type <br>**值**：application/json |
|||||

![发送请求以手动触发逻辑应用](./media/migrate-from-scheduler-to-logic-apps/postman-send-post-request.png)

发送调用后，逻辑应用的响应会显示在“正文”选项卡中的“raw”框下方 。 

<a name="workflow-run-id"></a>

> [!IMPORTANT]
>
> 如果稍后想取消作业，请选择“标头”选项卡。在响应中查找并复制 **x-ms-workflow-run-id** 标头值。 
>
> ![响应](./media/migrate-from-scheduler-to-logic-apps/postman-response.png)

## <a name="cancel-a-one-time-job"></a>取消一次性作业

在逻辑应用中，每个一次性作业均作为单个逻辑应用运行实例执行。 若要取消一次性作业，可以使用逻辑应用 REST API 中的[工作流运行 - 取消](https://docs.microsoft.com/rest/api/logic/workflowruns/cancel)。 发送对触发器的调用时，请提供[工作流运行 ID](#workflow-run-id)。

## <a name="schedule-recurring-jobs"></a>安排重复作业

1. 在 [Azure 门户](https://portal.azure.com)的逻辑应用设计器中创建一个空白的逻辑应用。

   有关基本步骤，请遵循[快速入门：创建第一个逻辑应用](../logic-apps/quickstart-create-first-logic-app-workflow.md)。

1. 在搜索框中，输入“定期”作为筛选器。 从触发器列表中选择此触发器：**定期**

   ![添加“重复”触发器](./media/migrate-from-scheduler-to-logic-apps/recurrence-trigger.png)

1. 如果需要，可以设置更高级的计划。

   ![高级计划](./media/migrate-from-scheduler-to-logic-apps/recurrence-advanced-schedule.png)

   有关高级计划选项的详细信息，请参阅[使用 Azure 逻辑应用创建和运行重复任务和工作流](../connectors/connectors-native-recurrence.md)。

1. 通过从[数百个现成的连接器](../connectors/apis-list.md)中进行选择，来添加所需的其他操作。 在触发器下，选择“下一步”。 查找并选择所需的操作。

   例如，可以包含向 URL 发送请求的 HTTP 操作，或包含处理存储队列、服务总线队列或服务总线主题的操作：

   ![HTTP 操作](./media/migrate-from-scheduler-to-logic-apps/recurrence-http-action.png)

1. 完成后，保存逻辑应用。

   ![保存逻辑应用](./media/migrate-from-scheduler-to-logic-apps/save-logic-app.png)

## <a name="advanced-setup"></a>高级设置

下面是可以自定义作业的其他方法。

### <a name="retry-policy"></a>重试策略

若要控制在出现间歇性失败时操作尝试在逻辑应用中重新运行的方式，可以在每个操作的设置中设置[重试策略](../logic-apps/logic-apps-exception-handling.md#retry-policies)，例如：

1. 打开该操作的省略号 (…) 菜单，选择“设置” 。

   ![打开操作设置](./media/migrate-from-scheduler-to-logic-apps/action-settings.png)

1. 选择所需的重试策略。 有关每个策略的详细信息，请参阅[重试策略](../logic-apps/logic-apps-exception-handling.md#retry-policies)。

   ![选择重试策略](./media/migrate-from-scheduler-to-logic-apps/retry-policy.png)

## <a name="handle-exceptions-and-errors"></a>处理异常和错误

在 Azure 计划程序中，如果默认操作无法运行，则可以运行解决错误情况的替代操作。 在 Azure 逻辑应用中，也可以执行相同的任务。

1. 在逻辑应用设计器中要处理的操作上方，将指针移到各步骤之间的箭头上，然后选择“添加并行分支”。

   ![添加并行分支](./media/migrate-from-scheduler-to-logic-apps/add-parallel-branch.png)

1. 查找并选择要作为替代操作运行的操作。

   ![添加并行操作](./media/migrate-from-scheduler-to-logic-apps/add-parallel-action.png)

1. 在替代操作上，打开省略号 (…) 菜单，选择“配置随后运行” 。

   ![配置随后运行](./media/migrate-from-scheduler-to-logic-apps/configure-run-after.png)

1. 清除“成功”属性的相应框。 选择以下属性：“失败”、“已跳过”和“已超时”  

   ![设置“随后运行”属性](./media/migrate-from-scheduler-to-logic-apps/select-run-after-properties.png)

1. 完成后，选择“完成”。

若要详细了解异常处理，请参阅[处理错误和异常 - RunAfter 属性](../logic-apps/logic-apps-exception-handling.md#control-run-after-behavior)。

## <a name="faq"></a>常见问题解答

<a name="retire-date"></a>

**问**：Azure 计划程序何时停用？ <br>
**答**：Azure 计划程序计划于 2019 年 12 月 31 日彻底停用。 如需查看要在此日期之前执行的重要步骤和详细的时间线，请参阅[将计划程序的停用日期延长至 2019 年 12 月 31 日](https://azure.microsoft.com/updates/extending-retirement-date-of-scheduler/)。 如需查看常规更新，请参阅 [Azure 更新 - 计划程序](https://azure.microsoft.com/updates/?product=scheduler)。

**问**：该服务停用后，对我的作业集合和作业有何影响？ <br>
**答**：所有计划程序作业集合和作业将从系统中删除。

**问**：在将我的计划程序作业迁移到逻辑应用之前，是否必须备份或执行任何其他任务？ <br>
**答**：最佳做法是始终备份你的工作。 在删除或禁用计划程序作业之前，请检查你创建的逻辑应用是否按预期运行。

**问**：是否有可以帮助我将作业从计划程序迁移到逻辑应用的工具？ <br>
**答**：每个计划程序作业都是唯一的，因此不存在通用的工具。 但是，可以根据需要[编辑此脚本，将 Azure 计划程序作业迁移到 Azure 逻辑应用](https://github.com/Azure/logicapps/tree/master/scripts/scheduler-migration)。

**问**：迁移计划程序作业时，可从何处获得支持？ <br>
**答**：以下是获得支持的一些方法：

**Azure 门户**

如果 Azure 订阅具有付费支持计划，则可以在 Azure 门户中创建技术支持请求。 如果没有，则可以选择其他支持选项。

1. 在 [Azure 门户](https://portal.azure.com)主菜单中，选择“帮助和支持”。

1. 在“支持”菜单中，选择“新建支持请求” 。 为请求提供的以下信息：

   | properties | 值 |
   |---------|-------|
   | **问题类型** | **技术** |
   | **订阅** | <*your-Azure-subscription*> |
   | **服务** | 在“监视和管理”下，选择“计划程序” 。 如果找不到“计划程序”，请先选择“所有服务” 。 |
   ||| 

1. 选择所需的支持选项。 如果有付费支持计划，请选择“下一步”。

**社区**

* [有关 Azure 逻辑应用的 Microsoft Q&A 问题页面](https://docs.microsoft.com/answers/topics/azure-logic-apps.html)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-scheduler)

## <a name="next-steps"></a>后续步骤

* [使用 Azure 逻辑应用创建定期运行的任务和工作流](../connectors/connectors-native-recurrence.md)