---
title: Azure ExpressRoute：向 VNet 添加网关：经典
description: 在 ExpressRoute 配置中使用 PowerShell 配置经典部署模型 VNet 的 VNet 网关。
services: expressroute
author: charwen
ms.service: expressroute
ms.topic: how-to
ms.date: 12/06/2019
ms.author: charwen
ms.custom: seodec18
ms.openlocfilehash: bcffd60596f8c2286b91eaf13be3e09bb8bec94f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "84736401"
---
# <a name="configure-a-virtual-network-gateway-for-expressroute-using-powershell-classic"></a>使用 PowerShell 配置 ExpressRoute 的虚拟网络网关（经典）
> [!div class="op_single_selector"]
> * [Resource Manager - PowerShell](expressroute-howto-add-gateway-resource-manager.md)
> * [经典 - PowerShell](expressroute-howto-add-gateway-classic.md)
> * [视频 - Azure 门户](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-a-vpn-gateway-for-your-virtual-network)
> 
> 

本文指导完成为预先存在的 VNet 添加、重设大小和删除虚拟网络 (VNet) 网关的步骤。 此配置的步骤专用于使用**经典部署模型**创建的、会在 ExpressRoute 配置中使用的 VNet。 

[!INCLUDE [expressroute-classic-end-include](../../includes/expressroute-classic-end-include.md)]

**关于 Azure 部署模型**

[!INCLUDE [vpn-gateway-classic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-beginning"></a>开始之前
确认已安装此配置所需的 Azure PowerShell cmdlet。

[!INCLUDE [classic powershell install instructions](../../includes/expressroute-poweshell-classic-install-include.md)]

[!INCLUDE [expressroute-gateway-classic-ps](../../includes/expressroute-gateway-classic-ps-include.md)]

## <a name="next-steps"></a>后续步骤
创建 VNet 网关之后，可以将 VNet 链接到 ExpressRoute 线路。 请参阅[将虚拟网络链接到 ExpressRoute 线路](expressroute-howto-linkvnet-classic.md)。

