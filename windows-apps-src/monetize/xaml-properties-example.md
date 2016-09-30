---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: "了解如何将 **AdControl** 属性分配给值。"
title: "XAML 属性示例"
translationtype: Human Translation
ms.sourcegitcommit: cf695b5c20378f7bbadafb5b98cdd3327bcb0be6
ms.openlocfilehash: 43d579d2d0a92a8f03f17efa2ec42707357e99f9


---

# XAML 属性示例


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

以下 XAML 示例演示了如何将 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 属性分配给值。 如果某个属性未设置，**AdControl** 将使用默认值来创建与应用的用户体验一致的广告。

这些值均为示例。 在你的代码中，你将根据应用的需要设置这些函数和属性的值。

``` syntax
Width="300",
Height="250",
AdUnitId="10865270",
ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
IsAutoRefreshEnabled="false",
AdRefreshed="OnAdRefresh",
ErrorOcurred="OnAdError",
IsEngagedChanged="OnAdEngagedChanged"
```

## 相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)

 



<!--HONumber=Jun16_HO4-->


