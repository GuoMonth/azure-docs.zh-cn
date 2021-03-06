---
title: 排查服务器的 Azure Arc 代理连接问题
description: 本文介绍如何通过 Azure Arc for 服务器（预览版）在尝试连接到服务时对连接的计算机代理进行故障排除和解决问题。
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 07/20/2020
ms.topic: conceptual
ms.openlocfilehash: 46096e1f3f4266e9c070bd1d67f328241163126b
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/23/2020
ms.locfileid: "87004539"
---
# <a name="troubleshoot-the-connected-machine-agent-connection-issues"></a>排查连接的计算机代理连接问题

本文介绍了在尝试为 Windows 或 Linux 配置适用于服务器（预览版）的 Azure Arc 时可能出现的故障排除和解决问题的相关信息。 包括配置与服务的连接时的交互式和大规模安装方法。 有关一般信息，请参阅[服务器的 Arc 概述](./overview.md)。

## <a name="agent-verbose-log"></a>代理详细日志

在遵循本文后面所述的故障排除步骤之前，你需要的最少信息为详细日志。 当使用 verbose （-v）参数时，它包含**azcmagent**工具命令的输出。 日志文件将写入 `%ProgramData%\AzureConnectedMachineAgent\Log\azcmagent.log` Windows 和 Linux `/var/opt/azcmagent/log/azcmagent.log` 。

### <a name="windows"></a>Windows

下面是在执行交互式安装时，使用已连接计算机代理为 Windows 启用详细日志记录的命令示例。

```
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```

下面是在使用服务主体执行大规模安装时，使用已连接计算机代理为 Windows 启用详细日志记录的命令示例。

```
& "$env:ProgramFiles\AzureConnectedMachineAgent\azcmagent.exe" connect `
  --service-principal-id "{serviceprincipalAppID}" `
  --service-principal-secret "{serviceprincipalPassword}" `
  --resource-group "{ResourceGroupName}" `
  --tenant-id "{tenantID}" `
  --location "{resourceLocation}" `
  --subscription-id "{subscriptionID}"
  --verbose
```

### <a name="linux"></a>Linux

下面是一个命令示例，用于在执行交互式安装时使用连接的计算机代理为 Linux 启用详细日志记录。

>[!NOTE]
>若要运行**azcmagent**，必须具有 Linux 计算机上的*根*访问权限。

```
azcmagent connect --resource-group "resourceGroupName" --tenant-id "tenantID" --location "regionName" --subscription-id "subscriptionID" --verbose
```

下面是在使用服务主体执行大规模安装时，使用已连接计算机代理为 Linux 启用详细日志记录的命令示例。

```
azcmagent connect \
  --service-principal-id "{serviceprincipalAppID}" \
  --service-principal-secret "{serviceprincipalPassword}" \
  --resource-group "{ResourceGroupName}" \
  --tenant-id "{tenantID}" \
  --location "{resourceLocation}" \
  --subscription-id "{subscriptionID}"
  --verbose
```

## <a name="agent-connection-issues-to-service"></a>服务的代理连接问题

下表列出了一些已知错误以及有关如何排查和解决问题的建议。

|Message |错误 |可能的原因 |解决方案 |
|--------|------|---------------|---------|
|未能获取授权令牌设备流 |`Error occurred while sending request for Device Authorization Code: Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/devicecode?api-version=1.0:  dial tcp 40.126.9.7:443: connect: network is unreachable.` |无法访问 `login.windows.net` 终结点 | 验证与终结点的连接。 |
|未能获取授权令牌设备流 |`Error occurred while sending request for Device Authorization Code: Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/devicecode?api-version=1.0:  dial tcp 40.126.9.7:443: connect: network is Forbidden`. |代理或防火墙正在阻止对 `login.windows.net` 终结点的访问。 | 验证与终结点的连接，并且它未被防火墙或代理服务器阻止。 |
|未能获取授权令牌设备流  |`Error occurred while sending request for Device Authorization Code: Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/devicecode?api-version=1.0:  dial tcp lookup login.windows.net: no such host`. | 启用 "组策略对象*计算机配置 \ 管理模板 \ 系统 \ 用户配置文件 \ 删除系统重启上的指定天数之前的用户配置文件"* 。 | 验证 GPO 是否已启用并以受影响的计算机为目标。 有关更多详细信息，请参阅脚注<sup>[1](#footnote1)</sup> 。 |
|未能从 SPN 获取授权令牌 |`Failed to execute the refresh request. Error = 'Post https://login.windows.net/fb84ce97-b875-4d12-b031-ef5e7edf9c8e/oauth2/token?api-version=1.0: Forbidden'` |代理或防火墙正在阻止对 `login.windows.net` 终结点的访问。 |验证与终结点的连接，并且它未被防火墙或代理服务器阻止。 |
|未能从 SPN 获取授权令牌 |`Invalid client secret is provided` |服务主体机密错误或无效。 |验证服务主体密码。 |
| 未能从 SPN 获取授权令牌 |`Application with identifier 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' was not found in the directory 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'. This can happen if the application has not been installed by the administrator of the tenant or consented to by any user in the tenant` |不正确的服务主体和/或租户 ID。 |验证服务主体和/或租户 ID。|
|获取 ARM 资源响应 |`The client 'username@domain.com' with object id 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx' does not have authorization to perform action 'Microsoft.HybridCompute/machines/read' over scope '/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.HybridCompute/machines/MSJC01' or the scope is invalid. If access was recently granted, please refresh your credentials."}}" Status Code=403` |凭据和/或权限错误 |验证你或服务主体是**Azure 连接的计算机加入**角色的成员。 |
|未能 AzcmagentConnect ARM 资源 |`The subscription is not registered to use namespace 'Microsoft.HybridCompute'` |Azure 资源提供程序未注册。 |注册[资源提供程序](./agent-overview.md#register-azure-resource-providers)。 |
|未能 AzcmagentConnect ARM 资源 |`Get https://management.azure.com/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.HybridCompute/machines/MSJC01?api-version=2019-03-18-preview:  Forbidden` |代理服务器或防火墙正在阻止对 `management.azure.com` 终结点的访问。 |验证与终结点的连接，并且它未被防火墙或代理服务器阻止。 |

<a name="footnote1"></a><sup>1</sup>如果此 GPO 已启用并且应用于具有连接计算机代理的计算机，则会删除与为*himds*服务指定的内置帐户关联的用户配置文件。 因此，它还会删除用于与缓存在本地证书存储中的服务进行通信的身份验证证书，30天。 在30天的限制之前，会尝试续订该证书。 若要解决此问题，请执行[注销计算机](manage-agent.md#unregister-machine)的步骤，然后将其重新注册到正在运行的服务 `azcmagent connect` 。

## <a name="next-steps"></a>后续步骤

如果未在此处看到你遇到的问题，或者无法解决你遇到的问题，请尝试以下途径之一以获取其他支持：

* 通过[Microsoft Q&](https://docs.microsoft.com/answers/topics/azure-arc.html)获取来自 Azure 专家的解答。

* 连接到 [@AzureSupport](https://twitter.com/azuresupport)，这是用于改进客户体验的官方 Microsoft Azure 帐户。 Azure 支持人员会将你连接到 Azure 社区，从中可以获得解答、支持和专家建议。

* 提出 Azure 支持事件。 请转到 [Azure 支持站点](https://azure.microsoft.com/support/options/)并选择“获取支持”。