---
title: Dav4 和 Dasv4 系列
description: Dav4 和 Dasv4 系列 Vm 的规格。
author: migerdes
ms.service: virtual-machines
ms.subservice: sizes
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: jushiman
ms.openlocfilehash: 06fe0cf14346b9a1a5a1f3c093abeec1d1be159a
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87292504"
---
# <a name="dav4-and-dasv4-series"></a>Dav4 和 Dasv4 系列

Dav4 系列和 Dasv4 系列是在多线程配置中利用 AMD 的 2.35 Ghz EPYC<sup>TM</sup> 7452 处理器的新大小，具有高达 256 mb l3 缓存，专用于 8 mb 的 l3 缓存到每8个内核，增加了运行其常规用途工作负载的客户选项。 Dav4 系列和 Dasv4 系列具有与 D 和 Dsv3 系列相同的内存和磁盘配置。

## <a name="dav4-series"></a>Dav4 系列

ACU：230-260

高级存储：不支持

高级存储缓存：不支持

实时迁移：支持

内存保留更新：支持

Dav4 系列大小基于 2.35 Ghz AMD EPYC<sup>TM</sup> 7452 处理器，可实现 3.35 ghz 的提升最大频率。 Dav4 系列大小为大多数生产工作负荷提供 vCPU、内存和临时存储的组合。 数据磁盘存储与虚拟机分开计费。 若要使用高级 SSD，请使用 Dasv4 大小。 Dasv4 大小的定价和计费标准与 Dav4 系列相同。

| 大小 | vCPU | 内存:GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 最大临时存储吞吐量：IOPS/读取 MBps/写入 MBps | 最大 NIC 数 | 预期网络带宽（MBps） |
|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2a_v4 |  2  | 8  | 50  | 4  | 3000/46/23   | 2 | 1000 |
| Standard_D4a_v4 |  4  | 16 | 100 | 8  | 6000/93/46   | 2 | 2000 |
| Standard_D8a_v4 |  8  | 32 | 200 | 16 | 12000/187/93 | 4 | 4000 |
| Standard_D16a_v4|  16 | 64 | 400 |32  | 24000/375/187 |8 | 8000 |
| Standard_D32a_v4|  32 | 128| 800 | 32 | 48000/750/375 |8 | 16000 |
| Standard_D48a_v4| 48 | 192| 1200 | 32 | 96000 / 1000 / 500 | 8 | 24000 |
| Standard_D64a_v4| 64 | 256 | 1600 | 32 | 96000 / 1000 / 500 | 8 | 30000 |
| Standard_D96a_v4| 96 | 384 | 2400 | 32 | 96000 / 1000 / 500 | 8 | 30000 |

## <a name="dasv4-series"></a>Dasv4 系列

ACU：230-260

高级存储：支持

高级存储缓存：支持

实时迁移：支持

内存保留更新：支持

Dasv4 系列大小基于 2.35 Ghz AMD EPYC<sup>TM</sup> 7452 处理器，可实现 3.35 ghz 的提升最大频率，并使用高级 SSD。 Dasv4 系列大小为大多数生产工作负荷提供 vCPU、内存和临时存储的组合。

| 大小 | vCPU | 内存:GiB | 临时存储 (SSD) GiB | 最大数据磁盘数 | 最大缓存吞吐量和临时存储吞吐量：IOPS/MBps（以 GiB 为单位的缓存大小） | 非缓存磁盘最大吞吐量：IOPS / MBps | 最大 NIC 数 | 预期网络带宽（MBps） |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Standard_D2as_v4|2|8|16|4|4000 / 32 (50)|3200 / 48|2 | 1000 |
| Standard_D4as_v4|4|16|32|8|8000 / 64 (100)|6400 / 96|2 | 2000 |
| Standard_D8as_v4|8|32|64|16|16000 / 128 (200)|12800 / 192|4 | 4000 |
| Standard_D16as_v4|16|64|128|32|32000/255 （400）|25600 / 384|8 | 8000 |
| Standard_D32as_v4|32|128|256|32|64000/510 （800）|51200 / 768|8 | 16000 |
| Standard_D48as_v4|48|192|384|32|96000/1020 （1200）|76800/1148|8 | 24000 |
| Standard_D64as_v4|64|256|512|32|128000/1020 （1600）|80000 / 1200|8 | 30000 | 
| Standard_D96as_v4|96|384|768|32|192000/1020 （2400）|80000 / 1200|8 | 30000 |

[!INCLUDE [virtual-machines-common-sizes-table-defs](../../includes/virtual-machines-common-sizes-table-defs.md)]

## <a name="other-sizes-and-information"></a>其他大小和信息

- [常规用途](sizes-general.md)
- [内存优化](sizes-memory.md)
- [存储优化](sizes-storage.md)
- [GPU 优化](sizes-gpu.md)
- [高性能计算](sizes-hpc.md)
- [前几代](sizes-previous-gen.md)

定价计算器：[定价计算器](https://azure.microsoft.com/pricing/calculator/)

有关磁盘类型的详细信息：[磁盘类型](https://docs.microsoft.com/azure/virtual-machines/linux/disks-types#ultra-ssd-preview/)

## <a name="next-steps"></a>后续步骤

了解有关 [Azure 计算单元 (ACU)](acu.md) 如何帮助跨 Azure SKU 比较计算性能的详细信息。
