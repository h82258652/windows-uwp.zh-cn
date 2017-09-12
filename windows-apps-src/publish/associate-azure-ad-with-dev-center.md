---
author: jnHs
Description: "若要添加和管理帐户用户，必须先将开发人员中心帐户与组织的 Azure Active Directory 相关联。"
title: "将 Azure Active Directory 与开发人员中心帐户相关联"
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10，uwp"
ms.openlocfilehash: ace9470cc707206461baa8c3dd72828ea68a8eb4
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2017
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>将 Azure Active Directory 与开发人员中心帐户相关联

若要[添加和管理帐户用户](add-users-groups-and-azure-ad-applications.md)，必须先将开发人员中心帐户与组织的 Azure Active Directory 相关联。 

Windows 开发人员中心利用 Azure AD 进行多用户管理和角色分配。 如果你的组织已经使用 Office 365 或 Microsoft 的其他业务服务，则你已经具有 Azure AD。 否则，你可以免费在开发人员中心中创建新的 Azure AD 租户。

> [!IMPORTANT]
> 若要将 Azure AD 与开发人员中心帐户相关联，则需要使用[全局管理员](http://go.microsoft.com/fwlink/?LinkId=746654)帐户登录 Azure AD 租户。
> 
> 在将开发人员中心帐户与 Azure AD 相关联后，你将始终需要使用 Azure AD 全局管理员帐户（而非个人 Microsoft 帐户）登录开发人员中心，以便添加和管理帐户用户。

请注意，仅一个开发人员中心帐户可与 Azure AD 租户相关联。 同样地，仅一个 Azure AD 租户可与开发人员中心帐户相关联。 建立此关联后，你将无法在不联系支持人员的情况下删除它。


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>将开发人员中心帐户与组织的现有 Azure AD 租户相关联

如果你的组织已使用 Azure AD，按照以下步骤链接你的开发人员中心帐户。

1.  转到你的**帐户设置**并单击**管理用户**。
2.  单击**将 Azure AD 与开发人员中心帐户关联**按钮。
3.  登录到 Azure AD 帐户。 此帐户必须具有[全局管理员](http://go.microsoft.com/fwlink/?LinkId=746654)权限才能设置关联。
4.  查看 Azure AD 租户的组织和域名。 若要完成关联，请单击**确认**。
5.  如果关联成功，则可以随时在开发人员中心的**管理用户**部分中添加和管理帐户用户。


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>创建要与开发人员中心帐户关联的全新 Azure AD

如果需要设置要与开发人员中心帐户链接在一起的新 Azure AD，请按照以下步骤操作。

1.  转到你的**帐户设置**并单击**管理用户**。
2.  单击**创建新的 Azure AD** 按钮。
3.  输入新的 Azure AD 的目录信息：
 - **域名**：用于 Azure AD 域的唯一名称，包含“.onmicrosoft.com”。 例如，如果你输入“example”，则 Azure AD 域将为“example.onmicrosoft.com”。
 - **联系人电子邮件**：我们在必要时就帐户情况与你联系的电子邮件地址。
 - **全局管理员用户帐户信息**：要用于新全局管理员帐户的名字、姓氏、用户名和密码。
4.  单击**创建**以确认新域和帐户信息。
5.  使用新的 Azure AD 全局管理员用户名和密码登录，以开始[添加和管理其他帐户用户](add-users-groups-and-azure-ad-applications.md)。



