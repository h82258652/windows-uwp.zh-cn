---
author: mcleanbyron
ms.assetid: 08b4ae43-69e8-4424-b3c0-a07c93d275c3
description: 了解如何在应用中捕获 AdControl 错误。
title: JavaScript 演练中的错误处理
ms.author: mcleans
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 广告, AdControl, 错误处理, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 6b6d8e2b9e4d2e61901bd7de304134e5236af672
ms.sourcegitcommit: 834992ec14a8a34320c96e2e9b887a2be5477a53
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/14/2018
ms.locfileid: "1880928"
---
# <a name="error-handling-in-javascript-walkthrough"></a>JavaScript 演练中的错误处理

本演练介绍如何在 JavaScript 应用中捕获与广告相关的错误。 本演练使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 显示横幅广告，但其中的普通概念也适用于间隙广告和本机广告。

这些示例假定你拥有一个 JavaScript 应用，它包含一个 **AdControl**。 有关演示如何向你的应用添加 **AdControl** 的分步说明，请参阅 [HTML 5 和 Javascript 中的 AdControl](adcontrol-in-html-5-and-javascript.md)。 有关演示如何将横幅广告添加到 JavaScript/HTML 应用的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

1.  In the default.html file, add a value for the **onErrorOccurred** event where you define the **data-win-options** in the **div** for the **AdControl**. 在 default.html 文件中找到以下代码。
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```
    Following the **adUnitId** attribute, add the value for the **onErrorOccurred** event.
    ``` HTML
    <div id="myAd" style="position: absolute; top: 53px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test', onErrorOccurred: errorLogger}">
  </div>
  ```

2.  创建将显示文本的 **div**，以便你可以看到所生成的消息。 为此，在 **div** 后面添加用于 **myAd** 的以下代码。
    ``` HTML
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

3.  创建将触发错误事件的 **AdControl**。 对于应用中的所有 **AdControl** 对象，只可以有一个应用程序 ID。 因此创建具有不同应用程序 ID 的其他对象将在运行时触发错误。 为此，在先前添加的 **div** 部分的后面，将以下代码添加到 default.html 页面的正文中。
    ``` HTML
    <!-- Because only one applicationId can be used, the following ad control will fire an error event. -->
    <div id="liveAd" style="position: absolute; top:500px; left:0px; width:480px; height:80px"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '00000000-0000-0000-0000-000000000000', adUnitId: 'test', onErrorOccurred: errorLogger }" >
    </div>
    ```

4.  在项目的 default.js 文件中，在默认的初始化函数的后面，需要添加用于 **errorLogger** 的事件处理程序。 滚动到文件末尾，在最后一个分号的后面就是你要添加以下代码的位置。
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing(
    window.errorLogger = function (sender, evt) {
        adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
        sender.element.id + " error: " + evt.errorMessage + " error code: " +
        evt.errorCode + "<br>" + adEvents.innerHTML;
        console.log("errorhandler hit. \n");
    });
    ```

5.  生成并运行该文件。 你将看到来自你先前生成的示例应用中的原始广告，以及该广告下描述错误的文本。 你将不会看到具有 **liveAd** 的 ID 的广告。

## <a name="related-topics"></a>相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)
