---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: "了解如何处理 AdControl 类的事件。"
title: "C 中的 AdControl 事件#"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: e25e0f915c0b9b6ec2423d2a95386b45b4502253

---

# <a name="adcontrol-events-in-c"></a>C\ 中的 AdControl 事件# #  


以下示例演示了下面 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 事件（[ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.erroroccurred.aspx)、[AdRefreshed](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.adrefreshed.aspx) 和 [IsEngagedChanged](https://msdn.microsoft.com/library/windows/apps/xaml/microsoft.advertising.winrt.ui.adcontrol.isengagedchanged.aspx)）的基本事件处理程序。 这些示例假设你已向 XAML 代码中的事件分配了事件处理程序。 有关如何执行此操作的详细信息，请参阅 [XAML 属性示例](xaml-properties-example.md)。

有关处理 C# 中的事件的详细信息，请参阅 [事件和路由事件概述（使用 C#/VB/C++ 和 XAML 的通用 Windows 应用）](http://msdn.microsoft.com/library/windows/apps/hh758286)。

## <a name="examples"></a>示例

> [!div class="tabbedCodeSnippets"]
[!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MainPage.xaml.cs#EventHandlers)]

## <a name="related-topics"></a>相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)
* [AdControl 错误处理](adcontrol-error-handling.md)
* [RoutedEventArgs 类](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Dec16_HO2-->


