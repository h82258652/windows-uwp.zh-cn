---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: "了解如何在 HTML 中分配 **AdControl** 属性。"
title: "HTML 属性示例"
translationtype: Human Translation
ms.sourcegitcommit: 5bf07d3001e92ed16931be516fe059ad33c08bb9
ms.openlocfilehash: 1898ed2ccad74ac33c5130c627363e0a9daebceb


---

# HTML 属性示例




以下示例演示了如何在 HTML 中分配 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 属性。 **applicationId** 和 **adUnitId** 是必需属性。 其他属性和事件是可选的。 如果没有提供可选属性的值，控件将使用默认值，可创建与应用的用户体验一致的广告。

最后五行是将函数注册到 **AdControl** 事件的示例。 仅可以注册已在 JavaScript 代码中定义的函数。

这些值是示例值。 在你的代码中，你将设置这些函数和属性的值，以便它们适用于你的应用。

``` syntax
data-win-control="MicrosoftNSJS.Advertising.AdControl"
data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                    adUnitId: '10865270',
                    isAutoRefreshEnabled: false,
                    onAdRefreshed: myAdRefreshed,
                    onErrorOccurred: myAdError,
                    onEngagedChanged: myAdEngagedChanged,
                    onPointerDown: myPointerDown,
                    onPointerUp: myPointerUp }"
```

## 相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)

 



<!--HONumber=Aug16_HO3-->


