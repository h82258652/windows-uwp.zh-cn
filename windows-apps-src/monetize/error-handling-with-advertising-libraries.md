---
author: Xansky
ms.assetid: cb7380d0-bc14-4936-aa1c-206304b3dc70
description: 了解如何处理由 Microsoft Advertising 库中的 AdControl 类生成的错误。
title: 处理广告错误
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 广告, 投放广告, 错误处理, javascript, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: 49748a616654ae69c496dca74b25fd5e925e80ee
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5401109"
---
# <a name="handle-ad-errors"></a>处理广告错误

[AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol)、[InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 和 [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) 类均具有 **ErrorOccurred** 事件，该时间在发生广告相关错误时引发。 应用代码可以处理此事件并检查事件参数对象的 [ErrorCode](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errorcode) 和  [ErrorMessage](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs.errormessage) 属性，以帮助确定错误原因。

<span id="bkmk-dotnet"/>

## <a name="xaml-apps"></a>XAML 应用

若要在 XAML 应用中处理与广告相关的错误：

1. 将 **AdControl**、**InterstitialAd** 或 **NativeAdsManagerV2** 对象的 **ErrorOccurred** 事件分配到事件处理程序委托的名称。

2. 对错误事件处理委托进行编码，以便它可以处理两个参数：发送者的 **Object** 和 [AdErrorEventArgs](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.aderroreventargs) 对象。

以下示例中，一个名为 **OnAdError** 的委托被分配给名为 **myBannerAdControl** 的 **AdControl** 对象的 *ErrorOccurred* 事件。

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

若要在 JavaScript 应用中处理 **ErrorOccur** 错误：

1.  将 **onErrorOccurred** 事件分配给事件处理程序。

2.  对事件处理程序进行编码。

以下示例中，一个名为 **errorLogger** 的事件处理程序被分配到 **AdControl** 对象的 **ErrorOccurred** 事件。

> [!div class="tabbedCodeSnippets"]
``` html
<div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 250px; height: 250px; z-index: 1"
     data-win-control="MicrosoftNSJS.Advertising.AdControl"
     data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
</div>
```

该错误处理程序函数具有声明性，必须括在 [markSupportedForProcessing](http://msdn.microsoft.com/library/windows/apps/Hh967819.aspx) 函数中。

当发生错误时，该错误处理程序会捕获 JavaScript 错误对象。 该错误对象向错误处理程序提供两个参数。 有关详细信息，请参阅[异步 Windows 运行时方法的特殊错误属性](http://msdn.microsoft.com/library/windows/apps/hh994690.aspx)。

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
