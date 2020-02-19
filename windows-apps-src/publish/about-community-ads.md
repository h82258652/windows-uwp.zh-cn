---
Description: 你可以使用其他开发人员发布的应用交叉推广你的应用。 我们将此功能称为社区广告。
title: 有关社区广告
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8da380cb49d584e56f584f3ad321d601d211faf0
ms.sourcegitcommit: 6af7ce0e3c27f8e52922118deea1b7aad0ae026e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/19/2020
ms.locfileid: "77463439"
---
# <a name="about-community-ads"></a>有关社区广告

>[!WARNING]
> 从2020年6月1日起，将关闭适用于 Windows UWP 应用的 Microsoft Ad 盈利平台。 [了解详细信息](https://aka.ms/ad-monetization-shutdown)

如果你的应用程序[显示横幅或横幅插播式广告](../monetize/display-ads-in-your-app.md)，则可以免费在 Microsoft Store 中将应用与其他开发人员进行交叉提升。 我们将此功能称为*社区广告*。  

以下是此计划的运作方式：

* 按如下所述选择加入社区广告后，可以[创建免费的社区广告活动](create-an-ad-campaign-for-your-app.md)。 然后，你的应用将与也选择加入社区广告的其他开发人员共享促销广告空间。 你的应用将会显示其他加入社区广告的开发人员发布的应用的广告，而他们的应用也会显示你的应用的广告。
* 通过在你的应用中显示社区广告，你可以获取在其他应用中拥有推广广告空间的信用。 信用根据以下过程计算：
  * 对于每个有提供社区广告的应用的国家或地区，该国家或地区的当前市场行情 eCPM（每千次印象有效成本）值乘以你的应用在该国家或地区进行的社区广告请求数。 此值就是你在该国家或地区为你的应用获取的信用。
  * 你在给定时间段内获取的总信用等于每个提供社区广告的应用在每个国家或地区获取的所有信用之和。
* 你的信用会在所有活动社区广告市场活动中平均分配，并且会根据你的社区广告市场活动面向的国家/地区的当前市场行情 eCPM 值转换为应用广告印象。
* 若要在应用中跟踪社区广告的性能，请参阅[广告性能报告](advertising-performance-report.md)。

### <a name="opt-in-to-community-ads"></a>选择加入社区广告

你必须在[合作伙伴中心](https://partner.microsoft.com/dashboard)的**盈利**&gt;**应用内广告**页中选择启用，然后才能为你的某个应用创建社区广告活动。

选择加入 UWP 应用的社区广告：

1. 选择要在应用中使用的 ad 单位，并向下滚动到 "**中介设置**"。
2. 如果选择 "**让 Microsoft 优化我的设置**"，则会自动为你的 ad 单位启用社区广告。 否则，请在 "**目标**" 下拉列表中选择基准配置或特定于市场的配置，然后查看 "**其他 ad 网络**" 列表中的 " **Microsoft 社区广告**" 框。

    > [!NOTE]
    > 可使用**权重**字段指定要从付费网络和其他 ad 网络（包括社区广告）显示的广告的比率。

你不需要在完成选择后重新发布应用。 一旦你已选择加入，将能够在**创建广告市场活动**时选择[社区广告（免费）](create-an-ad-campaign-for-your-app.md)作为市场活动类型。

### <a name="related-topics"></a>相关主题

* [应用内广告](in-app-ads.md)
* [为应用创建 ad 市场活动](create-an-ad-campaign-for-your-app.md)
