---
title: 适用于 Linux 的 Azure 虚拟机上的 SQL Server 概述 |Microsoft Docs
description: 了解如何在适用于 Linux 的 Azure 虚拟机上运行完整 SQL Server 版本。 获取到所有 Linux SQL Server VM 映像和相关内容的直接链接。
services: virtual-machines-linux
documentationcenter: ''
author: MashaMSFT
tags: azure-service-management
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.workload: iaas-sql-server
ms.date: 04/10/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 721f30f5b17b078f3a3905204f6be56db25adead
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "84669464"
---
# <a name="overview-of-sql-server-on-azure-virtual-machines-linux"></a>Azure 虚拟机 (Linux) 上的 SQL Server 概述
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

> [!div class="op_single_selector"]
> * [Windows](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md)
> * [Linux](sql-server-on-linux-vm-what-is-iaas-overview.md)

使用 Azure 虚拟机上的 SQL Server，可以在云中使用完整版本的 SQL Server，而无需管理任何本地硬件。 使用即用即付时，SQL Server VM 还可以简化许可成本。

Azure 虚拟机在全球许多不同的[地理区域](https://azure.microsoft.com/regions/)运行， 并提供各种[虚拟机大小](../../../virtual-machines/windows/sizes.md)。 使用虚拟机映像库可以创建 SQL Server VM，而且版本和操作系统都很正确。 因此，虚拟机适用于许多不同的 SQL Server 工作负荷。 

## <a name="get-started-with-sql-server-vms"></a><a id="create"></a>SQL Server Vm 入门

若要开始，请选择一个 SQL Server 虚拟机映像，其中包含所需的版本和操作系统。 以下部分提供了有关 SQL Server 虚拟机库映像到 Azure 门户的直接链接。

> [!TIP]
> 有关如何了解 SQL Server 映像定价的详细信息，请参阅[运行 SQL Server 的 Linux vm 的定价页](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)。

| 版本 | 操作系统 | 版本 |
| --- | --- | --- |
| **SQL Server 2017** | Red Hat Enterprise Linux (RHEL) 7.4 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74)、[Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74)、[Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74)、[Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74)、[Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74) |
| **SQL Server 2017** | SUSE Linux Enterprise Server (SLES) v12 SP2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2)、[Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2)、[Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2)、[Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2)、[Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2) |
| **SQL Server 2017** | Ubuntu 16.04 LTS |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS)、[Standard](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)、[Web](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)、[Express](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS)、[Developer](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS) |

> [!NOTE]
> 若要查看 Windows 的可用 SQL Server 虚拟机映像，请参阅[Azure 虚拟机（Windows）上的 SQL Server 概述](../windows/sql-server-on-azure-vm-iaas-what-is-overview.md)。

## <a name="installed-packages"></a><a id="packages"></a> 已安装程序包

配置 Linux 上的 SQL Server 时，将根据你的要求安装数据库引擎包和多个可选包。 用于 SQL Server 的 Linux 虚拟机自动安装大多数包。 下表显示为每个分发安装了哪些包。

| 分发 | [数据库引擎](https://docs.microsoft.com/sql/linux/sql-server-linux-setup) | [工具](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-tools) | [SQL Server 代理](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-sql-agent) | [全文搜索](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-full-text-search) | [SSIS](https://docs.microsoft.com/sql/linux/sql-server-linux-setup-ssis) | [HA 外接程序](https://docs.microsoft.com/sql/linux/sql-server-linux-business-continuity-dr) |
|---|---|---|---|---|---|---|
| RHEL | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![否](./media/sql-server-on-linux-vm-what-is-iaas-overview/no.png) |
| SLES | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![否](./media/sql-server-on-linux-vm-what-is-iaas-overview/no.png) | ![否](./media/sql-server-on-linux-vm-what-is-iaas-overview/no.png) |
| Ubuntu | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) | ![是](./media/sql-server-on-linux-vm-what-is-iaas-overview/yes.png) |

## <a name="related-products-and-services"></a>相关产品和服务

### <a name="linux-virtual-machines"></a>Linux 虚拟机

* [Azure 虚拟机概述](../../../virtual-machines/linux/overview.md)

### <a name="storage"></a>存储

* [Microsoft Azure 存储简介](../../../storage/common/storage-introduction.md)

### <a name="networking"></a>网络

* [虚拟网络概述](../../../virtual-network/virtual-networks-overview.md)
* [Azure 中的 IP 地址](../../../virtual-network/public-ip-addresses.md)
* [在 Azure 门户中创建完全限定的域名](../../../virtual-machines/windows/portal-create-fqdn.md)

### <a name="sql"></a>SQL

* [“Linux 上的 SQL Server”文档](https://docs.microsoft.com/sql/linux)
* [Azure SQL 数据库比较](../../azure-sql-iaas-vs-paas-what-is-overview.md)

## <a name="next-steps"></a>后续步骤

开始 Linux 上的 SQL Server 虚拟机：

* [在 Azure 门户中创建 SQL Server VM](sql-vm-create-portal-quickstart.md)

获取有关 Linux 上的 SQL Server Vm 的常见问题的解答：

* [Azure 虚拟机中的 SQL Server 常见问题](frequently-asked-questions-faq.md)
