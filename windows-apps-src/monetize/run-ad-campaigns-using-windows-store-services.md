---
author: mcleanbyron
ms.assetid: 8e6c3d3d-0120-40f4-9f90-0b0518188a1a
description: "使用 Windows 应用商店促销 API，针对已注册到你的或组织的 Windows 开发人员中心帐户的应用以编程方式管理促销广告活动。"
title: "使用 Windows 应用商店服务开展广告活动"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 应用商店促销 API, 广告活动"
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: ec245f07098a662c80517de49ba5637a69b30f35
ms.lasthandoff: 02/08/2017

---

# <a name="run-ad-campaigns-using-windows-store-services"></a>使用 Windows 应用商店服务开展广告活动

使用 *Windows 应用商店促销 API*，针对已注册到你的或组织的 Windows 开发人员中心帐户的应用以编程方式管理促销广告活动。 你可借助该 API 创建、更新和监控你的活动和其他相关资产，如目标市场设定和创意。 该 API 尤其适用于创建了大量活动的开发人员，及希望不借助 Windows 开发人员中心仪表板创建活动的开发人员。 该 API 使用 Azure Active Directory (Azure AD) 验证来自应用或服务的调用。

以下步骤介绍端到端过程：

1.  确保已完成所有[先决条件](#prerequisites)。
2.  在 Windows 应用商店促销 API 中调用某个方法之前，请先[获取 Azure AD 访问令牌](#obtain-an-azure-ad-access-token)。 获取访问令牌后，可以在 60 分钟的令牌有效期内，使用该令牌调用 Windows 应用商店促销 API。 该令牌到期后，可以重新生成一个。
3.  [调用 Windows 应用商店促销 API](#call-the-windows-store-promotions-api)。

或者，你可以使用 Windows 开发人员中心仪表板创建和管理广告活动，也可以在仪表板中访问通过 Windows 应用商店促销 API 以编程方式创建的任何广告活动。 有关在仪表板中管理广告活动的详细信息，请参阅[为你的应用创建广告活动](../publish/create-an-ad-campaign-for-your-app.md)。

>**注意**&nbsp;&nbsp;任何拥有 Windows 开发人员中心帐户的开发人员均可使用 Windows 应用商店促销 API 管理其应用的广告活动。 媒体机构还可以请求访问该 API 代表他们的广告商开展广告活动。 如果你是希望了解该 API 详情或请求其访问权限的媒体机构，请将请求发送至 storepromotionsapi@microsoft.com。

<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-promotions-api"></a>步骤 1：完成使用 Windows 应用商店促销 API 的先决条件

在开始编写调用 Windows 应用商店促销 API 的代码之前，确保已完成以下先决条件。

* 你（或你的组织）必须具有 Azure AD 目录，并且你必须具有该目录的[全局管理员](http://go.microsoft.com/fwlink/?LinkId=746654)权限。 如果你已使用 Office 365 或 Microsoft 的其他业务服务，表示你已经具有 Azure AD 目录。 否则，你可以免费[在开发人员中心中创建新的 Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users)。

* 你必须将 Azure AD 应用程序与你的开发人员中心帐户相关联、检索租户 ID 和应用程序的客户端 ID 并生成密钥。 Azure AD 应用程序是指你想要从中调用 Windows 应用商店促销 API 的应用或服务。 需要租户 ID、客户端 ID 和密钥，才可以获取将传递给 API 的 Azure AD 访问令牌。

  >**注意**&nbsp;&nbsp;只需执行一次此任务。 获取租户 ID、客户端 ID 和密钥后，当你需要创建新的 Azure AD 访问令牌时，可以随时重复使用它们。

若要将 Azure AD 应用程序与你的开发人员中心帐户相关联并检索所需值：

1.  在开发人员中心中，转到**帐户设置**、单击**管理用户**，然后将你的组织的开发人员中心帐户与你的组织的 Azure AD 目录相关联。 有关详细说明，请参阅[管理帐户用户](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users)。

2.  在**管理用户**页面中，单击**添加 Azure AD 应用程序**、添加 Azure AD 应用程序以代表将用于管理开发人员中心帐户中促销活动的应用或服务，然后为其分配**管理者**角色。 如果此应用程序已存在于你的 Azure AD 目录中，你可以在**添加 Azure AD 应用程序**页面上选择它，以将其添加到你的开发人员中心帐户。 如果没有此应用程序，你可以在**添加 Azure AD 应用程序**页面上创建新的 Azure AD 应用程序。 有关详细信息，请参阅[添加和管理 Azure AD 应用程序](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)。

3.  返回到**管理用户**页面、单击 Azure AD 应用程序的名称以转到应用程序设置，然后记下**租户 ID** 和**客户端 ID** 值。

4. 单击**添加新密钥**。 在接下来的屏幕上，记下**密钥**值。 在离开此页面后，你将无法再访问该信息。 有关详细信息，请参阅[添加和管理 Azure AD 应用程序](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications)中有关管理密钥的信息。

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>步骤 2：获取 Azure AD 访问令牌

在 Windows 应用商店促销 API 中调用任何方法之前，首先必须获取将传递给该 API 中每个方法的 **Authorization** 标头的 Azure AD 访问令牌。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以对它进行刷新，以便可以在之后调用该 API 时继续使用。

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

对于 POST URI 中的 *tenant\_id*、*client\_id* 和 *client\_secret* 参数，请为从上一部分的开发人员中心中检索到的应用程序指定租户 ID、客户端 ID 和密钥。 对于 *resource* 参数，必须指定 ```https://manage.devcenter.microsoft.com```。

在你的访问令牌到期后，可以按照[此处](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的说明刷新令牌。

<span id="call-the-windows-store-promotions-api" />
## <a name="step-3-call-the-windows-store-promotions-api"></a>步骤 3：调用 Windows 应用商店促销 API

获取 Azure AD 访问令牌后，可以随时调用 Windows 应用商店促销 API。 必须将访问令牌传递到每个方法的 **Authorization** 标头。

在 Windows 应用商店促销 API 的上下文中，广告活动涵盖了包含活动相关高级信息的*活动*对象，以及代表广告活动*投放渠道*、*目标市场配置文件*和*创意*的其他对象。 API 包括按所述对象类型分组的不同方法。 通常需要针对每一对象调用不同 POST 方法，从而创建活动。 该 API 还提供了可用于检索任何对象的 GET 方法，以及可用于编辑活动、投放渠道和目标市场配置文件对象的 PUT 方法。

有关这些对象及其相关方法的详细信息，请参阅下表。


| 对象       | 说明   |
|---------------|-----------------|
| 活动 |  此对象代表广告活动，位于广告活动对象模型层次结构的顶部。 此对象标识了你目前开展的活动类型（付费广告、自家广告或社区广告）、活动目标、活动的投放渠道及其他详细信息。 每项活动可能仅与一个应用相关联。<br/><br/>有关此对象相关方法的详细信息，请参阅[管理广告活动](manage-ad-campaigns.md)。<br/><br/>**注意**&nbsp;&nbsp;创建广告活动后，可以通过使用 [Windows 应用商店分析 API](access-analytics-data-using-windows-store-services.md) 中的[获取广告活动性能数据](get-ad-campaign-performance-data.md)方法检索广告活动的性能数据。  |
| 投放渠道 | 每项广告活动具有一个或多个投放渠道，用于购买存货和投放广告。 可以针对每个投放渠道设定目标市场、进行标价，并通过设定预算及与希望启用的创意连接来决定预期费用。<br/><br/>有关此对象相关方法的详细信息，请参阅[管理广告活动的投放渠道](manage-delivery-lines-for-ad-campaigns.md)。 |
| 目标市场定位配置文件 | 每个投放渠道都有一份目标市场定位配置文件，指明了你设定的用户、地区和库存类型目标。 可创建目标市场配置文件作为模板，并在各投放渠道之间共享。<br/><br/>有关此对象相关方法的详细信息，请参阅[管理广告活动的目标市场配置文件](manage-targeting-profiles-for-ad-campaigns.md)。 |
| 创意 | 每个投放渠道都有一个或多个创意，作为广告活动的一部分向客户展示广告。 如果某一创意始终代表同一应用，则其可能与一个投放渠道或各广告活动间的多个投放渠道相关。<br/><br/>有关此对象相关方法的详细信息，请参阅[管理广告活动的创意](manage-creatives-for-ad-campaigns.md)。 |


下图说明了活动、投放渠道、目标市场配置文件和创意之间的关系。

![广告活动层次结构](images/ad-campaign-hierarchy.png)

## <a name="code-example"></a>代码示例

以下代码示例演示了如何获取 Azure AD 访问令牌以及如何从 C# 控制台应用调用 Windows 应用商店促销 API。 若要使用此代码示例，请将 *tenantId*、*clientId*、*clientSecret* 和 *appID* 变量分配给你的方案的相应值。 此示例需要 Newtonsoft 中的 [Json.NET 程序包](http://www.newtonsoft.com/json)，以便反序列化 Windows 应用商店促销 API 返回的 JSON 数据。

> [!div class="tabbedCodeSnippets"]
[!code-cs[PromotionsApi](./code/StoreServicesExamples_Promotions/cs/Program.cs#PromotionsApiExample)]

## <a name="related-topics"></a>相关主题

* [管理广告活动](manage-ad-campaigns.md)
* [管理广告活动的投放渠道](manage-delivery-lines-for-ad-campaigns.md)
* [管理广告活动的目标市场配置文件](manage-targeting-profiles-for-ad-campaigns.md)
* [管理广告活动的创意](manage-creatives-for-ad-campaigns.md)
* [获取广告活动性能数据](get-ad-campaign-performance-data.md)


 

