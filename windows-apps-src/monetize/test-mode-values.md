---
author: mcleanbyron
ms.assetid: 2ed21281-f996-402d-a968-d1320a4691df
description: "请使用本文中的测试应用程序 ID 和广告单元 ID 值，查看应用在测试期间如何呈现广告。"
title: "测试模式值"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 93b20954ba82b613bde96db30a000902dec3b844

---

# 测试模式值


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

当你使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 或 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 在应用中显示广告时，必须指定应用程序 ID 和广告单元 ID。 当你开发应用时，请使用本文中的测试应用程序 ID 和广告单元 ID 值，查看应用在测试期间如何呈现广告。

> **重要提示** 如果你的应用使用广告中介（即，它使用 **AdMediatorControl** 对象），你不需要指定广告单元。 在此情况下，将为你自动生成广告单元。 有关详细信息，请参阅[区别是什么：AdMediatorControl 或 AdControl](what-is-the-difference-admediatorcontrol-or-adcontrol.md)。

如果你尝试在发布动态应用后在其中测试值，你的应用不会收到广告。 若要在你已发布的应用中接收广告，必须更新你的代码才能使用 Windows 开发人员中心仪表板提供的应用程序 ID 和 广告单元 ID。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md)。
 

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



 

 



<!--HONumber=Jun16_HO4-->


