---
author: mcleanbyron
ms.assetid: 2fba38c4-11be-4058-bfa3-5f979390791c
description: 了解如何处理 AdControl 类的事件。
title: C 中的 AdControl 事件#
---

# C\ 中的 AdControl 事件# #  


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

以下示例演示了如何处理 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 类的事件。 这些示例假设你之前已向 XAML 中的 **AdControl** 事件分配了事件处理程序。 有关如何执行此操作的详细信息，请参阅 [XAML 属性示例](xaml-properties-example.md)。

有关处理 C# 中的事件的详细信息，请参阅[事件和路由事件概述（使用 C#/VB/C++ 和 XAML 的通用 Windows 应用）](http://msdn.microsoft.com/library/windows/apps/hh758286)。

## 示例


``` syntax
private void OnAdError(object sender, AdErrorEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}

private void OnAdRefresh(object sender, RoutedEventArgs e) {
  // place code here that you wish to execute when the ad refreshes.
 return;
}

private void OnAdEngagedChanged(object sender, RoutedEventArgs e) {
  // place code here for when there is an error serving an ad
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes
  return;
}
```

## 相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)
* [AdControl 错误处理](adcontrol-error-handling.md)
* [RoutedEventArgs 类](http://msdn.microsoft.com/en-us/library/system.windows.routedeventargs.aspx)

 

 


<!--HONumber=May16_HO2-->


