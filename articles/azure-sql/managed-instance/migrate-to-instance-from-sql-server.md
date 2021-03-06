---
title: 从 SQL Server 迁移到 Azure SQL 托管实例
description: 了解如何将数据库从 SQL Server 迁移到 Azure SQL 托管实例。
services: sql-database
ms.service: sql-managed-instance
ms.subservice: migration
ms.custom: seo-lt-2019, sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: douglas, carlrab
ms.date: 07/11/2019
ms.openlocfilehash: 3ef109dc5fad73a19eabefb8eb872c02d62698ba
ms.sourcegitcommit: 124f7f699b6a43314e63af0101cd788db995d1cb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2020
ms.locfileid: "86087565"
---
# <a name="sql-server-instance-migration-to-azure-sql-managed-instance"></a>SQL Server 实例迁移到 Azure SQL 托管实例
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

本文介绍将 SQL Server 2005 或更高版本实例迁移到[AZURE SQL 托管实例](sql-managed-instance-paas-overview.md)的方法。 有关迁移到单个数据库或弹性池的信息，请参阅[迁移到 SQL 数据库](../database/migrate-to-database-from-sql-server.md)。 有关从其他平台迁移的迁移信息，请参阅 [Azure 数据库迁移指南](https://datamigration.microsoft.com/)。

> [!NOTE]
> 若要快速启动和试用 Azure SQL 托管实例，你可能需要转到[快速入门指南](quickstart-content-reference-guide.md)，而不是此页。

概括而言，数据库迁移过程如下所示：

![迁移过程](./media/migrate-to-instance-from-sql-server/migration-process.png)

- [评估 SQL 托管实例的兼容性](#assess-sql-managed-instance-compatibility)，应确保不存在可能阻止迁移的阻碍性问题。
  
  此步骤还包括创建[性能基线](#create-a-performance-baseline)来确定源 SQL Server 实例的资源使用情况。 如果要部署适当大小的托管实例，并在迁移后验证性能不会受到影响，则需要执行此步骤。
- [选择 "应用连接选项](connect-application-instance.md)"。
- [部署到优化大小的托管实例](#deploy-to-an-optimally-sized-managed-instance)，你将在其中选择托管实例的技术特征（vcore 数、内存量）和性能层（业务关键常规用途）。
- [选择迁移方法并迁移](#select-a-migration-method-and-migrate)使用脱机迁移（本机备份/还原、数据库导入/导出）或联机迁移（Azure 数据迁移服务，事务复制）迁移数据库的位置。
- [监视应用程序](#monitor-applications)，确保获得预期的性能。

> [!NOTE]
> 若要将单个数据库迁移到单个数据库或弹性池，请参阅将[SQL Server 数据库迁移到 AZURE SQL database](../database/migrate-to-database-from-sql-server.md)。

## <a name="assess-sql-managed-instance-compatibility"></a>评估 SQL 托管实例的兼容性

首先，确定 SQL 托管实例是否与应用程序的数据库要求兼容。 SQL 托管实例旨在为大多数使用 SQL Server 的现有应用程序提供轻松的提升和移位迁移。 但是，有时可能需要用到一些目前尚不支持的功能，而实现某种解决方法的成本过高。

使用[数据迁移助手](https://docs.microsoft.com/sql/dma/dma-overview)检测影响 Azure SQL 数据库上的数据库功能的潜在兼容性问题。 如果有一些报告的阻止问题，可能需要考虑使用其他选项，例如[AZURE VM 上的 SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/)。 下面是一些示例：

- 如果需要直接访问操作系统或文件系统，例如，在 SQL Server 的同一虚拟机上安装第三方或自定义代理。
- 如果严格依赖于仍不受支持的功能，例如 FileStream/FileTable、PolyBase 和跨实例事务。
- 如果确实需要停留在特定版本的 SQL Server （例如，2012）。
- 如果计算要求远远低于托管实例提供（例如，一个 vCore），并且数据库合并不是可接受的选项。

如果已解析所有已标识的迁移阻止程序并继续迁移到 SQL 托管实例，请注意，某些更改可能会影响工作负荷的性能：

- 如果你定期使用简单/批量记录的模型或按需停止备份，则强制性完整恢复模型和定期自动备份计划可能会影响工作负荷或维护/ETL 操作的性能。
- 不同的服务器或数据库级别配置，例如跟踪标志或兼容性级别。
- 使用的新功能（例如透明数据库加密 (TDE) 或自动故障转移组）可能会影响 CPU 和 IO 使用率。

即使在关键情况下，SQL 托管实例仍可保证99.99% 的可用性，因此无法禁用这些功能导致的开销。 有关详细信息，请参阅[可能导致 SQL Server 和 AZURE SQL 托管实例上的性能不同的根本原因](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/)。

### <a name="create-a-performance-baseline"></a>创建性能基线

如果需要将托管实例上的工作负荷性能与 SQL Server 上运行的原始工作负荷进行比较，则需要创建一个将用于比较的性能基线。

性能基线是一组参数，如平均/最大 CPU 使用率、平均/最大磁盘 IO 延迟、吞吐量、IOPS、平均/最大页生存期以及 tempdb 的平均最大大小。 迁移后，你希望获得相似甚至更好的参数，因此，测量并记录这些参数的基线值非常重要。 除了系统参数外，还需要在工作负荷中选择一组代表查询或最重要的查询，并测量所选查询的最小/平均/最大持续时间和 CPU 使用率。 这些值可让你将托管实例上运行的工作负荷的性能与源 SQL Server 实例上的原始值进行比较。

需要在 SQL Server 实例上测量的一些参数如下：

- [监视 SQL Server 实例上的 CPU 使用率](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Monitor-CPU-usage-on-SQL-Server/ba-p/680777#M131)，并记录平均和峰值 CPU 使用率。
- [监视 SQL Server 实例上的内存使用率](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-memory-usage)，并确定不同组件（例如缓冲池、计划缓存、列存储池、[内存中 OLTP](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/monitor-and-troubleshoot-memory-usage?view=sql-server-2017)等）使用的内存量。此外，应查找 "页生存期" 的 "生存期" 和 "最大值" 页。
- 使用 [sys.dm_io_virtual_file_stats](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-io-virtual-file-stats-transact-sql) 视图或[性能计数器](https://docs.microsoft.com/sql/relational-databases/performance-monitor/monitor-disk-usage)监视源 SQL Server 实例上的磁盘 IO 使用率。
- 通过检查动态管理视图或从 SQL Server 2016 + 版本迁移时查询存储来监视工作负荷和查询性能或 SQL Server 实例。 确定工作负荷中最重要查询的平均持续时间和 CPU 使用率，以将其与托管实例上运行的查询进行比较。

> [!Note]
> 如果你注意到 SQL Server 工作负荷有任何问题（例如 CPU 使用率过高、内存不足或 tempdb 或参数化问题），则在进行基线和迁移之前，应尝试在源 SQL Server 实例上解决这些问题。 将已知问题迁移到任何新系统可能会导致意外的结果，并使任何性能比较无效。

作为此活动的结果，你应该为源系统上的 CPU、内存和 IO 使用记录平均和最大值，以及工作负荷中主要和最关键查询的平均和最大持续时间和 CPU 使用率。 以后应使用这些值，将托管实例上的工作负荷的性能与源 SQL Server 实例上的工作负荷基线性能进行比较。

## <a name="deploy-to-an-optimally-sized-managed-instance"></a>部署到大小最适合的托管实例

SQL 托管实例适用于计划迁移到云的本地工作负荷。 它引入了一个[新的购买模型](../database/service-tiers-vcore.md)，这让你在为工作负荷选择适当的资源级别时更具灵活性。 在本地环境中，你可能习惯于使用物理核心和 IO 带宽来调整这些工作负荷的大小。 托管实例的购买模型以虚拟核心 (vCore) 为依据，同时单独提供更多存储和 IO 资源。 借助 vCore 模型可以更方便地根据当前在本地使用的计算资源，来了解云中的计算要求。 使用此新模型可以适当地调整云中目标环境的大小。 下面介绍了一些可帮助你选择适当服务层级和特征的常规指导：

- 根据基线 CPU 使用情况，你可以预配与 SQL Server 上使用的核心数相匹配的托管实例，请记住，可能需要缩放 CPU 特征，使其与[托管实例的安装位置](resource-limits.md#hardware-generation-characteristics)相匹配。
- 根据基线内存使用情况，选择[具有匹配的内存的服务层](resource-limits.md#hardware-generation-characteristics)。 不能直接选择内存量，因此，需要选择具有匹配内存的 Vcore 数量（例如，Gen5 中的 5.1 GB/vCore）的托管实例。
- 根据文件子系统的基线 IO 延迟，选择常规用途（延迟大于 5 ms）和业务关键（延迟小于 3 ms）服务层。
- 根据基线吞吐量，预先分配数据或日志文件的大小，以获取预期的 IO 性能。

你可以在部署时选择计算和存储资源，然后在不使用[Azure 门户](../database/scale-resources.md)为应用程序提供停机时间的情况下对其进行更改：

![托管实例大小调整](./media/migrate-to-instance-from-sql-server/managed-instance-sizing.png)

若要了解如何创建 VNet 基础结构和托管实例，请参阅[创建托管实例](instance-create-quickstart.md)。

> [!IMPORTANT]
> 必须根据[托管实例 vnet 要求](connectivity-architecture-overview.md#network-requirements)保留目标 VNet 和子网。 任何不兼容性问题都可能导致无法创建新实例或使用已创建的实例。 详细了解如何[新建](virtual-network-subnet-create-arm-template.md)网络和[配置现有](vnet-existing-add-subnet.md)网络。

## <a name="select-a-migration-method-and-migrate"></a>选择迁移方法并迁移

SQL 托管实例面向需要从本地或 Azure VM 数据库实现进行大容量数据库迁移的用户方案。 当你需要提升和移动应用程序的后端，并且这些应用程序经常使用实例级别和/或跨数据库功能时，它们是最佳选择。 若采用此方案，可将整个实例转移到 Azure 中对应的环境，而无需重新构建应用程序。

若要转移 SQL 实例，需要认真规划：

- 迁移需要并置的所有数据库（在同一实例上运行的数据库）。
- 迁移你的应用程序所依赖的实例级对象，包括登录名、凭据、SQL 代理作业和运算符以及服务器级触发器。

SQL 托管实例是一种托管服务，可让你将一些常规 DBA 活动委托给内置的平台。 因此，在中内置了[高可用性](../database/high-availability-sla.md)的情况下，不需要迁移某些实例级别的数据，例如进行定期备份或 Always On 配置的维护作业。

SQL 托管实例支持以下数据库迁移选项（目前仅支持以下两种迁移方法）：

- Azure 数据库迁移服务-在几乎不停机的情况上进行迁移。
- 本机 `RESTORE DATABASE FROM URL` - 使用来自 SQL Server 的本机备份，且需要停机一段时间。

### <a name="azure-database-migration-service"></a>Azure 数据库迁移服务

[Azure 数据库迁移服务](../../dms/dms-overview.md)是一项完全托管的服务，旨在实现从多个数据库源到 Azure 数据平台的无缝迁移，且停机时间最短。 此服务可简化将现有第三方和 SQL Server 数据库迁移到 Azure 所需的任务。 公共预览版中的部署选项包括 Azure SQL 数据库中的数据库和 Azure 虚拟机中的 SQL Server 数据库。 对于企业工作负荷，建议使用数据库迁移服务。

如果你在本地 SQL Server 使用 SQL Server Integration Services （SSIS），则数据库迁移服务还不支持迁移存储 SSIS 包的 SSIS 目录（SSISDB），但你可以在 Azure 数据工厂中预配 Azure-SSIS Integration Runtime （IR），这会在托管实例中创建新的 SSISDB，以便你可以将包重新部署到其中。 请参阅[在 Azure 数据工厂中创建 Azure-SSIS IR](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime)。

若要详细了解此方案以及数据库迁移服务的配置步骤，请参阅[使用数据库迁移服务将本地数据库迁移到托管实例](../../dms/tutorial-sql-server-to-managed-instance.md)。  

### <a name="native-restore-from-url"></a>从 URL 本机还原

从 SQL Server 实例（在[Azure 存储](https://azure.microsoft.com/services/storage/)中提供）获取的本机备份（.bak 文件）的还原是 SQL 托管实例的一项重要功能，可实现快速轻松的脱机数据库迁移。

下图高度概括了该过程：

![迁移流](./media/migrate-to-instance-from-sql-server/migration-flow.png)

下表提供了可以根据所运行的源 SQL Server 版本使用的方法的详细信息：

|步骤|SQL 引擎和版本|备份/还原方法|
|---|---|---|
|将备份放入 Azure 存储|早于 2012 SP1 CU2|将 .bak 文件直接上传到 Azure 存储|
||2012 SP1 CU2 - 2016|使用已弃用的 [WITH CREDENTIAL](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql) 语法直接备份|
||2016 和更高版本|使用 [WITH SAS CREDENTIAL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url) 直接备份|
|从 Azure 存储还原到托管实例|[使用 SAS CREDENTIAL 执行 RESTORE FROM URL](restore-sample-database-quickstart.md)|

> [!IMPORTANT]
>
> - 使用本机还原选项将受[透明数据加密](../database/transparent-data-encryption-tde-overview.md)保护的数据库迁移到托管实例时，需要在还原数据库之前迁移本地或 Azure VM SQL Server 中的相应证书。 有关详细步骤，请参阅将[TDE 证书迁移到托管实例](tde-certificate-migrate.md)。
> - 不支持还原系统数据库。 若要迁移实例级别对象（存储在 master 或 msdb 数据库中），我们建议在目标实例上编写脚本并运行 T-sql 脚本。

有关介绍如何使用 SAS 凭据将数据库备份还原到托管实例的快速入门，请参阅[从备份还原到托管实例](restore-sample-database-quickstart.md)。

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

## <a name="monitor-applications"></a>监视应用程序

完成到托管实例的迁移后，应跟踪工作负荷的应用程序行为和性能。 此过程包括以下活动：

- 将[托管实例上运行的工作负荷的性能](#compare-performance-with-the-baseline)与[您在源 SQL Server 实例上创建的性能基线](#create-a-performance-baseline)进行比较。
- 持续[监视工作负荷的性能](#monitor-performance)，以识别潜在问题并做出改善。

### <a name="compare-performance-with-the-baseline"></a>将性能与基线进行比较

成功迁移后需要立即采取的第一个活动是将工作负荷的性能与基线工作负荷性能进行比较。 此活动的目的是确认托管实例上的工作负荷性能满足你的需求。

在大多数情况下，将数据库迁移到托管实例会保留数据库设置及其原始兼容性级别。 在可能的情况下保留原始设置，以便降低某些性能下降与源 SQL Server 实例的风险。 如果某个用户数据库的兼容级别在迁移之前为 100 或更高，则迁移后，其兼容级别保持相同。 如果在迁移前用户数据库的兼容级别为90，则在升级后的数据库中，兼容级别将设置为100，这是托管实例中支持的最低兼容级别。 系统数据库的兼容级别为 140。 由于迁移到托管实例实际上已迁移到最新版本的 SQL Server 数据库引擎，因此应注意，需要重新测试工作负荷的性能，以避免出现一些惊人的性能问题。

作为先决条件，请确保已完成以下活动：

- 通过调查各种实例、数据库、tempdb 设置和配置，将托管实例上的设置与源 SQL Server 实例的设置对齐。 在运行首次性能比较之前，请确保未更改兼容性级别或加密等设置，否则需要承受启用的某些新功能影响某些查询的风险。 为了减少迁移风险，请只在完成性能监视之后更改数据库兼容级别。
- [为常规用途实施存储最佳做法准则](https://techcommunity.microsoft.com/t5/DataCAT/Storage-performance-best-practices-and-considerations-for-Azure/ba-p/305525)，例如，预先分配文件大小以获得更好的性能。
- 了解[可能导致托管实例和 SQL Server 之间性能差异的关键环境差异](https://azure.microsoft.com/blog/key-causes-of-performance-differences-between-sql-managed-instance-and-sql-server/)，并确定可能影响性能的风险。
- 请确保在托管实例上始终启用查询存储和自动优化。 这些功能可让你衡量工作负荷的性能，并自动修复潜在的性能问题。 了解如何使用查询存储作为在数据库兼容性级别更改前后获取有关工作负荷性能的最佳工具，如在[升级到较新的 SQL Server 版本期间保持性能稳定性](https://docs.microsoft.com/sql/relational-databases/performance/query-store-usage-scenarios#CEUpgrade)中所述。
准备好尽量与本地环境相当的环境后，可以开始运行工作负荷并衡量性能。 度量过程应包含你在[源 SQL Server 实例上创建工作负荷度量值的基线性能时](#create-a-performance-baseline)测量的相同参数。
因此，应将性能参数与基线进行比较，并识别关键差异。

> [!NOTE]
> 在许多情况下，你将无法获取托管实例和 SQL Server 的完全匹配的性能。 Azure SQL 托管实例是一种 SQL Server 数据库引擎，但托管实例上的基础结构和高可用性配置可能会导致一些差异。 您可能会希望某些查询的速度更快，而有些查询可能会更慢。 比较的目标是验证托管实例中的工作负荷性能是否与 SQL Server 上的性能相匹配（平均值），并识别性能不符合原始性能的任何关键查询。

性能比较的结果可能是：

- 托管实例上的工作负荷性能与 SQL Server 上的工作负荷性能对齐或更佳。 在这种情况下，已成功确认迁移成功。
- 大多数性能参数和工作负荷中的查询都可以正常工作，但有些例外会降低性能。 对于这种情况，需要识别差异及其重要性。 如果有一些重要的查询的性能下降，则应该调查基础 SQL 计划是否已更改，或者查询是否遇到了某些资源限制。 这种情况下的缓解措施可以是直接或使用计划指南、重新生成或创建可能会影响计划的统计信息和索引，对关键查询（例如，更改的兼容级别、旧基数估计器）应用一些提示。
- 与源 SQL Server 实例相比，托管实例上的大多数查询速度较慢。 在这种情况下，请尝试确定差异的根本原因[，例如 IO](resource-limits.md#service-tier-characteristics)限制、内存限制、实例日志速率限制等。如果没有可能导致差异的资源限制，请尝试更改数据库的兼容级别或更改数据库设置（如旧基数估计），然后重新启动测试。 查看托管实例或查询存储视图提供的建议，以确定回归性能的查询。

> [!IMPORTANT]
> Azure SQL 托管实例具有内置的自动计划更正功能，默认情况下已启用。 此功能确保过去正常工作的查询在将来不会导致性能下降。 在更改新设置之前，请确保已启用此功能，并且已使用旧设置执行了工作负荷的足够长时间，以使托管实例能够了解基线性能和计划。

更改参数或升级服务层级以融合到最佳配置，直到工作负荷的性能符合需求为止。

### <a name="monitor-performance"></a>监视性能

SQL 托管实例提供很多用于监视和故障排除的高级工具，你应该使用这些工具来监视实例的性能。 需要监视的某些参数如下：

- 实例上的 CPU 使用率，以确定你预配的 Vcore 数是否适合你的工作负荷。
- 托管实例上的页生存期，确定[是否需要额外的内存](https://techcommunity.microsoft.com/t5/Azure-SQL-Database/Do-you-need-more-memory-on-Azure-SQL-Managed-Instance/ba-p/563444)。
- 统计信息（如 `INSTANCE_LOG_GOVERNOR` 或 `PAGEIOLATCH` ）将告诉你是否有存储 IO 问题，尤其是在常规用途层上，你可能需要预先分配文件才能获得更好的 IO 性能。

## <a name="leverage-advanced-paas-features"></a>利用高级 PaaS 功能

一旦你使用完全托管的平台，并且已验证工作负载的性能与 SQL Server 工作负荷性能匹配，就可以使用作为服务的一部分自动提供的优势。

即使在迁移过程中未在托管实例中进行一些更改，在操作实例时，还可以使用一些新的功能来充分利用数据库引擎的最新改进。 一些更改只会在[数据库兼容性级别已更改](https://docs.microsoft.com/sql/relational-databases/databases/view-or-change-the-compatibility-level-of-a-database)后才启用。

例如，无需在托管实例上创建备份，服务会自动为你执行备份。 不再需要考虑计划、创建和管理备份。 SQL 托管实例使你能够使用[时间点恢复（PITR）](../database/recovery-using-backups.md#point-in-time-restore)还原到此保留期内的任何时间点。 此外，您无需担心设置高可用性，因为在中内置了[高可用性](../database/high-availability-sla.md)。

若要增强安全性，请考虑使用[Azure Active Directory 身份验证](../database/security-overview.md)、[审核](auditing-configure.md)、[威胁检测](../database/advanced-data-security.md)、[行级别安全性](https://docs.microsoft.com/sql/relational-databases/security/row-level-security)和[动态数据屏蔽](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking)。

除了高级管理和安全功能，托管实例还提供了一组可帮助你[监视和优化工作负荷](../database/monitor-tune-overview.md)的高级工具。 [Azure SQL Analytics](https://docs.microsoft.com/azure/azure-monitor/insights/azure-sql)使你能够监视大量的托管实例，并集中监视大量实例和数据库。 托管实例中的[自动优化](https://docs.microsoft.com/sql/relational-databases/automatic-tuning/automatic-tuning#automatic-plan-correction)会持续监视 SQL 计划执行统计信息的性能，并自动修复已识别的性能问题。

## <a name="next-steps"></a>后续步骤

- 有关 Azure SQL 托管实例的信息，请参阅[什么是 AZURE sql 托管实例？](sql-managed-instance-paas-overview.md)。
- 有关介绍了如何从备份还原的教程，请参阅[创建托管实例](instance-create-quickstart.md)。
- 有关使用数据库迁移服务进行迁移的教程，请参阅[使用数据库迁移服务将本地数据库迁移到 AZURE SQL 托管实例](../../dms/tutorial-sql-server-to-managed-instance.md)。  
