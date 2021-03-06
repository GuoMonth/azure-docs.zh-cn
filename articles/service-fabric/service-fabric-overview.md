---
title: Azure 上的 Service Fabric 概述
description: 概述了 Service Fabric，其中应用程序由许多微服务组成，以提供扩展性和恢复能力。 Service Fabric 是一个分布式系统平台，可用于构建面向云的可扩展、可靠且易管理的应用程序。
ms.topic: overview
ms.date: 01/07/2020
ms.custom: sfrev
ms.openlocfilehash: 3c8eb7ead7851c311c79c2f9e9bdc7e703c3af71
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "75747502"
---
# <a name="overview-of-azure-service-fabric"></a>Azure Service Fabric 概述

Azure Service Fabric 是一款分布式系统平台，可方便用户轻松打包、部署和管理可缩放的可靠微服务和容器。 Service Fabric 还解决了开发和管理云原生应用程序面临的重大难题。 开发人员和管理员不需解决复杂的基础结构问题，只需专注于实现具有严苛要求的任务关键型工作负荷，即那些可缩放、可靠且易于管理的工作负荷。 Service Fabric 代表了下一代平台，用于生成和管理在容器中运行的企业级单层云规模应用程序。

以下短视频介绍了 Service Fabric 和微服务：
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-Service-Fabric/player]

## <a name="compliance"></a>合规性

