---
title: Azure Migrate 中的 VMware 迁移支持
description: 了解 Azure Migrate 中对 VMware VM 迁移的支持。
ms.topic: conceptual
ms.date: 06/08/2020
ms.openlocfilehash: 9de0609361e67d5251b25df798b61a4ab13e432c
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87387415"
---
# <a name="support-matrix-for-vmware-migration"></a>VMware 迁移的支持矩阵

本文汇总了有关迁移 VMware Vm 与[Azure Migrate：服务器迁移](migrate-services-overview.md#azure-migrate-server-migration-tool)的支持设置和限制。 如果你正在寻找有关评估要迁移到 Azure 的 VMware Vm 的信息，请查看[评估支持矩阵](migrate-support-matrix-vmware.md)。


## <a name="migration-options"></a>迁移选项

可以通过以下几种方式迁移 VMware Vm：

- **使用无代理迁移**：迁移 vm，无需在其上安装任何内容。 为无代理迁移部署[Azure Migrate 设备](migrate-appliance.md)。
- **使用基于代理的迁移**：在要复制的 VM 上安装代理。 对于基于代理的迁移，需要部署[复制设备](migrate-replication-appliance.md)。

查看[本文](server-migrate-overview.md)，了解要使用的方法。

## <a name="migration-limitations"></a>迁移限制

- 对于复制，最多可以选择10个 Vm。 如果要迁移更多计算机，请在10个组中进行复制。
- 对于 VMware 无代理迁移，可以同时运行多达300的复制。

## <a name="agentless-migration"></a>无代理迁移 

本部分概述无代理迁移的要求。

### <a name="vmware-requirements-agentless"></a>VMware 要求（无代理）

该表总结了 VMware 虚拟机监控程序要求。

**VMware** | **详细信息**
--- | ---
**VMware vCenter 服务器** | 版本5.5、6.0、6.5 或6.7。
**VMware vSphere ESXI 主机** | 版本5.5、6.0、6.5 或6.7。
**vCenter Server 权限** | 无代理迁移使用[迁移设备](migrate-appliance.md)。 设备需要这些权限才能 vCenter Server：<br/><br/> - **数据存储. 浏览**：允许浏览 VM 日志文件来排除快照创建和删除故障。<br/><br/> - **LowLevelFileOperations**：允许 "数据存储浏览器" 中的读/写/删除/重命名操作，用于排查快照创建和删除的问题。<br/><br/> - **VirtualMachine.Configu。DiskChangeTracking**：允许启用或禁用 VM 磁盘的更改跟踪，以便在快照之间请求更改的数据块。<br/><br/> - **VirtualMachine.Configu。DiskLease**：允许 VM 使用磁盘租约操作，使用 VMware vSphere 虚拟磁盘开发工具包（VDDK）读取磁盘。<br/><br/> - **VirtualMachine. DiskAccess**：（专门针对 vSphere 6.0 及更高版本）允许在 VM 上打开磁盘，以便使用 VDDK 在磁盘上进行随机读取访问。<br/><br/> - **VirtualMachine. ReadOnlyDiskAccess**：允许在 VM 上打开磁盘，使用 VDDK 读取磁盘。<br/><br/> - **VirtualMachine. DiskRandomAccess**：允许在 VM 上打开磁盘，使用 VDDK 读取磁盘。<br/><br/> - **VirtualMachine. VirtualMachineDownload**：允许对与 VM 关联的文件执行读取操作，下载日志，并在发生故障时进行故障排除。<br/><br/> - **VirtualMachine. SnapshotManagement. \* **：允许创建和管理用于复制的 VM 快照。<br/><br/> - **虚拟机。** 关机：允许 VM 在迁移到 Azure 期间关闭。



### <a name="vm-requirements-agentless"></a>VM 要求（无代理）

该表汇总了 VMware Vm 的无代理迁移要求。

**支持** | **详细信息**
--- | ---
**受支持的操作系统** | 你可以迁移 Azure 支持的[Windows](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines)和[Linux](../virtual-machines/linux/endorsed-distros.md)操作系统。
**Azure 中的 Windows Vm** | 在迁移之前，你可能需要对 Vm[进行一些更改](prepare-for-migration.md#verify-required-changes-before-migrating)。 
**Azure 中的 Linux Vm** | 某些 VM 可能需要经过更改才能在 Azure 中运行。<br/><br/> 对于 Linux，Azure Migrate 会自动对这些操作系统进行更改：<br/> -Red Hat Enterprise Linux 6.5 +、7.0 +<br/> -CentOS 6.5 +、7.0 +</br> -SUSE Linux Enterprise Server 12 SP1 +<br/> -Ubuntu 14.04 LTS、16.04 LTS、18.04 LTS<br/> -Debian 7、8。 对于其他操作系统，请手动进行[所需的更改](prepare-for-migration.md#verify-required-changes-before-migrating)。
**Linux 启动** | 如果/boot 位于专用分区上，则它应驻留在 OS 磁盘上，而不会分布在多个磁盘上。<br/> 如果/boot 是根（/）分区的一部分，则 "/" 分区应在 OS 磁盘上，而不是在其他磁盘上。
**UEFI 启动** | 迁移不支持具有 UEFI 引导的 Vm。
**磁盘大小** | 2 TB 操作系统磁盘;8 TB （适用于数据磁盘）。
**磁盘限制** |  每个虚拟机最多60个磁盘。
**加密磁盘/卷** | 不支持对具有加密磁盘/卷的 Vm 进行迁移。
**共享磁盘群集** | 不支持。
**独立磁盘** | 不支持。
**RDM/传递磁盘** | 如果 Vm 具有 RDM 或传递磁盘，则这些磁盘不会复制到 Azure。
**NFS** | 不会复制装载为 Vm 上的卷的 NFS 卷。
**iSCSI 目标** | 具有 iSCSI 目标的 Vm 不支持无代理迁移。
**多路径 IO** | 不支持。
**存储 vMotion** | 不支持。 如果 VM 使用 storage vMotion，则复制将不起作用。
**成组 Nic** | 不支持。
**IPv6** | 不支持。
**目标磁盘** | Vm 只能迁移到 Azure 中的托管磁盘（标准 HDD、高级 SSD）。
**同时复制** | 300 Vm/vCenter Server。 如果有更多的，请按批次300进行迁移。


### <a name="appliance-requirements-agentless"></a>设备要求（无代理）

无代理迁移使用[Azure Migrate 设备](migrate-appliance.md)。 你可以使用 .OVA 模板将设备部署为 VMware VM，并将其导入 vCenter Server 或使用[PowerShell 脚本](deploy-appliance-script.md)。

- 了解 VMware 的[设备要求](migrate-appliance.md#appliance---vmware)。
- 了解设备需要在[公有](migrate-appliance.md#public-cloud-urls)云和[政府](migrate-appliance.md#government-cloud-urls)云中访问的 URL。
- 在 Azure 政府中，必须[使用脚本](deploy-appliance-script-government.md)部署设备。

### <a name="port-requirements-agentless"></a>端口需求（无代理）

**设备** | **Connection**
--- | ---
设备 | 端口443上的出站连接，将复制的数据上传到 Azure，并与 Azure Migrate 服务进行通信协调复制和迁移。
vCenter 服务器 | 端口443上的入站连接，使设备能够协调复制-创建快照、复制数据、发布快照
vSphere/ESXI 主机 | TCP 端口902上的入站，用于从快照复制数据。

## <a name="agent-based-migration"></a>基于代理的迁移 


本部分总结了基于代理的迁移的要求。


### <a name="vmware-requirements-agent-based"></a>VMware 要求（基于代理）

此表总结了 VMware 虚拟化服务器的评估支持和限制。

**VMware 要求** | **详细信息**
--- | ---
**VMware vCenter 服务器** | 版本5.5、6.0、6.5 或6.7。
**VMware vSphere ESXI 主机** | 版本5.5、6.0、6.5 或6.7。
**vCenter Server 权限** | VCenter Server 的只读帐户。

### <a name="vm-requirements-agent-based"></a>VM 要求（基于代理）

该表总结了 vmware VM 对你要使用基于代理的迁移迁移的 VMware vm 的支持。

**支持** | **详细信息**
--- | ---
**计算机工作负载** | Azure Migrate 支持迁移受支持计算机上运行的任何工作负荷（假设 Active Directory、SQL server 等）。
**操作系统** | 有关最新信息，请查看 Site Recovery 的[操作系统支持](../site-recovery/vmware-physical-azure-support-matrix.md#replicated-machines)。 Azure Migrate 提供完全相同的 VM 操作系统支持。
**Linux 文件系统/来宾存储** | 有关最新信息，请查看[Linux 文件系统](../site-recovery/vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage)对 Site Recovery 的支持。 Azure Migrate 具有相同的 Linux 文件系统支持。
**网络/存储** | 有关最新信息，请查看 Site Recovery 的[网络](../site-recovery/vmware-physical-azure-support-matrix.md#network)和[存储](../site-recovery/vmware-physical-azure-support-matrix.md#storage)必备组件。 Azure Migrate 提供完全相同的网络/存储要求。
**Azure 要求** | 有关最新信息，请查看 Site Recovery 的[Azure 网络](../site-recovery/vmware-physical-azure-support-matrix.md#azure-vm-network-after-failover)、[存储](../site-recovery/vmware-physical-azure-support-matrix.md#azure-storage)和[计算](../site-recovery/vmware-physical-azure-support-matrix.md#azure-compute)要求。 对于 VMware 迁移，Azure Migrate 具有相同的要求。
**移动服务** | 必须在要迁移的每个 VM 上安装移动服务代理。
**UEFI 启动** | Azure 中迁移的 VM 将自动转换为 BIOS 启动 VM。<br/><br/> OS 磁盘最多应有四个分区，卷应使用 NTFS 进行格式化。
**目标磁盘** | Vm 只能迁移到 Azure 中的托管磁盘（标准 HDD、高级 SSD）。
**磁盘大小** | 2 TB 操作系统磁盘;8 TB （适用于数据磁盘）。
**磁盘限制** |  每个虚拟机最多63个磁盘。
**加密磁盘/卷** | 不支持对具有加密磁盘/卷的 Vm 进行迁移。
**共享磁盘群集** | 不支持。
**独立磁盘** | 。
**传递磁盘** | 。
**NFS** | 不会复制装载为 Vm 上的卷的 NFS 卷。
**iSCSI 目标** | 具有 iSCSI 目标的 Vm 不支持无代理迁移。
**多路径 IO** | 不支持。
**存储 vMotion** | 支持
**成组 Nic** | 不支持。
**IPv6** | 不支持。




### <a name="appliance-requirements-agent-based"></a>设备要求（基于代理）

当使用 Azure Migrate 中心提供的 .OVA 模板设置复制设备时，设备将运行 Windows Server 2016 并符合支持要求。 如果在物理服务器上手动设置复制设备，请确保它符合要求。

- 了解 VMware 的[复制设备要求](migrate-replication-appliance.md#appliance-requirements)。
- MySQL 必须安装在设备上。 了解[安装选项](migrate-replication-appliance.md#mysql-installation)。
- 了解复制设备需要在[公共](migrate-replication-appliance.md#url-access)和[政府](migrate-replication-appliance.md#azure-government-url-access)云中访问的 url。
- 查看复制设备需要访问的[端口](migrate-replication-appliance.md#port-access)。

### <a name="port-requirements-agent-based"></a>端口要求（基于代理）

**设备** | **Connection**
--- | ---
VM | Vm 上运行的移动服务与用于复制管理的端口 HTTPS 443 入站上的本地复制设备（配置服务器）进行通信。<br/><br/> VM 将复制数据发送到 HTTPS 9443 入站端口上的进程服务器（在配置服务器计算机上运行）。 可以修改此端口。
复制设备 | 复制设备通过端口 HTTPS 443 出站来协调与 Azure 的复制。
进程服务器 | 进程服务器接收复制数据、优化和加密数据，然后通过 443 出站端口将其发送到 Azure 存储。<br/> 默认情况下，进程服务器在复制设备上运行。

## <a name="azure-vm-requirements"></a>Azure VM 要求

复制到 Azure、无代理或基于代理的迁移的所有本地 Vm 必须满足此表中汇总的 Azure VM 要求。 

**组件** | **要求** 
--- | --- | ---
来宾操作系统 | 验证支持迁移的 VMware VM 操作系统。<br/> 你可以迁移在受支持的操作系统上运行的任何工作负荷。 
来宾操作系统体系结构 | 64 位。 
操作系统磁盘大小 | 最大 2,048 GB。 
操作系统磁盘计数 | 1 
数据磁盘计数 | 64 或更少。 
数据磁盘大小 | 最大 8095 GB
网络适配器 | 支持多个适配器。
共享 VHD | 不支持。 
FC 磁盘 | 不支持。 
BitLocker | 不支持。<br/><br/> 在迁移计算机之前，必须先禁用 BitLocker。
VM 名称 | 1 到 63 个字符。<br/><br/> 限制为字母、数字和连字符。<br/><br/> 计算机名称必须以字母或数字开头和结尾。 
迁移后连接-Windows | 若要在迁移后连接到运行 Windows 的 Azure Vm：<br/><br/> -迁移之前，请在本地 VM 上启用 RDP。<br/><br/> 请确保为**公共**配置文件添加了 TCP 和 UDP 规则，并确保在**Windows 防火墙**"  >  **允许的应用**" 中针对所有配置文件允许 RDP。<br/><br/> 对于站点到站点 VPN 访问，请启用 rdp，并允许**Windows 防火墙**中  ->  的 rdp 允许用于**域和专用**网络的**应用和功能**。<br/><br/> 此外，请检查操作系统的 SAN 策略是否设置为**OnlineAll**。 [了解详细信息](prepare-for-migration.md)。
迁移后连接-Linux | 使用 SSH 迁移后连接到 Azure Vm：<br/><br/> 在迁移之前，请在本地计算机上检查安全外壳服务是否设置为 "启动"，以及防火墙规则是否允许 SSH 连接。<br/><br/> 故障转移后，在 Azure VM 上，允许已故障转移的 VM 上的网络安全组和连接到的 Azure 子网的 SSH 端口建立传入连接。<br/><br/> 此外，为 VM 添加公共 IP 地址。  


## <a name="next-steps"></a>后续步骤

[选择](server-migrate-overview.md)VMware 迁移选项。
