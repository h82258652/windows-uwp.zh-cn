---
author: mcleanbyron
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: 了解如何处理由 Microsoft Advertising 库中的 AdControl 类生成的错误。
title: 处理广告错误
ms.author: mcleans
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 广告, 投放广告, 错误处理, javascript, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: fb60f870aa220a123bab185ef98ccca1f6a8881a
ms.sourcegitcommit: 834992ec14a8a34320c96e2e9b887a2be5477a53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2018
ms.locfileid: "1880968"
---
# <a name="handle-ad-errors"></a>处理广告错误

The [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx),  [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx), and **NativeAdsManagerV2** classes each have an **ErrorOccurred** event that is raised if an ad-related error occurs. 应用代码可以处理此事件并检查事件参数对象的 [ErrorCode](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.aderroreventargs.errorcode.aspx) 和  [ErrorMessage](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.aderroreventargs.errormessage.aspx) 属性，以帮助确定错误原因。

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>XAML 应用

若要在 XAML 应用中处理与广告相关的错误：

1. Assign the **ErrorOccurred** event of your **AdControl**, **InterstitialAd**, or **NativeAdsManagerV2** object to the name of an event handler delegate.

2. 对错误事件处理委托进行编码，以便它可以处理两个参数：发送者的 **Object** 和 [AdErrorEventArgs](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.aderroreventargs.aspx) 对象。

Here is an example that assigns a delegate named **OnAdError** to the **ErrorOccurred** event of an **AdControl** object named *myBannerAdControl*.

> [!div class="tabbedCodeSnippets"]
``` csharp
myBannerAdControl.ErrorOccurred = OnAdError;
```

以下是 **OnAdError** 委托的示例定义，该委托将错误信息写入 Visual Studio 中的输出窗口。

> [!div class="tabbedCodeSnippets"]
``` csharp
private void OnAdError(object sender, AdErrorEventArgs e)
{
    System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name + "): " + e.Error +
        " ErrorCode: " + e.ErrorCode.ToString());
}
```

有关在 XAML 和 C# 中演示 **AdControl** 错误处理的演练，请参阅 [XAML/C# 中的错误处理演练](error-handling-in-xamlc-walkthrough.md)。

<span id="bkmk-javascript"/>

## <a name="javascripthtml-apps"></a>JavaScript/HTML 应用

To handle **ErrorOccur** errors in a JavaScript app:

1.  Assign the **onErrorOccurred** event to an event handler.

2.  对事件处理程序进行编码。

Here is an example that assigns an event handler named **errorLogger** to the **ErrorOccurred** event of an **AdControl** object.

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

该错误处理程序函数具有声明性，必须括在 [markSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) 函数中。

当发生错误时，该错误处理程序会捕获 JavaScript 错误对象。 该错误对象向错误处理程序提供两个参数。 有关详细信息，请参阅[异步 Windows 运行时方法的特殊错误属性](http://msdn.microsoft.com/library/windows/apps/hh994690.aspx)。

Here is an example of an error handling function named **errorLogger** that handles the **onErrorOccurred** event.

> [!div class="tabbedCodeSnippets"]
``` javascript
WinJS.Utilities.markSupportedForProcessing(
window.errorLogger = function (sender, evt) {
    console.log(new Date()).toLocaleTimeString() + ": " + sender.element.id + " error: " + evt.errorMessage +
    " error code: " + evt.errorCode + \n");
});
```

有关在 JavaScript 中演示 **AdControl** 错误处理的演练，请参阅 [JavaScript 中的错误处理演练](error-handling-in-javascript-walkthrough.md)。
