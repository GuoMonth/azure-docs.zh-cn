---
title: 有关 Avere vFXT for Azure 的其他链接
description: 指向有关 Avere vFXT for Azure 的其他信息的链接
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: rohogue
ms.openlocfilehash: 78e0500057baf407f1d26afe29d9df1e525cbf3d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "76153762"
---
# <a name="additional-documentation"></a>其他文档

本文包含指向有关 Avere 控制面板管理界面和相关主题的其他文档链接。

## <a name="avere-cluster-documentation"></a>Avere 群集文档

可以在 <https://azure.github.io/Avere/> 的网站上找到其他 Avere 群集文档。 这些文档可帮助你了解群集的功能及其设置的配置方法。

* [FXT 群集创建指南](<https://azure.github.io/Avere/#fxt_cluster>)专为由物理硬件节点组成的群集而设计，但文档中的某些信息也与 vFXT 群集相关。 具体而言，新 vFXT 群集管理员可受益于阅读这些部分：
  * [自定义支持和监视设置](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_support.html#config-support>)介绍如何自定义支持上传设置并启用远程监视。
  * [配置 VServers 和全局命名空间](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_vserver.html#config-vserver>)具有创建面向客户端的命名空间的信息。
  * [为 Avere 群集配置 DNS](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_network.html#dns-overview>) 介绍如何配置轮询 DNS。
  * [添加后端存储](<https://azure.github.io/Avere/legacy/create_cluster/4_8/html/config_core_filer.html#add-core-filer>)文档如何添加核心文件程序。

* [群集配置指南](<https://azure.github.io/Avere/#operations>)是 Avere 群集的设置和选项的完整参考。 vFXT 群集使用这些选项的子集，但大多数相同的配置页面都适用。

* [仪表板指南](<https://azure.github.io/Avere/#operations>)介绍如何使用 Avere 控制面板的群集监视功能。

## <a name="vfxt-creation-and-management-documentation"></a>vFXT 的创建和管理文档

GitHub 上提供了使用 vfxt.py （一种基于脚本的云群集创建和管理实用工具）的完整指南：[使用 vfxt.py 的云群集管理](https://github.com/Azure/AvereSDK/blob/master/docs/README.md)。
