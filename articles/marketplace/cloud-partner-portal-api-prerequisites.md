---
title: API 先决条件-Azure Marketplace
description: 使用云合作伙伴门户 Api 的先决条件。
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/14/2020
ms.openlocfilehash: 2cce5d3463ced126a3e6e77323110e4a70024acb
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/28/2020
ms.locfileid: "87292904"
---
<a name="api-prerequisites"></a>API 先决条件
================

> [!NOTE]
> 云合作伙伴门户 Api 与集成，并将在合作伙伴中心继续工作。 转换引入了少量更改。 查看[云合作伙伴门户 API 参考](./cloud-partner-portal-api-overview.md)中列出的更改，确保你的代码在转换到合作伙伴中心后继续工作。 CPP Api 仅适用于过渡到合作伙伴中心之前已集成的现有产品;新产品应使用合作伙伴中心提交 Api。

使用云合作伙伴门户 API 时需要两个必需的编程资产：服务主体和 Azure Active Directory (Azure AD) 访问令牌。


<a name="create-a-service-principal-in-your-azure-active-directory-tenant"></a>在 Azure Active Directory 租户中创建服务主体
----------------------------------------------------------------

首先，需要在你的 Azure AD 租户中创建一个服务主体。 将在云合作伙伴门户中为此租户分配其自己的一组权限。 你的代码将使用此租户而非使用你的个人凭据来调用 API。  有关创建服务主体的完整说明，请参阅[如何：使用门户创建可访问资源的 Azure AD 应用程序和服务主体](../active-directory/develop/howto-create-service-principal-portal.md)。


<a name="add-the-service-principal-to-your-account"></a>将服务主体添加到你的帐户
-----------------------------------------

现在，你已在你的租户中创建了服务主体，可以将其作为用户添加到你的云合作伙伴门户帐户。 就像用户一样，服务主体可以成为门户的所有者或参与者。

使用以下步骤添加服务主体：

1. 登录到云合作伙伴门户。 
2. 在左侧菜单栏上单击“用户”**** 并选择“添加用户”。****

   ![向门户中添加用户](./media/cloud-partner-portal-api-prerequisites/add-user.jpg)

3. 从“类型”**** 下拉列表中，选择“服务主体”**** 并添加以下详细信息：

-   服务主体的**友好名称**，例如 `spAccount`。
-   **应用程序 ID**。 若要查找此标识符，请依次单击 " [Azure 门户](https://portal.azure.com)"、" **Azure Active Directory**"、"**应用注册**"，然后单击你的应用程序。
-   你的 Azure AD 租户的**租户 ID**，也称为**目录 ID**。 在 [Azure 门户](https://portal.azure.com)中，可以在 Azure Active Directory 页面上的“属性”**** 下找到此标识符。
-   你的服务主体对象的**对象 ID**。 可以从 Azure 门户获取此标识符。 转到“Azure Active Directory”****，选择“应用注册”****，在“本地目录中的托管应用程序”下单击你的应用并单击应用名称。**** 然后，转到“属性”**** 页来查找对象 ID。 请确保不是获取你的应用上的初始对象 ID，而是获取托管应用程序中的对象 ID。
-   与帐户关联的**角色**，它将用于 RBAC。

     ![向门户中添加托管应用](./media/cloud-partner-portal-api-prerequisites/managed-app.png)

1. 单击“添加”将服务主体添加到你的帐户。****

   ![添加服务主体](./media/cloud-partner-portal-api-prerequisites/add-service-principal.jpg)


<a name="get-an-azure-ad-access-token"></a>获取 Azure AD 访问令牌
----------------------------

云合作伙伴门户 API 在身份验证期间使用以下资产和协议：

- 一个 JSON Web 令牌 (JWT) 持有者令牌，用来请求对资源的访问权限
- [OpenID Connect](https://openid.net/connect/) (OIDC) 协议，用来验证标识
- [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md)，用作标识颁发机构

有两个原则方法可用来以编程方式获取 JWT 令牌：

- 使用适用于 .NET 的 Microsoft 身份验证库 ([MSAL.NET](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet))。  建议 .NET 开发者使用此较高级别的方法。 
- 向 Azure AD oauth **令牌**终结点发送一个 **HTTP POST** 请求，它采用以下形式：

``` HTTP
POST https://login.microsoftonline.com/<tenant-id>/oauth2/token
    client_id: <application-id>
    client_secret:<application-secret>
    grant_type: client_credentials
    resource: https://cloudpartner.azure.com
```

现在，可以在 API 请求的授权标头中传递此令牌。

``` HTTP
GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview 
    Accept: application/json
    Authorization: Bearer <access-token>
```
> [!NOTE]
> 对于本参考资料中的所有 API，始终假定授权标头已传递，因此没有显式提及。

如果在请求中遇到身份验证错误，请参阅[对身份验证错误进行故障排除](./cloud-partner-portal-api-troubleshooting-authentication-errors.md)。
