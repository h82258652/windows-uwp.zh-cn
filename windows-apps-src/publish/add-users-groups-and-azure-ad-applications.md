---
Description: You can add users, groups, and Azure AD applications to your Partner Center account.
title: Add users, groups, and Azure AD applications to your Partner Center account
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad application, aad, user, group, multiple users, multi-user
ms.localizationpriority: medium
ms.openlocfilehash: 41467f51e02f3cc700e3759f33d6fd6eea3ac7a6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260078"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Add users, groups, and Azure AD applications to your Partner Center account

The **Users** section of [Partner Center](https://partner.microsoft.com/dashboard) (under **Account settings**) lets you use Azure Active Directory to add users to your Partner Center account. 为每个用户分配一个角色（或自定义权限集），并且该角色定义他们的帐户访问权限。 You can also add [groups of users](#groups) and [Azure AD applications](#azure-ad-applications) to grant them access to your Partner Center account.

将用户添加到帐户后，你可以[编辑帐户详细信息](#edit)、更改[角色和权限](set-custom-permissions-for-account-users.md)或[删除用户](#remove)。

> [!IMPORTANT]
> In order to add users to your account, you must first [associate your Partner Center account with your organization's Azure Active Directory tenant](associate-azure-ad-with-partner-center.md). 

When adding users, you will need to specify their access to your Partner Center account by assigning them a [role or set of custom permissions](set-custom-permissions-for-account-users.md). 

Keep in mind that all Partner Center users (including groups and Azure AD applications) must have an active account in [an Azure AD tenant that is associated with your Partner Center account](associate-azure-ad-with-partner-center.md). 您可以在一个租户中一次性完成用户管理工作；但你必须使用你想要在其中添加或编辑用户的租户的管理员帐户登录。 Creating a new user in Partner Center will also create an account for that user in the Azure AD tenant to which you are signed in, and making changes to a user's name in Partner Center will make the same changes in your organization's Azure AD tenant.

> [!NOTE]
> If your organization uses [directory integration](https://docs.microsoft.com/previous-versions/azure/azure-services/jj573653(v=azure.100)?redirectedfrom=MSDN) to sync the on-premises directory service with your Azure AD, you won't be able to create new users, groups, or Azure AD applications in Partner Center. You (or another admin in your on-premises directory) will need to create them directly in the on-premises directory before you'll be able to see and add them in Partner Center.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Add users to your Partner Center account

To add users to your Partner Center account, go to the **Users** page in **Account settings** and select **Add users.** 你必须使用你想要在其中工作的 Azure AD 租户的管理员帐户登录。 

### <a name="add-existing-users"></a>添加现有用户 

You can select users who already exist in your organization's tenant and give them access to your Partner Center account. 

<span id="from-directory" />

1.  Select the gear icon (near the upper right corner of Partner Center) and then select **Developer settings**. 在“设置”菜单中，选择“用户”。
2.  从**用户**页面，选择**添加用户**。 
3.  从出现的列表中选择一个或多个用户。 你可以使用搜索框来搜索特定用户。
    > [!TIP]
    > If you select more than one user to add to your Partner Center account, you must assign them the same role or set of custom permissions. 若要添加具有不同角色/权限的多个用户，请针对每个角色或自定义权限集重复以下步骤。
4.  当你完成选择用户时，请单击**添加选定项**。
5.  在**角色**部分中，为选定的用户指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。
6.  单击 **“保存”** 。

### <a name="additional-methods-for-adding-users"></a>其他用户添加方法

If you are signed in with a Manager account which also has [global administrator](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) permissions for the Azure AD tenant you're working in, you will have additional options to add users to your Partner Center account. 你需要选择其中一个选项：

-   **Add existing users**: Choose users who already exist in your organization's directory and give them access to your Partner Center account, using the method described above.
-   **Create new users**: Create brand new user accounts to add to both your organization's directory and your Partner Center account
-   **邀请外部用户**：向当前不在你的组织目录中的用户发送电子邮件邀请。 They will be invited to access your Partner Center account, and a new [guest user](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) account will be created for them in your Azure AD tenant.

<span id="new-user" />

### <a name="create-new-users"></a>创建新用户

> [!IMPORTANT]
> 你必须使用 Azure AD 租户中的全局管理员帐户登录才能创建新用户。

1.  From the **Users** page (under **Account settings**), select **Add users**, then choose **Create new users**.
2.  输入新用户的名字、姓氏和用户名。
3.  如果希望新用户在组织目录中拥有[全局管理员帐户](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)，请选中标有**使此用户成为 Azure AD 中的全局管理员，并且全权管理所有目录资源**的框。 这可使该用户完整访问公司 Azure AD 中的所有管理功能。 They'll be able to add and manage users in your organization's directory (though not in Partner Center, unless you grant the account the appropriate [role/permissions](set-custom-permissions-for-account-users.md)). 如果选中此框，需要为用户提供**密码恢复电子邮件**。
4.  如果你选中了**使此用户成为 Azure AD 中的全局管理员**框，请输入用户在需要恢复密码时可以使用的电子邮件。
5.  在**组成员身份**部分中，选择任何希望新用户加入的组。
6.  在**角色**部分中，为用户指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。
7.  单击 **“保存”** 。
8.  在确认页上，你将看到新用户的登录信息，包括临时密码。 请确保记下此信息，并将其提供给新用户，因为在离开此页面后可能无法访问临时密码。


<span id="email" />

### <a name="invite-outside-users"></a>邀请外部用户

> [!IMPORTANT]
> 你必须使用 Azure AD 租户中的全局管理员帐户登录才能邀请外部用户。

1.  From the **Users** page (under **Account settings**), select **Add users**, then choose **Invite users by email**.
1.  输入一个或多个电子邮件地址（最多十个），用逗号或分号隔开。
2.  在**角色**部分中，为用户指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。
3.  单击 **“保存”** 。

你邀请的用户将获得加入你帐户的电子邮件邀请，并且会在你的 Azure AD 租户中为其创建新的[来宾用户](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)帐户。 每个用户需要接受邀请才能访问帐户。

如果需要重新发送邀请，请在**用户**页上查找用户，然后选择他们的电子邮件地址（或者显示**邀请挂起**的文本）。 然后在页面底部单击**重新发送邀请**。

> [!IMPORTANT]
> Outside users that you invite to join your Partner Center account can be assigned the same roles and permissions as other users. 但是，外部用户无法在 Visual Studio 中执行某些任务，例如将应用与 Microsoft Store 关联，或创建要上传到 Microsoft Store 的程序包。 如果用户需要执行这些任务，请选择**创建新用户**而不是**邀请外部用户**。 （如果你不希望将这些用户添加到现有 Azure AD 租户，可以[创建新租户](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)，然后在该租户中为他们创建新用户帐户。） 


### <a name="changing-a-users-directory-password"></a>更改用户的目录密码

如果在创建用户帐户时已经提供了**密码恢复电子邮件**，那么用户可以在有需要时自行更改密码。 你还可以按照以下步骤更新用户密码（如果你使用 Azure AD 租户中的全局管理员帐户登录以更改用户密码）。 Note that this will change the user's password in your Azure AD tenant, along with the password they use to access Partner Center. 

1.  From the **Users** page (under **Account settings**), select the name of the user account that you want to edit.
2.  Select the **Reset password** button at the bottom of the page.
3.  将显示一个确认页，用于显示用户的登录信息，包括临时密码。

    > [!IMPORTANT]
    >  Be sure to print or copy this info and provide it to the user, as you won't be able to access the temporary password after you leave this page.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Add groups to your Partner Center account

You can add a group from your organization's directory to your Partner Center account. 当你执行此操作时，属于该组成员的每个用户都将能够访问它，并且具有与组的分配角色关联的权限。

### <a name="add-groups-from-your-organizations-directory"></a>从组织的目录添加组

1.  Select the gear icon (near the upper right corner of Partner Center) and then select **Developer settings**. 在“设置”菜单中，选择“用户”。
2. From the **Users** page, select **Add groups**.
2.  从出现的列表中选择一个或多个组。 你可以使用搜索框来搜索特定组。
    > [!TIP]
    > If you select more than one group to add to your Partner Center account, you must assign them the same role or set of custom permissions. 若要添加具有不同角色/权限的多个组，请针对每个角色或自定义权限集重复以下步骤。

3.  完成选择组后，请单击**添加选定项**。
4.  在**角色**部分中，为选定的组指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。 All members of the group will be able to access your Partner Center account with the permissions you apply to the group, regardless of the roles/permissions associated with their individual account.
5.  单击 **“保存”** 。


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Create a new group account in your organization's directory and add it to your Partner Center account

If you want to grant Partner Center access to a brand new group, you can create a new group in the **Users** section. Note that the new group will be created in your organization's directory, not just in your Partner Center account.

1.  From the **Users** page (under **Developer settings**), click **Add groups**.
2.  On the next page, select **New group**.
3.  输入新组的显示名称。
4.  为组指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。 All members of the group will be able to access your Partner Center account with the permissions you apply to the group, regardless of the roles/permissions associated with their individual account.
5.  从出现的列表中选择要分配给新组的用户。 你可以使用搜索框来搜索特定用户。
6.  完成选择用户后，请单击**添加选定项**将其添加到新组中。
7.  单击 **“保存”** 。


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Add Azure AD applications to your Partner Center account

You can allow applications or services that are part of your organization's Azure AD to access your Partner Center account. 这些 Azure AD 应用程序用户帐户可用于调用 [Microsoft Store 服务](../monetize/using-windows-store-services.md)提供的 REST API。


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>从组织的目录中添加 Azure AD 应用程序

1.  1.  Select the gear icon (near the upper right corner of Partner Center) and then select **Developer settings**. 在“设置”菜单中，选择“用户”。
2. 从**用户**页面，选择**添加 Azure AD 应用程序**。
3.  从出现的列表中选择一个或多个 Azure AD 应用程序。 你可以使用搜索框来搜索特定的 Azure AD 应用程序。
    > [!TIP]
    > If you select more than one Azure AD application to add to your Partner Center account, you must assign them the same role or set of custom permissions. 若要添加具有不同角色/权限的多个 Azure AD 应用程序，请针对每个角色或自定义权限集重复以下步骤。

4.  完成选择 Azure AD 应用程序后，请单击**添加选定项**。
5.  在**角色**部分中，为选定的 Azure AD 应用程序指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。
6.  单击 **“保存”** 。


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Create a new Azure AD application account in your organization's directory and add it to your Partner Center account

If you want to grant Partner Center access to a brand new Azure AD application account, you can create one in the **Users** section. Note that this will create a new account in your organization's directory, not just in your Partner Center account.

> [!TIP]
> If you are primarily using this Azure AD application for Partner Center authentication, and don't need users to access it directly, you can enter any valid address for the **Reply URL** and **App ID URI**, as long as those values are not used by any other Azure AD application in your directory.

1.  From the **Users** page (under **Account settings**), select **Add Azure AD applications**.
2.  On the next page, select **New Azure AD application**.
3.  输入新的 Azure AD 应用程序的**回复 URL**。 这是用户可以登录并使用 Azure AD 应用程序的 URL（有时也称为“应用 URL”或“登录 URL”）。 **回复 URL**不能超过 256 个字符，且在你目录中必须是唯一的。
4.  输入新 Azure AD 应用程序的**应用程序 ID URI**。 这是 Azure AD 应用程序在向 Azure AD 发送单一登录请求时所显示的逻辑标识符。 请注意，每个 Azure AD 应用程序的**应用 ID URI** 在目录中必须唯一，且长度不得超过 256 个字符。 有关**应用 ID URI** 的详细信息，请参阅[将应用程序与 Azure Active Directory 集成](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant)。
5.  在**角色**部分中，为 Azure AD 应用程序指定[角色或自定义权限](set-custom-permissions-for-account-users.md)。
6.  单击 **“保存”** 。

添加或创建 Azure AD 应用程序后，可以返回到**用户**部分，然后选择应用程序名称查看应用程序的设置，包括租户 ID、客户端 ID、回复 URL 和应用 ID URI。

> [!NOTE]
> 如果要使用 [Microsoft Store 服务](../monetize/using-windows-store-services.md)提供的 REST API，将需要此页面上显示的租户 ID 和客户端 ID 值，以获取可用于对服务调用进行身份验证的 Azure AD 访问令牌。   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>管理 Azure AD 应用程序的密钥

如果 Azure AD 应用程序在 Microsoft Azure AD 中读写数据，它将需要密钥。 You can create keys for an Azure AD application by editing its info in Partner Center. 还可以删除不再需要的密钥。

1.  From the **Users** page (under **Account settings**), select the name of the Azure AD application.
    > [!TIP]
    > When you click the name of the Azure AD application, you'll see all of the active keys for the Azure AD application, including the date on which the key was created and when it will expire. 若要删除不再需要的密钥，请单击“删除”。

2.  To add a new key, select **Add new key**.
3.  你将看到一个显示“客户端 ID”和“密钥”值的屏幕。
    > [!IMPORTANT]
    > Be sure to print or copy this info, as you won't be able to access it again after you leave this page.

4.  If you want to create more keys, select **Add another key**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>编辑用户、组或 Azure AD 应用程序

After you've added users, groups, and/or Azure AD applications to your Partner Center account, you can make changes to their account info. 

> [!IMPORTANT]
> Changes made to [roles or permissions](set-custom-permissions-for-account-users.md) will only affect Partner Center access. All other changes (such as changing a user's name or group membership, or the Reply URL and App ID URI for an Azure AD application) will be reflected in your organization's Azure AD tenant as well as in your Partner Center account. 

1.  From the **Users** page (under **Account settings**), select the name of the user, group, or Azure AD application account that you want to edit.
2.  进行所需更改。 你可以编辑的项目如下所示：
    -   对于**用户**，你可以编辑用户的名字、姓氏或用户名。 你还可以在**组成员身份**部分中选择或取消选择组，以更新其组成员身份。
    -   对于**组**，你可以编辑组的名称。 （若要更新组成员身份，请编辑要添加或从组中删除的用户，并对**组成员身份**部分进行更改。）
    -   对于 **Azure AD 应用程序**，你可以输入**回复 URL** 或**应用 ID URI** 的新值。
    Remember that these changes will be made in your organization's directory as well as in your Partner Center account.
3.  For changes related to Partner Center access, select or deselect the role(s) that you want to apply, or select **Customize permissions** and make the desired changes. These changes only impact Partner Center access and will not change any permissions within your organization's Azure AD tenant.
3.  单击 **“保存”** 。


## <a name="view-history-for-account-users"></a>查看帐户用户的历史记录

作为帐户所有者，你可以查看已添加到帐户中的任何其他用户的详细浏览历史记录。

On the **Users** page (under **Account settings**), select the link shown under **Last activity** for the user whose browsing history you’d like to review. 你可以查看此用户在过去 30 天内访问过的所有网页的 URL。

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>删除用户、组和 Azure AD 应用程序

To remove a user, group, or Azure AD application from your Partner Center account, select the **Remove** link that appears by their name on the **Users** page. After confirming that you want to remove it, that user, group, or Azure AD application will no longer be able to access to your Partner Center account (unless you add it again later).

> [!IMPORTANT]
> Removing a user, group, or Azure AD application means that it will no longer have access to your Partner Center account. 它**不会**从你的组织的目录中删除用户、组或 Azure AD 应用程序。

 

