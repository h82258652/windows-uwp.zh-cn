---
author: jnHs
Description: "如果你的应用使用广告中介，或者显示来自 Microsoft Advertising 的横幅或间隙视频广告，请使用“盈利”&gt;“利用广告来盈利”页面管理你的广告的使用。"
title: "通过广告盈利"
ms.assetid: 09970DE3-461A-4E2A-88E3-68F2399BBCC8
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 97eeeedb9e73b6c67abe6e2ff8cadbc744a6a7c4

---

# 通过广告盈利


如果你的应用使用 **AdMediatorControl**、**AdControl** 或者 **InterstitialAd** 控件来显示横幅或间隙视频广告，请使用“盈利”****&gt;“利用广告来盈利”****页面管理你的广告的使用。

## Windows 广告中介


如果你的应用使用广告中介，可以使用本部分配置中介设置，并为每个你的应用使用的广告网络添加所需的参数。 有关此部分中的选项的详细信息，请参阅[提交应用和配置广告中介](https://msdn.microsoft.com/library/windows/apps/mt219689)。

广告中介使你可以通过调解来自多个广告网络的横幅广告请求来优化你的应用内广告收益。 有关广告中介的详细信息，请参阅[使用广告中介使收益最大化](https://msdn.microsoft.com/library/windows/apps/mt219691)。

## COPPA 合规性

根据《儿童在线隐私保护法》（“COPPA”），如果你的应用面向 13 岁以下的儿童，则你必须通知 Microsoft。 如果你使用开发人员中心向 Microsoft 指示你的应用将会面向13 岁以下的儿童，则 Microsoft 将采取措施在向你的应用发送广告时禁用其行为广告服务。 如果你的应用面向 13 岁以下的儿童，则你承担 COPPA 规定的某些义务。

有关 COPPA 规定的义务的详细信息，请参阅[此页面](http://go.microsoft.com/fwlink/p/?linkid=536558)。

## Microsoft 关联广告

如果想要在你的应用中显示 Microsoft 关联广告，请选中此部分中的框。 如果选中此框，当其他广告网络没有可用的广告时，将为你的应用在应用商店中提供这些产品（包括音乐、游戏、电影、应用、硬件和软件）的广告。 当用户在给定属性窗口的应用商店中单击广告和总线产品时，你将赚取已批准购买的佣金。

如果更改此选择，你无需重新发布应用以让更改生效。 有关 Microsoft 关联广告的详细信息，请参阅[关于关联广告](about-affiliate-ads.md)。

> **注意** 如果你的应用使用广告中介（即，它使用 **AdMediatorControl** 显示广告），则只有当你的广告中介设置配置为从 Microsoft 显示广告时，你的应用才可以显示关联广告。

## 社区广告

如果想要使用其他开发人员的应用来交叉推广你的应用，请选中此部分中的框。 如果选中此复选框并[创建社区广告市场活动](create-an-ad-campaign-for-your-app.md)，你的应用将显示由其他开发人员（还创建了社区广告市场活动）发布的应用的广告，并其应用的广告将显示在你的应用中。 社区广告免费，并且它们仅在其他广告网络没有可用的广告时才会显示。

如果更改此选择，你无需重新发布应用以让更改生效。 有关社区广告的详细信息，请参阅[有关社区广告](about-community-ads.md)。

## Microsoft Advertising 广告单元

使用此部分来创建 Microsoft Advertising 广告单元。 只需要在以下场景中创建广告单元：

-   你的应用通过使用 [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) 对象显示来自 Microsoft Advertising 的横幅广告。
-   你的应用通过使用 [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx) 对象显示来自 Microsoft Advertising 的间隙视频广告。

创建适用于以下场景的广告单元：

1.  即，所谓的广告单元。
2.  选择广告单元类型（“横幅”****或“间隙视频”****）。
3.  选择设备类型（“移动设备”****或“PC/平板电脑”****）。
4.  单击“创建广告单元”****。

你的广告单元将显示在本部分底部的表格中。 你将会看到每个广告单元的“应用程序 ID”****和“广告单元 ID”****。 若要在应用中显示广告，需要在你的代码中使用这些值：

-   如果你的应用显示横幅广告，请将这些值分配给 [AdControl](https://msdn.microsoft.com/library/mt313154.aspx) 对象的 [ApplicationId](https://msdn.microsoft.com/library/mt313174.aspx) 和 [AdUnitId](https://msdn.microsoft.com/library/mt313171.aspx) 属性。
-   如果你的应用显示间隙视频广告，请将这些值传递给 [InterstitialAd](https://msdn.microsoft.com/library/mt313189.aspx) 对象的 [RequestAd](https://msdn.microsoft.com/library/mt313192.aspx) 方法。

> **注意** 如果你的应用使用广告中介显示来自 Microsoft Advertising 的横幅广告（即，它使用 **AdMediatorControl** 对象），你不需要请求广告单元。 在此情况下，将为你自动生成 Microsoft Advertising 广告单元。

 

 

 



<!--HONumber=Jun16_HO4-->


