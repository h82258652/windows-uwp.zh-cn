---
author: JnHs
Description: "通过个性化内容锁定特定目标客户群体，以提升参与度、忠诚度和增加盈利。"
title: "使用定向优惠最大程度地提高参与度和转换率"
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10，uwp，定向优惠，优惠/产品/服务，通知"
ms.openlocfilehash: b30add7a1aa55ffd852bff57fcebb7a5f40e114b
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/17/2017
---
# <a name="use-targeted-offers-to-maximize-engagement-and-conversions"></a>使用定向优惠最大程度地提高参与度和转换率

通过极富吸引力的个性化内容锁定特定目标客户群体，以提升参与度、忠诚度和增加盈利。 在某些情况下，定向优惠与 Microsoft 赞助的促销关联，针对每次成功转换向开发人员支付奖励，从而增加你的盈利。

> [!IMPORTANT]
> 定向优惠只能用于包含加载项的 UWP 应用。

## <a name="targeted-offer-overview"></a>定向优惠概述

在高级别上，需要完成以下三件事才能使用定向优惠：

1. **在仪表板中创建优惠。** 导航到**参与 > 定向优惠**页面，以创建优惠。 下面介绍了关于此流程的详细信息。
2. **实施应用内优惠体验。** 在应用代码中使用 *Windows 应用商店定向优惠 API*，检索面向给定用户的可用优惠，并声明优惠（如果与促销相关联）。 还需要为该定向优惠创建应用内体验。 有关详细信息，请参阅[使用应用商店服务管理定向优惠](../monetize/manage-targeted-offers-using-windows-store-services.md)。
3. **向应用商店提交应用。** 应用必须与到位的应用内优惠体验一起发布，以便向客户提供优惠。

完成这些步骤后，使用你的应用的客户此时会看到适用于他们的优惠，具体视其与优惠关联的类别成员身份而定。 请注意，我们将尽可能向你的客户显示所有适用优惠，但仍可能会（偶尔）发生影响优惠可用性的问题。


## <a name="to-create-and-send-a-targeted-offer"></a>创建和发送定向优惠

按照以下这些步骤在仪表板中创建定向优惠。 

1.  在 Windows 开发人员中心仪表板的中，展开左侧导航菜单中的**参与**，然后选择**定向优惠**。
2.  在**定向优惠**页上，查看可用优惠。 针对任何想要实施的优惠，选择**创建新优惠**。 

    > [!NOTE]
    > 随时间推移以及基于帐户标准，所看到的可用优惠可能会有所不同。 最初，使用预定义的类别条件时会看到一个或多个优惠。 很快你将能够基于[你定义的客户类别](create-customer-segments.md)创建定向优惠。

3.  在可用优惠下方显示的新行中，选择可使用优惠的产品（应用）。 然后，选择希望与优惠关联的加载项。 
4.  如果要创建更多优惠，请重复步骤 2 和 3。 对于同一个应用，只要每次为优惠选择的加载项不同，就可以多次实施同一种类型的优惠。 此外，可将同一个加载项与多个优惠类型关联。
5.  创建完优惠后，单击**保存**。

实施优惠后，可返回仪表板中的**定向优惠**页面，查看每个优惠的转换总数。 

如果决定不使用优惠（或如果不想再继续使用它），请单击**删除**。

> [!IMPORTANT]
> 请务必确保已发布代码以用于检索针对给定用户的可用优惠及创建应用内体验。 有关详细信息，请参阅[使用应用商店服务管理定向优惠](../monetize/manage-targeted-offers-using-windows-store-services.md)。
> 
> 当考虑定向优惠的内容时，请记住，与所有应用内容一样，优惠中的内容必须遵循应用商店[内容策略](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#content_policies)。
> 
> 同时也请知悉，如果使用你的应用（并且在确定类别成员身份时使用 Microsoft 帐户登录）的客户将其设备给其他人使用，则其他用户可能会看到面向原始客户的优惠。 