Azure Service Fabric 资源提供程序在所有 Azure 区域中都可用，并符合 Azure 所具有的全部合规性证书；这包括以下内容：SOC、ISO、PCI DSS、HIPAA 和 GDPR。 要获取合规性证书的完整列表，请查看：[合规性方案](https://www.microsoft.com/trustcenter/compliance/complianceofferings)

## <a name="applications-composed-of-microservices"></a>由微服务组成的应用程序

借助 Service Fabric，可以生成和管理由微服务构成的可缩放且可靠的应用程序。 这些分布式微服务在计算机的共享池上以高密度运行，它们被称为群集。 Service Fabric 提供了一种复杂的轻型运行时，可支持无状态和有状态微服务。 它还提供了全面的应用程序管理功能，用于预配、部署、监视、升级/修补和删除所部署的应用程序。

Service Fabric 专为创建云原生服务而设计，这些服务可能刚开始规模很小（根据需要），随后成长为包含成百上千台计算机的大规模服务。 当今的 Internet 规模的服务是使用微服务构建而成的。 微服务的例子包括协议网关、用户配置文件、购物车、清单处理、队列和缓存等。

Service Fabric 为当今很多 Microsoft 服务（包括 Azure SQL 数据库、Azure Cosmos DB、Cortana、Microsoft Power BI、Microsoft Intune、Azure 事件中心、Azure IoT 中心、Dynamics 365、Skype for Business 以及其他许多核心 Azure 服务）提供技术支持。

Service Fabric 将微服务托管到跨 Service Fabric 群集部署和激活的容器内部。 从虚拟机移动到容器可能使密度出现数量级增长。 同样，如果从容器移动到这些容器中的微服务，也可能会出现另一个密度数量级。 例如，单个 Azure SQL 数据库群集包含数百台计算机，这些计算机运行数以万计的容器，这些容器总共托管数十万个数据库。 每个数据库都是一个 Service Fabric 有状态微服务。

有关微服务方法的详细信息，请阅读[为什么使用微服务方法生成应用程序？](service-fabric-overview-microservices.md)

## <a name="container-deployment-and-orchestration"></a>容器部署和业务流程

Service Fabric 是跨计算机群集部署微服务的 Microsoft [容器业务流程协调程序](service-fabric-cluster-resource-manager-introduction.md)。 微服务的开发方法有多种，从使用 [Service Fabric 编程模型](service-fabric-choose-framework.md)、[ASP.NET Core](service-fabric-reliable-services-communication-aspnetcore.md) 到部署[任意选定代码](service-fabric-guest-executables-introduction.md)。 重要的是，可以在同一个应用程序中将进程中的服务和容器中的服务混用。 如果只需要[部署和管理容器](service-fabric-containers-overview.md)，那么 Service Fabric 是容器业务流程协调程序的理想选择。

## <a name="any-os-any-cloud"></a>不限 OS 和云

Service Fabric 可以在所有环境中运行。 可以在许多环境中（包括：在 Azure 中或本地、在 Windows Server 中或在 Linux 中）创建 Service Fabric 群集。 甚至可以在其他公有云上创建群集。 此外，SDK 中的开发环境与生产环境**完全相同**，都不涉及模拟器。 也就是说，在本地开发群集上运行的内容会部署到其他环境中的群集。

![Service Fabric 平台][Image1]

为了便于 Windows 开发，Service Fabric .NET SDK 已与 Visual Studio 和 Powershell 集成。 请参阅[在 Windows 上准备开发环境](service-fabric-get-started.md)。 为了便于 Linux 开发，Service Fabric Java SDK 已与 Eclipse 集成，并且 Yeoman 被用来为 Java、.NET Core 和容器应用程序生成模板。 请参阅[在 Linux 上准备开发环境](service-fabric-get-started-linux.md)。

有关创建群集的详细信息，请阅读[在 Windows Server 或 Linux 上创建群集](service-fabric-deploy-anywhere.md)；有关在 Azure 中创建群集的详细信息，请阅读[通过 Azure 门户创建群集](service-fabric-cluster-creation-via-portal.md)。

## <a name="stateless-and-stateful-microservices-for-service-fabric"></a>无状态和有状态 Service Fabric 微服务

使用 Service Fabric，可以生成包含微服务或容器的应用程序。 无状态微服务（例如协议网关和 Web 代理）不在请求以及服务对请求的响应之外维持可变状态。 Azure 云服务辅助角色是无状态服务的一个示例。 有状态微服务（例如，用户帐户、数据库、设备、购物车、队列）在请求及其响应之外维持可变的授权状态。 当今的 Internet 规模的应用程序包含无状态和有状态微服务的组合。 

Service Fabric 的关键区别在于非常注重使用[内置编程模型](service-fabric-choose-framework.md)或容器化有状态服务生成有状态服务。 [应用程序方案](service-fabric-application-scenarios.md)介绍了可使用有状态服务的方案。

## <a name="application-lifecycle-management"></a>应用程序生命周期管理

Service Fabric 为包含容器的云应用程序的完整生命周期和 CI/CD 提供支持。 生命周期包括从开发到部署、到日常管理和维护，再到最终解除授权。

利用 Service Fabric 应用程序生命周期管理功能，应用程序管理员和 IT 操作人员能够使用简单的低接触式工作流来预配、部署、修补和监视应用程序。 这些内置的工作流极大地减少了 IT 操作人员在保持应用程序持续可用这一方面的负担。

大多数应用程序都由无状态微服务、有状态微服务、容器以及与它们一起部署的其他可执行文件组合而成。 Service Fabric 在应用程序上采用强类型，因此可用于部署多个应用程序实例。 每个实例将单独进行管理和升级。 重点是，Service Fabric 能够部署容器或任何可执行文件，并确保它们的可靠性。 例如，Service Fabric 可部署 .NET、ASP.NET Core、Python、Node.js、Windows 容器、Linux 容器、Java 虚拟机、脚本、Angular 或应用程序的其他任何组成部分。

Service Fabric 与 [Azure Pipelines](https://www.visualstudio.com/team-services/)、[Jenkins](https://jenkins.io/index.html) 和 [Octopus Deploy](https://octopus.com/) 等 CI/CD 工具集成，并可与其他任何常用 CI/CD 工具配合使用。

有关应用程序生命周期管理的详细信息，请参阅[应用程序生命周期](service-fabric-application-lifecycle.md)。 若要详细了解如何部署任意代码，请参阅[部署来宾可执行文件](service-fabric-deploy-existing-app.md)。

## <a name="key-capabilities"></a>关键功能

通过使用 Service Fabric，可以：

* 部署到 Azure 或部署到运行 Windows 或 Linux 的本地数据中心，而无需改变任何代码。 只需编写一次，即可部署到任意位置的任意 Service Fabric 群集。
* 使用 Service Fabric 编程模型、容器或任意代码，开发由微服务组成的可缩放应用程序。
* 开发高度可靠的无状态和有状态微服务。 使用有状态微服务，简化应用程序设计。 
* 使用新的 Reliable Actors 编程模型创建具有独立式代码和状态的云对象。
* 部署并协调容器，包括 Windows 容器和 Linux 容器。 Service Fabric 是可感知数据的有状态容器业务流程协调程序。
* 几秒内就可以高密度部署应用程序，即每台计算机部署成百上千个应用程序或容器。
* 以并行方式部署同一应用程序的不同版本，且可以单独升级每个应用程序。
* 无需停机，即可管理应用程序生命周期，包括中断升级和非中断升级。
* 横向扩展或横向缩减群集中的节点数。 缩放节点数的同时，应用程序也会随之自动缩放。
* 监视并诊断应用程序的运行状况，并设置用来执行自动修复的策略。
* 观察资源均衡器如何安排应用程序在群集中的重新分布。 Service Fabric 可从故障中恢复，并基于可用资源优化负载分布。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>后续步骤

* 更多相关信息：
  * [为什么使用微服务方法生成应用程序？](service-fabric-overview-microservices.md)
  * [术语概述](service-fabric-technical-overview.md)
* 设置 [Windows 开发环境](service-fabric-get-started.md)  
* 设置 [Linux 开发环境](service-fabric-get-started-linux.md)
* 了解 [Service Fabric 支持选项](service-fabric-support.md)

[Image1]: media/service-fabric-overview/Service-Fabric-Overview.png
