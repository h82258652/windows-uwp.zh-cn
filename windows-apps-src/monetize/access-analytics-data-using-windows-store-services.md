---
ms.assetid: 4BF9EF21-E9F0-49DB-81E4-062D6E68C8B1
description: 使用 Microsoft Store analytics API 以编程方式检索注册到你或你的组织的 Windows 合作伙伴中心帐户的应用的分析数据。
title: 使用应用商店服务访问分析数据
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 3b732da8f92c258647f905e6939dc3cb1b9c9f87
ms.sourcegitcommit: 0426013dc04ada3894dd41ea51ed646f9bb17f6d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2020
ms.locfileid: "78853354"
---
# <a name="access-analytics-data-using-store-services"></a>使用应用商店服务访问分析数据

使用*Microsoft Store ANALYTICS API*以编程方式检索注册到你或你的组织的 Windows 合作伙伴中心帐户的应用的分析数据。 此 API 使你可以针对应用和加载项（也称为应用内产品或 IAP）购置、错误、应用评分和评价检索数据。 此 API 使用 Azure Active Directory (Azure AD) 验证来自应用或服务的调用。

以下步骤介绍端到端过程：

1.  确保已完成所有[先决条件](#prerequisites)。
2.  在 Microsoft Store 分析 API 中调用某个方法之前，请先[获取 Azure AD 访问令牌](#obtain-an-azure-ad-access-token)。 获取访问令牌后，可以在 60 分钟的令牌有效期内，使用该令牌调用 Microsoft Store 分析 API。 该令牌到期后，可以重新生成一个。
3.  [调用 Microsoft Store 分析 API](#call-the-windows-store-analytics-api)。

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-analytics-api"></a>步骤 1：完成使用 Microsoft Store 分析 API 的先决条件

在开始编写调用 Microsoft Store 分析 API 的代码之前，确保已完成以下先决条件。

* 你（或你的组织）必须具有 Azure AD 目录，并且你必须具有该目录的[全局管理员](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)权限。 如果你已使用 Office 365 或 Microsoft 的其他业务服务，表示你已经具有 Azure AD 目录。 否则，你可以免费[在合作伙伴中心中创建新的 Azure AD](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)。

* 必须将 Azure AD 应用程序与合作伙伴中心帐户相关联，并检索应用程序的租户 ID 和客户端 ID，并生成一个密钥。 Azure AD 应用程序是指你想要从中调用 Microsoft Store 分析 API 的应用或服务。 需要租户 ID、客户端 ID 和密钥，才可以获取将传递给 API 的 Azure AD 访问令牌。
    > [!NOTE]
    > 你只需执行一次此任务。 获取租户 ID、客户端 ID 和密钥后，当你需要创建新的 Azure AD 访问令牌时，可以随时重复使用它们。

若要将 Azure AD 应用程序与合作伙伴中心帐户关联并检索所需的值：

1.  在合作伙伴中心，[将组织的合作伙伴中心帐户与组织的 Azure AD 目录相关联](../publish/associate-azure-ad-with-partner-center.md)。

2.  接下来，从合作伙伴中心的 "**帐户设置**" 部分的 "**用户**" 页中，添加表示将用于访问合作伙伴中心帐户分析数据的应用或服务的[Azure AD 应用程序](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account)。 请确保为此应用程序分配**管理员**角色。 如果 Azure AD 目录中尚不存在该应用程序，则可以[在合作伙伴中心创建新的 Azure AD 应用程序](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account)。

3.  返回到**用户**页面、单击 Azure AD 应用程序的名称以转到应用程序设置，然后记下**租户 ID** 和**客户端 ID** 值。

4. 单击**添加新密钥**。 在接下来的屏幕上，记下“密钥”值。 在离开此页面后，你将无法再访问该信息。 有关详细信息，请参阅[管理 Azure AD 应用程序的密钥](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)。

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>步骤 2：获取 Azure AD 访问令牌

在 Microsoft Store 分析 API 中调用任何方法之前，首先必须获取将传递给该 API 中每个方法的 **Authorization** 标头的 Azure AD 访问令牌。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以对它进行刷新，以便可以在之后调用该 API 时继续使用。

若要获取访问令牌，请按照 [使用客户端凭据的服务到服务调用](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) 中的说明将 HTTP POST 发送到 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 终结点。 示例请求如下所示。

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

对于 POST URI 中的*租户\_id*值和客户端 *\_id*和*客户端\_机密*参数，为你在上一节中的合作伙伴中心检索的应用程序指定租户 id、客户端 id 和密钥。 对于 *resource* 参数，必须指定 ```https://manage.devcenter.microsoft.com```。

在你的访问令牌到期后，可以按照[此处](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的说明刷新令牌。

<span id="call-the-windows-store-analytics-api" />

## <a name="step-3-call-the-microsoft-store-analytics-api"></a>步骤 3：调用 Microsoft Store 分析 API

获取 Azure AD 访问令牌后，可以随时调用 Microsoft Store 分析 API。 必须将访问令牌传递到每个方法的 **Authorization** 标头。

### <a name="methods-for-uwp-apps-and-games"></a>UWP 应用和游戏的方法
以下方法可用于应用和游戏收购和附加支持： 

* [获取你的游戏和应用的购置数据](acquisitions-data.md)
* [获取你的游戏和应用的附加项购置数据](add-on-acquisitions-data.md)

### <a name="methods-for-uwp-apps"></a>适用于 UWP 应用的方法 

以下分析方法适用于合作伙伴中心的 UWP 应用。

| 方案       | 方法      |
|---------------|--------------------|
| 购置、转换、安装和使用 |  <ul><li>[获取应用程序](get-app-acquisitions.md)（旧版）</li><li>[获取应用获取漏斗数据](get-acquisition-funnel-data.md)（旧版）</li><li>[按频道获取应用转换](get-app-conversions-by-channel.md)</li><li>[获取附加项购置](get-in-app-acquisitions.md)</li><li>[获取订阅附加项购置](get-subscription-acquisitions.md)</li><li>[按频道获取外接程序转换](get-add-on-conversions-by-channel.md)</li><li>[获取应用安装](get-app-installs.md)</li><li>[获取每日应用使用情况](get-app-usage-daily.md)</li><li>[获取每月应用使用情况](get-app-usage-monthly.md)</li></ul> |
| 应用错误 | <ul><li>[获取错误报告数据](get-error-reporting-data.md)</li><li>[获取应用中错误的详细信息](get-details-for-an-error-in-your-app.md)</li><li>[获取应用中错误的堆栈跟踪](get-the-stack-trace-for-an-error-in-your-app.md)</li><li>[下载应用中错误的 CAB 文件](download-the-cab-file-for-an-error-in-your-app.md)</li></ul> |
| 洞察 | <ul><li>[获取应用的 insights 数据](get-insights-data-for-your-app.md)</li></ul>  |
| 评分和评价 | <ul><li>[获取应用分级](get-app-ratings.md)</li><li>[获取应用评审](get-app-reviews.md)</li></ul> |
| 应用内广告和广告活动 | <ul><li>[获取 ad 性能数据](get-ad-performance-data.md)</li><li>[获取 ad 市场活动性能数据](get-ad-campaign-performance-data.md)</li></ul> |

### <a name="methods-for-desktop-applications"></a>适用于桌面应用程序的方法

属于 [Windows 桌面应用程序计划](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program)的开发人员帐户可以使用以下分析方法。

| 方案       | 方法      |
|---------------|--------------------|
| 安装次数 |  <ul><li>[获取桌面应用程序安装](get-desktop-app-installs.md)</li></ul> |
| Blocks |  <ul><li>[获取桌面应用程序的升级块](get-desktop-block-data.md)</li><li>[获取桌面应用程序的升级块详细信息](get-desktop-block-data-details.md)</li></ul> |
| 应用程序错误 |  <ul><li>[获取桌面应用程序的错误报告数据](get-desktop-application-error-reporting-data.md)</li><li>[获取桌面应用程序中错误的详细信息](get-details-for-an-error-in-your-desktop-application.md)</li><li>[获取桌面应用程序中的错误的堆栈跟踪](get-the-stack-trace-for-an-error-in-your-desktop-application.md)</li><li>[下载适用于桌面应用程序中的错误的 CAB 文件](download-the-cab-file-for-an-error-in-your-desktop-application.md)</li></ul> |
| 洞察 | <ul><li>[获取桌面应用程序的 insights 数据](get-insights-data-for-your-desktop-app.md)</li></ul>  |

### <a name="methods-for-xbox-live-services"></a>适用于 Xbox Live 服务的方法

使用 [Xbox Live 服务](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md)的游戏开发人员帐户可以使用以下其他方法。

| 方案       | 方法      |
|---------------|--------------------|
| 常规分析 |  <ul><li>[获取 Xbox 实时分析数据](get-xbox-live-analytics.md)</li><li>[获取 Xbox Live 成就数据](get-xbox-live-achievements-data.md)</li><li>[获取 Xbox Live 并发使用数据](get-xbox-live-concurrent-usage-data.md)</li></ul> |
| 运行状况分析 |  <ul><li>[获取 Xbox Live 运行状况数据](get-xbox-live-health-data.md)</li></ul> |
| 社区分析 |  <ul><li>[获取 Xbox Live 游戏中心数据](get-xbox-live-game-hub-data.md)</li><li>[获取 Xbox Live 俱乐部数据](get-xbox-live-club-data.md)</li><li>[获取 Xbox Live 多玩家数据](get-xbox-live-multiplayer-data.md)</li></ul>  |

### <a name="methods-for-hardware-and-drivers"></a>适用于硬件和驱动程序的方法

属于[Windows 硬件仪表板程序](https://docs.microsoft.com/windows-hardware/drivers/dashboard/get-started-with-the-hardware-dashboard)的开发人员帐户可以访问一组额外的方法来检索硬件和驱动程序的分析数据。 有关详细信息，请参阅[硬件仪表板 API](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-api)。

## <a name="code-example"></a>代码示例

以下代码示例演示如何获取 Azure AD 访问令牌以及如何从 C# 控制台应用调用 Microsoft Store 分析 API。 若要使用此代码示例，请将 *tenantId*、*clientId*、*clientSecret* 和 *appID* 变量分配给你的方案的相应值。 此示例需要 Newtonsoft 中的 [Json.NET 程序包](https://www.newtonsoft.com/json)，以便反序列化 Microsoft Store 分析 API 返回的 JSON 数据。

> [!div class="tabbedCodeSnippets"]
[!code-csharp[AnalyticsApi](./code/StoreServicesExamples_Analytics/cs/Program.cs#AnalyticsApiExample)]

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
