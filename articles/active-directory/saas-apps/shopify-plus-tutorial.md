---
title: 教程：Azure Active Directory 单一登录 (SSO) 与 Shopify Plus 的集成 | Microsoft Docs
description: 了解如何在 Azure Active Directory 和 Shopify Plus 之间配置单一登录。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4f3f2888-1bc8-42c6-94d5-163d05eeb66d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 06/18/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f3740415652407c834ec258730f89e46398a9f79
ms.sourcegitcommit: 1e6c13dc1917f85983772812a3c62c265150d1e7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86170473"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-shopify-plus"></a>教程：Azure Active Directory 单一登录 (SSO) 与 Shopify Plus 的集成

本教程介绍如何将 Shopify Plus 与 Azure Active Directory (Azure AD) 集成。 将 Shopify Plus 与 Azure AD 集成后，可以：

* 在 Azure AD 中控制谁有权访问 Shopify Plus。
* 让用户使用其 Azure AD 帐户自动登录 Shopify Plus。
* 在一个中心位置（Azure 门户）管理帐户。

若要了解有关 SaaS 应用与 Azure AD 集成的详细信息，请参阅 [Azure Active Directory 的应用程序访问与单一登录是什么](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)。

## <a name="prerequisites"></a>先决条件

若要开始操作，需备齐以下项目：

