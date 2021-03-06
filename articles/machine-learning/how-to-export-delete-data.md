---
title: 导出或删除工作区数据
titleSuffix: Azure Machine Learning
description: 了解如何使用 Azure 机器学习工作室、CLI、SDK 和经过身份验证的 REST API 导出或删除工作区。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: jmartens
author: lobrien
ms.author: laobri
ms.date: 04/24/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: 657f6dd32c18b5b0745883da02563e9f5257d92d
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87307210"
---
# <a name="export-or-delete-your-machine-learning-service-workspace-data"></a>导出或删除机器学习服务工作区数据

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

在 Azure 机器学习中，可以使用经过身份验证的 REST API 导出或删除工作区数据。 本文介绍了相关实现方法。

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-dsr-and-stp-note.md)]

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="control-your-workspace-data"></a>控制工作区数据

通过 Azure 机器学习工作室、CLI、SDK 和经过身份验证的 REST API，可导出和删除 Azure 机器学习存储的产品内数据。 可通过 Azure 隐私门户访问遥测数据。 

在 Azure 机器学习中，个人数据包括运行历史记录文档中的用户信息以及用户与服务之间的一些交互的遥测数据记录。

## <a name="delete-workspace-data-with-the-rest-api"></a>使用 REST API 删除工作区数据

若要删除数据，可以使用 HTTP DELETE 谓词进行以下 API 调用。 它们是通过在请求中放置 `Authorization: Bearer <arm-token>` 标头进行授权的，其中 `<arm-token>` 是 `https://management.core.windows.net/` 终结点的 AAD 访问令牌。  

若要了解如何获取此令牌并调用 Azure 终结点，请参阅[使用 REST 管理 ML 资源](how-to-manage-rest.md)和 [Azure REST API 文档](https://docs.microsoft.com/rest/api/azure/)。  

在下面的示例中，请将 {} 中的文本替换为决定了相关资源的实例名称。

### <a name="delete-an-entire-workspace"></a>删除整个工作区

使用以下调用来删除整个工作区。  
> [!WARNING]
> 所有信息将被删除，工作区将不再可用。

`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2019-11-01`

### <a name="delete-models"></a>删除模型

使用以下调用来获取模型及其 ID 的列表：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2019-11-01`

可以使用以下调用删除各个模型：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2019-11-01`

### <a name="delete-assets"></a>删除资产

使用以下调用来获取资产及其 ID 的列表：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2019-11-01`

可以使用以下调用删除各个资产：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2019-11-01`

### <a name="delete-images"></a>删除映像

使用以下调用来获取映像及其 ID 的列表：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2019-11-01`

可以使用以下调用删除各个映像：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2019-11-01`

### <a name="delete-services"></a>删除服务

使用以下调用来获取服务及其 ID 的列表：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2019-11-01`

可以使用以下调用删除各个服务：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2019-11-01`

## <a name="export-service-data-with-the-rest-api"></a>使用 REST API 导出服务数据

若要导出数据，可以使用 HTTP GET 谓词进行以下 API 调用。 它们是通过在请求中放置 `Authorization: Bearer <arm-token>` 标头进行授权的，其中，`<arm-token>` 是终结点 `https://management.core.windows.net/` 的 AAD 访问令牌。  

若要了解如何获取此令牌并调用 Azure 终结点，请参阅[使用 REST 管理 ML 资源](how-to-manage-rest.md)和 [Azure REST API 文档](https://docs.microsoft.com/rest/api/azure/)。   

在下面的示例中，请将 {} 中的文本替换为决定了相关资源的实例名称。

### <a name="export-workspace-information"></a>导出工作区信息

使用以下调用来获取所有工作区的列表：

`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces?api-version=2019-11-01`

可以使用以下调用获取有关各个工作区的信息：

`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}?api-version=2019-11-01`

### <a name="export-compute-information"></a>导出计算信息

可以使用以下调用获取附加到工作空间的所有计算目标：

`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes?api-version=2019-11-01`

可以使用以下调用获取有关单一计算目标的信息：

`https://management.azure.com/subscriptions/{subscriptionId}/resourceGroup/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/computes/{computeName}?api-version=2019-11-01`

### <a name="export-run-history-data"></a>导出运行历史记录数据

使用以下调用来获取所有试验及其信息的列表：

`https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments`

可以使用以下调用获取特定试验的所有运行：

`https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs`

可以使用以下调用获取运行历史记录项：

`https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/runs/{runId}`

可以使用以下调用获取试验的所有运行指标：

`https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics`

可以使用以下调用获取单一运行指标：

`https://{location}.experiments.azureml.net/history/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/experiments/{experimentName}/metrics/{metricId}`

### <a name="export-artifacts"></a>导出组件

使用以下调用来获取所有组件及其路径的列表：

`https://{location}.experiments.azureml.net/artifact/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/artifacts/origins/ExperimentRun/containers/{runId}`

### <a name="export-notifications"></a>导出通知

使用以下调用来获取存储任务的列表：

`https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/tasks`

可以使用以下调用获取单一任务的通知：

`https://{location}.experiments.azureml.net/notification/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}tasks/{taskId}`

### <a name="export-data-stores"></a>导出数据存储

使用以下调用来获取数据存储的列表：

`https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores`

可以使用以下调用获取各个数据存储：

`https://{location}.experiments.azureml.net/datastore/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/datastores/{name}`

### <a name="export-models"></a>导出模型

使用以下调用来获取模型及其 ID 的列表：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models?api-version=2019-11-01`

可以使用以下调用获取各个模型：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/models/{id}?api-version=2019-11-01`

### <a name="export-assets"></a>导出资产

使用以下调用来获取资产及其 ID 的列表：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets?api-version=2019-11-01`

可以使用以下调用获取各个资产：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/assets/{id}?api-version=2019-11-01`

### <a name="export-images"></a>导出映像

使用以下调用来获取映像及其 ID 的列表：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images?api-version=2019-11-01`

可以使用以下调用获取各个映像：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/images/{id}?api-version=2019-11-01`

### <a name="export-services"></a>导出服务

使用以下调用来获取服务及其 ID 的列表：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services?api-version=2019-11-01`

可以使用以下调用获取各个服务：

`https://{location}.modelmanagement.azureml.net/api/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspace}/services/{id}?api-version=2019-11-01`

### <a name="export-pipeline-experiments"></a>导出管道试验

可以使用以下调用获取各个试验：

`https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Experiments/{experimentId}`

### <a name="export-pipeline-graphs"></a>导出管道图

可以使用以下调用获取各个图：

`https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Graphs/{graphId}`

### <a name="export-pipeline-modules"></a>导出管道模块

可以使用以下调用获取模块：

`https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Modules/{id}`

### <a name="export-pipeline-templates"></a>导出管道模板

可以使用以下调用获取模板：

`https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/Templates/{templateId}`

### <a name="export-pipeline-data-sources"></a>导出管道数据源

可以使用以下调用获取数据源：

`https://{location}.aether.ms/api/v1.0/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.MachineLearningServices/workspaces/{workspaceName}/DataSources/{id}`

## <a name="delete-assets-in-the-designer"></a>在设计器中删除资产

在创建了试验的设计器中删除各个资产：

1. 转到设计器

    ![删除资产](./media/how-to-export-delete-data/delete-experiment.png)

1. 在列表中，选择要删除的单个管道草案。

1. 选择“删除” 。

### <a name="delete-datasets-in-the-designer"></a>在设计器中删除数据集

若要在设计器中删除数据集，请使用 Azure 门户或存储资源管理器导航到已连接的存储帐户，删除其中的数据集。 在设计器中注销数据集只会删除存储中的引用点。

