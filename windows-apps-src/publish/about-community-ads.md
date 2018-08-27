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
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "2864876"
---
# <a name="about-community-ads"></a>有关社区广告

如果[显示横幅间隙广告](../monetize/display-ads-in-your-app.md)您的应用程序，您可以跨-将升级您的应用程序与其他开发人员提供的 Microsoft 存储区中的应用程序免费。 我们将此功能称为*社区广告*。  

以下是此计划的运作方式：

* 您加入后向社区广告如下所述，您可以[创建免费的社区 ad 市场活动](create-an-ad-campaign-for-your-app.md)。 然后，您的应用程序将与其他开发人员还加入社区广告共享促销 ad 空间。 你的应用将会显示其他加入社区广告的开发人员发布的应用的广告，而他们的应用也会显示你的应用的广告。
* 通过在你的应用中显示社区广告，你可以获取在其他应用中拥有推广广告空间的信用。 信用根据以下过程计算：
  * 对于每个有提供社区广告的应用的国家或地区，该国家或地区的当前市场行情 eCPM（每千次印象有效成本）值乘以你的应用在该国家或地区进行的社区广告请求数。 此值就是你在该国家或地区为你的应用获取的信用。
  * 你在给定时间段内获取的总信用等于每个提供社区广告的应用在每个国家或地区获取的所有信用之和。
* 你的信用会在所有活动社区广告市场活动中平均分配，并且会根据你的社区广告市场活动面向的国家/地区的当前市场行情 eCPM 值转换为应用广告印象。
* 若要在应用中跟踪社区广告的性能，请参阅[广告性能报告](advertising-performance-report.md)。

### <a name="opt-in-to-community-ads"></a>选择加入社区广告

您可以为您的应用程序之一创建社区 ad 市场活动之前，您必须在**Monetize**选择在&gt;Windows 开发人员中心仪表板中的**应用程序内广告**页。

选择社区广告 UWP 应用程序：

1. 在**应用程序内 ads 的关系**页上的**中介设置**部分中，选择应用程序中使用的 ad 单位。
2. 如果选择**允许 Microsoft 选择您的应用程序的最佳中介设置**选项，则社区 ads 的关系是自动启用 ad 单位。 否则为在**目标**下拉列表中选择基准配置或市场特定配置，然后检查**Microsoft 社区**ads**其他 ad 网络**列表中。

    > [!NOTE]
    > **权重**字段可用于指定您想要从付费的网络和其他 ad 网络，包括社区广告显示的广告的比率。

为 Windows 加入社区广告 8.x 或 Windows Phone 8.x 应用程序

1. 在**应用程序内 ads 的关系**页上，选中**显示我的应用程序中的社区广告**框。

你不需要在完成选择后重新发布应用。 一旦你已选择加入，将能够在[创建广告市场活动](create-an-ad-campaign-for-your-app.md)时选择**社区广告（免费）** 作为市场活动类型。

### <a name="related-topics"></a>相关主题

* [应用内广告](in-app-ads.md)
* [为你的应用创建广告市场活动](create-an-ad-campaign-for-your-app.md)
