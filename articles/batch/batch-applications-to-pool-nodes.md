---
title: 将应用程序和数据复制到池节点
description: 了解如何将应用程序和数据复制到池节点。
ms.topic: how-to
ms.date: 02/17/2020
ms.openlocfilehash: e21b8551fb62c4335910fd05bb9590eaf6f7e35a
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/05/2020
ms.locfileid: "85954887"
---
# <a name="copy-applications-and-data-to-pool-nodes"></a>将应用程序和数据复制到池节点

Azure Batch 支持用多种方式来将数据和应用程序提取到计算节点中，使这些数据和应用程序可供任务使用。 运行整个作业可能需要数据和应用程序，因此需要在每个节点上安装它们。 一些可能只是特定任务需要，或者需要针对作业进行安装，而不需要在每个节点上安装。 Batch 为上述每种场景都提供了相关工具。

- **池启动任务资源文件**：针对需要在池中每个节点上安装的应用程序或数据。 将此方法与应用程序包或启动任务的资源文件集合一起使用来执行安装命令。  

示例： 
- 使用启动任务命令行来移动或安装应用程序

- 在 Azure 存储帐户中指定特定文件或容器的列表。 有关详细信息，请参阅 [REST 文档中的 add#resourcefile](/rest/api/batchservice/pool/add#resourcefile)

- 池中运行的每个作业都会运行 MyApplication.exe，后者必须先使用 MyApplication.msi 进行安装。 如果使用此机制，需要将启动任务的“等待成功”属性设置为 true 。 有关详细信息，请参阅 [REST 文档中的 add#starttask](/rest/api/batchservice/pool/add#starttask)。

- **池中的应用程序包引用**：针对需要在池中每个节点上安装的应用程序或数据。 没有与应用程序包关联的安装命令，但你可使用启动任务来运行任何安装命令。 如果应用程序无需安装或者包含大量文件，则可使用此方法。 应用程序包非常适合大量文件，这是因为它们会将大量文件引用组合到一个小的有效负载中。 如果尝试将 100 个以上单独的资源文件包含到一个任务中，Batch 服务可能会遇到单任务内部系统限制。 此外，如果你有严格的版本控制需求，你可能有同一应用程序的多个不同版本，且需要在这些版本之间进行选择，那么请使用应用程序包。 有关详细信息，请参阅[使用 Batch 应用程序包将应用程序部署到计算节点](./batch-application-packages.md)。

- **作业准备任务资源文件**：针对为使作业运行而必须安装，但无需在整个池中安装的应用程序或数据。 例如，如果你的池有多个不同类型的作业，但只有一个作业类型需要 MyApplication.msi 才能运行，则在作业准备任务中进行安装很合理。 有关作业准备任务的详细信息，请参阅[在 Batch 计算节点上运行作业准备和作业发布任务](./batch-job-prep-release.md)。

- **任务资源文件**：适合应用程序或数据仅与单个任务相关的情况。 例如：你有 5 项任务，每一项处理一个不同的文件，然后将输出写入 Blob 存储。  在这种情况下，应在任务资源文件上指定输入文件，因为每项任务都有自己的输入文件。

## <a name="determine-the-scope-required-of-a-file"></a>确定文件所需的范围

需要确定文件的范围，即需要文件的是池、作业还是任务。 范围设为池的文件应使用池应用程序包或启动任务。 范围设为作业的文件应使用作业准备任务。 范围设在池或作业级别的文件的一个很好的例子就是应用程序。 范围设为任务的文件应使用任务资源文件。

### <a name="other-ways-to-get-data-onto-batch-compute-nodes"></a>将数据提取到 Batch 计算节点的其他方式

还有其他方法可将数据提取到未正式集成到 Batch REST API 的 Batch 计算节点。 你可控制 Azure Batch 节点且可运行自定义可执行文件，因此你能够从任意数量的自定义源中拉取数据，前提是 Batch 节点与目标相连，并且你在 Azure Batch 节点上具有该源的凭据。 一些常见示例包括：

- 从 SQL 下载数据
- 从其他 Web 服务/自定义位置下载数据
- 映射网络共享

### <a name="azure-storage"></a>Azure 存储

Blob 存储具有下载可伸缩性目标。 对单个 Blob 来说，Azure 存储文件共享可伸缩性目标是相同的。 大小将影响你所需的节点数和池数。

