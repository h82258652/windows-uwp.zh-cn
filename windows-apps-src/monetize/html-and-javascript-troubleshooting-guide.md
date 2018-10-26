---
author: Xansky
ms.assetid: 7a61c328-77be-4614-b117-a32a592c9efe
description: 阅读 JavaScript/HTML 应用中有关 Microsoft Advertising 库的常见开发问题的解决方案。
title: HTML 和 JavaScript 疑难解答指南
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
keywords: windows 10, uwp, 广告, advertising, AdControl, 疑难解答, HTML, javascript
ms.localizationpriority: medium
ms.openlocfilehash: 1cd19b574d6128fa1f88196c8c35f368b12c3531
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5567797"
---
# <a name="html-and-javascript-troubleshooting-guide"></a>HTML 和 JavaScript 疑难解答指南

本主题包含 JavaScript/HTML 应用中有关 Microsoft Advertising 库的常见开发问题的解决方案。

* [HTML](#html)
  * [AdControl 不显示](#html-notappearing)
  * [黑盒闪烁和消失](#html-blackboxblinksdisappears)
  * [广告不刷新](#html-adsnotrefreshing)

* [JavaScript](#js)
  * [AdControl 不显示](#js-adcontrolnotappearing)
  * [黑盒闪烁和消失](#js-blackboxblinksdisappears)
  * [广告不刷新](#js-adsnotrefreshing)

## <a name="html"></a>HTML

<span id="html-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl 不显示

1.  确保在 Package.appxmanifest 中已选择“Internet(客户端)”**** 功能。

2.  确保存在 JavaScript 参考。 如果 &lt;head&gt; 部分没有 ad.js 参考（位于 default.js 参考之后），**AdControl** 将无法显示，并且在生成期间还会发生错误。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <head>
        ...
        <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
        ...
    </head>
    ```

3.  检查应用程序 ID 和广告单元 ID。 这些 ID 必须匹配在 Windows 开发人员中心中获取的应用程序 ID 和广告单元 ID。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md#live-ad-units)。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

4.  查看 **height** 和 **width** 属性。 这些属性必须设置为[横幅广告的受支持广告大小](supported-ad-sizes-for-banner-ads.md)之一。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 50px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

5.  查看元素定位 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 必须在可视区域内。

6.  检查 **visibility** 属性。 此属性禁止设置为折叠或隐藏。 此属性可内联设置（如下所示）或在外部样式表中设置。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

7.  检查 **position** 属性。 position 属性必须设置为相应值，具体取决于元素的其他属性（例如父元素和 Z 索引的边距）。 此属性可内联设置（如下所示）或在外部样式表中设置。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

8.  检查 **z-index** 属性。 **z-index** 属性必须设置得足够高，才能使 **AdControl** 始终显示在其他元素顶部。 此属性可内联设置（如下所示）或在外部样式表中设置。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

9.  检查外部样式表。 如果属性是在 **AdControl** 元素上通过外部样式表设置的，请确保上述所有属性均已正确设置。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="visibility: visible; position: absolute; top: 1025px;
                          left: 500px; width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID'}">
    </div>
    ```

10. 查看 **AdControl** 的父元素。 如果 **AdControl** 驻留在父元素中，则父元素必须处于活动状态并且可见。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position: absolute; width: 500px; height: 500px;">
        <div id="myAd" style="position: relative; top: 0px; left: 100px;
                              width: 250px; height: 250px; z-index: 1"
             data-win-control="MicrosoftNSJS.Advertising.AdControl"
             data-win-options="{applicationId: 'ApplicationID',
                                adUnitId: 'AdUnitID'}">
        </div>
    </div>
    ```

11. 确保 **AdControl** 在视口中可见。 **AdControl** 必须可见才能正确显示广告。

12. [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 和 [AdUnitId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.adunitid) 的动态值不应在仿真器中测试。 若要确保 **AdControl** 正常运行，请使用 **ApplicationId** 和 **AdUnitId** 的[测试值](set-up-ad-units-in-your-app.md#test-ad-units)。

<span id="html-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>黑盒闪烁和消失

1.  仔细检查前面的 [AdControl 不显示](#html-notappearing)部分中的所有步骤。

2.  Handle the **onErrorOccurred** event, and use the message that is passed to the event handler to determine whether an error occurred and what type of error was thrown. 更多详细信息可在 [JavaScript 中的错误处理演练](error-handling-in-javascript-walkthrough.md)中找到。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 728px; height: 90px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger}">
    </div>
    <div style="position:absolute; width:100%; height:130px; top:300px; left:0px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    导致黑盒的最常见错误是“无广告可用”。 此错误意味着请求返回不了任何广告。

3.  **AdControl** 行为正常。 默认情况下，**AdControl** 在它无法显示广告时会折叠。 如果其他元素均是相同父元素的子元素，它们可能会移动以填充折叠 **AdControl** 的间距，并在下一次提出请求时展开。

<span id="html-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>广告不刷新

1.  查看 **isAutoRefreshEnabled** 属性。 默认情况下，此可选属性设置为 true。 在设置为 false 时，必须使用 **refresh** 方法检索其他广告。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: true}">
    </div>
    ```

2.  查看对 **refresh** 方法的调用。 当使用自动刷新时，**refresh** 无法用于检索其他广告。 当使用手动刷新时，**refresh** 应仅在最少 30 到 60 秒后调用，具体取决于设备的当前数据连接。

    此示例演示了如何使用 **refresh** 方法。 以下的 HTML 代码显示如何通过将 **isAutoRefreshEnabled** 设置为 false 来实例化 **AdControl** 的示例。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl"
         data-win-options="{ applicationId: 'ApplicationID',
                            adUnitId: 'AdUnitID',
                            onErrorOccurred: errorLogger,
                            isAutoRefreshEnabled: false}">
    </div>
    ```

    此示例演示了如何使用 **refresh** 函数。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl** 行为正常。 有时如果广告不刷新，相同的广告会连续出现多次。

<span id="js"/>

## <a name="javascript"></a>JavaScript

<span id="js-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl 不显示

1.  确保在 Package.appxmanifest 中选择“Internet (客户端)”**** 功能。

2.  确保 **AdControl** 已实例化。 如果 **AdControl** 未实例化。 它将不可用。

    以下代码段显示了实例化 **AdControl** 的示例。 此 HTML 代码显示了设置 **AdControl** UI 的示例

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    以下 JavaScript 代码显示了实例化 **AdControl** 的示例

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    app.onactivated = function (args) {
        if (args.detail.kind === activation.ActivationKind.launch) {
            if (args.detail.previousExecutionState !==
                    activation.ApplicationExecutionState.terminated)
            {
                var adDiv = document.getElementById("myAd");
                var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
                {
                    applicationId: "{ApplicationID}",
                    adUnitId: "{AdUnitID}"
                 });                
                 myAdControl.onErrorOccurred = myAdError;
            } else {
                ...
            }
        }
    }
    ```

3.  查看父元素。 父元素 **&lt;div&gt;** 必须正确分配、处于活动状态，并且可见。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var adDiv = document.getElementById("myAd");
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

4.  检查应用程序 ID 和广告单元 ID。 这些 ID 必须匹配在 Windows 开发人员中心中获取的应用程序 ID 和广告单元 ID。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md#live-ad-units)。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv, {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });  
    ```

5.  查看 **AdControl** 的父元素。 父元素必须处于活动状态并且可见。

6.  **ApplicationId** 和 **AdUnitId** 的动态值不应在仿真器中测试。 若要确保 **AdControl** 正常运行，请使用 **ApplicationId** 和 **AdUnitId** 的[测试值](set-up-ad-units-in-your-app.md#test-ad-units)。

<span id="js-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>黑盒闪烁和消失

1.  仔细检查 [AdControl 不显示](#js-adcontrolnotappearing)部分中的所有步骤。

2.  Handle the **onErrorOccurred** event, and use the message that is passed to the event handler to determine whether an error occurred and what type of error was thrown. 更多详细信息可在 [JavaScript 中的错误处理演练](error-handling-in-javascript-walkthrough.md)中找到。

    此示例展示了如何实现可报告错误消息的错误处理程序。 此 HTML 代码的代码段提供了如何设置 UI 以显示错误消息的示例。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div style="position:absolute; width:100%; height:130px; top:300px">
        <b>Ad Events</b><br />
        <div id="adEvents" style="width:100%; height:110px; overflow:auto"></div>
    </div>
    ```

    此示例展示了如何实例化 **AdControl**。 此函数将插入 app.onactivated 文件中。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
        applicationId: "{ApplicationID}",
        adUnitId: "{AdUnitID}"
    });                
    myAdControl.onErrorOccurred = myAdError;
    ```

    此示例展示了如何报告错误。 此函数将插入到 default.js 文件的自主运行的函数下面。

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    WinJS.Utilities.markSupportedForProcessing
    (
        window.errorLogger = function (sender, evt)
        {
            adEvents.innerHTML = (new Date()).toLocaleTimeString() + ": " +
            sender.element.id + " error: " + evt.errorMessage + " error code: " +
            evt.errorCode + "<br>" + adEvents.innerHTML;
        }
    );
    ```

    导致黑盒的最常见错误是“无广告可用”。 此错误意味着请求返回不了任何广告。

3.  **AdControl** 行为正常。 有时如果广告不刷新，相同的广告会连续出现多次。

<span id="js-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>广告不刷新

1.  检查 **AdControl** 的 [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) 属性是否设置为 false。 默认情况下，此可选属性设置为 **true**。 在设置为 **false** 时，必须使用 **Refresh** 方法检索其他广告。

2.  检查 [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) 方法的调用。 使用自动刷新（**IsAutoRefreshEnabled** 为 **true**）时，不能将 **Refresh** 用于检索其他广告。 使用手动刷新（**IsAutoRefreshEnabled** 为 **false**）时，根据设备的当前数据连接，只应至少在 30 到 60 秒后调用**Refresh**。

    此示例展示了如何为 **AdControl** 创建 **div**。

    > [!div class="tabbedCodeSnippets"]
    ``` html
    <div id="myAd" style="position: absolute; top: 0px; left: 0px;
                          width: 250px; height: 250px; z-index: 1"
         data-win-control="MicrosoftNSJS.Advertising.AdControl">
    </div>
    ```

    此示例显示了如何使用 **Refresh** 函数

    > [!div class="tabbedCodeSnippets"]
    ``` javascript
    var myAdControl = new MicrosoftNSJS.Advertising.AdControl(adDiv,
    {
      applicationId: "{ApplicationID}",
      adUnitId: "{AdUnitID}",
      isAutoRefreshEnabled: false
    });
    ...
    args.setPromise(WinJS.UI.processAll()
        .then(function (args) {
            window.setInterval(function()
            {
                document.getElementById("myAd").winControl.refresh();
            }, 60000)
        })
    );
    ```

3.  **AdControl** 行为正常。 有时如果广告不刷新，相同的广告会连续出现多次。

 

 
