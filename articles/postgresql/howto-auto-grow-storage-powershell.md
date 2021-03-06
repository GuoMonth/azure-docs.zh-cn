---
title: 自动增长存储-Azure PowerShell-Azure Database for PostgreSQL
description: 本文介绍如何在 Azure Database for PostgreSQL 中使用 PowerShell 启用自动增长存储。
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: how-to
ms.date: 06/08/2020
ms.openlocfilehash: d8ea8a03a0ce6f5819bb603c9d0ad7b4d65d5c32
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2020
ms.locfileid: "86118114"
---
# <a name="auto-grow-storage-in-azure-database-for-postgresql-server-using-powershell"></a>使用 PowerShell 自动增长 Azure Database for PostgreSQL server 中的存储

本文介绍如何将 Azure Database for PostgreSQL 服务器存储配置为在不影响工作负荷的情况下增长。

存储自动增长可防止服务器[达到存储限制](/azure/postgresql/concepts-pricing-tiers#reaching-the-storage-limit)并变为只读。 对于预配存储为 100 GB 或更低的服务器，当可用空间低于 10% 时，大小将增加 5 GB。 对于预配存储超过 100 GB 的服务器，当可用空间低于 10 GB 时，大小将增加 5%。 最大存储限制适用于 " [Azure Database for PostgreSQL 定价层](/azure/postgresql/concepts-pricing-tiers#storage)的" 存储 "部分中指定的限制。

> [!IMPORTANT]
> 请记住，存储只能增加，不能减少。

## <a name="prerequisites"></a>先决条件

若要完成本操作指南，需要：

- 在本地安装[Az PowerShell 模块](https://docs.microsoft.com/powershell/azure/install-az-ps)或在浏览器中[Azure Cloud Shell](https://shell.azure.com/)
- [Azure Database for PostgreSQL 服务器](quickstart-create-postgresql-server-database-using-azure-powershell.md)

> [!IMPORTANT]
> 尽管 Az.PostgreSql PowerShell 模块为预览版，但必须使用以下命令从 Az PowerShell 模块单独安装它：`Install-Module -Name Az.PostgreSql -AllowPrerelease`。
> Az.PostgreSql PowerShell 模块正式版推出后，它会包含在将来的 Az PowerShell 模块发行版中，并在 Azure Cloud Shell 中原生提供。

如果选择在本地使用 PowerShell，请使用[AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) cmdlet 连接到 Azure 帐户。

## <a name="enable-postgresql-server-storage-auto-grow"></a>启用 PostgreSQL 服务器存储自动增长

使用以下命令在现有服务器上启用服务器自动增长存储：

```azurepowershell-interactive
Update-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup -StorageAutogrow Enabled
```

使用以下命令创建新服务器时启用服务器自动增长存储：

```azurepowershell-interactive
$Password = Read-Host -Prompt 'Please enter your password' -AsSecureString
New-AzPostgreSqlServer -Name mydemoserver -ResourceGroupName myresourcegroup -Sku GP_Gen5_2 -StorageAutogrow Enabled -Location westus -AdministratorUsername myadmin -AdministratorLoginPassword $Password
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [如何使用 PowerShell 创建和管理 Azure Database for PostgreSQL 中的读取副本](howto-read-replicas-powershell.md)。
