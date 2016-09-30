---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: "了解如何使用 JavaScript 以编程方式创建 **AdControl**。"
title: "使用 JavaScript 创建 AdControl"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 481f9d785181ca197debdb807bb0b0c7b4168632


---

# 使用 JavaScript 创建 AdControl


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

此示例介绍如何使用 JavaScript 以编程方式创建 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)。

## AdControl 的 HTML div

**AdControl** 需要在将显示广告的 html 页面上具有 **div**。 下面的代码提供了此类 **div** 的示例。

``` syntax
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## 用于创建 AdControl 的 JavaScript

以下示例假设你在带有 ID **myAd** 的 HTML 中使用现有的 **div**。

在 **app.onactivated** 函数中实例化 **AdControl**。

``` syntax
// TODO: This application has been newly launched. Initialize
// your application here.
var adDiv = document.getElementById("myAd");
var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "3f83fe91-d6be-434d-a0ae-7351c5a997f1",
        adUnitId: "10865270"
    });
myAdControl.isAutoRefreshEnabled = false;
myAdControl.onErrorOccurred = myAdError;
myAdControl.onAdRefreshed = myAdRefreshed;
myAdControl.onEngagedChanged = myAdEngagedChanged;
```

这些值均为示例。 在你的代码中，你将根据应用需要设置这些函数和属性的值。

如果你使用此代码，并且没有看到广告，则可以尝试将 **position:relative** 的属性插入包含 **AdControl** 的 **div** 中。 这将替代 **IFrame** 的默认设置。 广告将正确显示，除非它们由于此属性的值而没有显示。 请注意，新的广告单元可能在长达 30 分钟内不可用。

## 相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)

 

 



<!--HONumber=Jun16_HO4-->


