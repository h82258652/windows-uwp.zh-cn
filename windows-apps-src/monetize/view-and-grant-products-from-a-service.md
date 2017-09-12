---
author: mcleanbyron
ms.assetid: B071F6BC-49D3-4E74-98EA-0461A1A55EFB
description: "如果你有应用和加载项的目录，你可以使用 Windows 应用商店收集 API 和 Windows 应用商店购买 API 访问你的服务中的这些产品的所有权信息。"
title: "管理服务中的产品权益"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, Windows 应用商店收集 API, Windows 应用商店购买 API, 查看产品, 授予产品"
ms.openlocfilehash: 6ecc9d6014692cac52f5554f78a0773dfee3fb81
ms.sourcegitcommit: e7e8de39e963b73ba95cb34d8049e35e8d5eca61
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/16/2017
---
# <a name="manage-product-entitlements-from-a-service"></a>管理服务中的产品权益

如果你有应用和加载项的目录，你可以使用 *Windows 应用商店收集 API* 和 *Windows 应用商店购买 API* 访问你的服务中的这些产品的权益信息。 *权益*表示客户使用通过 Windows 应用商店发布的应用或加载项的权利。

这些 API 由 REST 方法组合而成，旨在供开发人员用于跨平台服务支持的加载项目录。 这些 API 支持你执行以下操作：

-   Windows 应用商店收集 API：[查询用户拥有的产品](query-for-products.md)和[将可消费产品报告为已完成](report-consumable-products-as-fulfilled.md)。
-   Windows 应用商店购买 API：[向用户授予免费产品](grant-free-products.md)、[获取用户订阅](get-subscriptions-for-a-user.md)和[更改用户订阅的计费状态](change-the-billing-state-of-a-subscription-for-a-user.md)。

