---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: "请使用本文中的测试应用程序 ID 和广告单元 ID 值，查看应用在测试期间如何呈现广告。"
title: "测试模式值"
translationtype: Human Translation
ms.sourcegitcommit: c6e0cf98c6eb2cdc656d5b4555d794ff6a94d2bc
ms.openlocfilehash: e1462ae48e8aae8f5ed0e5a7e46a6660bf33e786

---

# 测试模式值




当你使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 在应用中显示广告时，必须指定应用程序 ID 和广告单元 ID。 当你开发应用时，请使用本文中的测试应用程序 ID 和广告单元 ID 值，查看应用在测试期间如何呈现广告。


如果你尝试在发布动态应用后在其中使用测试值，应用将不会收到广告。 若要在你已发布的应用中接收广告，必须更新你的代码才能使用 Windows 开发人员中心仪表板提供的应用程序 ID 和 广告单元 ID。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md)。
 

下面是要用于间隙视频和横幅广告的测试值。

* 对于间隙视频广告：

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>11389925</p></td>
    <td align="left"><p>d25517cb-12d4-4699-8bdc-52040c712cab</p></td>
    </tr>
    </tbody>
    </table>

     
* 对于横幅广告：

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">AdUnitId</th>
    <th align="left">AppId</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left"><p>10865270</p></td>
    <td align="left"><p>3f83fe91-d6be-434d-a0ae-7351c5a997f1</p></td>
    </tr>
    </tbody>
    </table>


> **重要提示** 实时广告的大小由 **AdControl** 的 **Width** 和 **Height** 属性定义。 为获得最佳结果，请确保代码中的 **Width** 和 **Height** 属性是[横幅广告的受支持广告大小](supported-ad-sizes-for-banner-ads.md)之一。 **Width** 和 **Height** 属性不会根据实时广告的大小而发生更改。



 

 



<!--HONumber=Aug16_HO3-->


