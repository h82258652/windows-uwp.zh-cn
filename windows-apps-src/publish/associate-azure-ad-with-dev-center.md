---
author: jnHs
Description: In order to add and manage account users, you must first associate your Partner Center account with your organization's Azure Active Directory.
title: 将 Azure Active Directory 与你的合作伙伴中心帐户相关联
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, azure 租户, aad 租户, azure ad 租户, 租户管理, 租户
ms.localizationpriority: medium
ms.openlocfilehash: 9f44d5bc0e07ab40a396c103d2a8ba6db5427ae8
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2018
ms.locfileid: "7578543"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>将 Azure Active Directory 与你的合作伙伴中心帐户相关联

为了[添加和管理帐户用户](add-users-groups-and-azure-ad-applications.md)，必须先将你的合作伙伴中心帐户关联与你的组织的 Azure Active Directory 中。 

[合作伙伴中心](https://partner.microsoft.com/dashboard)利用 Azure AD 进行多用户帐户访问和管理。 如果你的组织已经使用 Office 365 或 Microsoft 的其他业务服务，则你已经具有 Azure AD。 否则，你可以创建一个新的 Azure AD 租户从合作伙伴中心内产生任何附加费用。

> [!TIP]
> 本主题仅适用于 Windows 应用开发人员计划的[合作伙伴中心](https://partner.microsoft.com/dashboard)中，不过租户关联和用户管理流程类似 Windows 桌面应用程序计划中的帐户 （请参阅[Windows 桌面应用程序](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)详细信息） 在 Windows 硬件开发人员计划 （其中到了**管理员**角色的引用内容也适用于硬件帐户具有**管理员**角色; 有关详细信息，请参阅[仪表板管理](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration)）。

一个 Azure AD 租户可与多个合作伙伴中心帐户相关联。 你只需拥有一个即可添加多个帐户用户，与你的合作伙伴中心帐户相关联的 Azure AD 租户，但你还可以选择将多个 Azure AD 租户添加到一个合作伙伴中心帐户。 任何拥有合作伙伴中心帐户中了**管理员**角色的用户将可以选择添加和从帐户中删除 Azure AD 租户。

> [!IMPORTANT]
> 你将你的合作伙伴中心帐户与 Azure AD 租户相关联后，以便添加和管理帐户用户，在该租户，你将需要在同一租户拥有了**管理员**角色的用户身份登录到合作伙伴中心。


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>将你的合作伙伴中心帐户与你的组织的现有 Azure AD 租户相关联

如果你的组织已使用 Azure AD，请按照以下步骤链接你的合作伙伴中心帐户。

1.  从[合作伙伴中心](https://partner.microsoft.com/dashboard)中，选择 （附近的仪表板右上角） 的齿轮图标，然后选择**开发人员设置**。 在**设置**菜单中，选择**租户**。
2.  选择**将你的合作伙伴中心帐户与 Azure AD 关联**。
3.  输入想要关联的租户 Azure AD 凭据。
4.  查看 Azure AD 租户的组织和域名。 若要完成关联，请选择**确认**。
5.  如果关联成功，则将准备好添加和管理合作伙伴中心中的**用户**部分中的帐户用户。

> [!IMPORTANT]
> 若要创建新用户或者对 Azure AD 进行其他更改，你需要使用对 Azure AD 租户拥有[全局管理员权限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的帐户登录到该租户。 但是，不需要全局管理员权限即可关联租户，或将已存在的用户在该租户中添加到你的合作伙伴中心帐户。


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>创建要与合作伙伴中心帐户相关联的全新 Azure AD

如果你需要将新的 Azure AD 设置为与你的合作伙伴中心帐户链接，请按照这些步骤。

1.  从[合作伙伴中心](https://partner.microsoft.com/dashboard)中，选择 （附近的仪表板右上角） 的齿轮图标，然后选择**开发人员设置**。 在**设置**菜单中，选择**租户**。
2.  选择**创建新 Azure AD**。
3.  输入新的 Azure AD 的目录信息：
    - **域名**：用于 Azure AD 域的唯一名称，包含“.onmicrosoft.com”。 例如，如果你输入“example”，则 Azure AD 域将为“example.onmicrosoft.com”。
    - **联系人电子邮件**：我们在必要时就帐户情况与你联系的电子邮件地址。
    - **全局管理员用户帐户信息**：要用于新全局管理员帐户的名字、姓氏、用户名和密码。
4.  单击**创建**以确认新域和帐户信息。
5.  使用新的 Azure AD 全局管理员用户名和密码登录，以开始[添加和管理其他帐户用户](add-users-groups-and-azure-ad-applications.md)。


## <a name="manage-azure-ad-tenant-associations"></a>管理 Azure AD 租户关联

具有与你的合作伙伴中心帐户相关联的 Azure AD 租户后，你可以添加新租户或删除的**租户**页面中的现有租户。


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>将多个 Azure AD 租户添加到你的合作伙伴中心帐户

凡是有了**管理员**角色的合作伙伴中心帐户的用户可以将 Azure AD 租户与帐户相关联。

若关联新租户，请选择**关联其他 Azure AD 租户**，然后按照上方提示的步骤操作。 请注意，系统会提示你输入要关联的 Azure AD 租户的凭据。


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>从合作伙伴中心帐户中删除 Azure AD 租户

凡是有了**管理员**角色的合作伙伴中心帐户的用户可以从帐户中删除 Azure AD 租户。

> [!IMPORTANT]
> 当你删除租户时，从该租户添加到合作伙伴中心帐户的所有用户将不再能够登录到帐户。 

若要删除租户，**租户**页面上查找其名称，（在**帐户设置**），然后选择**删除**。 系统将提示你确认删除租户。 一旦你执行此操作，在该租户中的任何用户都将无法登录到合作伙伴中心帐户，并将删除你已配置这些用户的任何权限。

> [!TIP]
> 如果你当前正登录到合作伙伴中心在同一租户中使用的帐户，你无法删除该租户。 若要删除租户，你必须登录到合作伙伴中心作为**管理器**与帐户相关联的另一个租户。 如果该帐户只关联了一个租户，则只有登录打开该帐户的 Microsoft 帐户后才能删除该租户。


