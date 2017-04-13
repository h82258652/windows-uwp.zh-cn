---
author: mcleanbyron
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: "了解如何处理由 Microsoft Advertising 库中的 AdControl 类生成的错误。"
title: "Advertising 库的错误处理"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 广告, AdControl, 错误处理, javascript, XAML, c#"
ms.openlocfilehash: 7c65f424341517072b06aaba30929f17303dcf1f
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="error-handling-with-the-advertising-libraries"></a>Advertising 库的错误处理

本主题提供有关如何处理由 Microsoft Advertising 库中的 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 类所生成错误的基本信息。

<span id="bkmk-javascript"/>
## <a name="javascripthtml-apps"></a>JavaScript/HTML 应用

若要在 JavaScript 应用中处理 **AdControl** 错误：

1.  将 **onErrorOccurred** 事件分配给事件处理程序。

2.  对事件处理程序进行编码。

在 **AdControl** 的 **div** 的 **data-win-options** 属性中设置 **onErrorOccurred** 事件处理程序。 在以下示例中，将 **onErrorOccurred** 事件设置为由名为 **errorLogger** 的函数进行处理。

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: 'd25517cb-12d4-4699-8bdc-52040c712cab', adUnitId: 'ADPT33', onErrorOccurred: errorLogger}">
</div>
```

该错误处理程序函数具有声明性，必须括在 [markSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) 函数中。

当发生 **AdControl** 错误时，该错误处理程序会捕获 JavaScript 错误对象。 该错误对象向错误处理程序提供两个参数。 有关详细信息，请参阅[异步 Windows 运行时方法的特殊错误属性](http://msdn.microsoft.com/library/windows/apps/hh994690.aspx)。

以下是一个名为 **errorLogger** 的错误处理函数的示例，该函数可处理 **onErrorOccurred** 事件。

> [!div class="tabbedCodeSnippets"]
``` javascript
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage +
    " error code: " + evt.errorCode + \n");
});
```

有关在 JavaScript 中演示 **AdControl** 错误处理的演练，请参阅 [JavaScript 中的错误处理演练](error-handling-in-javascript-walkthrough.md)。

<span id="bkmk-dotnet"/>
## <a name="xaml-apps"></a>XAML 应用

若要在 XAML 应用中处理 **AdControl** 错误：

* 将 **ErrorOccurred** 事件分配到事件处理程序委托的名称。

* 对错误事件处理委托进行编码，以便它可以处理两个参数：发送者的 **Object** 和 **AdErrorEventArgs** 对象。

以下是一个将名为 **OnAdError** 的委托分配给 **ErrorOccurred** 事件的示例。

> [!div class="tabbedCodeSnippets"]
``` csharp
this.ErrorOccurred = OnAdError;
```

以下是将错误信息写入 Visual Studio 中的输出窗口的 **OnAdError** 委托的示例定义。

> [!div class="tabbedCodeSnippets"]
``` csharp
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error +
        " ErrorCode: " + e.ErrorCode.ToString());
}
```

有关在 XAML 和 C# 中演示 **AdControl** 错误处理的演练，请参阅 [XAML/C# 中的错误处理演练](error-handling-in-xamlc-walkthrough.md)。

 

 
