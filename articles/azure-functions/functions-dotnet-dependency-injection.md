---
title: 在 .NET Azure Functions 中使用依赖项注入
description: 了解如何在 .NET 函数中使用依赖项注入来注册和使用服务
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: cshoe
ms.reviewer: jehollan
ms.openlocfilehash: 02cb862c5ec6f75d546aabcd6e8ac97a4de961a4
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87082947"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>在 .NET Azure Functions 中使用依赖项注入

Azure Functions 支持依赖项注入 (DI) 软件设计模式，这是一种在类与其依赖项之间实现[控制反转 (IoC)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) 的方法。

- Azure Functions 中的依赖项注入基于 .NET Core 依赖项注入功能构建。 建议熟悉 [.NET Core 依赖项注入](/aspnet/core/fundamentals/dependency-injection)。 两者在如何替代依赖项以及如何使用 Azure Functions 对消耗计划读取配置值方面存在差异。

- 从 Azure Functions 2.x 支持依赖项注入。

## <a name="prerequisites"></a>先决条件

必须先安装以下 NuGet 包，然后才能使用依赖项注入：

- [Microsoft.Azure.Functions.Extensions](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Microsoft.NET.Sdk.Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) 包版本 1.0.28 或更高版本

## <a name="register-services"></a>注册服务

若要注册服务，请创建一个方法来配置组件并将组件添加到 `IFunctionsHostBuilder` 实例。  Azure Functions 主机会创建 `IFunctionsHostBuilder` 的实例，并将其直接传递到方法中。

若要注册方法，请添加指定在启动过程中使用的类型名称的 `FunctionsStartup` 程序集特性。

```csharp
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Extensions.DependencyInjection;

[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();

            builder.Services.AddSingleton<IMyService>((s) => {
                return new MyService();
            });

            builder.Services.AddSingleton<ILoggerProvider, MyLoggerProvider>();
        }
    }
}
```

此示例使用在启动时注册 `HttpClient` 所需的 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) 包。

### <a name="caveats"></a>注意事项

在运行时处理启动类之前和之后会运行一系列注册步骤。 因此，请注意以下事项：

- 启动类仅用于设置和注册。 避免使用在启动时注册的服务。 例如，请勿尝试在记录器中记录在启动过程中注册的消息。 此注册过程点对你的服务来说太早，因此无法使用。 在运行 `Configure` 方法以后，Functions 运行时会继续注册其他依赖项，这可能影响服务的运行。

- 依赖项注入容器仅存储显式注册的类型。 能够用作可注入类型的服务仅限在 `Configure` 方法中设置的服务。 因此，特定于 Functions 的类型（例如 `BindingContext` 和 `ExecutionContext`）在设置中不可用，也不能用作可注入类型。

## <a name="use-injected-dependencies"></a>使用注入的依赖项

使用构造函数注入以后，依赖项即可在函数中使用。 使用构造函数注入要求不要将静态类用于注入的服务或用于函数类。

下面的示例演示如何将 `IMyService` 和 `HttpClient` 依赖项注入到 HTTP 触发的函数中。

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.Extensions.Logging;
using System.Net.Http;
using System.Threading.Tasks;

namespace MyNamespace
{
    public class MyHttpTrigger
    {
        private readonly HttpClient _client;
        private readonly IMyService _service;

        public MyHttpTrigger(HttpClient httpClient, MyService service)
        {
            this._client = httpClient;
            this._service = service;
        }