* 一个 Azure AD 订阅。 如果没有订阅，可以获取一个[免费帐户](https://azure.microsoft.com/free/)。
* 已启用单一登录 (SSO) 的 Shopify Plus 订阅。

## <a name="scenario-description"></a>方案描述

本教程在测试环境中配置并测试 Azure AD SSO。

* Shopify Plus 支持 SP 和 IDP 发起的 SSO

* 配置 Shopify Plus 后，就可以强制实施会话控制，实时防止组织的敏感数据外泄和渗透。 会话控制从条件访问扩展而来。 [了解如何通过 Microsoft Cloud App Security 强制实施会话控制](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。

## <a name="adding-shopify-plus-from-the-gallery"></a>从库中添加 Shopify Plus

要配置 Shopify Plus 与 Azure AD 的集成，需要从库中将 Shopify Plus 添加到托管 SaaS 应用列表。

1. 使用工作或学校帐户或个人 Microsoft 帐户登录到 [Azure 门户](https://portal.azure.com)。
1. 在左侧导航窗格中，选择“Azure Active Directory”服务。
1. 导航到“企业应用程序”，选择“所有应用程序” 。
1. 若要添加新的应用程序，请选择“新建应用程序”。
1. 在“从库中添加”部分的搜索框中，键入“Shopify Plus” 。
1. 从结果面板中选择“Shopify Plus”，然后添加该应用。 在该应用添加到租户时等待几秒钟。


## <a name="configure-and-test-azure-ad-single-sign-on-for-shopify-plus"></a>配置和测试 Shopify Plus 的 Azure AD 单一登录

使用名为 B.Simon 的测试用户配置并测试 Shopify Plus 的 Azure AD SSO。 要正常使用 SSO，需要在 Azure AD 用户与 Shopify Plus 中的相关用户之间建立链接关系。

要配置并测试 Shopify Plus 的 Azure AD SSO，请完成以下构建基块：

1. **[配置 Azure AD SSO](#configure-azure-ad-sso)** - 使用户能够使用此功能。
    1. **[创建 Azure AD 测试用户](#create-an-azure-ad-test-user)** - 使用 B. Simon 测试 Azure AD 单一登录。
    1. **[分配 Azure AD 测试用户](#assign-the-azure-ad-test-user)** - 使 B. Simon 能够使用 Azure AD 单一登录。
1. **[配置 Shopify Plus SSO](#configure-shopify-plus-sso)** - 在应用程序端配置单一登录设置。
    1. **[创建 Shopify Plus 测试用户](#create-shopify-plus-test-user)** - 在 Shopify Plus 中创建 B.Simon 的对应用户，并将其链接到用户的 Azure AD 表示形式。
1. **[测试 SSO](#test-sso)** - 验证配置是否正常工作。

## <a name="configure-azure-ad-sso"></a>配置 Azure AD SSO

按照下列步骤在 Azure 门户中启用 Azure AD SSO。

1. 在 [Azure 门户](https://portal.azure.com/)的“Shopify Plus”应用程序集成页上，找到“管理”部分，选择“单一登录”  。
1. 在“选择单一登录方法”页上选择“SAML” 。
1. 在“使用 SAML 设置单一登录”页上，单击“基本 SAML 配置”的编辑/笔形图标以编辑设置 。

   ![编辑基本 SAML 配置](common/edit-urls.png)

1. 如果要在“IDP”发起的模式下配置应用程序，请在“基本 SAML 配置”部分中输入以下字段的值 ：

    在“回复 URL”文本框中，使用以下模式键入 URL：`https://accounts.shopify.com/saml/consume/organization/<ORGANIZATION_ID>`

1. 如果要在 SP 发起的模式下配置应用程序，请单击“设置其他 URL”，并执行以下步骤：

    在“登录 URL”文本框中，键入 URL：`https://shopify.plus/login`

    > [!NOTE]
    > 答复 URL 值不是真实值。 请使用实际回复 URL 更新此值。 请联系 [Shopify Plus 客户端支持团队](mailto:plus-user-management@shopify.com)来获取此值。 还可以参考 Azure 门户中的“基本 SAML 配置”部分中显示的模式。

1. Shopify Plus 应用程序需要特定格式的 SAML 断言，这要求将自定义属性映射添加到 SAML 令牌属性配置。 以下屏幕截图显示了默认属性的列表。

    ![image](common/default-attributes.png)

1. 除了上述属性，Shopify Plus 应用程序还要求在 SAML 响应中传递回更多的属性，如下所示。 这些属性也是预先填充的，但可以根据要求查看它们。

    | 名称 | 源属性|
    | ---- | --------------- |
    | 电子邮件 | user.mail |

1. 将“名称 ID”格式更改为“永久” 。 选择“唯一用户标识符(名称 ID)”选项，然后选择“名称标识符”格式 。 对于此选项选择“永久”。 保存所做更改。
1. 在“设置 SAML 单一登录”页上的“SAML 签名证书”部分，选择“复制”按钮以复制“应用联合元数据 URL”，并将其保存在计算机上  。

    ![证书下载链接](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>创建 Azure AD 测试用户

在本部分，我们将在 Azure 门户中创建名为 B.Simon 的测试用户。

1. 在 Azure 门户的左侧窗格中，依次选择“Azure Active Directory”、“用户”和“所有用户”  。
1. 选择屏幕顶部的“新建用户”。
1. 在“用户”属性中执行以下步骤：
   1. 在“名称”字段中，输入 `B.Simon`。  
   1. 在“用户名”字段中输入 username@companydomain.extension。 例如，`B.Simon@contoso.com`。
   1. 选中“显示密码”复选框，然后记下“密码”框中显示的值。 
   1. 单击“创建”。

### <a name="assign-the-azure-ad-test-user"></a>分配 Azure AD 测试用户

在本部分中，通过授予 B.Simon 访问 Shopify Plus 的权限，允许其使用 Azure 单一登录。

1. 在 Azure 门户中，依次选择“企业应用程序”、“所有应用程序”。 
1. 在“应用程序”列表中，选择“Shopify Plus”。
1. 在应用的概述页中，找到“管理”部分，选择“用户和组” 。

   ![“用户和组”链接](common/users-groups-blade.png)

1. 选择“添加用户”，然后在“添加分配”对话框中选择“用户和组”。  

    ![“添加用户”链接](common/add-assign-user.png)

1. 在“用户和组”对话框中，从“用户”列表中选择“B.Simon”，然后单击屏幕底部的“选择”按钮。  
1. 如果在 SAML 断言中需要任何角色值，请在“选择角色”对话框的列表中为用户选择合适的角色，然后单击屏幕底部的“选择”按钮。 
1. 在“添加分配”对话框中，单击“分配”按钮。 

## <a name="configure-shopify-plus-sso"></a>配置 Shopify Plus SSO

要查看全部步骤，请参阅 [Shopify 有关设置 SAML 集成的文档](https://help.shopify.com/en/manual/shopify-plus/saml)。

若要在“Shopify Plus”端配置单一登录，请从 Azure Active Directory 复制“应用联合元数据 URL” 。 然后，登录[组织管理员](https://shopify.plus)并转到“用户” > “安全性” 。 选择“设置配置”，然后将应用联合元数据 URL 粘贴到“标识提供者元数据 URL”部分中********。 若要完成此步骤，请选择“添加”。

### <a name="create-shopify-plus-test-user"></a>创建 Shopify Plus 测试用户

在本部分中，将在 Shopify Plus 中创建名为 B.Simon 的用户。 返回到“用户”部分，并通过输入用户的电子邮件和访问权限来添加用户。 使用单一登录前，必须先创建并激活用户。

### <a name="enforce-saml-authentication"></a>强制执行 SAML 身份验证

> [!NOTE]
> 建议在广泛应用这种集成之前，通过使用单独用户来测试集成。

单独用户：
1. 通过由 Azure AD 管理并在 Shopify Plus 中进行验证的电子邮件域，转到 Shopify Plus 中的单独用户页面。
1. 在“SAML 身份验证”部分中，选择“编辑”，再选择“必需”，然后选择“保存”******** 。
1. 测试此用户是否可以通过 IdP 启动和 SP 启动的流成功登录。

对于电子邮件域下的所有用户：
1. 返回到“安全性”页面。
1. 为 SAML 身份验证设置选择“必需”。 这将在 Shopify Plus 中为具有该电子邮件域的所有用户强制实施 SAML。
1. 选择“保存”。

> [!IMPORTANT]
> 为电子邮件域下的所有用户启用 SAML 会影响使用此应用程序的所有用户。 用户将无法使用其常规登录页面登录。 他们只能通过 Azure Active Directory 访问应用。 Shopify 不提供用户可以使用其常规用户名和密码登录的备份登录 URL。 如有必要，可以联系 Shopify 客户支持以关闭 SAML。

## <a name="test-sso"></a>测试 SSO 

在本部分中，使用访问面板测试 Azure AD 单一登录配置。

在访问面板中单击“Shopify Plus”磁贴时，应会自动登录到设置了 SSO 的 Shopify Plus。 有关访问面板的详细信息，请参阅 [Introduction to the Access Panel](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)（访问面板简介）。

## <a name="additional-resources"></a>其他资源

- [有关如何将 SaaS 应用与 Azure Active Directory 集成的教程列表](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory 的应用程序访问与单一登录是什么？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [什么是 Azure Active Directory 中的条件访问？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [通过 Azure AD 试用 Shopify Plus](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security 中的会话控制是什么？](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [如何使用高级可见性和控制保护 Shopify Plus](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
