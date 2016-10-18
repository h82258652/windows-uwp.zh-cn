---
author: mcleanbyron
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: "使用 Windows 应用商店提交 API，以编程方式创建和管理已注册到 Windows 开发人员中心帐户的应用的提交。"
title: "使用 Windows 应用商店服务创建和管理提交"
translationtype: Human Translation
ms.sourcegitcommit: 47e0ac11178af98589e75cc562631c6904b40da4
ms.openlocfilehash: 0a566dfee8f7fe08c06ce4963435a70c30b1650d

---

# 使用 Windows 应用商店服务创建和管理提交


使用 *Windows 应用商店提交 API*，针对你的或组织的 Windows 开发人员中心帐户的应用、加载项（也称为应用内产品或 IAP）和软件包外部测试版，以编程方式查询和创建提交。 如果你的帐户管理多个应用或加载项，并且想要自动执行并优化这些资源的提交过程，此 API 非常有用。 此 API 使用 Azure Active Directory (Azure AD) 验证来自应用或服务的调用。

以下步骤介绍了使用 Windows 应用商店提交 API 的端到端过程：

1.  确保已完成所有[先决条件](#prerequisites)。
3.  在 Windows 应用商店提交 API 中调用某个方法之前，请先[获取 Azure AD 访问令牌](#obtain-an-azure-ad-access-token)。 获取访问令牌后，可以在 60 分钟的令牌有效期内，使用该令牌调用 Windows 应用商店提交 API。 该令牌到期后，可以重新生成一个。
4.  [调用 Windows 应用商店提交 API](#call-the-windows-store-submission-api)。



<span id="not_supported" />
>**重要提示**

> * 此 API 只可以用于已授权使用该 API 的 Windows 开发人员中心帐户。 会阶段性地向开发人员帐户启用此权限，但此时所有帐户并非都已启用了此权限。 若要请求先前的访问权限，请登录到开发人员中心仪表板、单击仪表板底部的“反馈”****、选择反馈区域的“提交 API”****，然后提交你的请求。 当为你的帐户启用了此权限时，你会收到一封电子邮件。

> * 此 API 不可用于使用以下功能的应用或加载项：2016 年 8 月引入开发人员中心仪表板的功能，包括但不限于强制应用更新和应用商店管理的可消费加载项。 如果将 Windows 应用商店提交 API 用于使用以下功能之一的应用或加载项，该 API 会返回错误代码 409。 在这种情况下，必须使用仪表板管理应用或加载项的提交。

<span id="prerequisites" />
## 步骤 1：完成使用 Windows 应用商店提交 API 的先决条件

在开始编写调用 Windows 应用商店提交 API 的代码之前，确保已完成以下先决条件。

* 你（或你的组织）必须具有 Azure AD 目录，并且你必须具有该目录的[全局管理员](http://go.microsoft.com/fwlink/?LinkId=746654)权限。 如果你已使用 Office 365 或 Microsoft 的其他业务服务，表示你已经具有 Azure AD 目录。 否则，你可以免费[在开发人员中心中创建新的 Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users)。

* 必须[将 Azure AD 应用程序与你的 Windows 开发人员中心帐户相关联](#associate-an-azure-ad-application-with-your-windows-dev-center-account)，获取租户 ID、客户端 ID 和密钥。 获取 Azure AD 访问令牌（该令牌用于调用 Windows 应用商店提交 API）需要这些值。

* 应用使用 Windows 应用商店提交 API 的准备工作：

  * 如果开发人员中心中尚不存在你的应用，请[在开发人员中心仪表板中创建应用](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name)。 无法在开发人员中心中使用 Windows 应用商店提交 API 创建应用；必须使用仪表板创建应用，然后才可以使用该 API 访问应用并以编程方式创建它的提交。 不过，可以使用该 API 以编程方式创建加载项和软件包外部测试版，然后再为它们创建提交。

  * 在可以使用此 API 为给定应用创建提交之前，首先必须[在开发人员中心仪表板为应用创建一个提交](https://msdn.microsoft.com/windows/uwp/publish/app-submissions)，其中包括回答[年龄分级](https://msdn.microsoft.com/windows/uwp/publish/age-ratings)问题。 完成此操作后，才可以使用该 API 为此应用以编程方式创建新的提交。 无需创建加载项提交或软件包外部测试版提交，即可将该 API 用于这些类型的提交。

  * 如果你要创建或更新应用提交并需要包括应用包，请事先[准备应用包](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements)。

  * 如果你要创建或更新应用提交并需要包括应用商店一览的屏幕截图或图像，请事先[准备应用屏幕截图和图像](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images)。

  * 如果你要创建或更新加载项提交并需要包括图标，请事先[准备图标](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon)。

<span id="associate-an-azure-ad-application-with-your-windows-dev-center-account" />
### 如何将 Azure AD 应用程序与你的 Windows 开发人员中心帐户相关联

在可以使用 Windows 应用商店提交 API 之前，必须将 Azure AD 应用程序与你的开发人员中心帐户相关联、检索该应用程序的租户 ID 和客户端 ID，然后生成一个密钥。 Azure AD 应用程序是指你想要从中调用 Windows 应用商店提交 API 的应用或服务。 需要租户 ID、客户端 ID 和密钥，才可以获取将传递给 API 的 Azure AD 访问令牌。

>**注意**&nbsp;&nbsp;只需执行一次此任务。 获取租户 ID、客户端 ID 和密钥后，当你需要创建新的 Azure AD 访问令牌时，可以随时重复使用它们。

1.  在开发人员中心中，转到“帐户设置”****、单击“管理用户”****，然后将你的组织的开发人员中心帐户与你的组织的 Azure AD 目录相关联。 有关详细说明，请参阅[管理帐户用户](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users)。

2.  在“管理用户”****页面中，单击“添加 Azure AD 应用程序”****、添加 Azure AD 应用程序（是指要用于访问你的开发人员中心帐户的提交的应用或服务），然后为其分配“管理者”****角色。 如果此应用程序已存在于你的 Azure AD 目录中，你可以在“添加 Azure AD 应用程序”****页面上选择它，以将其添加到你的开发人员中心帐户。 如果没有此应用程序，你可以在“添加 Azure AD 应用程序”****页面上创建新的 Azure AD 应用程序。 有关详细信息，请参阅[添加和管理 Azure AD 应用程序](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)。

3.  返回到“管理用户”****页面、单击 Azure AD 应用程序的名称以转到应用程序设置，然后记下“租户 ID”****和“客户端 ID”****值。

4. 单击“添加新密钥”****。 在接下来的屏幕上，记下“密钥”****值。 在离开此页面后，你将无法再访问该信息。 有关详细信息，请参阅[添加和管理 Azure AD 应用程序](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)中有关管理密钥的信息。

<span id="obtain-an-azure-ad-access-token" />
## 步骤 2：获取 Azure AD 访问令牌

在 Windows 应用商店提交 API 中调用任何方法之前，首先必须获取将传递给该 API 中每个方法的 **Authorization** 标头的 Azure AD 访问令牌。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以对它进行刷新，以便可以在之后调用该 API 时继续使用。

若要获取访问令牌，请按照[使用客户端凭据的服务到服务调用](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/)中的说明将 HTTP POST 发送到以下 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 终结点。 示例请求如下所示。

```
POST https://login.microsoftonline.com/<your_tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

对于 *tenant\_id*、*client\_id* 和 *client\_secret* 参数，请为从上一部分的开发人员中心中检索得到的应用程序指定租户 ID、客户端 ID 和密钥。 对于 *resource* 参数，必须指定 ```https://manage.devcenter.microsoft.com``` URI。

在你的访问令牌到期后，可以按照[此处](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的说明刷新令牌。

<span id="call-the-windows-store-submission-api">
## 步骤 3：使用 Windows 应用商店提交 API

获取 Azure AD 访问令牌后，可以在 Windows 应用商店提交 API 中调用方法。 API 中包含的许多方法按照所适用的应用、加载项和软件包外部测试版方案进行分组。 若要创建或更新提交，通常在 Windows 应用商店提交 API 中按特定顺序调用多个方法。 有关每个方案和每个方法的语法的信息，请参阅下表中的文章。

>**注意**&nbsp;&nbsp;获取访问令牌后，可以在 60 分钟的令牌有效期内，在 Windows 应用商店提交 API 中调用方法。

| 方案       | 描述                                                                 |
|---------------|----------------------------------------------------------------------|
| 应用 |  检索已注册到 Windows 开发人员中心帐户的所有应用的数据，然后创建这些应用的提交。 有关这些方法的详细信息，请参阅以下文章： <ul><li>[获取应用数据](get-app-data.md)</li><li>[管理应用提交](manage-app-submissions.md)</li></ul> |
| 加载项 | 获取、创建或删除应用的加载项，然后获取、创建或删除这些加载项的提交。 有关这些方法的详细信息，请参阅以下文章： <ul><li>[管理加载项](manage-add-ons.md)</li><li>[管理加载项提交](manage-add-on-submissions.md)</li></ul> |
| 软件包外部测试版 | 获取、创建或删除应用的软件包外部测试版，然后获取、创建或删除这些软件包外部测试版的提交。 有关这些方法的详细信息，请参阅以下文章： <ul><li>[管理软件包外部测试版](manage-flights.md)</li><li>[管理软件包外部测试版提交](manage-flight-submissions.md)</li></ul> |

<span />

## 代码示例

下文中提供了详细的代码示例，演示如何以不同的多种编程语言使用 Windows 应用商店提交 API。

* [C# 代码示例](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Java 代码示例](java-code-examples-for-the-windows-store-submission-api.md)
* [Python 代码示例](python-code-examples-for-the-windows-store-submission-api.md)

## 疑难解答

| 问题      | 解决方法                                          |
|---------------|---------------------------------------------|
| 在通过 PowerShell 调用 Windows 应用商店提交 API 后，如果使用 [ConvertFrom-Json](https://technet.microsoft.com/en-us/library/hh849898.aspx) cmdlet 将该 API 的响应数据从 JSON 格式转换为 PowerShell 对象，然后使用 [ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) cmdlet 将响应数据转换回为 JSON 格式，该响应数据会损坏。 |  默认情况下，[ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) cmdlet 的 *-Depth* 参数设置为 2 级对象，这对于大多数由 Windows 应用商店提交 API 返回的 JSON 对象而言深度不够。 调用 [ConvertTo-Json](https://technet.microsoft.com/en-us/library/hh849922.aspx) cmdlet 时，请将 *-Depth* 参数设置为较大数值（如 20）。 |

## 其他帮助

如果你对 Windows 应用商店提交 API 有疑问或需要使用此 API 管理你的提交的帮助，请使用以下资源：

* 在我们的[论坛](https://social.msdn.microsoft.com/Forums/windowsapps/en-us/home?forum=wpsubmit)上提问。
* 访问我们的[支持页面](https://developer.microsoft.com/windows/support)，请求一个开发人员中心仪表板的辅助支持选项。 如果系统提示你选择问题类型和类别，请分别选择“应用提交和认证”****和“提交应用”****。  

## 相关主题

* [获取应用数据](get-app-data.md)
* [管理应用提交](manage-app-submissions.md)
* [管理加载项](manage-add-ons.md)
* [管理加载项提交](manage-add-on-submissions.md)
* [管理软件包外部测试版](manage-flights.md)
* [管理软件包外部测试版提交](manage-flight-submissions.md)
 



<!--HONumber=Sep16_HO1-->


