---
title: '使用 c # 代码从 Gen1 环境查询数据-Azure 时序见解 Gen1 |Microsoft Docs'
description: '了解如何使用以 c # 编写的自定义应用从 Azure 时序见解 Gen1 环境查询数据。'
ms.service: time-series-insights
services: time-series-insights
author: deepakpalled
ms.author: dpalled
manager: diviso
ms.devlang: csharp
ms.workload: big-data
ms.topic: conceptual
ms.date: 07/07/2020
ms.custom: seodec18
ms.openlocfilehash: c794cfb5f0d652e25007131d9ca39fb73e2218e2
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87080669"
---
# <a name="query-data-from-the-azure-time-series-insights-gen1-environment-using-c"></a>使用 C 从 Azure 时序见解 Gen1 环境查询数据#

此 c # 示例演示如何使用[Gen1 查询 api](https://docs.microsoft.com/rest/api/time-series-insights/ga-query)从 Azure 时序见解 Gen1 环境中查询数据。

> [!TIP]
> 查看 Gen1 c # 代码示例 [https://github.com/Azure-Samples/Azure-Time-Series-Insights](https://github.com/Azure-Samples/Azure-Time-Series-Insights/tree/master/csharp-tsi-ga-sample) 。

## <a name="summary"></a>总结

下面的示例代码演示了以下功能：

* 如何使用 [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) 通过 Azure Active Directory 获取访问令牌。

* 如何在后续查询 API 请求的 `Authorization` 标头中传递该获得的访问令牌。 

* 该示例调用了每个 Gen1 查询 Api，其中演示了如何对发出 HTTP 请求：
    * [获取环境 API](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environments-api)，用于返回用户有权访问的环境
    * [获取环境可用性 API](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-availability-api)
    * [获取环境元数据 API](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-metadata-api)，用于检索环境元数据
    * [获取环境事件 API](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-events-api)
    * [获取环境聚合 API](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-aggregates-api)
    
* 如何使用 WSS 与 Gen1 查询 Api 进行交互以发送消息：

   * [获取流式处理的环境事件 API](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-events-streamed-api)
   * [获取流式处理的环境聚合 API](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api#get-environment-aggregates-streamed-api)

## <a name="prerequisites-and-setup"></a>先决条件和设置

在编译和运行示例代码之前，请完成以下步骤：

1. [预配 Gen1 Azure 时序见解](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-get-started)环境。
1. 为 Azure Active Directory 配置 Azure 时序见解环境，如[身份验证和授权](time-series-insights-authentication-and-authorization.md)中所述。 
1. 安装必需的项目依赖项。
1. 编辑下面的示例代码，将每个 **#DUMMY#** 替换为相应的环境标识符。
1. 在 Visual Studio 中执行代码。

## <a name="project-dependencies"></a>项目依赖项

建议使用最新版 Visual Studio：

* [Visual Studio 2019](https://visualstudio.microsoft.com/vs/) - 版本 16.4.2+

示例代码有两个必需的依赖项：

* [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) -  3.13.9 包。
* [Newtonsoft.Json](https://www.nuget.org/packages/Newtonsoft.Json) - 9.0.1 包。

在 Visual Studio 2019 中，通过选择“生成” > “生成解决方案”选项来下载程序包。

或者，使用 [NuGet 2.12+](https://www.nuget.org/) 添加这些包：

* `dotnet add package Newtonsoft.Json --version 9.0.1`
* `dotnet add package Microsoft.IdentityModel.Clients.ActiveDirectory --version 3.13.9`

## <a name="c-sample-code"></a>C# 示例代码

[!code-csharp[csharpquery-example](~/samples-tsi/csharp-tsi-ga-sample/Program.cs)]

## <a name="next-steps"></a>后续步骤

- 若要详细了解查询，请阅读[查询 API 参考](https://docs.microsoft.com/rest/api/time-series-insights/ga-query-api)。

- 阅读如何[使用客户端 SDK 将 JavaScript 应用连接到时序见解](https://github.com/microsoft/tsiclient)。
