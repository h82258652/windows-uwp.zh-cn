---
author: jnHs
Description: Partner Center gives you several options to let testers try out your app before you offer it to the public.
title: beta 版本测试和定向分发
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10，uwp, beta 测试版本, 有限分发, beta 版本, 测试, 测试人员
ms.localizationpriority: medium
ms.openlocfilehash: 49641007f939faf333ea5aca357266225f8484c8
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5813865"
---
# <a name="beta-testing-and-targeted-distribution"></a>beta 版本测试和定向分发

无论你如何仔细地测试应用，都不如通过让其他人使用它来进行实际测试。 测试人员可能会发现你所忽略的问题，例如拼写错误、混乱的应用流程或可能导致该应用崩溃的错误。 然后，你将有机会在向公众发布提交之前修复这些问题，从而推出更完善的最终产品。 

合作伙伴中心为你提供了几个选项让测试人员提供给公众之前试用你的应用。

无论你选择哪种方法，对应用进行 beta 版本测试时需要牢记以下几点。

- 测试人员下载应用后，你无法撤销对该应用的访问权限。 一旦他们下载应用，便可以继续使用它，并且将获得你随后发布的任何更新。
- 你将需要确定要如何收集测试人员的反馈。 请考虑提供一个链接，让测试人员能够轻松地通过电子邮件（或[反馈中心](../monetize/launch-feedback-hub-from-your-app.md)，如果不存在保密性问题的话）提供反馈。 
- 你可以查看应用的[分析报告](analytics.md)，包括使用情况和运行情况报告以及测试人员留下的任何评级或评论。
- 可以在将应用分配给测试人员时包含加载项。 你可能不希望向他们收取某个加载项的费用，因此可以[生成促销代码](generate-promotional-codes.md)并将它们分发给测试人员，以便他们获取免费的加载项，你也可以在测试期间将加载项的价格设置为**免费**（然后在将应用提供给其他客户之前，创建一个新的加载项提交以更改其价格）。 请注意：一个 Microsoft 帐户只能购买每个加载项一次，因此同一测试人员将无法对加载项购买过程进行多次测试。 
- 你可以通过创建一个新提交来使用新包随时为测试人员提供应用的更新版本。 通过认证过程后，测试人员将像获得原始包一样获得更新，但其他人无法获取更新（除非你进行其他更改，例如将应用从**私人受众**转移为**公共受众**或更改可以获取更新的组成员）。

## <a name="private-audience"></a>私人受众

如果你要让测试人员在应用公开发布前就使用它，并确保没有其他人可以看到该应用的一览，请使用[可见性](choose-visibility-options.md)（位于你的提交的**定价和可用性**页面）下的**私人受众**选项。 只有通过这种方法，才可以在将应用分配给测试人员时，完全防止其他人看到该应用的 Microsoft Store 一览（即使其他人能够键入直接链接）。 

**私人受众**选项只能时不已发布你的应用向公共受众。 你可以使用此选项与应用面向任何操作系统版本，但测试人员必须运行 Windows 10 版本 1607年或更高版本 （包括 Xbox One)，并且必须使用与你提供的电子邮件地址关联的 Microsoft 帐户登录。

