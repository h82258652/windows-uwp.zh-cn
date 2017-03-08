---
author: mcleanbyron
ms.assetid: d074e9d5-b3e0-4f16-b1e4-02b32ac99b2c
description: "了解如何将 **AdControl** 属性分配给值。"
title: "AdControl XAML 属性示例"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, ads, 广告, AdControl, XAML, 属性"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: dbcc08b1373c7f73d5b9ebf541ec82bb01fd9df3
ms.lasthandoff: 02/07/2017

---

# <a name="adcontrol-xaml-properties-example"></a>AdControl XAML 属性示例

以下 XAML 示例演示了如何将 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 属性分配给值。 如果某个属性未设置，**AdControl** 将使用默认值来创建与应用的用户体验一致的广告。

这些值均为示例。 在你的代码中，你将设置适合你应用的函数和属性的值。

> [!div class="tabbedCodeSnippets"]
``` xml
<UI:AdControl Width="300",
    Height="250",
    AdUnitId="10865270",
    ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1",
    IsAutoRefreshEnabled="false",
    AdRefreshed="OnAdRefresh",
    ErrorOcurred="OnAdError",
    IsEngagedChanged="OnAdEngagedChanged" />
```

## <a name="related-topics"></a>相关主题

* [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)
* [GitHub 上的广告示例](http://aka.ms/githubads)

 

