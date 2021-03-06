---
title: 为 Hyper-v 设置 Azure Migrate 设备
description: 了解如何设置 Azure Migrate 设备来评估和迁移 Hyper-v Vm。
ms.topic: article
ms.date: 03/23/2020
ms.openlocfilehash: 56b034709309a3afe9d18df7af9ababc74a24cee
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2020
ms.locfileid: "86109699"
---
# <a name="set-up-an-appliance-for-hyper-v-vms"></a>为 Hyper-v Vm 设置设备

按照本文所述设置 Azure Migrate 设备，以便通过[Azure Migrate：服务器评估](migrate-services-overview.md#azure-migrate-server-assessment-tool)工具评估 hyper-v vm。

[Azure Migrate 设备](migrate-appliance.md)是 Azure Migrate 使用的轻型设备，用于发现本地 hyper-v vm，并将 VM 元数据/性能数据发送到 Azure。

可以使用几种方法来部署设备：

- 使用下载的 VHD 在 Hyper-v VM 上设置。 这是本文中所述的方法。
- 使用 PowerShell 安装程序脚本在 Hyper-V VM 或物理计算机上进行设置。 如果无法使用 VHD 设置 VM，或者在 Azure 政府版中，则应使用[此方法](deploy-appliance-script.md)。

创建设备后，请检查它是否可以连接到 Azure Migrate:服务器评估，首次配置该设备，并将其注册到 Azure Migrate 项目。

## <a name="appliance-deployment-vhd"></a>设备部署（VHD）

使用 VHD 模板设置设备：

- 从 Azure 门户下载压缩的 Hyper-V VHD。
- 创建设备，并检查它是否可以连接到 Azure Migrate 服务器评估。
- 完成设备的首次配置，并将其注册到 Azure Migrate 项目。

## <a name="download-the-vhd"></a>下载 VHD

下载设备的压缩 VHD 模板。

1. 在“迁移目标” > “服务器” > “Azure Migrate:  服务器评估”中，单击“发现”。
2. 在“发现计算机” > “计算机是否已虚拟化?”中，单击“是，使用 Hyper-V”。  
3. 单击“下载”以下载 VHD 文件。

    ![下载 VM](./media/how-to-set-up-appliance-hyper-v/download-appliance-hyperv.png)


### <a name="verify-security"></a>验证安全性

在部署压缩文件之前检查其安全性。

1. 在下载文件的计算机上，打开管理员命令窗口。
2. 运行以下命令以生成 VHD 的哈希
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - 用法示例：```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.vhd SHA256```
3.  对于设备版本2.19.11.12，生成的哈希应与这些[设置](./tutorial-assess-hyper-v.md#verify-security)相符。




## <a name="create-the-appliance-vm"></a>创建设备 VM

导入下载的文件，然后创建 VM。

1. 将压缩的 VHD 文件解压缩到托管设备 VM 的 Hyper-V 主机上的某个文件夹中。 将解压缩三个文件夹。
2. 打开 Hyper-V 管理器。 在“操作”中，单击“导入虚拟机”。 

    ![部署 VHD](./media/how-to-set-up-appliance-hyper-v/deploy-vhd.png)

2. 在“导入虚拟机向导”>“开始之前”中，单击“下一步”。 
3. 在“查找文件夹”中，指定包含已解压缩的 VHD 的文件夹。**** 然后单击 **“下一步”** 。
1. 在“选择虚拟机”中，单击“下一步”。 
2. 在“选择导入类型”中，单击“复制虚拟机(创建新的唯一 ID)”。  然后单击“下一步”。
3. 在“选择目标”中保留默认设置。 单击“下一步” 。
4. 在“存储文件夹”中保留默认设置。 单击“下一步”。
5. 在“选择网络”中，指定 VM 要使用的虚拟交换机。 该交换机需要与 Internet 建立连接才能向 Azure 发送数据。
6. 在“摘要”中检查设置。 然后单击“完成”。
7. 在“Hyper-V 管理器”>“虚拟机”中启动 VM。


### <a name="verify-appliance-access-to-azure"></a>验证设备的 Azure 访问权限

确保设备 VM 可以连接到[公有云](migrate-appliance.md#public-cloud-urls)和[政府云](migrate-appliance.md#government-cloud-urls)的 Azure URL。

## <a name="configure-the-appliance"></a>配置设备

首次设置设备。 如果使用脚本而不是 VHD 来部署设备，则过程中的前两个步骤不适用。

1. 在“Hyper-V 管理器”>“虚拟机”中，右键单击该 VM 并选择“连接”。 
2. 提供设备的语言、时区和密码。
3. 在可连接到该 VM 的任一计算机上打开浏览器，然后打开设备 Web 应用的 URL：**https://*设备名称或 IP 地址*:** 44368。

   或者，可以在设备桌面上单击应用快捷方式打开该应用。
1. 在 Web 应用 >“设置必备组件”中执行以下操作：
    - **许可证**：接受许可条款，并阅读第三方信息。
    - **连接**：应用将检查 VM 是否可访问 Internet。 如果 VM 使用代理：
        - 单击“代理设置”，并以 http://ProxyIPAddress 或 http://ProxyFQDN 格式指定代理地址和侦听端口。
        - 如果代理需要身份验证，请指定凭据。
        - 仅支持 HTTP 代理。
    - **时间同步**：将验证时间。 设备上的时间应与 Internet 时间同步，这样才能正常发现 VM。
    - **安装更新**：Azure Migrate 服务器评估将检查设备上是否安装了最新更新。

### <a name="register-the-appliance-with-azure-migrate"></a>将设备注册到 Azure Migrate

1. 单击“登录”。 如果未显示该按钮，请确保已在浏览器中禁用弹出窗口阻止程序。
2. 在新的标签页中，使用 Azure 凭据登录。
    - 使用用户名和密码登录。
    - 不支持使用 PIN 登录。
3. 成功登录后，返回到 Web 应用。
4. 选择在其中创建了 Azure Migrate 项目的订阅。 然后选择该项目。
5. 指定设备的名称。 该名称应是字母数字，长度为 14 个或更少的字符。
6. 单击“注册”。


### <a name="delegate-credentials-for-smb-vhds"></a>为 SMB VHD 委托凭据

如果在 SMB 上运行 VHD，必须启用从设备到 Hyper-V 主机的凭据委托。 若要从设备执行此操作：

1. 在设备 VM 上运行此命令。 HyperVHost1/HyperVHost2 是示例主机名。

    ```
    Enable-WSManCredSSP -Role Client -DelegateComputer HyperVHost1.contoso.com HyperVHost2.contoso.com -Force
    ```

2. 或者，在设备上的本地组策略编辑器中执行此操作：
    - 在“本地计算机策略” > “计算机配置”中，单击“管理模板” > “系统” > “凭据委托”。    
    - 双击“允许委托新凭据”，并选择“已启用”。 
    - 在“选项”中单击“显示”，将要发现的每台 Hyper-V 主机添加到列表中，并使用 wsman/ 作为前缀。  
    - 在“凭据委托”中，双击“允许允许新凭据并使用仅限 NTLM 的服务器身份验证”。******** 再次将要发现的每台 Hyper-V 主机添加到列表中，并使用 **wsman/** 作为前缀。

## <a name="start-continuous-discovery"></a>启动持续发现

从设备连接到 Hyper-V 主机或群集，并启动 VM 发现。

1. 在“用户名”和“密码”中，指定设备用来发现 VM 的帐户凭据。  指定凭据的易记名称，然后单击“保存详细信息”。
2. 单击“添加主机”，然后指定 Hyper-V 主机/群集详细信息。
3. 单击 **“验证”** 。 验证后，将显示每个主机/群集上可发现的 VM 数。
    - 如果主机验证失败，请将鼠标悬停在“状态”列中的图标上以查看错误。 解决问题并再次验证。
    - 若要删除主机或群集，请选择“删除”。
    - 无法删除群集中的特定主机。 只能删除整个群集。
    - 即使群集中的特定主机出现问题，你也可以添加群集。
4. 验证之后，单击“保存并启动发现”以启动发现过程。

随即会启动发现。 大约 15 分钟后，已发现的 VM 的元数据将显示在 Azure 门户中。

## <a name="verify-vms-in-the-portal"></a>在门户中验证 VM

发现完成后，可以验证 VM 是否出现在门户中。

1. 打开 Azure Migrate 仪表板。
2. 在“Azure Migrate - 服务器” > “Azure Migrate: 服务器评估”页中，单击显示了**已发现服务器**计数的图标。


## <a name="next-steps"></a>后续步骤

试用 Azure Migrate Server 评估的[hyper-v 评估](tutorial-assess-hyper-v.md)。
