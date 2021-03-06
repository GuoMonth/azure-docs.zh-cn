---
title: 配置 Azure VM 连接
titleSuffix: Azure SQL Managed Instance
description: 使用 Azure 虚拟机中的 SQL Server Management Studio 连接到 Azure SQL 托管实例。
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein, carlrab, srbozovi, bonova
ms.date: 02/18/2019
ms.openlocfilehash: 76c4e2c5052e70c4c6cb8ff631151a5e6fc544e5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "84706352"
---
# <a name="quickstart-configure-an-azure-vm-to-connect-to-azure-sql-managed-instance"></a>快速入门：配置 Azure VM 以连接到 Azure SQL 托管实例
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

本快速入门介绍如何使用 SQL Server Management Studio （SSMS）将 Azure 虚拟机配置为连接到 Azure SQL 托管实例。 


有关演示如何使用点到站点连接从本地客户端计算机进行连接的快速入门，请参阅[配置点到站点连接](point-to-site-p2s-configure.md)。

## <a name="prerequisites"></a>先决条件

此快速入门使用在 "[创建托管实例](instance-create-quickstart.md)" 中创建的资源作为其起点。

## <a name="sign-in-to-the-azure-portal"></a>登录到 Azure 门户

登录到 [Azure 门户](https://portal.azure.com/)。

## <a name="create-a-new-subnet-vnet"></a>创建新的子网 VNet

以下步骤在 SQL 托管实例 VNet 中创建新的子网，以便 Azure 虚拟机可以连接到托管实例。 SQL 托管实例子网专用于托管实例。 不能在该子网中创建任何其他的资源（例如 Azure 虚拟机）。

1. 打开在[创建托管实例](instance-create-quickstart.md)快速入门中创建的托管实例的资源组。 选择托管实例的虚拟网络。

   ![SQL 托管实例资源](./media/connect-vm-instance-configure/resources.png)

2. 选择“子网”****，然后选“+ 子网”择****，以便创建新的子网。

   ![SQL 托管实例子网](./media/connect-vm-instance-configure/subnets.png)

3. 根据此表中的信息填充窗体：

   | 设置| 建议的值 | 描述 |
   | ---------------- | ----------------- | ----------- |
   | **名称** | 任何有效的名称|请参阅[命名规则和限制](/azure/architecture/best-practices/resource-naming)，了解什么是有效的名称。|
   | **地址范围(CIDR 块)** | 有效的范围 | 默认值适用于本快速入门。|
   | **网络安全组** | None | 默认值适用于本快速入门。|
   | **路由表** | None | 默认值适用于本快速入门。|
   | **服务终结点** | 未选择任何项 | 默认值适用于本快速入门。|
   | **子网委派** | None | 默认值适用于本快速入门。|

   ![客户端 VM 的新 SQL 托管实例子网](./media/connect-vm-instance-configure/new-subnet.png)

4. 选择 **"确定"** 以在 SQL 托管实例 VNet 中创建此附加子网。

## <a name="create-a-vm-in-the-new-subnet"></a>在新子网中创建 VM 

以下步骤说明如何在新子网中创建虚拟机以连接到 SQL 托管实例。

## <a name="prepare-the-azure-virtual-machine"></a>准备 Azure 虚拟机

由于 SQL 托管实例放置在你的专用虚拟网络中，因此需要使用已安装的 SQL 客户端工具（如 SQL Server Management Studio 或 Azure Data Studio）创建 Azure VM。 此工具可用于连接到 SQL 托管实例和执行查询。 本快速入门使用 SQL Server Management Studio。

若要使用所有必需的工具创建一个客户端虚拟机，最容易的方式是使用 Azure 资源管理器模板。

1. 请确保已登录到其他浏览器选项卡中的 Azure 门户。然后，选择以下按钮创建客户端虚拟机并安装 SQL Server Management Studio：

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fjovanpop-msft%2Fazure-quickstart-templates%2Fsql-win-vm-w-tools%2F201-vm-win-vnet-sql-tools%2Fazuredeploy.json" target="_blank"><img src="https://azuredeploy.net/deploybutton.png"/></a>

2. 根据下表中的信息填充窗体：

   | 设置| 建议的值 | 描述 |
   | ---------------- | ----------------- | ----------- |
   | **订阅** | 有效的订阅 | 必须是有权在其中创建新资源的订阅。 |
   | **资源组** |在[创建 SQL 托管实例](instance-create-quickstart.md)快速入门中指定的资源组|此资源组必须是 VNet 所在的资源组。|
   | **位置** | 资源组的位置 | 此值基于所选资源组进行填充。 |
   | **虚拟机名称**  | 任何有效的名称 | 请参阅[命名规则和限制](/azure/architecture/best-practices/resource-naming)，了解什么是有效的名称。|
   |**管理员用户名**|任何有效的用户名|请参阅[命名规则和限制](/azure/architecture/best-practices/resource-naming)，了解什么是有效的名称。 不要使用“serveradmin”，因为这是保留的服务器级角色。<br>在[连接到 VM](#connect-to-the-virtual-machine) 时使用此用户名。|
   |**密码**|任何有效的密码|密码必须至少 12 个字符长，且符合[定义的复杂性要求](../../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm)。<br>在[连接到 VM](#connect-to-the-virtual-machine) 时使用此密码。|
   | **虚拟机大小** | 任何有效的大小 | 此模板中的默认值 **Standard_B2s** 足以满足本快速入门的要求。 |
   | **位置**|[resourceGroup().location]。| 请勿更改此值。 |
   | **虚拟网络名称**|在其中创建托管实例的虚拟网络|
   | **子网名称**|在前述过程中创建的子网的名称| 不要选择在其中创建托管实例的子网。|
   | **artifacts 位置** | [deployment().properties.templateLink.uri] | 请勿更改此值。 |
   | **项目位置 Sas 令牌** | 留空 | 请勿更改此值。 |

   ![创建客户端 VM](./media/connect-vm-instance-configure/create-client-sql-vm.png)

   如果在[创建 SQL 托管实例](instance-create-quickstart.md)中使用了建议的 VNet 名称和默认子网，则不需要更改最后两个参数。 否则，应将这些值更改为设置网络环境时输入的值。

3. 选择“我同意上述条款和条件”复选框。
4. 选择“购买”，在网络中部署 Azure VM。****
5. 选择“通知”图标，查看部署的状态。****

> [!IMPORTANT]
> 创建虚拟机后约 15 分钟内，不要继续操作，以便创建后脚本有时间来安装 SQL Server Management Studio。

## <a name="connect-to-the-virtual-machine"></a>连接到虚拟机

以下步骤说明如何使用远程桌面连接连接到新创建的虚拟机。

1. 部署完成后，转到虚拟机资源。

    ![VM](./media/connect-vm-instance-configure/vm.png)  

2. 选择“连接”。

   此时会显示远程桌面协议文件（.rdp 文件）窗体，其中包含虚拟机的公共 IP 地址和端口号。

   ![RDP 窗体](./media/connect-vm-instance-configure/rdp.png)  

3. 选择“下载 RDP 文件”。

   > [!NOTE]
   > 也可使用 SSH 连接到 VM。

4. 关闭“连接到虚拟机”窗体。****
5. 若要连接到 VM，请打开下载的 RDP 文件。
6. 出现提示时，选择“连接”****。 在 Mac 上，需要一个 RDP 客户端，例如 Mac 应用商店中的[远程桌面客户端](https://apps.apple.com/app/microsoft-remote-desktop-10/id1295203466?mt=12)。

7. 输入创建虚拟机时指定的用户名和密码，然后选择 **"确定"**。

8. 你可能会在登录过程中收到证书警告。 选择“是”或“继续”以继续连接。********

已连接到“服务器管理器”仪表板中的虚拟机。

## <a name="connect-to-sql-managed-instance"></a>连接到 SQL 托管实例 

1. 在虚拟机中，打开 SQL Server Management Studio。

   需要一段时间才能打开，因为它需要完成其配置，因为这是第一次启动 SSMS。
2. 在 "**连接到服务器**" 对话框中，在 "**服务器名称**" 框中为托管实例输入完全限定的**主机名**。 选择“SQL Server 身份验证”，提供用户名和密码，然后选择“连接”。 

    ![SSMS 连接](./media/connect-vm-instance-configure/ssms-connect.png)  

连接后，可以在“数据库”节点中查看系统和用户数据库，以及在“安全性”、“服务器对象”、“复制”、“管理”、“SQL Server 代理”和“XEvent 探查器”节点中查看各种对象。

## <a name="next-steps"></a>后续步骤

- 如需介绍如何使用点到站点连接从本地客户端计算机进行连接的快速入门，请参阅[配置点到站点连接](point-to-site-p2s-configure.md)。
- 有关应用程序的连接选项概述，请参阅[将应用程序连接到 SQL 托管实例](connect-application-instance.md)。
- 若要将现有 SQL Server 数据库从本地还原到托管实例，可以使用[Azure 数据库迁移服务进行迁移](../../dms/tutorial-sql-server-to-managed-instance.md)，或使用[t-sql restore 命令](restore-sample-database-quickstart.md)从数据库备份文件还原。