有关详细信息，请参阅[私人受众](choose-visibility-options.md#audience)。


## <a name="package-flights"></a>软件包外部测试版

如果你已发布了应用，则可以创建软件包外部测试版来将不同的软件包集分配给指定用户。 甚至可以为相同应用创建多个软件包外部测试版，以用于不同组的用户。 这是同时尝试不同软件包的绝佳方法，并且如果你决定软件包已准备好分发给所有人，则可以将外部测试版中的软件包加入到非外部测试版提交中。

软件包外部测试版适用于面向任何操作系统版本的应用，但测试人员只有在运行 Windows.Desktop 版本 10586 或更高版本、Windows.Mobile 版本 10586.63 或更高版本或 Xbox One 的情况下才可以获得该应用。

有关详细信息，请参阅[软件包外部测试版](package-flights.md)。


<span id="hide" />

## <a name="hiding-the-app-in-the-store-and-using-promotional-codes"></a>在 Microsoft Store 中隐藏应用和使用促销代码

此选项提供了另一种方法来限制应用于某些组的测试人员分发时阻止其他任何用户发现你的应用商店中的应用 （或促销充值码获取它）。 但是，与私人受众选项不同，如果有直接链接，任何人都可能看到你的应用的一览。 如果保密性对于你的提交至关重要，我们建议改为发布到私人受众。

隐藏应用和使用促销代码适用于面向任何操作系统版本的应用，但测试人员只有在运行 Windows 10 的情况下才可以获得该应用。

要使用此选项，请执行以下操作：

- 在**定价和可用性**页面的**可见性**部分中，选择[可发现性](choose-visibility-options.md#discoverability)下方的**使此产品在 Microsoft Store 中可用，但不可被发现**。 选择**停止购置：任何具有直接链接的客户均可看到产品的应用商店一览，但仅当曾经拥有过该产品或拥有一个促销代码，同时使用的是 Windows 10 设备时，这些客户才能够下载该产品**的选项。 
- 应用通过认证后，为应用[生成促销代码](generate-promotional-codes.md)并分发给测试人员。 你可以在六个月期间内为单个应用生成最多允许 1600 次兑换的充值码。 这些代码将为测试人员提供指向应用一览的直接链接，并且将允许他们免费下载该应用，即使你在创建提交时为其设置了价格。
- 当你准备好向公众提供应用时，请创建一个新提交并将**可见性**选项更改为**使此产品在 Microsoft Store 中可用，并使其可被发现**（你也可以更改其他需要改动的内容）。


## <a name="targeted-distribution-with-a-link-to-your-apps-listing"></a>使用指向应用一览的链接定向分发

与上述选项不同，此选项适用于 Windows Phone 8.1 以及 Windows 10 客户（而不适用于 Windows 8.x 客户）。 任何客户都不能通过在 Microsoft Store 中搜索或浏览找到该应用，但如果有 Microsoft Store 一览的直接链接，任何人都可以在运行 Windows Phone 8.1 或更早版本的设备或在 Windows 10 设备上下载它。 请记住，为了让测试人员免费下载应用，必须将其价格设置为**免费**。

要使用此选项，请执行以下操作：
- 在**定价和可用性**页面的**可见性**部分中，选择[可发现性](choose-visibility-options.md#discoverability)下方的**使此产品在 Microsoft Store 中可用，但不可被发现**。 选择**仅直接链接：任何具有指向产品一览的直接链接的客户均可下载，Windows 8.x 除外**选项。
- 在发布产品后，将链接（[应用标识页面](view-app-identity-details.md)上的 **URL**）分发给测试人员，以便他们进行试用。
- 当你准备好向公众提供应用时，请创建一个新提交并将**可见性**选项更改为**使此产品在 Microsoft Store 中可用，并使其可被发现**（你也可以更改其他需要改动的内容）。

> [!IMPORTANT]
> 从 2018 年 10 月 31 日起新创建的产品不能包含程序包面向 Windows Phone 8.x 或更早版本。 有关详细信息，请参阅此[博客文章](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97)。

## <a name="targeted-distribution-to-windows-phone-customers-with-specified-email-addresses"></a>向具有指定电子邮件地址的 Windows Phone 客户定向分发

> [!IMPORTANT]
> 此选项不适用于新的提交。 如果你之前已经为面向 Windows Phone 8.1 或更早版本的某个应用选择了此选项，你将能够让该应用继续使用此选项。 你可以通过创建新的提交来更改测试人员（最多 10,000 名）列表。 

使用此选项，具有你所指定的电子邮件地址的人员可以通过使用应用一览的直接链接来下载该应用（仅限在运行 Windows Phone 8.1 或之前版本的设备上）。 其他任何客户都无法下载该应用（即使他们拥有该链接），并且他们无法通过搜索或浏览在 Microsoft Store 中找到该应用。 为了让测试人员能够下载该应用，你需要为他们提供链接（[应用标识页面](view-app-identity-details.md)上的 **URL**），并且他们必须使用与你提供的电子邮件地址相关联的 Microsoft 帐户登录。 你还可以让应用访问 windows 10 设备上的测试人员通过[生成促销充值码](generate-promotional-codes.md);即使你未输入其电子邮件，与你的应用的促销充值码之一的任何人都可以在 windows 10 设备上，下载它。
