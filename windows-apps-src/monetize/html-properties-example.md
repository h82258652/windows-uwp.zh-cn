---
author: mcleanbyron
ms.assetid: 5fa16a27-fdc0-43b2-84cd-8547fd4915de
description: "了解如何在 HTML 中分配 **AdControl** 属性。"
title: "AdControl HTML 属性示例"
translationtype: Human Translation
ms.sourcegitcommit: f88a71491e185aec84a86248c44e1200a65ff179
ms.openlocfilehash: 741cf19ee0310c84d1a85f4a1e82b353d88d1b9e

---

# <a name="adcontrol-html-properties-example"></a>AdControl HTML 属性示例

以下示例演示了如何在 HTML 中分配 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 属性。 **applicationId** 和 **adUnitId** 是必需属性。 其他属性和事件是可选的。 如果没有提供可选属性的值，控件将使用默认值，可创建与应用的用户体验一致的广告。

最后五行是将函数注册到 **AdControl** 事件的示例。 仅可以注册已在 JavaScript 代码中定义的函数。

这些值是示例值。 在你的代码中，你将设置这些函数和属性的值，以便它们适用于你的应用。

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl"
    data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1',
                       adUnitId: '10865270',
                       isAutoRefreshEnabled: false,
                       onAdRefreshed: myAdRefreshed,
                       onErrorOccurred: myAdError,
                       onEngagedChanged: myAdEngagedChanged,
                       onPointerDown: myPointerDown,
                       onPointerUp: myPointerUp }" />
```

## <a name="related-topics"></a>相关主题

* [HTML 5 和 JavaScript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)
* [GitHub 上的广告示例](http://aka.ms/githubads)

 



<!--HONumber=Dec16_HO2-->


