---
Description: 您可以添加用户、 组和 Azure AD 应用程序到合作伙伴中心帐户。
title: 添加用户、 组和 Azure AD 应用程序到合作伙伴中心帐户
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10、 uwp、 azure ad 应用程序、 aad、 用户、 组、 多个用户、 多用户
ms.localizationpriority: medium
ms.openlocfilehash: 326bb547ac5b0d31f5112d7d5737ddad0d592dd5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610122"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>添加用户、 组和 Azure AD 应用程序到合作伙伴中心帐户

**用户**一部分[合作伙伴中心](https://partner.microsoft.com/dashboard)(在**帐户设置**)，可以使用 Azure Active Directory 将用户添加到你的合作伙伴中心帐户。 为每个用户分配一个角色（或自定义权限集），并且该角色定义他们的帐户访问权限。 您还可以添加[用户组](#groups)并[Azure AD 应用程序](#azure-ad-applications)以授予他们访问合作伙伴中心帐户。

将用户添加到帐户后，你可以[编辑帐户详细信息](#edit)、更改[角色和权限](set-custom-permissions-for-account-users.md)或[删除用户](#remove)。

> [!IMPORTANT]
> 若要将用户添加到你的帐户，你必须首先[将你的合作伙伴中心帐户与你组织的 Azure Active Directory 租户相关联](associate-azure-ad-with-partner-center.md)。 

在添加用户时，您将需要指定其访问合作伙伴中心帐户通过将他们分配[角色或自定义权限集的](set-custom-permissions-for-account-users.md)。 

请注意，所有合作伙伴中心用户 （包括组和 Azure AD 应用程序） 必须都具有一个有效的帐户在[与你的合作伙伴中心帐户相关联的 Azure AD 租户](associate-azure-ad-with-partner-center.md)。 您可以在一个租户中一次性完成用户管理工作；但你必须使用你想要在其中添加或编辑用户的租户的管理员帐户登录。 在合作伙伴中心中创建新的用户还将创建该用户的帐户向其进行签名，并对合作伙伴中心中的用户的名称进行更改将在你组织的 Azure AD 租户中进行相同的更改的 Azure AD 租户中。

> [!NOTE]
> 如果你的组织使用[目录集成](https://go.microsoft.com/fwlink/p/?LinkID=724033)若要同步的本地目录服务与 Azure AD，你将无法在合作伙伴中心中创建新用户、 组或 Azure AD 应用程序。 你 （或另一个管理员，你的本地目录中） 将需要创建它们直接在本地目录中，你将能够查看并将其添加在合作伙伴中心之前。


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>将用户添加到你的合作伙伴中心帐户

若要将用户添加到你的合作伙伴中心帐户，请转到**用户**页面**帐户设置**，然后选择**添加用户。** 你必须使用你想要在其中工作的 Azure AD 租户的管理员帐户登录。 

### <a name="add-existing-users"></a>添加现有用户 

您可以选择组织的租户中存在，将它们提供给你的合作伙伴中心帐户的访问权限的用户。 

<span id="from-directory" />

1.  选择齿轮图标 （附近的合作伙伴中心右上角），然后选择**开发人员设置**。 在中**设置**菜单中，选择**用户**。
2.  从**用户**页面，选择**添加用户**。 
3.  从出现的列表中选择一个或多个用户。 你可以使用搜索框来搜索特定用户。
    > [!TIP]
    > 如果选择多个用户将添加到合作伙伴中心帐户，你必须将它们分配相同的角色或自定义权限集。 若要添加具有不同角色/权限的多个用户，请针对每个角色或自定义权限集重复以下步骤。
4.  当你完成选择用户时，请单击**添加选定项**。
5.  在**角色**部分中，为选定的用户指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。
6.  单击 **“保存”**。

### <a name="additional-methods-for-adding-users"></a>其他用户添加方法

如果已使用，还提供了一个管理器帐户登录[全局管理员](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)您正在使用的 Azure AD 租户的权限，则必须将用户添加到你的合作伙伴中心帐户的其他选项。 你需要选择其中一个选项：

-   **将现有用户添加**:选择已存在于组织的目录中并使他们能够访问你的合作伙伴中心帐户，使用上面所述的方法的用户。
-   **创建新用户**:创建全新的用户帐户将添加到同时贵组织的目录和你的合作伙伴中心帐户
-   **邀请外部用户**:发送电子邮件邀请到当前不在你组织的目录中的用户。 它们会受邀访问你的合作伙伴中心帐户，和一个新[来宾用户](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)帐户为其创建在 Azure AD 租户中。

<span id="new-user" />

### <a name="create-new-users"></a>创建新用户

> [!IMPORTANT]
> 你必须使用 Azure AD 租户中的全局管理员帐户登录才能创建新用户。

1.  从**用户**页 (下**帐户设置**)，选择**将用户添加**，然后选择**创建新用户**。
2.  输入新用户的名字、姓氏和用户名。
3.  如果希望新用户在组织目录中拥有[全局管理员帐户](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)，请选中标有**使此用户成为 Azure AD 中的全局管理员，并且全权管理所有目录资源**的框。 这可使该用户完整访问公司 Azure AD 中的所有管理功能。 他们将能够添加和管理组织的目录中的用户 (但不在合作伙伴中心，除非该帐户授予适当[角色/权限](set-custom-permissions-for-account-users.md))。 如果选中此框，需要为用户提供**密码恢复电子邮件**。
4.  如果你选中了**使此用户成为 Azure AD 中的全局管理员**框，请输入用户在需要恢复密码时可以使用的电子邮件。
5.  在**组成员身份**部分中，选择任何希望新用户加入的组。
6.  在**角色**部分中，为用户指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。
7.  单击 **“保存”**。
8.  在确认页上，你将看到新用户的登录信息，包括临时密码。 请确保记下此信息，并将其提供给新用户，因为在离开此页面后可能无法访问临时密码。


<span id="email" />

### <a name="invite-outside-users"></a>邀请外部用户

> [!IMPORTANT]
> 你必须使用 Azure AD 租户中的全局管理员帐户登录才能邀请外部用户。

1.  从**用户**页 (下**帐户设置**)，选择**将用户添加**，然后选择**通过电子邮件邀请用户**。
1.  输入一个或多个电子邮件地址（最多十个），用逗号或分号隔开。
2.  在**角色**部分中，为用户指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。
3.  单击 **“保存”**。

你邀请的用户将获得加入你帐户的电子邮件邀请，并且会在你的 Azure AD 租户中为其创建新的[来宾用户](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)帐户。 每个用户需要接受邀请才能访问帐户。

如果需要重新发送邀请，请在**用户**页上查找用户，然后选择他们的电子邮件地址（或者显示**邀请挂起**的文本）。 然后在页面底部单击**重新发送邀请**。

> [!IMPORTANT]
> 外部用户邀请以加入合作伙伴中心帐户分配相同的角色和权限为其他用户。 但是，外部用户无法在 Visual Studio 中执行某些任务，例如将应用与 Microsoft Store 关联，或创建要上传到 Microsoft Store 的程序包。 如果用户需要执行这些任务，请选择**创建新用户**而不是**邀请外部用户**。 （如果你不希望将这些用户添加到现有 Azure AD 租户，可以[创建新租户](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)，然后在该租户中为他们创建新用户帐户。） 


### <a name="changing-a-users-directory-password"></a>更改用户的目录密码

如果在创建用户帐户时已经提供了**密码恢复电子邮件**，那么用户可以在有需要时自行更改密码。 你还可以按照以下步骤更新用户密码（如果你使用 Azure AD 租户中的全局管理员帐户登录以更改用户密码）。 请注意，这将更改你的 Azure AD 租户中的用户的密码，他们用来访问合作伙伴中心以及密码。 

1.  从**用户**页 (下**帐户设置**)，选择你想要编辑的用户帐户的名称。
2.  选择**重置密码**在页面底部的按钮。
3.  将显示一个确认页，用于显示用户的登录信息，包括临时密码。

    > [!IMPORTANT]
    >  如您将无法访问临时密码，在您离开此页后，则请确保打印或复制此信息并将其提供给用户。

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>将组添加到你的合作伙伴中心帐户

到合作伙伴中心帐户，可以从你组织的目录中添加一个组。 当你执行此操作时，属于该组成员的每个用户都将能够访问它，并且具有与组的分配角色关联的权限。

### <a name="add-groups-from-your-organizations-directory"></a>从组织的目录添加组

1.  选择齿轮图标 （附近的合作伙伴中心右上角），然后选择**开发人员设置**。 在中**设置**菜单中，选择**用户**。
2. 从**用户**页上，选择**添加组**。
2.  从出现的列表中选择一个或多个组。 你可以使用搜索框来搜索特定组。
    > [!TIP]
    > 如果选择多个组将添加到合作伙伴中心帐户，你必须将它们分配相同的角色或自定义权限集。 若要添加具有不同角色/权限的多个组，请针对每个角色或自定义权限集重复以下步骤。

3.  完成选择组后，请单击**添加选定项**。
4.  在**角色**部分中，为选定的组指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。 组的所有成员将都能够访问合作伙伴中心帐户的权限应用于组，而不考虑其个人帐户与关联的角色/权限。
5.  单击 **“保存”**。


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>在您组织的目录中创建新的组帐户，并将其添加到合作伙伴中心帐户

如果你想要授予对新推出的组合作伙伴中心访问权限，可以创建一个新组**用户**部分。 请注意，将在你组织的目录中，而不仅仅是在合作伙伴中心帐户中创建新组。

1.  从**用户**页 (下**开发人员设置**)，单击**将组添加**。
2.  在下一页上，选择**新的组**。
3.  输入新组的显示名称。
4.  为组指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。 组的所有成员将都能够访问合作伙伴中心帐户的权限应用于组，而不考虑其个人帐户与关联的角色/权限。
5.  从出现的列表中选择要分配给新组的用户。 你可以使用搜索框来搜索特定用户。
6.  完成选择用户后，请单击**添加选定项**将其添加到新组中。
7.  单击 **“保存”**。


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>添加到合作伙伴中心帐户的 Azure AD 应用程序

您可以允许应用程序或服务属于你组织的 Azure AD 访问合作伙伴中心帐户。 这些 Azure AD 应用程序用户帐户可用于调用 [Microsoft Store 服务](../monetize/using-windows-store-services.md)提供的 REST API。


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>从组织的目录中添加 Azure AD 应用程序

1.  1.  选择齿轮图标 （附近的合作伙伴中心右上角），然后选择**开发人员设置**。 在中**设置**菜单中，选择**用户**。
2. 从**用户**页面，选择**添加 Azure AD 应用程序**。
3.  从出现的列表中选择一个或多个 Azure AD 应用程序。 你可以使用搜索框来搜索特定的 Azure AD 应用程序。
    > [!TIP]
    > 如果选择多个 Azure AD 应用程序将添加到合作伙伴中心帐户，你必须将它们分配相同的角色或自定义权限集。 若要添加具有不同角色/权限的多个 Azure AD 应用程序，请针对每个角色或自定义权限集重复以下步骤。

4.  完成选择 Azure AD 应用程序后，请单击**添加选定项**。
5.  在**角色**部分中，为选定的 Azure AD 应用程序指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。
6.  单击 **“保存”**。


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>创建一个新 Azure AD 应用程序所在组织的目录的帐户，并将其添加到合作伙伴中心帐户

如果你想要授予对品牌新的 Azure AD 应用程序帐户的合作伙伴中心访问权限，则可以创建一个在**用户**部分。 请注意，在你组织的目录中，而不仅仅是在合作伙伴中心帐户中，这将创建一个新的帐户。

> [!TIP]
> 如果您主要使用此 Azure AD 应用程序对于合作伙伴中心身份验证，且不需要用户直接访问，可以输入任何有效的地址作为**回复 URL**并**应用程序 ID URI**，只要为这些值不是使用任何其他 Azure AD 应用程序目录中。

1.  从**用户**页 (下**帐户设置**)，选择**将 Azure AD 应用程序添加**。
2.  在下一页上，选择**新建 Azure AD 应用程序**。
3.  输入新的 Azure AD 应用程序的**回复 URL**。 这是用户可以登录并使用 Azure AD 应用程序的 URL（有时也称为“应用 URL”或“登录 URL”）。 **回复 URL**不能超过 256 个字符，且在你目录中必须是唯一的。
4.  输入新 Azure AD 应用程序的**应用程序 ID URI**。 这是 Azure AD 应用程序在向 Azure AD 发送单一登录请求时所显示的逻辑标识符。 请注意，每个 Azure AD 应用程序的**应用 ID URI** 在目录中必须唯一，且长度不得超过 256 个字符。 有关**应用 ID URI** 的详细信息，请参阅[将应用程序与 Azure Active Directory 集成](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant)。
5.  在**角色**部分中，为 Azure AD 应用程序指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。
6.  单击 **“保存”**。

添加或创建 Azure AD 应用程序后，可以返回到**用户**部分，然后选择应用程序名称查看应用程序的设置，包括租户 ID、客户端 ID、回复 URL 和应用 ID URI。

> [!NOTE]
> 如果要使用 [Microsoft Store 服务](../monetize/using-windows-store-services.md)提供的 REST API，将需要此页面上显示的租户 ID 和客户端 ID 值，以获取可用于对服务调用进行身份验证的 Azure AD 访问令牌。   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>管理 Azure AD 应用程序的密钥

如果 Azure AD 应用程序在 Microsoft Azure AD 中读写数据，它将需要密钥。 可以通过编辑其信息在合作伙伴中心创建 Azure AD 应用程序的密钥。 还可以删除不再需要的密钥。

1.  从**用户**页 (下**帐户设置**)，选择 Azure AD 应用程序的名称。
    > [!TIP]
    > 当您单击 Azure AD 应用程序的名称时，您将看到所有 Azure AD 应用程序，包括日期上创建的密钥和到期时间的活动密钥。 若要删除不再需要的密钥，请单击 **“删除”**。

2.  若要添加新的密钥，请选择**添加新的密钥**。
3.  你将看到一个显示 **Client ID** 和 **Key** 值的屏幕。
    > [!IMPORTANT]
    > 如您将无法在您离开此页后再次访问它，则请确保打印或复制此信息。

4.  如果你想要创建多个密钥，请选择**添加另一个密钥**。

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>编辑用户、组或 Azure AD 应用程序

添加用户、 组和/或 Azure AD 应用程序到合作伙伴中心帐户后，您可以对其帐户信息进行更改。 

> [!IMPORTANT]
> 对所做更改[角色或权限](set-custom-permissions-for-account-users.md)只会影响合作伙伴中心访问权限。 （例如 Azure AD 应用程序更改用户的名称或组成员身份或回复 URL 和应用程序 ID URI） 的所有其他更改将反映在贵组织的 Azure AD 租户以及作为你的合作伙伴中心帐户。 

1.  从**用户**页 (下**帐户设置**)，选择用户、 组或你想要编辑的 Azure AD 应用程序帐户的名称。
2.  进行所需更改。 你可以编辑的项目如下所示：
    -   对于**用户**，你可以编辑用户的名字、姓氏或用户名。 你还可以在**组成员身份**部分中选择或取消选择组，以更新其组成员身份。
    -   对于**组**，你可以编辑组的名称。 （若要更新组成员身份，请编辑要添加或从组中删除的用户，并对**组成员身份**部分进行更改。）
    -   对于 **Azure AD 应用程序**，你可以输入**回复 URL** 或**应用 ID URI** 的新值。
    请记住将组织的目录以及在合作伙伴中心帐户中进行这些更改。
3.  有关与合作伙伴中心访问相关的更改，请选择或取消选择你想要应用，或选择的角色**自定义权限**并进行所需的更改。 这些更改仅影响合作伙伴中心访问并不会更改你的组织的 Azure AD 租户中的任何权限。
3.  单击 **“保存”**。


## <a name="view-history-for-account-users"></a>查看帐户用户的历史记录

作为帐户所有者，你可以查看已添加到帐户中的任何其他用户的详细浏览历史记录。

上**用户**页 (下**帐户设置**)，选择下显示链接**上次活动**用户想要查看其浏览历史记录。 你可以查看此用户在过去 30 天内访问过的所有网页的 URL。

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>删除用户、组和 Azure AD 应用程序

若要在合作伙伴中心帐户中删除用户、 组或 Azure AD 应用程序，请选择**删除**按其名称显示在链接**用户**页。 在确认你想要将其删除，该用户、 组或 Azure AD 应用程序将不再能够访问到合作伙伴中心帐户 （除非稍后添加）。

> [!IMPORTANT]
> 删除用户、 组或 Azure AD 应用程序意味着它将不再有权访问合作伙伴中心帐户。 它**不会**从你的组织的目录中删除用户、组或 Azure AD 应用程序。

 

