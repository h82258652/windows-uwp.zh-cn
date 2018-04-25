---
author: jnHs
Description: In order to add and manage account users, you must first associate your Dev Center account with your organization's Azure Active Directory.
title: 将 Azure Active Directory 与开发人员中心帐户相关联
ms.author: wdg-dev-content
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, azure ad, azure 租户, aad 租户, azure ad 租户, 租户管理, 租户
ms.localizationpriority: high
ms.openlocfilehash: f488a72b916e2950a46a5b48e79be2795806a900
ms.sourcegitcommit: b8c77ac8e40a27cf762328d730c121c28de5fbc4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/21/2018
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>将 Azure Active Directory 与开发人员中心帐户相关联

若要[添加和管理帐户用户](add-users-groups-and-azure-ad-applications.md)，必须先将开发人员中心帐户与组织的 Azure Active Directory 相关联。 

Windows 开发人员中心利用 Azure AD 进行多用户帐户访问和管理。 如果你的组织已经使用 Office 365 或 Microsoft 的其他业务服务，则你已经具有 Azure AD。 否则，你可以免费在开发人员中心中创建新的 Azure AD 租户。

> [!TIP]
> 本部分具体介绍 Windows 应用开发人员计划，不过租户关联和用户管理流程与 Windows 桌面应用程序计划（请参阅 [Windows 桌面应用程序计划](https://msdn.microsoft.com/library/windows/desktop/mt826504#users)了解详细信息）和 Windows 硬件开发人员计划（其中对**管理员 (Manager)** 角色的引用也适用于具有**管理员 (Administrator)** 角色的“硬件”帐户；请参阅[仪表板管理](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration)了解详细信息）中的帐户类似。

一个 Azure AD 租户可与多个开发人员中心帐户相关联。 你只需拥有一个与你的开发人员中心帐户相关联的 Azure AD 租户，即可添加多个帐户用户；不过，你也可以选择将多个 Azure AD 租户添加到一个开发人员中心帐户。 任何在开发人员中心帐户中拥有**管理员**角色的用户均可选择添加和删除 Azure AD 租户。

> [!IMPORTANT]
> 在将开发人员中心帐户与 Azure AD 租户相关联后，你将需要以在同一租户中拥有**管理员**角色的用户身份登录开发人员中心，以便在该租户中添加和管理帐户用户。


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>将开发人员中心帐户与组织的现有 Azure AD 租户相关联

如果你的组织已使用 Azure AD，按照以下步骤链接你的开发人员中心帐户。

1.  转到你的**帐户设置**并单击**租户**。
2.  选择**将 Azure AD 与开发人员中心帐户关联**。
3.  输入想要关联的租户 Azure AD 凭据。
4.  查看 Azure AD 租户的组织和域名。 若要完成关联，请选择**确认**。
5.  如果关联成功，则可以随时在开发人员中心的**用户**部分中添加和管理帐户用户。

> [!IMPORTANT]
> 若要创建新用户或者对 Azure AD 进行其他更改，你需要使用对 Azure AD 租户拥有[全局管理员权限](http://go.microsoft.com/fwlink/?LinkId=746654)的帐户登录到该租户。 不过，你无需全局管理员权限即可关联租户或向开发人员中心帐户中添加已在租户中的用户。


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>创建要与开发人员中心帐户关联的全新 Azure AD

如果需要设置要与开发人员中心帐户链接在一起的新 Azure AD，请按照以下步骤操作。

1.  转到你的**帐户设置**并单击**租户**。
2.  选择**创建新 Azure AD**。
3.  输入新的 Azure AD 的目录信息：
    - **域名**：用于 Azure AD 域的唯一名称，包含“.onmicrosoft.com”。 例如，如果你输入“example”，则 Azure AD 域将为“example.onmicrosoft.com”。
    - **联系人电子邮件**：我们在必要时就帐户情况与你联系的电子邮件地址。
    - **全局管理员用户帐户信息**：要用于新全局管理员帐户的名字、姓氏、用户名和密码。
4.  单击**创建**以确认新域和帐户信息。
5.  使用新的 Azure AD 全局管理员用户名和密码登录，以开始[添加和管理其他帐户用户](add-users-groups-and-azure-ad-applications.md)。


## <a name="manage-azure-ad-tenant-associations"></a>管理 Azure AD 租户关联

将 Azure AD 租户与开发人员中心帐户关联后，你可以在**租户**页面添加新租户或删除现有租户。


### <a name="add-multiple-azure-ad-tenants-to-your-dev-center-account"></a>将多个 Azure AD 租户添加到开发人员中心帐户

任何拥有开发人员中心帐户**管理员**角色的用户均可以将 Azure AD 租户关联到帐户。

若关联新租户，请选择**关联其他 Azure AD 租户**，然后按照上方提示的步骤操作。 请注意，系统会提示你输入要关联的 Azure AD 租户的凭据。


### <a name="remove-an-azure-ad-tenant-from-your-dev-center-account"></a>从开发人员中心帐户中删除 Azure AD 租户

任何拥有开发人员中心帐户**管理器**角色的用户均可以从帐户中删除 Azure AD 租户。

> [!IMPORTANT]
> 删除租户后，从该租户添加到开发人员中心帐户的所有用户都无法再登录帐户。 

若要删除租户，请在**租户**页面上查找其名称，然后选择**删除**。 系统将提示你确认删除租户。 执行此操作后，该租户中的所有开发人员中心用户都无法再登录开发人员中心帐户，并将失去你向其配置的所有权限。

> [!TIP]
> 如果当前正使用租户中的帐户登录到开发人员中心，则将无法删除该租户。 若要删除租户，你必须以关联到帐户的其他租户**管理员**身份登录开发人员中心。 如果该帐户只关联了一个租户，则只有登录打开该帐户的 Microsoft 帐户后才能删除该租户。


