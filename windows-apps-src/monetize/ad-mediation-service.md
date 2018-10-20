---
author: Xansky
description: Microsoft 广告中介能够显示多个广告网络的广告，让你最大程度地增加广告收益，并充分利用应用促销功能。
title: Microsoft 广告中介服务
ms.author: mhopkins
ms.date: 06/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 广告, 投放广告, 广告中介
ms.localizationpriority: medium
ms.openlocfilehash: cfcb2402a9a0246060a619cbc65337e2b2c69d78
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2018
ms.locfileid: "5170501"
---
# <a name="microsoft-ad-mediation-service"></a>Microsoft 广告中介服务

当你使用 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp) [在应用中显示广告](display-ads-in-your-app.md)时，可以选择使用 Microsoft 广告中介服务来最大程度地增加广告收益。 本文概要介绍广告中介服务及其目标。

广告中介服务是 [Microsoft 广告盈利平台](https://developer.microsoft.com/windows/ad-monetization-platform)的一部分。 该平台由以下部分组成。

![addreferences](images/ad-mediation-service.png)

广告中介服务旨在通过构建这些功能最大程度地增加应用的广告收益。

## <a name="diversity-of-demand-by-market-and-format"></a>市场和格式的需求多样性

广告中介服务采用不同的广告格式（横幅、间隙横幅、间隙视频和本机广告）与各种广告网络集成。 广告中介服务适用于每个广告网络，以确保广告可以最大可能地吸引最终用户。 我们将继续添加其他广告合作伙伴以满足更多需求并提高质量。

## <a name="manage-complexity-of-ad-network-relationships"></a>管理广告网络关系的复杂性  

广告中介服务与大量广告网络集成，你无需进行此项工作。 使用 Microsoft 广告 SDK 在应用中显示广告后，你可以[使用开发人员中心仪表板](../publish/in-app-ads.md#mediation-settings)修改广告中介设置以显示来自多个广告网络的广告。 无需对代码进行任何更改，便可以收到来自新广告网络的广告。

我们代表你管理与广告网络的端到端关系。 从网络集成到投放广告、报告和付款，所有工作都由我们来处理，你无需进行任何操作。

## <a name="machine-learning-based-yield-optimization-algorithms"></a>基于机器学习的收益优化算法

广告中介服务的目的是实现最高的开发人员收益。 为此，它采用基于机器学习的收益优化算法。 对于每个广告调用，该算法都会确定最佳*瀑布*顺序，要使收益最大化，需要按此顺序调用广告网络。 完成这项工作需要考虑很多因素，包括：

* 发出请求的用户。
* 请求所针对的应用程序。
* 广告网络的过往性能。
* 广告格式和请求所在的市场。
* 广告调用的时间。
* 应用内容的类别。
* 用户段。
* 用户位置。

新的广告网络通过学习预算自动包含并评估性能。 在很短的时间内，它们就会在瀑布中找到自己的位置。 这使得广告网络更具竞争力，并且可以帮助开发人员通过应用获得最大收益。

强烈建议使用[推荐的中介设置](../publish/in-app-ads.md#mediation-settings)使应用中的广告产生的收益最大化。 我们的算法可以借此让你的应用产生最大收益。 不过，你也可以在开发人员中心仪表板中自由选择自己的中介设置，以便更好地控制投放广告的广告网络和广告投放顺序。

## <a name="rich-data-and-signals"></a>丰富的数据和信号

广告中介服务利用各种广告网络以改进用户定向，为每位用户显示更相关的广告。 这是通过将有关用户和应用的更丰富的信号发送到广告网络实现的，此过程遵守隐私要求。

## <a name="related-topics"></a>相关主题

* [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp)
* [中介设置](../publish/in-app-ads.md#mediation-settings)
* [广告性能报告](../publish/advertising-performance-report.md)
