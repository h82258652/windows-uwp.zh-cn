---
author: mcleanbyron
ms.assetid: 2383296e-c3d7-4b49-bcd2-621391228fdb
description: "了解如何处理 AdControl 类的事件。"
title: "JavaScript 中的 AdControl 事件"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: d24030dfae92451924000ba4f1ac19cf6c4d4abe


---

# JavaScript 中的 AdControl 事件




以下示例演示了如何处理 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 类的事件。 这些示例假设你之前已向 **AdControl** 事件分配了事件处理程序。 有关如何执行此操作的详细信息，请参阅 [HTML 属性示例](html-properties-example.md)。

在 JavaScript 中，**AdControl** 事件必须包含在 [MarkSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) 函数中。 有关处理 JavaScript 中的事件的详细信息，请参阅[编码基本应用 (HTML)](https://msdn.microsoft.com/library/windows/apps/hh780660.aspx#adding-event-handlers)。

## 示例

``` syntax
WinJS.Utilities.markSupportedForProcessing(
window.myAdError = function (sender, msg) {
  // place code here for when there is an error serving an ad.
  // e.g. you may opt to show a default experience, or reclaim the div for other purposes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdRefreshed = function (sender) {
  // place code here that you wish to execute when the ad refreshes.
});

WinJS.Utilities.markSupportedForProcessing(
window.myAdEngagedChanged = function (sender) {
  if (true == sender.isEngaged) {
    // code here for when user engaged with ad, e.g. if a game, pause it.
  }
  else {
    // user no longer engaged with ad, include code to unpause.
  }
});
```

## 相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)
* [AdControl 错误处理](adcontrol-error-handling.md)
* [RoutedEventArgs 类](http://msdn.microsoft.com/library/system.windows.routedeventargs.aspx)

 

 



<!--HONumber=Aug16_HO3-->


