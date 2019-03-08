---
Description: 你可以使用其他开发人员发布的应用交叉推广你的应用。 我们将此功能称为社区广告。
title: 有关社区广告
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f8acf83e2b39ece5fcd46c3d89d921e4f3013b67
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635782"
---
# <a name="about-community-ads"></a>有关社区广告

如果您的应用程序[显示横幅或标题插播式广告](../monetize/display-ads-in-your-app.md)，可以跨-将提升您的应用程序与 Microsoft Store 中的应用与其他开发人员免费。 我们将此功能称为*社区广告*。  

以下是此计划的运作方式：

* 选择在之后到社区广告如下所述，你可以[创建免费的社区的广告活动](create-an-ad-campaign-for-your-app.md)。 然后，您的应用程序将与其他开发人员还加入社区广告共享促销 ad 空间。 你的应用将会显示其他加入社区广告的开发人员发布的应用的广告，而他们的应用也会显示你的应用的广告。
* 通过在你的应用中显示社区广告，你可以获取在其他应用中拥有推广广告空间的信用。 信用根据以下过程计算：
  * 对于每个有提供社区广告的应用的国家或地区，该国家或地区的当前市场行情 eCPM（每千次印象有效成本）值乘以你的应用在该国家或地区进行的社区广告请求数。 此值就是你在该国家或地区为你的应用获取的信用。
  * 你在给定时间段内获取的总信用等于每个提供社区广告的应用在每个国家或地区获取的所有信用之和。
* 你的信用会在所有活动社区广告市场活动中平均分配，并且会根据你的社区广告市场活动面向的国家/地区的当前市场行情 eCPM 值转换为应用广告印象。
* 若要在应用中跟踪社区广告的性能，请参阅[广告性能报告](advertising-performance-report.md)。

### <a name="opt-in-to-community-ads"></a>选择加入社区广告

可以为您的应用程序之一创建社区的广告活动之前，你必须在选择中**Monetize** &gt; **应用内广告**页[合作伙伴中心](https://partner.microsoft.com/dashboard)。

可以选择将社区广告的 UWP 应用：

1. 选择在应用中使用和向下滚动到的 ad 单位**中介设置**。
2. 如果**告知 Microsoft 优化我的设置**是选择，社区广告的广告单元会自动启用。 否则，请选择基线配置或在特定于市场的配置**目标**下拉列表，然后检查**Microsoft Community 广告**框中**其他广告网络**列表。

    > [!NOTE]
    > 可以使用**权重**字段，用于指定你想要显示来自付费的网络和其他 ad 网络包括社区广告的广告的比率。

你不需要在完成选择后重新发布应用。 一旦你已选择加入，将能够在[创建广告市场活动](create-an-ad-campaign-for-your-app.md)时选择**社区广告（免费）** 作为市场活动类型。

### <a name="related-topics"></a>相关主题

* [应用内广告](in-app-ads.md)
* [创建您的应用程序的广告活动](create-an-ad-campaign-for-your-app.md)
