---
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: 使用 Microsoft Store 促销 API 以编程方式管理应用注册到您或您组织的合作伙伴中心帐户促销广告市场活动。
title: 使用 Microsoft Store 服务开展广告活动
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 促销 API, 广告活动
ms.localizationpriority: medium
ms.openlocfilehash: 58325074a2f59dcd146a9534054b302b3ce9956b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57619142"
---
# <a name="run-ad-campaigns-using-store-services"></a>使用 Microsoft Store 服务开展广告活动

使用*API 的 Microsoft Store 促销*以编程方式管理应用注册到您或您组织的合作伙伴中心帐户促销广告市场活动。 你可借助此 API 创建、更新和监控你的活动和其他相关资产，如目标市场设定和创意。 此 API 是谁创建大量的市场活动，并希望为此，而无需使用合作伙伴中心的开发人员尤其有用。 此 API 使用 Azure Active Directory (Azure AD) 验证来自应用或服务的调用。

以下步骤介绍端到端过程：

1.  确保已完成所有[先决条件](#prerequisites)。
2.  在 Microsoft Store 促销 API 中调用某个方法之前，请先[获取 Azure AD 访问令牌](#obtain-an-azure-ad-access-token)。 获取访问令牌后，可以在 60 分钟的令牌有效期内，使用该令牌调用 Microsoft Store 促销 API。 该令牌到期后，可以重新生成一个。
3.  [调用 Microsoft Store 促销 API](#call-the-windows-store-promotions-api)。

或者，您可以创建和管理 ad 市场活动使用合作伙伴中心和以编程方式创建通过 API 还可以访问在合作伙伴中心的 Microsoft Store 提升任何广告市场活动。 有关管理合作伙伴中心中的广告市场活动的详细信息，请参阅[创建您的应用程序的广告活动](../publish/create-an-ad-campaign-for-your-app.md)。

> [!NOTE]
> 合作伙伴中心帐户与任何开发人员可以使用的 Microsoft Store 提升 API 来管理其应用的广告市场活动。 媒体机构还可以请求访问该 API 代表他们的广告商开展广告活动。 如果你是希望了解该 API 详情或请求其访问权限的媒体机构，请将请求发送至 storepromotionsapi@microsoft.com。

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-promotions-api"></a>第 1 步：完成使用 Microsoft Store 提升 API 的先决条件

在开始编写调用 Microsoft Store 促销 API 的代码之前，确保已完成以下先决条件。

* 成功创建并启动 ad 活动使用此 API 之前，你必须首先[创建一个 ad 付费营销活动使用**广告市场活动**在合作伙伴中心页](../publish/create-an-ad-campaign-for-your-app.md)，并且必须添加至少一个付款此页上的检测。 完成以上操作之后，你就能够使用此 API 为广告活动成功创建计费投放渠道。 用于创建使用此 API 的广告市场活动传递行会自动按上所选的默认付款方式**广告市场活动**合作伙伴中心中的页。

* 你（或你的组织）必须具有 Azure AD 目录，并且你必须具有该目录的[全局管理员](https://go.microsoft.com/fwlink/?LinkId=746654)权限。 如果你已使用 Office 365 或 Microsoft 的其他业务服务，表示你已经具有 Azure AD 目录。 否则，你可以免费[在合作伙伴中心中创建新的 Azure AD](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account)。

* 必须将 Azure AD 应用程序与你的合作伙伴中心帐户相关联、 检索租户 ID 和应用程序的客户端 ID 和生成密钥。 Azure AD 应用程序是指要从中调用 Microsoft Store 促销 API 的应用或服务。 需要租户 ID、客户端 ID 和密钥，才可以获取将传递给 API 的 Azure AD 访问令牌。
    > [!NOTE]
    > 你只需执行一次此任务。 获取租户 ID、客户端 ID 和密钥后，当你需要创建新的 Azure AD 访问令牌时，可以随时重复使用它们。

若要将 Azure AD 应用程序与你的合作伙伴中心帐户相关联，并检索所需的值：

1.  在合作伙伴中心[将组织的合作伙伴中心帐户与你组织的 Azure AD 目录相关联](../publish/associate-azure-ad-with-partner-center.md)。

2.  接下来，从**用户**页面**帐户设置**合作伙伴中心部分[添加 Azure AD 应用程序](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account)，表示应用或服务，你将使用管理合作伙伴中心帐户促销活动。 请确保为此应用程序分配**管理员**角色。 如果不存在该应用程序尚未在 Azure AD 目录，你可以[创建一个新 Azure AD 应用程序在合作伙伴中心](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account)。 

3.  返回到**用户**页面、单击 Azure AD 应用程序的名称以转到应用程序设置，然后记下**租户 ID** 和**客户端 ID** 值。

4. 单击**添加新密钥**。 在接下来的屏幕上，记下**密钥**值。 在离开此页面后，你将无法再访问该信息。 有关详细信息，请参阅[管理 Azure AD 应用程序的密钥](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys)。

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>步骤 2：获取 Azure AD 访问令牌

在 Microsoft Store 促销 API 中调用任何方法之前，首先必须获取将传递给该 API 中每个方法的 **Authorization** 标头的 Azure AD 访问令牌。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以对它进行刷新，以便可以在之后调用该 API 时继续使用。

若要获取访问令牌，请按照 [使用客户端凭据的服务到服务调用](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) 中的说明将 HTTP POST 发送到 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 终结点。 示例请求如下所示。

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

有关*租户\_id* POST URI 中的值和*客户端\_id*并*客户端\_机密*参数，指定的租户ID、 客户端 ID 和从上一节中的合作伙伴中心检索应用程序的密钥。 对于 *resource* 参数，必须指定 ```https://manage.devcenter.microsoft.com```。

在你的访问令牌到期后，可以按照[此处](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的说明刷新令牌。

<span id="call-the-windows-store-promotions-api" />

## <a name="step-3-call-the-microsoft-store-promotions-api"></a>步骤 3:调用 API 的 Microsoft Store 提升

获取 Azure AD 访问令牌后，可以随时调用 Microsoft Store 促销 API。 必须将访问令牌传递到每个方法的 **Authorization** 标头。

在 Microsoft Store 促销 API 的上下文中，广告活动涵盖了包含活动相关高级信息的*活动* 对象，以及代表广告活动*投放渠道*、*目标市场配置文件* 和*创意* 的其他对象。 API 包括按所述对象类型分组的不同方法。 通常需要针对每一对象调用不同 POST 方法，从而创建活动。 该 API 还提供了可用于检索任何对象的 GET 方法，以及可用于编辑活动、投放渠道和目标市场配置文件对象的 PUT 方法。

有关这些对象及其相关方法的详细信息，请参阅下表。


| 对象       | 描述   |
|---------------|-----------------|
| 活动 |  此对象代表广告活动，位于广告活动对象模型层次结构的顶部。 此对象标识了你目前开展的活动类型（付费广告、自家广告或社区广告）、活动目标、活动的投放渠道及其他详细信息。 每项活动可能仅与一个应用相关联。<br/><br/>有关此对象相关方法的详细信息，请参阅[管理广告活动](manage-ad-campaigns.md)。<br/><br/>**注意**&nbsp;&nbsp;创建广告活动后，可以通过使用 [Microsoft Store 分析 API](access-analytics-data-using-windows-store-services.md) 中的[获取广告活动性能数据](get-ad-campaign-performance-data.md)方法检索广告活动的性能数据。  |
| 投放渠道 | 每项广告活动具有一个或多个投放渠道，用于购买存货和投放广告。 你可以针对每个投放渠道设定目标市场、标价，并通过设定预算及与你希望启用的创意连接来判断你的预期费用。<br/><br/>有关此对象相关方法的详细信息，请参阅[管理广告活动的投放渠道](manage-delivery-lines-for-ad-campaigns.md)。 |
| 目标市场定位配置文件 | 每个投放渠道都有一份目标市场定位配置文件，指明了你设定的用户、地区和库存类型目标。 可创建目标市场配置文件作为模板，并在各投放渠道之间共享。<br/><br/>有关此对象相关方法的详细信息，请参阅[管理广告活动的目标市场配置文件](manage-targeting-profiles-for-ad-campaigns.md)。 |
| 创意 | 每个投放渠道都有一个或多个创意，作为广告活动的一部分向客户展示广告。 如果某一创意始终代表同一应用，则其可能与一个投放渠道或各广告活动间的多个投放渠道相关。<br/><br/>有关此对象相关方法的详细信息，请参阅[管理广告活动的创意](manage-creatives-for-ad-campaigns.md)。 |


下图说明了活动、投放渠道、目标市场配置文件和创意之间的关系。

![广告活动层次结构](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>代码示例

以下代码示例演示如何获取 Azure AD 访问令牌以及如何从 C# 控制台应用调用 Microsoft Store 促销 API。 若要使用此代码示例，请将 *tenantId*、*clientId*、*clientSecret* 和 *appID* 变量分配给你的方案的相应值。 此示例需要 Newtonsoft 中的 [Json.NET 程序包](https://www.newtonsoft.com/json)，以便反序列化 Microsoft Store 促销 API 返回的 JSON 数据。

[!code-cs[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>相关主题

* [管理广告市场活动](manage-ad-campaigns.md)
* [管理 ad 市场活动传递行](manage-delivery-lines-for-ad-campaigns.md)
* [管理 ad 市场活动的目标配置文件](manage-targeting-profiles-for-ad-campaigns.md)
* [管理 creatives ad 市场活动](manage-creatives-for-ad-campaigns.md)
* [获取 ad 市场活动的性能数据](get-ad-campaign-performance-data.md)


 
