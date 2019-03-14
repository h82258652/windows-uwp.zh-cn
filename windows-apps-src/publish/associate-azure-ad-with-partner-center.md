---
Description: 要添加和管理帐户的用户，必须先将你的合作伙伴中心帐户关联与组织的 Azure Active Directory 中。
title: 将 Azure Active Directory 与你的合作伙伴中心帐户相关联
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, azure 租户, aad 租户, azure ad 租户, 租户管理, 租户
ms.localizationpriority: medium
ms.openlocfilehash: aacfdb0044fa9b9368ecbd032629ed5e572ece99
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605312"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>将 Azure Active Directory 与你的合作伙伴中心帐户相关联

为了[添加和管理帐户用户](add-users-groups-and-azure-ad-applications.md)，必须先将你的合作伙伴中心帐户关联与组织的 Azure Active Directory。 

[合作伙伴中心](https://partner.microsoft.com/dashboard)利用为多个用户帐户访问和管理 Azure AD。 如果你的组织已经使用 Office 365 或 Microsoft 的其他业务服务，则你已经具有 Azure AD。 否则，您可以创建一个新的 Azure AD 租户从合作伙伴中心内不收取附加费用。

> [!TIP]
> 本主题是特定于在 Windows 应用开发人员计划[合作伙伴中心](https://partner.microsoft.com/dashboard)，但将租户相关联以及管理用户的工作方式类似 Windows 桌面应用程序中的帐户 (请参阅[Windows桌面应用程序](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)有关详细信息) 和 Windows 硬件开发人员计划 (其中引用**Manager**角色还将应用于具有硬件帐户**管理员**角色，请参见[仪表板管理](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration)的详细信息)。

将单个 Azure AD 租户可以与多个合作伙伴中心帐户相关联。 只需拥有一个 Azure AD 租户与合作伙伴中心帐户关联以添加多个帐户用户，但您还可以选择将多个 Azure AD 租户添加到单个的合作伙伴中心帐户。 具有的任何用户**Manager**合作伙伴中心帐户中的角色将具有添加和删除帐户中的 Azure AD 租户的选项。

> [!IMPORTANT]
> 你将在合作伙伴中心帐户与 Azure AD 租户相关联后，若要添加和管理该租户中的帐户用户需要为具有同一租户中的用户登录到合作伙伴中心**Manager**角色。


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>将你的合作伙伴中心帐户与你组织的现有 Azure AD 租户相关联

如果你的组织已使用 Azure AD，请按照下列步骤，将合作伙伴中心帐户链接。

1.  从[合作伙伴中心](https://partner.microsoft.com/dashboard)，选择齿轮图标 （附近仪表板的右上角），然后选择**开发人员设置**。 在中**设置**菜单中，选择**租户**。
2.  选择**关联 Azure AD 与合作伙伴中心帐户**。
3.  输入想要关联的租户 Azure AD 凭据。
4.  查看 Azure AD 租户的组织和域名。 若要完成关联，请选择**确认**。
5.  如果关联是成功，你将准备好添加和管理帐户中的用户**用户**合作伙伴中心中的部分。

> [!IMPORTANT]
> 若要创建新用户或者对 Azure AD 进行其他更改，你需要使用对 Azure AD 租户拥有[全局管理员权限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的帐户登录到该租户。 但是，不需要全局管理员授予的权限，以便将相关联的租户，或将该租户中已存在的用户添加到合作伙伴中心帐户。


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>创建新的 Azure AD 能够在合作伙伴中心帐户相关联

如果您需要设置新的 Azure AD，若要在合作伙伴中心帐户与链接，请执行以下步骤。

1.  从[合作伙伴中心](https://partner.microsoft.com/dashboard)，选择齿轮图标 （附近仪表板的右上角），然后选择**开发人员设置**。 在中**设置**菜单中，选择**租户**。
2.  选择**创建新 Azure AD**。
3.  输入新的 Azure AD 的目录信息：
    - **域名**:我们将使用你的 Azure AD 域，并使用的唯一名称"。 onmicrosoft.com"。 例如，如果你输入“example”，则 Azure AD 域将为“example.onmicrosoft.com”。
    - **联系人电子邮件**:电子邮件地址，其中我们可以与您联系有关你的帐户有必要。
    - **全局管理员用户帐户信息**:第一个名称、 最后一个名称、 用户名和你想要使用新的全局管理员帐户的密码。
4.  单击“创建”以确认新域和帐户信息。
5.  使用新的 Azure AD 全局管理员用户名和密码登录，以开始[添加和管理其他帐户用户](add-users-groups-and-azure-ad-applications.md)。


## <a name="manage-azure-ad-tenant-associations"></a>管理 Azure AD 租户关联

已与你的合作伙伴中心帐户关联的 Azure AD 租户后，您可以添加新租户或删除从现有租户**租户**页。


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>将多个 Azure AD 租户添加到你的合作伙伴中心帐户

任何用户都具有**Manager**合作伙伴中心帐户角色可以与帐户关联的 Azure AD 租户。

若关联新租户，请选择**关联其他 Azure AD 租户**，然后按照上方提示的步骤操作。 请注意，系统会提示你输入要关联的 Azure AD 租户的凭据。


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>从合作伙伴中心帐户中删除 Azure AD 租户

任何用户都具有**Manager**合作伙伴中心帐户角色可以从帐户中删除 Azure AD 租户。

> [!IMPORTANT]
> 当您删除某个租户时，已添加到合作伙伴中心帐户从该租户的所有用户将不再能够登录到帐户。 

若要删除某个租户，查找其名称在**租户**页 (在**帐户设置**)，然后选择**删除**。 系统将提示你确认删除租户。 一旦您这样做，该租户中的任何用户将能够登录到合作伙伴中心帐户，并将删除任何已配置为这些用户的权限。

> [!TIP]
> 如果你当前登录到合作伙伴中心使用同一租户中的帐户，不能删除租户。 若要删除某个租户，你必须登录到合作伙伴中心作为**Manager**与帐户相关联的另一个租户。 如果该帐户只关联了一个租户，则只有登录打开该帐户的 Microsoft 帐户后才能删除该租户。


