---
title: 在 Azure AD 的权利管理中查看访问包的访问权限
description: 了解如何在 Azure Active Directory 访问评审（预览版）中完成对授权管理访问包的访问评审。
services: active-directory
documentationCenter: ''
author: msaburnley
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.subservice: compliance
ms.date: 06/18/2020
ms.author: ajburnle
ms.reviewer: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d4de2ac3ee74d60eb532bd469b20523fa937db0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2020
ms.locfileid: "85078581"
---
# <a name="review-access-of-an-access-package-in-azure-ad-entitlement-management"></a>在 Azure AD 的权利管理中查看访问包的访问权限

Azure AD 的权利管理简化了企业如何管理对组、应用程序和 SharePoint 站点的访问。 本文介绍如何针对作为指定审阅者分配到访问包的其他用户执行访问评审。

## <a name="prerequisites"></a>先决条件

若要查看用户的活动访问包分配，你必须满足执行访问评审的先决条件：
- Azure AD Premium P2
- 全局管理员
- 指定的用户管理员、目录所有者或访问包管理器

有关详细信息，请参阅[许可证要求](entitlement-management-overview.md#license-requirements)。


## <a name="open-the-access-review"></a>打开访问评审

使用以下步骤查找并打开访问评审：

1. 你可能会收到 Microsoft 发送的一封电子邮件，要求你评审访问权限。 找到电子邮件以打开访问评审。 下面是用于查看访问权限的示例电子邮件：
    
    ![访问评审审阅者电子邮件](./media/entitlement-management-access-reviews-review-access/review-access-reviewer-email.png)

1. 单击 "**查看用户访问权限**" 链接以打开访问评审。 

1. 如果没有电子邮件，可以直接导航到来查找待定的访问评审 https://myaccess.microsoft.com 。  （对于美国政府版，请改用 `https://myaccess.microsoft.us` 。）

1. 单击左侧导航栏上的 "**访问评审**" 可查看分配给你的待定访问评审的列表。
    
    ![选择访问权限审阅访问权限](./media/entitlement-management-access-reviews-review-access/review-access-myaccess-select-access-review.png)

1. 单击要开始的查看。
    
    ![选择访问评审](./media/entitlement-management-access-reviews-review-access/review-access-select-access-review.png)

## <a name="perform-the-access-review"></a>执行访问评审

打开访问评审后，你将看到需要查看的用户的名称。 可通过两种方式批准或拒绝访问权限：
- 你可以手动批准或拒绝一个或多个用户的访问权限
- 你可以接受系统建议

### <a name="manually-approve-or-deny-access-for-one-or-more-users"></a>手动批准或拒绝一个或多个用户的访问权限
1. 查看用户列表，并确定哪些用户需要继续拥有访问权限。

    ![要查看的用户的列表](./media/entitlement-management-access-reviews-review-access/review-access-list-of-users.png)

1. 若要批准或拒绝访问，请选择用户名称左侧的单选按钮。

1. 在用户名上方的栏中选择 "**批准**" 或 "**拒绝**"。

    ![选择用户](./media/entitlement-management-access-reviews-review-access/review-access-select-users.png)

1. 如果不确定，可以单击 "**不知道**" 按钮。

    如果选择了此选项，则用户将保留访问权限，并且此选择将出现在审核日志中。 该日志将显示你仍完成评审的任何其他审阅者。

1. 你可能需要为你的决策提供原因。 键入一个原因，然后单击 "**提交**"。

    ![批准或拒绝访问权限](./media/entitlement-management-access-reviews-review-access/review-access-decision-approve.png)

1. 您可以在评审结束之前随时更改您的决定。 为此，请从列表中选择用户并更改决策。 例如，你可以批准对你之前拒绝的用户的访问权限。

如果有多个评审者，将记录最后提交的响应。 假设有一个管理员指定两个审阅者（Alice 和 Bob）的示例。 Alice 首先打开评审并批准访问。 在评审结束之前，小明会打开评审并拒绝访问。 在这种情况下，将记录上次的拒绝访问决策。

>[!NOTE]
>如果用户被拒绝访问，则不会立即从访问包中删除这些用户。 审核结束时，用户将从访问包中删除，或管理员结束评审。

### <a name="approve-or-deny-access-using-the-system-generated-recommendations"></a>使用系统生成的建议批准或拒绝访问

若要更快地查看多个用户的访问权限，可以使用系统生成的建议，只需要单击一下即可接受建议。 建议是根据用户的登录活动生成的。

1.  在页面顶部的栏中，单击 "**接受建议**"。
    
    ![选择接受建议](./media/entitlement-management-access-reviews-review-access/review-access-use-recommendations.png)
    
    你将看到建议的操作的摘要。

1.  单击 "**提交**" 以接受建议。

## <a name="next-steps"></a>后续步骤

- [查看访问包](entitlement-management-access-reviews-self-review.md)