---
title: 使用 Azure 流分析错误代码进行故障排除
description: 通过内部错误代码排查 Azure 流分析问题。
ms.author: mamccrea
author: mamccrea
ms.topic: troubleshooting
ms.date: 05/07/2020
ms.service: stream-analytics
ms.openlocfilehash: 9dc3d873ddfef9a729f583cd914ca4700d562ff3
ms.sourcegitcommit: e132633b9c3a53b3ead101ea2711570e60d67b83
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2020
ms.locfileid: "86045223"
---
# <a name="azure-stream-analytics-internal-error-codes"></a>Azure 流分析内部错误代码

可以使用活动日志和资源日志来帮助调试 Azure 流分析作业中的意外行为。 本文列出了每个内部错误代码的说明。 内部错误是指当流分析无法区分错误是内部可用性错误还是系统中的 bug 时，流分析平台内引发的一般性错误。

## <a name="cosmosdboutputbatchsizetoolarge"></a>CosmosDBOutputBatchSizeTooLarge

* **原因**：用于写入 Cosmos DB 的批大小太大。 
* **建议**：使用较小的批大小重试。

## <a name="next-steps"></a>后续步骤

* [对输入连接进行故障排除](stream-analytics-troubleshoot-input.md)
* [对 Azure 流分析输出进行故障排除](stream-analytics-troubleshoot-output.md)
* [对 Azure 流分析查询进行故障排除](stream-analytics-troubleshoot-query.md)
* [使用资源日志对 Azure 流分析进行故障排除](stream-analytics-job-diagnostic-logs.md)
* [Azure 流分析数据错误](data-errors.md)