> [!NOTE]
> Windows 应用商店收集 API 和购买 API 使用 Azure Active Directory (Azure AD) 身份验证访问客户所有权信息。 要使用这些 API，你（或你的组织）必须具有 Azure AD 目录，并且你必须具有该目录的[全局管理员](http://go.microsoft.com/fwlink/?LinkId=746654)权限。 如果你已使用 Office 365 或 Microsoft 的其他业务服务，表示你已经具有 Azure AD 目录。

## <a name="overview"></a>概述

以下步骤介绍了使用 Windows 应用商店收集 API 和购买 API 的端到端过程：

1.  [在 Azure AD 中配置 Web 应用程序](#step-1)。
2.  [在 Windows 开发人员中心仪表板中将你的 Azure AD 客户端 ID 与应用程序相关联](#step-2)。
3.  在你的服务中，[创建 Azure AD 访问令牌](#step-3)，这些令牌表示你的发布者标识。
4.  在 Windows 应用的客户端代码中，[创建 Windows 应用商店 ID 密钥](#step-4)（表示当前用户的标识），并将 Windows 应用商店 ID 密钥传递回你的服务。
5.  在你具有所需的 Azure AD 访问令牌和 Windows 应用商店 ID 密钥后，[从你的服务调用 Windows 应用商店收集 API 或购买 API](#step-5)。

以下部分提供有关其中每个步骤的更多详细信息。

<span id="step-1"/>
## <a name="step-1-configure-a-web-application-in-azure-ad"></a>步骤 1：在 Azure AD 中配置 Web 应用程序

你必须先创建 Azure AD Web 应用程序，检索应用程序的租户 ID 和客户端 ID 并生成一个密钥，然后才能使用 Windows 应用商店收集 API 或购买 API。 Azure AD 应用程序是指你想要从中调用 Windows 应用商店收集 API 或购买 API 的应用或服务。 你需要租户 ID、客户端 ID 和密钥以获取传递给 API 的 Azure AD 访问令牌。

> [!NOTE]
> 你只需执行一次本部分中任务。 在更新 Azure AD 应用程序清单和获得租户 ID、客户端 ID 和客户端密钥后，你可以随时重复使用这些值来创建新的 Azure AD 访问令牌。

1.  按照[将应用程序与 Azure Active Directory 集成](http://go.microsoft.com/fwlink/?LinkId=722502)中的说明将 Web 应用程序添加到 Azure AD。
    > [!NOTE]
    > 在**向我们说明你的应用程序页**上，确保你选择 **Web 应用程序和/或 Web API**。 这是必需的，以便你可以为你的应用程序检索密钥（也称为*客户端密码*）。 若要调用 Windows 应用商店收集 API 或购买 API，必须在稍后步骤从 Azure AD 中请求访问令牌时提供客户端密码。

2.  在 [Azure 管理门户](http://manage.windowsazure.com/)中，导航到 **Active Directory**。 选择你的目录、单击顶部的**应用程序**选项卡，然后选择你的应用程序。
3.  单击**配置**选项卡。 在此选项卡上，为你的应用程序获取客户端 ID 并请求密钥（这在稍后的步骤中称为*客户端密码*）。
4.  在屏幕底部，单击**管理清单**。 下载你的 Azure AD 应用程序清单并使用以下文本替换 `"identifierUris"` 部分。

    ```json
    "identifierUris" : [                                
            "https://onestore.microsoft.com",
            "https://onestore.microsoft.com/b2b/keys/create/collections",
            "https://onestore.microsoft.com/b2b/keys/create/purchase"
        ],
    ```

    这些字符串表示你的应用程序支持的受众。 在稍后的步骤中，你将创建与其中每个受众值关联的 Azure AD 访问令牌。 有关如何下载应用程序清单的详细信息，请参阅[了解 Azure Active Directory 应用程序清单](http://go.microsoft.com/fwlink/?LinkId=722500)。

5.  保存你的应用程序清单，并在 [Azure 管理门户](http://manage.windowsazure.com/)中将其上传到你的应用程序。

<span id="step-2"/>
## <a name="step-2-associate-your-azure-ad-client-id-with-your-app-in-windows-dev-center"></a>步骤 2：在 Windows 开发人员中心中将你的 Azure AD 客户端 ID 与应用相关联

你必须先在开发人员中心仪表板中将 Azure AD 客户端 ID 与此应用（或者包含加载项的应用）关联，然后才能使用 Windows 应用商店收集 API 或购买 API 以在应用或加载项上操作。

> [!NOTE]
> 你只需执行一次此任务。

1.  登录[开发人员中心仪表板](https://dev.windows.com/overview)并选择你的应用。
2.  转到**服务**&gt;**产品收集和购买**页并将你的 Azure AD 客户端 ID 输入到可用字段之一。

<span id="step-3"/>
## <a name="step-3-create-azure-ad-access-tokens"></a>第 3 步：创建 Azure AD 访问令牌

你的服务必须先创建几个不同的表示你的发布者标识的 Azure AD 访问令牌，然后你才能检索 Windows 应用商店 ID 密钥或调用 Windows 应用商店收集 API 或购买 API。 每个令牌将与不同的 API 一起使用。 每个令牌的生命周期为 60 分钟，你可以在它们到期后进行刷新。

> [!IMPORTANT]
> 仅在服务的上下文而非应用中创建 Azure AD 访问令牌。 客户端密码在发送到你的应用时可能会遭泄露。

<span id="access-tokens" />
### <a name="understanding-the-different-tokens-and-audience-uris"></a>了解不同的令牌和受众 URI

根据你希望在 Windows 应用商店收集 API 或购买 API 中调用的方法，你必须创建两个或三个不同的令牌。 每个访问令牌都与不同的受众 URI（即你之前添加到 Azure AD 应用程序清单的 `"identifierUris"` 部分的 URI）关联。

  * 在所有情况下，你都必须使用 `https://onestore.microsoft.com` 受众 URI 创建令牌。 在稍后的步骤中，你要将此令牌传递到 Windows 应用商店收集 API 或购买 API 中的方法的**授权**标题。
      > [!IMPORTANT]
      > 将 `https://onestore.microsoft.com` 受众仅与安全存储在服务中的访问令牌一起使用。 在服务之外公开访问令牌和此受众会让你的服务易受到重播攻击。

  * 如果你想要在 Windows 应用商店收集 API 中调用某个方法以[查询用户拥有的产品](query-for-products.md)或[将可消费产品报告为已完成](report-consumable-products-as-fulfilled.md)，则还必须使用 `https://onestore.microsoft.com/b2b/keys/create/collections` 受众 URI 创建令牌。 在稍后的步骤中，你要将此令牌传递到 Windows SDK 中的客户端方法，以请求可与 Windows 应用商店收集 API 一起使用的 Windows 应用商店 ID 密钥。

  * 如果你想要调用 Windows 应用商店购买 API 中的方法来[向用户授予免费产品](grant-free-products.md)、[获取用户订阅](get-subscriptions-for-a-user.md)或[更改用户订阅的计费状态](change-the-billing-state-of-a-subscription-for-a-user.md)，则必须使用 `https://onestore.microsoft.com/b2b/keys/create/purchase` 受众 URI 创建一个令牌。 在稍后的步骤中，你要将此令牌传递到 Windows SDK 中的客户端方法，以请求可与 Windows 应用商店购买 API 一起使用的 Windows 应用商店 ID 密钥。

<span />
### <a name="create-the-tokens"></a>创建令牌

若要创建访问令牌，请按照[使用客户端凭据的服务到服务调用](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service)中的说明在服务中使用 OAuth 2.0 API，以便将 HTTP POST 发送到 ```https://login.microsoftonline.com/<tenant_id>/oauth2/token``` 终结点。 示例请求如下所示。

``` syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://onestore.microsoft.com
```

对于每个令牌，请指定以下参数数据：

* 对于 *client\_id* 和 *client\_secret* 参数，请为从 [Azure 管理门户](http://manage.windowsazure.com)中检索到的应用程序指定客户端 ID 和客户端密钥。 若要创建带有 Windows 应用商店收集 API 或购买 API 所需的身份验证级别的访问令牌，这两个参数都是必需的。

* 对于*资源*参数，请指定[上一节](#access-tokens)中列出的受众 URI 之一，具体取决于要创建的访问令牌的类型。

在你的访问令牌到期后，可以按照[此处](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens)的说明刷新令牌。 有关访问令牌的结构的更多详细信息，请参阅[支持的令牌和声明类型](http://go.microsoft.com/fwlink/?LinkId=722501)。

<span id="step-4"/>
## <a name="step-4-create-a-windows-store-id-key"></a>第 4 步：创建 Windows 应用商店 ID 密钥

你的应用必须先创建 Windows 应用商店 ID 密钥并将其发送给服务，然后你才能调用 Windows 应用商店收集 API 或购买 API 中的任何方法。 此密钥是 JSON Web 令牌 (JWT)，表示你想要访问其产品所有权信息的用户的标识。 有关此密钥中的声明的详细信息，请参阅 [Windows 应用商店 ID 密钥中的声明](#claims)。

当前，创建 Windows 应用商店 ID 密钥的唯一方法是通过你的应用中的客户端代码调用通用 Windows 平台 (UWP) API。 生成的密钥表示当前在设备上登录到 Windows 应用商店的用户的身份。

> [!NOTE]
> 每个 Windows 应用商店 ID 密钥的有效期为 90 天。 密钥到期后，可以[续订该密钥](renew-a-windows-store-id-key.md)。 我们建议你续订 Windows 应用商店 ID 密钥，而非创建新密钥。

<span />
### <a name="to-create-a-windows-store-id-key-for-the-windows-store-collection-api"></a>为 Windows 应用商店收集 API 创建 Windows 应用商店 ID 密钥

按照以下步骤创建可与 Windows 应用商店收集 API 一起使用的 Windows 应用商店 ID 密钥，以[查询用户拥有的产品](query-for-products.md)或[将可消费产品报告为已完成](report-consumable-products-as-fulfilled.md)。

1.  将你使用 `https://onestore.microsoft.com/b2b/keys/create/collections` 受众 URI 创建的 Azure AD 访问令牌从服务传递到客户端应用。

2.  在你的应用代码中，调用以下方法之一以检索 Windows 应用商店 ID 密钥：

  * 如果你的应用使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间中的 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类来管理应用内购买，请使用 [StoreContext.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomercollectionsidasync.aspx) 方法。

  * 如果你的应用使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间中的 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 类来管理应用内购买，请使用 [CurrentApp.GetCustomerCollectionsIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608674) 方法。

    将 Azure AD 访问令牌传递给该方法的 *serviceTicket* 参数。 可以选择将 ID 传递给在服务上下文中标识当前用户的 *publisherUserId* 参数。 如果你为服务维护用户 ID，可以使用此参数将这些用户 ID 与对 Windows 应用商店收集 API 进行的调用关联起来。

3.  在应用成功创建 Windows 应用商店 ID 密钥后，请将该密钥传递回服务。

<span />
### <a name="to-create-a-windows-store-id-key-for-the-windows-store-purchase-api"></a>为 Windows 应用商店购买 API 创建 Windows 应用商店 ID 密钥

按照以下步骤创建可与 Windows 应用商店购买 API 一起使用的 Windows 应用商店 ID 密钥，以[向用户授予免费产品](grant-free-products.md)、[获取用户订阅](get-subscriptions-for-a-user.md)或[更改用户订阅的计费状态](change-the-billing-state-of-a-subscription-for-a-user.md)。

1.  将你使用 `https://onestore.microsoft.com/b2b/keys/create/purchase` 受众 URI 创建的 Azure AD 访问令牌从服务传递到客户端应用。

2.  在你的应用代码中，调用以下方法之一以检索 Windows 应用商店 ID 密钥：

  * 如果你的应用使用 [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx) 命名空间中的 [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) 类来管理应用内购买，请使用 [StoreContext.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getcustomerpurchaseidasync.aspx) 方法。

  * 如果你的应用使用 [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) 命名空间中的 [CurrentApp](https://msdn.microsoft.com/library/windows/apps/hh779765) 类来管理应用内购买，请使用 [CurrentApp.GetCustomerPurchaseIdAsync](https://msdn.microsoft.com/library/windows/apps/mt608675) 方法。

    将 Azure AD 访问令牌传递给该方法的 *serviceTicket* 参数。 可以选择将 ID 传递给在服务上下文中标识当前用户的 *publisherUserId* 参数。 如果你为服务维护用户 ID，可以使用此参数将这些用户 ID 与对 Windows 应用商店购买 API 进行的调用关联起来。

3.  在应用成功创建 Windows 应用商店 ID 密钥后，请将该密钥传递回服务。

<span id="step-5"/>
## <a name="step-5-call-the-windows-store-collection-api-or-purchase-api-from-your-service"></a>步骤 5：从你的服务调用 Windows 应用商店收集 API 或购买 API

在你的服务具有允许其访问特定用户的产品所有权信息的 Windows 应用商店 ID 密钥后，你的服务可通过遵循以下说明调用 Windows 应用商店收集 API 或购买 API：

* [查询产品](query-for-products.md)
* [将可消费产品报告为已完成](report-consumable-products-as-fulfilled.md)
* [授予免费产品](grant-free-products.md)
* [获取用户订阅](get-subscriptions-for-a-user.md)
* [更改用户订阅的计费状态](change-the-billing-state-of-a-subscription-for-a-user.md)

对于每个方案，请将以下信息传递到 API：

-   你之前通过 `https://onestore.microsoft.com` 受众 URI 创建的 Azure AD 访问令牌。 此令牌代表你的发布者标识。 在请求标头中传递此令牌。
-   从应用中的客户端代码检索到的 Windows 应用商店 ID 密钥。 此密钥表示你想要访问其产品所有权信息的用户的标识。

<span id="claims"/>
## <a name="claims-in-a-windows-store-id-key"></a>Windows 应用商店 ID 密钥中的声明

Windows 应用商店 ID 密钥是 JSON Web 令牌 (JWT)，该令牌表示你想要访问其产品所有权信息的用户的标识。 当使用 Base64 解码时，Windows 应用商店 ID 密钥包含以下声明。

* `iat`：&nbsp;&nbsp;&nbsp;标识颁发密钥的时间。 此声明可用于确定令牌的有效期。 此值表示为纪元时间。
* `iss`：&nbsp;&nbsp;&nbsp;标识颁发者。 这与 `aud` 声明具有相同的值。
* `aud`：&nbsp;&nbsp;&nbsp;标识受众。 必须是下列值之一：`https://collections.mp.microsoft.com/v6.0/keys` 或 `https://purchase.mp.microsoft.com/v6.0/keys`。
* `exp`：&nbsp;&nbsp;&nbsp;标识在此时或之后不再接受密钥处理除续订密钥之外的任何操作的到期时间。 此声明的值表示为纪元时间。
* `nbf`：&nbsp;&nbsp;&nbsp;标识接受令牌进行处理的时间。 此声明的值表示为纪元时间。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId`：&nbsp;&nbsp;&nbsp;标识开发人员的客户端 ID。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload`：&nbsp;&nbsp;&nbsp;包含计划仅供 Windows 应用商店服务使用的信息的不透明负载（已加密，并使用 Base64 编码）。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId`：&nbsp;&nbsp;&nbsp;标识服务上下文中的当前用户的用户 ID。 此值与你传递到[用于创建密钥的方法](#step-4)的可选 *publisherUserId* 参数中的值相同。
* `http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri`：&nbsp;&nbsp;&nbsp;可用于续订密钥的 URI。

以下是一个解码的 Windows 应用商店 ID 密钥标头的示例。

```json
{
    "typ":"JWT",
    "alg":"RS256",
    "x5t":"agA_pgJ7Twx_Ex2_rEeQ2o5fZ5g"
}
```

以下是一个解码的 Windows 应用商店 ID 密钥声明集的示例。

```json
{
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/clientId": "1d5773695a3b44928227393bfef1e13d",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/payload": "ZdcOq0/N2rjytCRzCHSqnfczv3f0343wfSydx7hghfu0snWzMqyoAGy5DSJ5rMSsKoQFAccs1iNlwlGrX+/eIwh/VlUhLrncyP8c18mNAzAGK+lTAd2oiMQWRRAZxPwGrJrwiq2fTq5NOVDnQS9Za6/GdRjeiQrv6c0x+WNKxSQ7LV/uH1x+IEhYVtDu53GiXIwekltwaV6EkQGphYy7tbNsW2GqxgcoLLMUVOsQjI+FYBA3MdQpalV/aFN4UrJDkMWJBnmz3vrxBNGEApLWTS4Bd3cMswXsV9m+VhOEfnv+6PrL2jq8OZFoF3FUUpY8Fet2DfFr6xjZs3CBS1095J2yyNFWKBZxAXXNjn+zkvqqiVRjjkjNajhuaNKJk4MGHfk2rZiMy/aosyaEpCyncdisHVSx/S4JwIuxTnfnlY24vS0OXy7mFiZjjB8qL03cLsBXM4utCyXSIggb90GAx0+EFlVoJD7+ZKlm1M90xO/QSMDlrzFyuqcXXDBOnt7rPynPTrOZLVF+ODI5HhWEqArkVnc5MYnrZD06YEwClmTDkHQcxCvU+XUEvTbEk69qR2sfnuXV4cJRRWseUTfYoGyuxkQ2eWAAI1BXGxYECIaAnWF0W6ThweL5ZZDdadW9Ug5U3fZd4WxiDlB/EZ3aTy8kYXTW4Uo0adTkCmdLibw=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/userId": "infusQMLaYCrgtC0d/SZWoPB4FqLEwHXgZFuMJ6TuTY=",
    "http://schemas.microsoft.com/marketplace/2015/08/claims/key/refreshUri": "https://collections.mp.microsoft.com/v6.0/b2b/keys/renew",
    "iat": 1442395542,
    "iss": "https://collections.mp.microsoft.com/v6.0/keys",
    "aud": "https://collections.mp.microsoft.com/v6.0/keys",
    "exp": 1450171541,
    "nbf": 1442391941
}
```

## <a name="related-topics"></a>相关主题

* [查询产品](query-for-products.md)
* [将可消费产品报告为已完成](report-consumable-products-as-fulfilled.md)
* [授予免费产品](grant-free-products.md)
* [获取用户订阅](get-subscriptions-for-a-user.md)
* [更改用户订阅的计费状态](change-the-billing-state-of-a-subscription-for-a-user.md)
* [续订 Windows 应用商店 ID 密钥](renew-a-windows-store-id-key.md)
* [将应用程序与 Azure Active Directory 集成](http://go.microsoft.com/fwlink/?LinkId=722502)
* [了解 Azure Active Directory 应用程序清单]( http://go.microsoft.com/fwlink/?LinkId=722500)
* [支持的令牌和声明类型](http://go.microsoft.com/fwlink/?LinkId=722501)
