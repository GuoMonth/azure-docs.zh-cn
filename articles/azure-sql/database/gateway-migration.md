---
title: 网关流量迁移通知
description: 本文向用户提供有关 Azure SQL 数据库网关 IP 地址迁移的通知
services: sql-database
ms.service: sql-db-mi
ms.subservice: service
ms.custom: sqldbrb=1 
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: vanto
ms.date: 07/01/2019
ms.openlocfilehash: 22bfab5b9f00a392054fa1aef6a93195180fd968
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87373481"
---
# <a name="azure-sql-database-traffic-migration-to-newer-gateways"></a>将 Azure SQL 数据库流量迁移到更新的网关
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

Azure 基础结构改进后，Microsoft 会定期刷新硬件，以确保提供最佳的客户体验。 在接下来的几个月中，我们计划添加基于较新的硬件代构建的网关，将流量迁移到这些网关，并最终解除在某些区域中的旧硬件上构建网关。  

在每个区域中提供的任何网关更改之前，将通过电子邮件和 Azure 门户获取客户通知。 在[AZURE SQL 数据库网关 IP 地址](connectivity-architecture.md#gateway-ip-addresses)表中将保留最新的信息。

## <a name="status-updates"></a>状态更新

# <a name="in-progress"></a>[正在进行](#tab/in-progress-ip)
### <a name="september-2020"></a>2020 年 9 月

正在向以下区域添加新的 SQL 网关：

- 北欧：13.74.104.113 
- 西2：40.78.248.10 
- 西欧：52.236.184.163 
- 美国中南部：20.45.121.1、20.49.88。1 

现有 SQL 网关将开始接受以下区域中的流量：
- 日本东部：40.79.184.8、40.79.192。5

这些 SQL 网关应在2020年9月1日开始接受客户流量。 

### <a name="august-2020"></a>2020年8月

正在向以下区域添加新的 SQL 网关：

- 澳大利亚东部：13.70.112。9
- 加拿大中部：52.246.152.0、20.38.144。1 
- 美国西部2：40.78.240。8

这些 SQL 网关应在2020年8月10日开始接受客户流量。 

# <a name="completed"></a>[已完成](#tab/completed-ip)

以下网关迁移完成： 

### <a name="october-2019"></a>2019 年 10 月
- Brazil South
- 美国西部
- 西欧
- East US
- 美国中部
- 东南亚
- 美国中南部
- 北欧
- 美国中北部
- 日本西部
- Japan East
- 美国东部 2
- 东亚

---

## <a name="impact-of-this-change"></a>此更改的影响

流量迁移可能会更改 DNS 为 Azure SQL 数据库中的数据库解析的公共 IP 地址。
如果执行以下操作，可能会受到影响：

- 硬编码本地防火墙中任何特定网关的 IP 地址
- 具有使用 Microsoft .SQL 作为服务终结点但无法与网关 IP 地址通信的任何子网
- 对数据库使用[区域冗余配置](high-availability-sla.md#zone-redundant-configuration)

如果有以下情况，则不会受到影响：

- 作为连接策略的重定向
- 从 Azure 内部连接到 SQL 数据库并使用服务标记
- 使用受支持的 JDBC Driver for SQL Server 所建立的连接将不会产生任何影响。 有关支持的 JDBC 版本，请参阅[下载 MICROSOFT JDBC Driver for SQL Server](/sql/connect/jdbc/download-microsoft-jdbc-driver-for-sql-server)。

## <a name="what-to-do-you-do-if-youre-affected"></a>如果受到影响，应如何操作

建议为 TCP 端口1433上的区域中的所有[网关 ip 地址](connectivity-architecture.md#gateway-ip-addresses)和端口范围11000-11999 允许到 ip 地址的出站流量。 此建议适用于从本地连接的客户端，以及通过服务终结点连接的客户端。 有关端口范围的详细信息，请参阅[连接策略](connectivity-architecture.md#connection-policy)。

使用低于版本4.0 的 Microsoft JDBC 驱动程序从应用程序进行的连接可能无法通过证书验证。 较低版本的 Microsoft JDBC 依赖证书的 "使用者" 字段中的公用名（CN）。 缓解措施是确保将 hostNameInCertificate 属性设置为 *. database.windows.net。 有关如何设置 hostNameInCertificate 属性的详细信息，请参阅 "[连接加密](/sql/connect/jdbc/connecting-with-ssl-encryption)"。

如果上述缓解不起作用，请使用以下 URL 为 SQL 数据库或 SQL 托管实例提供支持请求：https://aka.ms/getazuresupport

## <a name="next-steps"></a>后续步骤

- 了解有关[AZURE SQL 连接体系结构](connectivity-architecture.md)的详细信息
