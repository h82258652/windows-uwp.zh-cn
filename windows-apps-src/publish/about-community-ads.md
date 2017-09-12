---
author: jnHs
Description: "你可以使用其他开发人员发布的应用交叉推广你的应用。 我们将此功能称为社区广告。"
title: "有关社区广告"
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.author: wdg-dev-content
ms.date: 07/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c16f474fe8242e2994350f5261c26c7148aea5e4
ms.sourcegitcommit: 10f8dcf69d37cdb61562fc9f4d268ccb499c368f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/07/2017
---
# <a name="about-community-ads"></a>有关社区广告

如果你的应用[显示横幅广告或横幅间隙广告](../monetize/display-ads-in-your-app.md)，则可以在 Windows 应用商店中通过应用与其他开发人员一起免费交叉推广你的应用。 我们将此功能称为*社区广告*。  

以下是此计划的运作方式：

* 在你[选择加入社区广告](#how-to-opt-in-to-community-ads)并[创建免费的社区广告市场活动](create-an-ad-campaign-for-your-app.md)后，你的应用将与其他同样选择加入社区广告的开发人员共享推广广告空间。 你的应用将会显示其他加入社区广告的开发人员发布的应用的广告，而他们的应用也会显示你的应用的广告。
* 通过在你的应用中显示社区广告，你可以获取在其他应用中拥有推广广告空间的信用。 信用根据以下过程计算：
  * 对于每个有提供社区广告的应用的国家或地区，该国家或地区的当前市场行情 eCPM（每千次印象有效成本）值乘以你的应用在该国家或地区进行的社区广告请求数。 此值就是你在该国家或地区为你的应用获取的信用。
  * 你在给定时间段内获取的总信用等于每个提供社区广告的应用在每个国家或地区获取的所有信用之和。
* 你的信用会在所有活动社区广告市场活动中平均分配，并且会根据你的社区广告市场活动面向的国家/地区的当前市场行情 eCPM 值转换为应用广告印象。
* 若要在应用中跟踪社区广告的性能，请参阅[广告性能报告](advertising-performance-report.md)。

### <a name="opt-in-to-community-ads"></a>选择加入社区广告

在你为应用之一创建社区广告市场活动之前，必须选择加入 Windows 开发人员中心仪表板中应用的**盈利** &gt; **利用广告来盈利**页。

若要选择加入，执行下列操作之一：
  * 如果你的应用是一个面向 Windows 10 的 UWP 应用，请转到页面的**广告中介**部分，在**其他广告网络**列表中选中 **Microsoft Community 广告**复选框。
  * 如果你的应用面向 Windows 8.x 或 Windows Phone 8.x，请转到页面的**社区广告**部分，并选中**在我的应用中显示社区广告**复选框。

你不需要在完成选择后重新发布应用。 一旦你已选择加入，将能够在[创建广告市场活动](create-an-ad-campaign-for-your-app.md)时选择**社区广告（免费）**作为市场活动类型。

### <a name="related-topics"></a>相关主题

* [利用广告来盈利](monetize-with-ads.md)
* [为你的应用创建广告市场活动](create-an-ad-campaign-for-your-app.md)
