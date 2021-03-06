---
title: 如何配置虚拟中心路由
titleSuffix: Azure Virtual WAN
description: 本文介绍如何配置虚拟中心路由
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 07/07/2020
ms.author: cherylmc
ms.openlocfilehash: 6d14094edc7ae21ca0d56b544fb9c2b19f1f0582
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2020
ms.locfileid: "86144998"
---
# <a name="how-to-configure-virtual-hub-routing"></a>如何配置虚拟中心路由

一个虚拟中心可包含多个网关，例如站点到站点 VPN 网关、ExpressRoute 网关、点到站点网关和 Azure 防火墙。 虚拟中心中的路由功能由路由器提供，用于管理所有路由，包括传输路由、使用边界网关协议 (BGP) 的网关。 此路由器还提供连接到虚拟中心的虚拟网络之间的传输连接，并且最多可支持 50 Gbps 的聚合吞吐量。 这些路由功能适用于标准虚拟 WAN 客户。

有关详细信息，请参阅[关于虚拟中心路由](about-virtual-hub-routing.md)。

> [!NOTE]
> 其中一些功能可能仍在推出。这预计会在8月3日完成。
>

## <a name="create-a-route-table"></a><a name="create-table"></a>创建路由表

1. 在 Azure 门户中，导航到 "虚拟中心"。
2. 在 "**连接**" 下，选择 "**路由**"。 在 "路由" 页上，可以看到**默认**路由表和**无**路由表。

   :::image type="content" source="./media/how-to-virtual-hub-routing/routing.png" alt-text="路由页":::
3. 选择 " **+ 创建路由表**" 以打开 "**创建路由表**" 页。
4. 在 "创建路由表" 页的 "**基本**信息" 选项卡上，填写以下字段。

   :::image type="content" source="./media/how-to-virtual-hub-routing/basics.png" alt-text="“基本信息”选项卡":::

   * **名称**
   * **路由**
   * **路由名称**
   * **目标类型**
   * **目标前缀**：可以聚合前缀。 例如： VNet 1： 10.1.0.0/24 和 VNet 2： 10.1.1.0/24 可聚合为 10.1.0.0/16。 **分支**路由适用于所有连接的 VPN 站点、ExpressRoute 线路和用户 VPN 连接。
   * **下一个跃点**：虚拟网络连接列表或 Azure 防火墙。

     如果选择虚拟网络连接，则会看到 "**配置静态路由**"。 这是一个可选的配置设置。 有关详细信息，请参阅[配置静态路由](about-virtual-hub-routing.md#static)。

      :::image type="content" source="./media/how-to-virtual-hub-routing/next-hop.png" alt-text="下一跃点":::

5. 选择 "**标签**" 选项卡以配置标签名称。 标签提供一种逻辑分组路由表的机制。

    :::image type="content" source="./media/how-to-virtual-hub-routing/labels.png" alt-text="配置标签名称":::

6. 选择 "**关联**" 选项卡，将连接关联到路由表。
你将看到 "**分支**"、"**虚拟网络**" 和 "连接" 的**当前设置**。

    :::image type="content" source="./media/how-to-virtual-hub-routing/associations.png" alt-text="与路由表的关联连接":::

7. 选择 "**传播" 选项卡**，将路由从连接传播到路由表。

    :::image type="content" source="./media/how-to-virtual-hub-routing/propagations.png" alt-text="传播路由":::

8. 选择 "**创建**" 以创建路由表。

## <a name="to-edit-a-route-table"></a><a name="edit-table"></a>编辑路由表

在 Azure 门户中，找到虚拟中心的路由表。 选择要编辑的路由表。

## <a name="to-delete-a-route-table"></a><a name="delete-table"></a>删除路由表

在 Azure 门户中，找到虚拟中心的路由表。 不能删除默认的或 None 路由表。 不过，您可以删除所有自定义路由表。 单击 **"..."**，然后选择 "**删除**"。

## <a name="to-view-effective-routes"></a><a name="view-routes"></a>查看有效路由

在 Azure 门户中，找到虚拟中心的路由表。 单击 **"..."** 并选择 "**有效路由**"，以查看由所选路由表获知的路由。 从连接到路由表的传播路由在路由表的**有效路由**中自动填充。 有关详细信息，请参阅[关于有效路由](effective-routes-virtual-hub.md)。

:::image type="content" source="./media/how-to-virtual-hub-routing/effective.png" alt-text="查看有效路由" lightbox="./media/how-to-virtual-hub-routing/effective-expand.png":::

## <a name="to-set-up-routing-configuration-for-a-virtual-network-connection"></a><a name="routing-configuration"></a>为虚拟网络连接设置路由配置

1. 在 Azure 门户中，导航到虚拟 WAN，然后在 "**连接**" 下，选择 "**虚拟网络连接**"。
1. 选择 " **+ 添加连接**"。
1. 从下拉列表中选择虚拟网络。
1. 设置要与路由表关联的路由配置。 对于**关联路由表**，请从下拉列表中选择路由表。
1. 设置路由配置以传播到一个或多个路由表。 对于**传播到路由表**，请从下拉列表中选择。
1. 对于**静态路由**，为网络虚拟设备配置静态路由 (如适用) 。 虚拟 WAN 支持一个用于虚拟网络连接中的静态路由的下一个跃点 IP。 例如，如果你有一个单独的虚拟设备用于入口和出口流量，则最好将虚拟设备放在单独的 Vnet 中，并将 Vnet 附加到虚拟中心。


:::image type="content" source="./media/how-to-virtual-hub-routing/routing-configuration.png" alt-text="设置路由配置" lightbox="./media/how-to-virtual-hub-routing/routing-configuration-expand.png":::

## <a name="next-steps"></a>后续步骤

有关虚拟中心路由的详细信息，请参阅[关于虚拟中心路由](about-virtual-hub-routing.md)。
有关虚拟 WAN 的详细信息，请参阅[常见问题解答](virtual-wan-faq.md)。
