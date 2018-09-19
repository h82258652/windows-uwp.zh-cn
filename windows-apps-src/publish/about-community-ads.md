---
author: jnHs
Description: You can cross-promote your app with apps published by other developers. We call this feature community ads.
title: 有关社区广告
ms.assetid: F55CE478-99AF-4B70-90D1-D16419562136
ms.author: wdg-dev-content
ms.date: 10/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ada2e0610e81e986deba3ddab5e87547e05fe108
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "4060917"
---
# <a name="about-community-ads"></a>有关社区广告

如果你的应用时[显示横幅广告或横幅间隙广告](../monetize/display-ads-in-your-app.md)，你可以免费交叉推广你的应用与其他开发人员的 Microsoft 应用商店中的应用。 我们将此功能称为*社区广告*。  

以下是此计划的运作方式：

* 你的选择后加入社区广告，如下所述，你可以[创建免费的社区广告市场活动](create-an-ad-campaign-for-your-app.md)。 然后，你的应用将与其他开发人员还选择加入社区广告共享推广广告空间。 你的应用将会显示其他加入社区广告的开发人员发布的应用的广告，而他们的应用也会显示你的应用的广告。
* 通过在你的应用中显示社区广告，你可以获取在其他应用中拥有推广广告空间的信用。 信用根据以下过程计算：
  * 对于每个有提供社区广告的应用的国家或地区，该国家或地区的当前市场行情 eCPM（每千次印象有效成本）值乘以你的应用在该国家或地区进行的社区广告请求数。 此值就是你在该国家或地区为你的应用获取的信用。
  * 你在给定时间段内获取的总信用等于每个提供社区广告的应用在每个国家或地区获取的所有信用之和。
* 你的信用会在所有活动社区广告市场活动中平均分配，并且会根据你的社区广告市场活动面向的国家/地区的当前市场行情 eCPM 值转换为应用广告印象。
* 若要在应用中跟踪社区广告的性能，请参阅[广告性能报告](advertising-performance-report.md)。

### <a name="opt-in-to-community-ads"></a>选择加入社区广告

你可以为应用之一创建社区广告市场活动之前，你必须选择加入**盈利**&gt;在 Windows 开发人员中心仪表板中的**应用内广告**页面。

若要选择加入社区广告的 UWP 应用：

1. 在**应用内广告**页面上的**中介设置**部分中，选择你的应用中使用某个广告单元。
2. 如果选择**让 Microsoft 选择最佳中介设置为你的应用**选项时，会自动将社区广告启用为广告单元。 否则为在**目标**下拉列表中选择基准配置或特定于市场的配置，然后查看**其他广告网络**列表中的**Microsoft 社区广告**框。

    > [!NOTE]
    > 你可以使用**权重**字段指定你想要显示来自付费的网络和其他广告网络，包括社区广告的广告的比例。

若要选择加入社区广告适用于 Windows 8.x 或 Windows Phone 8.x 应用

1. 在**应用内广告**页面上，选中**显示我的应用中的社区广告**框。

你不需要在完成选择后重新发布应用。 一旦你已选择加入，将能够在[创建广告市场活动](create-an-ad-campaign-for-your-app.md)时选择**社区广告（免费）** 作为市场活动类型。

### <a name="related-topics"></a>相关主题

* [应用内广告](in-app-ads.md)
* [为你的应用创建广告市场活动](create-an-ad-campaign-for-your-app.md)
