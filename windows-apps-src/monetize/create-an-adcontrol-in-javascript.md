---
author: mcleanbyron
ms.assetid: 48a1ef86-8514-4af8-9c93-81e869d36de7
description: "了解如何使用 JavaScript 以编程方式创建 **AdControl**。"
title: "使用 JavaScript 创建 AdControl"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 广告, 广告, AdControl, javascript,"
ms.openlocfilehash: b669925c3b630ddbfe82086231c46c951072244b
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-an-adcontrol-in-javascript"></a>使用 JavaScript 创建 AdControl




本文中的示例演示如何使用 JavaScript 以编程方式创建 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx)。 本文假定你已经把使用 **AdControl** 的必要引用添加到了项目中。 有关详细信息（包括在 HTML 标记而非 JavaScript 中详细演示创建和初始化 **AdControl**），请参阅 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。

## <a name="html-div-for-an-adcontrol"></a>AdControl 的 HTML div

**AdControl** 需要在将显示广告的 html 页面上具有 **div**。 下面的代码提供了此类 **div** 的示例。

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
    data-win-control="MicrosoftNSJS.Advertising.AdControl">
</div>
```

## <a name="javascript-for-creating-an-adcontrol"></a>用于创建 AdControl 的 JavaScript

以下示例假设你在带有 ID **myAd** 的 HTML 中使用现有的 **div**。

在 **app.onactivated** 函数中实例化 **AdControl**。

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

此示例假定你已经声明了名为 **myAdError**、**myAdRefreshed** 和 **myAdEngagedChanged** 的事件处理程序方法。

>**注意**&nbsp;&nbsp;此示例中显示的 *applicationId* 和 *adUnitId* 值是[测试模式值](test-mode-values.md)。 在提交应用之前，必须[将测试值替换为实时值](set-up-ad-units-in-your-app.md)。

如果你使用此代码，并且没有看到广告，则可以尝试将 **position:relative** 的属性插入包含 **AdControl** 的 **div** 中。 这将替代 **IFrame** 的默认设置。 广告将正确显示，除非它们由于此属性的值而没有显示。 请注意，新的广告单元可能在长达 30 分钟内不可用。

## <a name="related-topics"></a>相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)

 

 
