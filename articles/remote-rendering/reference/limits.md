---
title: 限制
description: SDK 功能的代码限制
author: erscorms
ms.author: erscor
ms.date: 02/11/2020
ms.topic: reference
ms.openlocfilehash: 17f98f452764abdc8458cdc38661d464ecb0a60d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "84808545"
---
# <a name="limitations"></a>限制

许多功能具有大小、数量或其他限制。

## <a name="azure-frontend"></a>Azure 前端

* 每个进程的 AzureFrontend 实例总数：16.
* 每个 AzureFrontend 的 AzureSession 实例总数：16.

## <a name="objects"></a>对象

* 单个类型允许的对象总数（Entity、CutPlaneComponent 等）：16,777,215。
* 允许的活动剪切平面总数：8.

## <a name="geometry"></a>Geometry

* 资产中允许的材料总数：65,535。
* 单个纹理的最大尺寸： 16384 x 16384。 转换过程会缩减更大的源纹理。

## <a name="overall-number-of-polygons"></a>多边形总数

所有已加载模型允许的多边形数量取决于传递给[会话管理 REST API](../how-tos/session-rest-api.md#create-a-session) 的 VM 大小：

| VM 大小 | 最大多边形数量 |
|:--------|:------------------|
|standard| 2000 万 |
|高级| 无限制 |

有关此限制的更多详细信息，请参阅[VM 大小](../reference/vm-sizes.md)章节。

## <a name="platform-limitations"></a>平台限制

**Windows 10 桌面**

* UWP/x86 是唯一受支持的 UWP 平台。 不支持 UWP/x64。
* Win32/x64 是唯一受支持的 Win32 平台。 Win32/x86 不受支持。

**Hololens 2**

* 不支持[从 PV 相机进行呈现](https://docs.microsoft.com/windows/mixed-reality/mixed-reality-capture-for-developers#render-from-the-pv-camera-opt-in)功能。
