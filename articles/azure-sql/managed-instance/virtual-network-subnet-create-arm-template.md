---
title: 创建虚拟网络
titleSuffix: Azure SQL Managed Instance
description: 本文介绍如何创建配置为支持部署 Azure SQL 托管实例的虚拟网络。
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 09/12/2019
ms.openlocfilehash: d6458794b8d3c5ba68109dbc90a075d6a2a577b9
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "84711317"
---
# <a name="create-a-virtual-network-for-azure-sql-managed-instance"></a>为 Azure SQL 托管实例创建虚拟网络
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

本文介绍如何创建可在其中部署 Azure SQL 托管实例的有效虚拟网络和子网。

必须在 Azure[虚拟网络](../../virtual-network/virtual-networks-overview.md)中部署 azure SQL 托管实例。 此部署支持以下方案：

- 保护专用 IP 地址
- 直接从本地网络连接到 SQL 托管实例
- 将 SQL 托管实例连接到链接服务器或其他本地数据存储
- 将 SQL 托管实例连接到 Azure 资源  

> [!NOTE]
> 你应在部署第一个实例之前[确定 SQL 托管实例的子网的大小](vnet-subnet-determine-size.md)。 在资源放入子网后，无法调整子网大小。
>
> 如果打算使用现有的虚拟网络，则需要修改该网络配置以适应 SQL 托管实例。 有关详细信息，请参阅[修改 SQL 托管实例的现有虚拟网络](vnet-existing-add-subnet.md)。
>
> 创建托管实例后，不支持将托管实例或虚拟网络移到另一个资源组或订阅。  也不支持将托管实例移到另一个子网。
>

## <a name="create-a-virtual-network"></a>创建虚拟网络

创建和配置虚拟网络的最简单方法是使用 Azure 资源管理器部署模板。

1. 登录到 Azure 门户。

2. 选择“部署到 Azure”按钮： 

   <a target="_blank" href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-sql-managed-instance-azure-environment%2Fazuredeploy.json" rel="noopener" data-linktype="external"> <img src="https://azuredeploy.net/deploybutton.png" data-linktype="external"> </a>

   此按钮将打开一个窗体，可用于配置可在其中部署 SQL 托管实例的网络环境。

   > [!Note]
   > 此 Azure 资源管理器模板将部署包含两个子网的虚拟网络。 一个名为**ManagedInstances**的子网为 SQL 托管实例保留，并且具有预先配置的路由表。 另一个名为 "**默认**" 的子网用于应该访问 SQL 托管实例的其他资源（例如，Azure 虚拟机）。

3. 配置网络环境。 在以下窗体上，可以配置网络环境的参数：

   ![用于配置 Azure 网络的资源管理器模板](./media/virtual-network-subnet-create-arm-template/create-mi-network-arm.png)

   可以更改虚拟网络和子网的名称，并调整与网络资源关联的 IP 范围。 选择“购买”按钮后，此窗体将创建并配置环境。  如果不需要两个子网，可以删除默认子网。

## <a name="next-steps"></a>后续步骤

- 有关概述，请参阅[什么是 SQL 托管实例？](sql-managed-instance-paas-overview.md)。
- 了解[SQL 托管实例中的连接体系结构](connectivity-architecture-overview.md)。
- 了解如何[修改 SQL 托管实例的现有虚拟网络](vnet-existing-add-subnet.md)。
- 有关演示如何创建虚拟网络、创建托管实例和从数据库备份还原数据库的教程，请参阅[创建托管实例](instance-create-quickstart.md)。
- 有关 DNS 问题，请参阅[配置自定义 DNS](custom-dns-configure.md)。
