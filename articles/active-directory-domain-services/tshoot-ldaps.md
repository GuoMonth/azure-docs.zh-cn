---
title: 排查 Azure AD 域服务中的安全 LDAP 问题 |Microsoft Docs
description: '了解如何排查 Azure Active Directory 域服务托管域的安全 LDAP (LDAPS) '
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 445c60da-e115-447b-841d-96739975bdf6
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: troubleshooting
ms.date: 07/09/2020
ms.author: iainfou
ms.openlocfilehash: c339893c12af74b5ecadb182b9aecb8000df65b9
ms.sourcegitcommit: f844603f2f7900a64291c2253f79b6d65fcbbb0c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/10/2020
ms.locfileid: "86222949"
---
# <a name="troubleshoot-secure-ldap-connectivity-issues-to-an-azure-active-directory-domain-services-managed-domain"></a>排查 Azure Active Directory 域服务托管域的安全 LDAP 连接问题

使用轻型目录访问协议 (LDAP) 的应用程序和服务与 Azure Active Directory 域服务通信 (Azure AD DS) 可以[配置为使用安全 LDAP](tutorial-configure-ldaps.md)。 为了使安全 LDAP 正常工作，必须打开相应的证书和所需的网络端口。

本文将帮助你排查 Azure AD DS 中的安全 LDAP 访问问题。

## <a name="common-connection-issues"></a>常见连接问题

如果在使用安全 LDAP 连接到 Azure AD DS 托管域时遇到问题，请查看以下故障排除步骤。 在每个故障排除步骤之后，尝试再次连接到托管域：

* 安全 LDAP 证书的证书颁发者链在客户端上必须受信任。 可以将根证书颁发机构 (CA) 添加到客户端上受信任的根证书存储中，以建立信任。
    * 请确保已[导出证书并将其应用于客户端计算机][client-cert]。
* 验证托管域的安全 LDAP 证书在 "*使用者*" 或 "*使用者备用名称*" 属性中具有 DNS 名称。
    * 查看[安全 LDAP 证书要求][certs-prereqs]，并根据需要创建替换证书。
* 验证 LDAP 客户端（如*ldp.exe*使用 DNS 名称（而不是 IP 地址）连接到安全 LDAP 终结点。
    * 应用于托管域的证书不包括服务的 IP 地址，只包括 DNS 名称。
* 检查 LDAP 客户端连接到的 DNS 名称。 它必须解析为托管域中安全 LDAP 的公共 IP 地址。
    * 如果 DNS 名称解析为内部 IP 地址，请更新 DNS 记录以解析为外部 IP 地址。
* 对于外部连接，网络安全组必须包含允许从 internet 发往 TCP 端口636的流量的规则。
    * 如果可以使用安全 LDAP 从直接连接到虚拟网络但不是外部连接的资源连接到托管域，请确保[创建网络安全组规则以允许安全 ldap 通信][ldaps-nsg]。

## <a name="next-steps"></a>后续步骤

如果仍有问题，请[发起 Azure 支持请求][azure-support]以获得额外的疑难解答帮助。

<!-- INTERNAL LINKS -->
[azure-support]: ../active-directory/fundamentals/active-directory-troubleshooting-support-howto.md
[configure-ldaps]: tutorial-configure-ldaps.md
[certs-prereqs]: tutorial-configure-ldaps.md#create-a-certificate-for-secure-ldap
[client-cert]: tutorial-configure-ldaps.md#export-a-certificate-for-client-computers
[ldaps-nsg]: tutorial-configure-ldaps.md#lock-down-secure-ldap-access-over-the-internet
