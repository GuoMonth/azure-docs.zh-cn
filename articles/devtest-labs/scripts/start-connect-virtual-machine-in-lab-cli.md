---
title: Azure CLI 脚本示例 - 在实验室中启动虚拟机 | Microsoft Docs
description: 此 Azure CLI 脚本在 Azure 开发测试实验室的实验室中启动虚拟机。
services: lab-services
author: spelluru
manager: ''
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2018
ms.author: spelluru
ms.custom: mvc
ms.openlocfilehash: a38e0eacdba2c37a18c41f307e4a8120e0e6285d
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87290339"
---
# <a name="use-azure-cli-to-start-a-virtual-machine-in-a-lab-in-azure-devtest-labs"></a>使用 Azure CLI 在 Azure 开发测试实验室的实验室中启动虚拟机

此 Azure CLI 脚本在实验室中启动虚拟机 (VM)。 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>示例脚本

[!code-azurecli-interactive[main](../../../cli_scripts/devtest-lab/start-connect-virtual-machine-in-lab/start-connect-virtual-machine-in-lab.sh "Start a VM")]


## <a name="script-explanation"></a>脚本说明

此脚本使用以下命令：

| Command | 说明 |
|---|---|
| [az lab vm start](/cli/azure/lab/vm?view=azure-cli-latest#az-lab-vm-start) | 在实验室中启动虚拟机 (VM)。 此操作可能需要一段时间才能完成。 |

## <a name="next-steps"></a>后续步骤

有关 Azure CLI 的详细信息，请参阅 [Azure CLI 文档](/cli/azure)。

可以在 [Azure 实验室服务 CLI 示例](../samples-cli.md)中找到其他 Azure 实验室服务 CLI 脚本示例。
