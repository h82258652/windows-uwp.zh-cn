---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: 使用 Microsoft Store 分析 API 以编程方式检索分析数据的应用已注册到你的或组织的 ' s Windows 合作伙伴中心帐户。
title: 使用应用商店服务访问分析数据
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0f0df87f8ed5339c977dbd468f8aa2a7877f0d9e
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8799314"
---
# <a name="access-analytics-data-using-store-services"></a>使用应用商店服务访问分析数据

使用*Microsoft Store 分析 API*以编程方式检索注册到你的或组织的 Windows 的合作伙伴中心帐户的应用的分析数据。 此 API 使你可以针对应用和加载项（也称为应用内产品或 IAP）购置、错误、应用评分和评价检索数据。 此 API 使用 Azure Active Directory (Azure AD) 验证来自应用或服务的调用。

以下步骤介绍端到端过程：

1.  确保已完成所有[先决条件](#prerequisites)。
2.  在 Microsoft Store 分析 API 中调用某个方法之前，请先[获取 Azure AD 访问令牌](#obtain-an-azure-ad-access-token)。 获取访问令牌后，可以在 60 分钟的令牌有效期内，使用该令牌调用 Microsoft Store 分析 API。 该令牌到期后，可以重新生成一个。
3.  [调用 Microsoft Store 分析 API](#call-the-windows-store-analytics-api)。

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>步骤 1：完成使用 Microsoft Store 分析 API 的先决条件

在开始编写调用 Microsoft Store 分析 API 的代码之前，确保已完成以下先决条件。

* 你（或你的组织）必须具有 Azure AD 目录，并且你必须具有该目录的[全局管理员](http://go.microsoft.com/fwlink/?LinkId=746654)权限。 如果你已使用 Office 365 或 Microsoft 的其他业务服务，表示你已经具有 Azure AD 目录。 否则，你可以为任何附加费用[创建合作伙伴中心中的新 Azure AD](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) 。

* 你必须将 Azure AD 应用程序与你的合作伙伴中心帐户相关联、 检索租户 ID 和应用程序的客户端 ID 并生成一个密钥。 Azure AD 应用程序是指你想要从中调用 Microsoft Store 分析 API 的应用或服务。 需要租户 ID、客户端 ID 和密钥，才可以获取将传递给 API 的 Azure AD 访问令牌。
    > [!NOTE]
    > 你只需执行一次此任务。 获取租户 ID、客户端 ID 和密钥后，当你需要创建新的 Azure AD 访问令牌时，可以随时重复使用它们。

若要将 Azure AD 应用程序与你的合作伙伴中心帐户相关联并检索所需的值：

1.  在合作伙伴中心，[将你的组织的合作伙伴中心帐户与你的组织的 Azure AD 目录相关联](../publish/associate-azure-ad-with-partner-center.md)。

2.  接下来，从合作伙伴中心、[添加 Azure AD 应用程序](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account)表示应用或服务并且将用于访问你的合作伙伴中心帐户的分析数据的**帐户设置**部分中的**用户**页。 请确保为此应用程序分配**管理员**角色。 如果应用程序不存在，但在你的 Azure AD 目录，你可以[创建一个新合作伙伴中心中的 Azure AD 应用程序](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account)。

3.  返回到**用户**页面、单击 Azure AD 应用程序的名称以转到应用程序设置，然后记下**租户 ID** 和**客户端 ID** 值。

4. 单击**添加新密钥**。 在接下来的屏幕上，记下**密钥**值。 在离开此页面后，你将无法再访问该信息。 有关详细信息，请参阅[管理 Azure AD 应用程序的密钥](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)。

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>步骤 2：获取 Azure AD 访问令牌

在 Microsoft Store 分析 API 中调用任何方法之前，首先必须获取将传递给该 API 中每个方法的 **Authorization** 标头的 Azure AD 访问令牌。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以对它进行刷新，以便可以在之后调用该 API 时继续使用。

若要获取访问令牌，请按照 [使用客户端凭据的服务到服务调用](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) 中的说明将 HTTP POST 发送到 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 终结点。 示例请求如下所示。

```
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

对于 POST URI 中的*client\_id*和*client\_secret*参数*tenant\_id*值，指定的租户 ID、 客户端 ID 和你从上一部分中的合作伙伴中心中检索的应用程序的密钥。 对于 *resource* 参数，必须指定 ```https://manage.devcenter.microsoft.com```。

在你的访问令牌到期后，可以按照[此处](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的说明刷新令牌。

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>步骤 3：调用 Microsoft Store 分析 API

获取 Azure AD 访问令牌后，可以随时调用 Microsoft Store 分析 API。 你必须将访问令牌传递到每个方法的 **Authorization** 标头。

### <a name="methods-for-uwp-apps"></a>适用于 UWP 应用的方法

适用于 UWP 应用在合作伙伴中心中提供以下分析方法。

| 方案       | 方法      |
|---------------|--------------------|
| 购置、 转换、 安装和使用情况 |  <ul><li>[获取应用购置](get-app-acquisitions.md)</li><li>[获取应用购置漏斗数据](get-acquisition-funnel-data.md)</li><li>[通过通道获取应用转换](get-app-conversions-by-channel.md)</li><li>[获取加载项购置](get-in-app-acquisitions.md)</li><li>[获取订阅加载项购置](get-subscription-acquisitions.md)</li><li>[通过通道获取加载项转换](get-add-on-conversions-by-channel.md)</li><li>[获取应用安装](get-app-installs.md)</li><li>[获取每日应用使用情况](get-app-usage-daily.md)</li><li>[获取每月的应用使用情况](get-app-usage-monthly.md)</li></ul> |
| 应用错误 | <ul><li>[获取错误报告数据](get-error-reporting-data.md)</li><li>[获取应用中的错误的详细信息](get-details-for-an-error-in-your-app.md)</li><li>[获取应用中的错误的堆栈跟踪](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[下载应用中错误的 CAB 文件](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| Insights | <ul><li>[获取应用的见解数据](get-insights-data-for-your-app.md)</li></ul>  |
| 评分和评价 | <ul><li>[获取应用评分](get-app-ratings.md)</li><li>[获取应用评价](get-app-reviews.md)</li></ul> |
| 应用内广告和广告活动 | <ul><li>[获取广告性能数据](get-ad-performance-data.md)</li><li>[获取广告活动性能数据](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>适用于桌面应用程序的方法

属于 [Windows 桌面应用程序计划](https://msdn.microsoft.com/library/windows/desktop/mt826504)的开发人员帐户可以使用以下分析方法。

| 方案       | 方法      |
|---------------|--------------------|
| 安装次数 |  <ul><li>[获取桌面应用程序安装](get-desktop-app-installs.md)</li></ul> |
| 块 |  <ul><li>[获取桌面应用程序的升级块](get-desktop-block-data.md)</li><li>[获取桌面应用程序的升级块详情](get-desktop-block-data-details.md)</li></ul> |
| 应用程序错误 |  <ul><li>[获取桌面应用程序的错误报告数据](get-desktop-application-error-reporting-data.md)</li><li>[获取桌面应用程序中的错误的详细信息](get-details-for-an-error-in-your-desktop-application.md)</li><li>[获取桌面应用程序中的错误的堆栈跟踪](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[下载桌面应用程序中错误的 CAB 文件](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| Insights | <ul><li>[获取桌面应用程序的见解数据](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>适用于 Xbox Live 服务的方法

使用 [Xbox Live 服务](../xbox-live/developer-program-overview.md)的游戏开发人员帐户可以使用以下其他方法。

| 方案       | 方法      |
|---------------|--------------------|
| 常规分析 |  <ul><li>[获取 Xbox Live 分析数据](get-xbox-live-analytics.md)</li><li>[获取 Xbox Live 成就数据](get-xbox-live-achievements-data.md)</li><li>[获取 Xbox Live 并发使用情况数据](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| 运行状况分析 |  <ul><li>[获取 Xbox Live 运行状况数据](get-xbox-live-health-data.md)</li></ul> |
| 社区分析 |  <ul><li>[获取 Xbox Live 游戏中心数据](get-xbox-live-game-hub-data.md)</li><li>[获取 Xbox Live 中心数据](get-xbox-live-club-data.md)</li><li>[获取 Xbox Live 多人游戏数据](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-xbox-one-games"></a>适用于 Xbox One 游戏的方法

以下其他方法是在 XDP 分析仪表板中提供，可供通过 Xbox 开发人员门户 (XDP) 引入的 Xbox One 游戏的开发人员帐户使用。

| 方案       | 方法      |
|---------------|--------------------|
| 购置 |  <ul><li>[获取 Xbox One 游戏购置](get-xbox-one-game-acquisitions.md)</li><li>[获取 Xbox One 的加载项购置](get-xbox-one-add-on-acquisitions.md)</li></ul> |
| 错误 |  <ul><li>[获取错误报告数据在 Xbox One 游戏](get-error-reporting-data-for-your-xbox-one-game.md)</li><li>[获取游戏在 Xbox One 中的错误的详细信息](get-details-for-an-error-in-your-xbox-one-game.md)</li><li>[获取你的 Xbox One 中的错误的堆栈跟踪游戏](get-the-stack-trace-for-an-error-in-your-xbox-one-game.md)</li><li>[下载你的 Xbox One 游戏中的错误的 CAB 文件](download-the-cab-file-for-an-error-in-your-xbox-one-game.md)</li></ul> |

### <a name="methods-for-hardware-and-drivers"></a>适用于硬件和驱动程序的方法

属于[Windows 硬件仪表板计划](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard)的开发人员帐户有权访问一组额外的方法来检索硬件和驱动程序的分析数据。 有关详细信息，请参阅[硬件仪表板 API](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api)。

## <a name="code-example"></a>代码示例

以下代码示例演示如何获取 Azure AD 访问令牌以及如何从 C# 控制台应用调用 Microsoft Store 分析 API。 若要使用此代码示例，请将 *tenantId*、*clientId*、*clientSecret* 和 *appID* 变量分配给你的方案的相应值。 此示例需要 Newtonsoft 中的 [Json.NET 程序包](http://www.newtonsoft.com/json)，以便反序列化 Microsoft Store 分析 API 返回的 JSON 数据。

> [!div class="tabbedCodeSnippets"]
[!code-cs[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

## <a name="error-responses"></a>错误响应

Microsoft Store 分析 API 在 JSON 对象中返回含有错误代码和消息的错误响应。 以下示例演示了由无效参数引起的错误响应。

```json
{
    "code":"BadRequest",
    "data":[],
    "details":[],
    "innererror":{
        "code":"InvalidQueryParameters",
        "data":[
            "top parameter cannot be more than 10000"
        ],
        "details":[],
        "message":"One or More Query Parameters has invalid values.",
        "source":"AnalyticsAPI"
    },
    "message":"The calling client sent a bad request to the service.",
    "source":"AnalyticsAPI"
}
```
