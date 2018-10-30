---
author: Xansky
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: 使用 Microsoft Store 定向优惠 API 获取对应用的当前用户可用的定向优惠。
title: 使用应用商店服务管理定向优惠
ms.author: mhopkins
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 定向优惠 API, 定向优惠
ms.localizationpriority: medium
ms.openlocfilehash: 706f48e64fb8e7534686b8fd7e9666b98dffd9b7
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5761579"
---
# <a name="manage-targeted-offers-using-store-services"></a>使用应用商店服务管理定向优惠

如果你在 Windows 开发人员中心仪表板的**参与 > 定向优惠**页面中为应用创建了*定向优惠*，请在应用代码中使用 *Microsoft Store 定向优惠 API* 检索信息，以便为此定向优惠实现应用内体验。 有关定向优惠和如何在仪表板中创建定向优惠的更多信息，请参阅[使用定向优惠最大化参与度和转换率](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)。

定向优惠 API 是一个简单的 REST API，可用来获取为当前用户提供的定向优惠，具体取决于用户是否属于该定向优惠所针对的客户细分的一部分。 若要在你的应用中使用此 API，请执行以下步骤：

1.  为当前已登录应用的用户[获取 Microsoft 帐户令牌](#obtain-a-microsoft-account-token)。
2.  [为当前用户获取定向优惠](#get-targeted-offers)。
3.  为与定向优惠之一关联的加载项实现应用内购买体验。 有关如何实现应用内购买的详细信息，请参阅[此文章](enable-in-app-purchases-of-apps-and-add-ons.md)。

有关演示所有上述步骤的完整代码示例，请参阅本文末尾的[代码示例](#code-example)。 以下部分提供有关每个步骤的更多详细信息。

<span id="obtain-a-microsoft-account-token" />

## <a name="get-a-microsoft-account-token-for-the-current-user"></a>为当前用户获取 Microsoft 帐户令牌

在你的应用代码中，为当前已登录的用户获取 Microsoft 帐户 (MSA) 令牌。 对于 Microsoft Store 定向优惠 API 中的每种方法，你都必须将此令牌传递到 ```Authorization``` 请求标头中。 此令牌供 Microsoft Store 用于检索为当前用户提供的定向优惠。

若要获取 MSA 令牌，请使用 [WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) 类来获取使用范围为 ```devcenter_implicit.basic,wl.basic``` 的令牌。 以下示例展示了如何进行此操作。 此示例是[完整示例](#code-example)]的一个片段，它需要完整示例中提供的 **using** 语句。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

有关获取 MSA 令牌的详细信息，请参阅 [Web 帐户管理器](../security/web-account-manager.md)。

<span id="get-targeted-offers" />

## <a name="get-the-targeted-offers-for-the-current-user"></a>为当前用户获取定向优惠

为当前用户获得了 MSA 令牌之后，请调用 ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` URI 的 GET 方法为当前用户获取可用的定向优惠。 有关此 REST 方法的更多信息，请参阅[获取定向优惠](get-targeted-offers.md)。

此方法将会返回与针对当前用户提供的定向优惠关联的加载项产品 ID。 你可以通过这些信息将一个或多个定向优惠作为应用内购买提供给用户。

下面的示例演示如何为当前用户获取定向优惠。 此示例是[完整示例](#code-example)的一个片段。 它需要 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 库和完整示例中提供的其他类和 **using** 语句。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="code-example" />

## <a name="complete-code-example"></a>完整代码示例

以下代码示例演示了以下任务：

* 为当前用户获取 MSA 令牌。
* 通过使用[获取定向优惠](get-targeted-offers.md)方法为当前用户获取所有定向优惠。
* 购买与定向优惠关联的加载项。

此示例需要 Newtonsoft 的 [Json.NET](http://www.newtonsoft.com/json) 库。 此示例使用此库对 JSON 格式的数据进行序列化和反序列化。

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>相关主题

* [使用定向优惠最大程度地提高参与度和转换率](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [获取定向优惠](get-targeted-offers.md)