        [FunctionName("MyHttpTrigger")]
        public async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req,
            ILogger log)
        {
            var response = await _client.GetAsync("https://microsoft.com");
            var message = _service.GetMessage();

            return new OkObjectResult("Response from function with injected dependencies.");
        }
    }
}
```

此示例使用在启动时注册 `HttpClient` 所需的 [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) 包。

## <a name="service-lifetimes"></a>服务生存期

Azure Functions 应用提供与 [ASP.NET 依赖项注入](/aspnet/core/fundamentals/dependency-injection#service-lifetimes)相同的服务生存期。 就 Functions 应用来说，不同的服务生存期表现如下：

- **暂时性**：每次请求此服务时，都会创建暂时性服务。
- **限定范围**：限定范围的服务的生存期与函数执行生存期相匹配。 作用域服务在每次执行时创建一次。 在执行期间对该服务的后续请求会重复使用现有服务实例。
- **单一实例**：单一实例服务生存期与主机生存期相匹配，并且在该实例上的各个函数执行之间重用。 对于连接和客户端（例如 `DocumentClient` 或 `HttpClient` 实例），建议使用单一实例生存期服务。

可在 GitHub 上查看或下载[不同服务生存期的示例](https://aka.ms/functions/di-sample)。

## <a name="logging-services"></a>日志记录服务

如果需要自己的日志记录提供程序，请将自定义类型注册为的实例 [`ILoggerProvider`](/dotnet/api/microsoft.extensions.logging.iloggerfactory) ，该实例可[Microsoft.Extensions.Logging.Abstractions](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Abstractions/)通过使用

Azure Functions 会自动添加 Application Insights。

> [!WARNING]
> - 请勿将 `AddApplicationInsightsTelemetry()` 添加到服务集合，因为它注册的服务与环境提供的服务发生冲突。
> - 如果使用内置 Application Insights 功能，请勿注册自己的 `TelemetryConfiguration` 或 `TelemetryClient`。 如果需要配置自己的 `TelemetryClient` 实例，请通过插入的 `TelemetryConfiguration` 创建一个实例，如[监视 Azure Functions](./functions-monitoring.md#version-2x-and-later-2) 中所示。

### <a name="iloggert-and-iloggerfactory"></a>ILogger<T> 和 ILoggerFactory

主机会将 `ILogger<T>` 和 `ILoggerFactory` 服务注入构造函数中。  但是，默认情况下，将筛选出函数日志中的新的日志记录筛选器。  你需要修改该 `host.json` 文件以选择加入其他筛选器和类别。

下面的示例演示如何 `ILogger<HttpTrigger>` 使用向主机公开的日志添加。

```csharp
namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly ILogger<HttpTrigger> _log;

        public HttpTrigger(ILogger<HttpTrigger> log)
        {
            _log = log;
        }

        [FunctionName("HttpTrigger")]
        public async Task<IActionResult> Run(
            [HttpTrigger(AuthorizationLevel.Anonymous, "get", "post", Route = null)] HttpRequest req)
        {
            _log.LogInformation("C# HTTP trigger function processed a request.");

            // ...
    }
}
```

下面的示例 `host.json` 文件添加了日志筛选器。

```json
{
    "version": "2.0",
    "logging": {
        "applicationInsights": {
            "samplingExcludedTypes": "Request",
            "samplingSettings": {
                "isEnabled": true
            }
        },
        "logLevel": {
            "MyNamespace.HttpTrigger": "Information"
        }
    }
}
```

## <a name="function-app-provided-services"></a>函数应用提供的服务

函数主机注册许多服务。 以下服务可以安全地用作应用程序中的依赖项：

|服务类型|生存期|说明|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|单一实例|运行时配置|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|单一实例|负责提供主机实例的 ID|

如果存在你要依赖的其他服务，请[在 GitHub 上创建一个问题并提出它们](https://github.com/azure/azure-functions-host)。

### <a name="overriding-host-services"></a>替代主机服务

目前不支持替代由主机提供的服务。  如果有要替代的服务，[请在 GitHub上创建并提出问题](https://github.com/azure/azure-functions-host)。

## <a name="working-with-options-and-settings"></a>使用选项和设置

在[应用设置](./functions-how-to-use-azure-function-app-settings.md#settings)中定义的值可以在 `IConfiguration` 实例中使用，这使得你可以读取启动类中的应用设置值。

可以将 `IConfiguration` 实例中的值提取为自定义类型。 如果将应用设置值复制到自定义类型，则通过使这些值可注入，可以轻松地测试服务。 读入到配置实例中的设置必须是简单的键/值对。

请考虑以下类，其中包含一个命名与应用设置一致的属性：

```csharp
public class MyOptions
{
    public string MyCustomSetting { get; set; }
}
```

还有一个 `local.settings.json` 文件，该文件可能按如下所示组织自定义设置：
```json
{
  "IsEncrypted": false,
  "Values": {
    "MyOptions:MyCustomSetting": "Foobar"
  }
}
```

在 `Startup.Configure` 方法内，可以使用以下代码从 `IConfiguration` 实例将值提取到自定义类型中：

```csharp
builder.Services.AddOptions<MyOptions>()
                .Configure<IConfiguration>((settings, configuration) =>
                                           {
                                                configuration.GetSection("MyOptions").Bind(settings);
                                           });
```

调用 `Bind` 可以将那些与属性名匹配的值从配置复制到自定义实例中。 IoC 容器中现在提供可以注入到函数中的选项实例。

选项对象作为泛型 `IOptions` 接口的实例注入函数中。 使用 `Value` 属性访问在配置中发现的值。

```csharp
using System;
using Microsoft.Extensions.Options;

public class HttpTrigger
{
    private readonly MyOptions _settings;

    public HttpTrigger(IOptions<MyOptions> options)
    {
        _settings = options.Value;
    }
}
```

有关使用选项的更多详细信息，请参阅 [ASP.NET Core 中的选项模式](/aspnet/core/fundamentals/configuration/options)。

> [!WARNING]
> 请避免尝试从有关消耗计划的 local.settings.json 或 appsettings.{environment}.json 等文件中读取值。 从与触发器连接相关的这些文件中读取的值在应用缩放时不可用，因为规模控制器创建应用的新实例时托管基础结构无法访问配置信息。

## <a name="next-steps"></a>后续步骤

有关详细信息，请参阅以下资源：

- [如何监视函数应用](functions-monitoring.md)
- [函数的最佳做法](functions-best-practices.md)
