---
author: Xansky
ms.assetid: 141900dd-f1d3-4432-ac8b-b98eaa0b0da2
description: 了解 XAML 应用中有关 Microsoft Advertising 库的常见开发问题的解决方案。
title: XAML 和 C# 疑难解答指南
ms.author: mhopkins
ms.date: 08/23/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ads, 广告, AdControl, 疑难解答, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: 4f3ebd2877dd786161c76d89e17a2667c07e65b3
ms.sourcegitcommit: 310a4555fedd4246188a98b31f6c094abb33ec60
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/19/2018
ms.locfileid: "5129754"
---
# <a name="xaml-and-c-troubleshooting-guide"></a>XAML 和 C# 疑难解答指南

本主题包含 XAML 应用中有关 Microsoft Advertising 库的常见开发问题的解决方案。

* [XAML](#xaml)
  * [AdControl 不显示](#xaml-notappearing)
  * [黑盒闪烁和消失](#xaml-blackboxblinksdisappears)
  * [广告不刷新](#xaml-adsnotrefreshing)

* [C#](#csharp)
  * [AdControl 不显示](#csharp-adcontrolnotappearing)
  * [黑盒闪烁和消失](#csharp-blackboxblinksdisappears)
  * [广告不刷新](#csharp-adsnotrefreshing)

<span id="xaml"/>

## <a name="xaml"></a>XAML

<span id="xaml-notappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl 不显示

1.  确保在 Package.appxmanifest 中选择“Internet（客户端）”**** 功能。

2.  检查应用程序 ID 和广告单元 ID。 这些 ID 必须匹配在 Windows 开发人员中心中获取的应用程序 ID 和广告单元 ID。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md#live-ad-units)。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}" ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

3.  查看 **Height** 和 **Width** 属性。 这些属性必须设置为[横幅广告的受支持广告大小](supported-ad-sizes-for-banner-ads.md)之一。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90" />
    ```

4.  检查元素位置。 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 必须在可视区域内。

5.  检查 **Visibility** 属性。 可选 **Visibility** 属性禁止设置为折叠或隐藏。 此属性可在内联（如下所示）或在外部样式表中设置。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Visibility="Visible"
                  Width="728" Height="90" />
    ```

6.  检查 **AdControl** 的父元素。 如果 **AdControl** 元素驻留在父元素中，则父元素必须处于活动状态且可见。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <StackPanel>
        <UI:AdControl AdUnitId="{AdUnitID}"
                      ApplicationId="{ApplicationID}"
                      Width="728" Height="90" />
    </StackPanel>
    ```

7.  确保 **AdControl** 在视口中可见。 **AdControl** 必须可见才能正确显示广告。

8.  **ApplicationId** 和 **AdUnitId** 的动态值不应在仿真器中测试。 若要确保 **AdControl** 正常运行，请使用 **ApplicationId** 和 **AdUnitId** 的[测试值](set-up-ad-units-in-your-app.md#test-ad-units)。

<span id="xaml-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>黑盒闪烁和消失

1.  仔细检查之前[未显示的 AdControl](#xaml-notappearing) 部分中的所有步骤。

2.  处理 **ErrorOccurred** 事件，并使用传递到事件处理程序的消息确定是否发生了错误以及引发了何种错误。 有关详细信息，请参阅 [XAML/C# 演练中的错误处理](error-handling-in-xamlc-walkthrough.md)。

    此示例演示了 **ErrorOccurred** 事件处理程序。 第一个代码段是 XAML UI 标记。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  ErrorOccurred="adControl_ErrorOccurred" />
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    此示例演示了相应的 C# 代码。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    private void adControl_ErrorOccurred(object sender,               
        Microsoft.Advertising.WinRT.UI.AdErrorEventArgs e)
    {
        TextBlock1.Text = e.ErrorMessage;
    }
    ```

    导致黑盒的最常见错误是“无可用广告”。 此错误意味着请求返回不了任何广告。

3.  [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 行为正常。

    默认情况下，**AdControl** 在它无法显示广告时会折叠。 如果其他元素均是相同父元素的子元素，它们可能会移动以填充折叠 **AdControl** 的间距，并在下一次提出请求时展开。

<span id="xaml-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>广告不刷新

1.  检查 [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled) 属性。 默认情况下，此可选属性设置为 **True**。 在设置为 **False** 时，必须使用 [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh) 方法检索其他广告。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl AdUnitId="{AdUnitID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="True" />
    ```

2.  检查 **Refresh** 方法的调用。 当使用自动刷新时，**Refresh** 无法用于检索其他广告。 当使用手动刷新时，**Refresh** 应仅在最少 30 到 60 秒后调用，具体取决于设备的当前数据连接。

    以下代码段显示了如何使用 **Refresh** 方法的示例。 第一个代码段是 XAML UI 标记。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <UI:AdControl x:Name="adControl1"
                  AdUnitId="{AdUnit_ID}"
                  ApplicationId="{ApplicationID}"
                  Width="728" Height="90"
                  IsAutoRefreshEnabled="False" />
    ```

    此代码片段显示了隐藏在 UI 标记后的 C# 代码的示例。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    public RefreshAds()
    {
        var timer = new DispatcherTimer() { Interval = TimeSpan.FromSeconds(60) };
        timer.Tick += (s, e) => adControl1.Refresh();
        timer.Start();
    }
    ```

3.  **AdControl** 行为正常。 有时如果广告不刷新，相同的广告会连续出现多次。

<span id="csharp"/>

## <a name="c"></a>C\# #

<span id="csharp-adcontrolnotappearing"/>

### <a name="adcontrol-not-appearing"></a>AdControl 不显示

1.  确保在 Package.appxmanifest 中选择“Internet (客户端)”**** 功能。

2.  确保 **AdControl** 已实例化。 如果 **AdControl** 未实例化，它将不可用。

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet1)]

3.  检查应用程序 ID 和广告单元 ID。 这些 ID 必须匹配在 Windows 开发人员中心中获取的应用程序 ID 和广告单元 ID。 有关详细信息，请参阅[在应用中设置广告单元](set-up-ad-units-in-your-app.md#live-ad-units)。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    ```

4.  检查 **Height** 和 **Width** 参数。 这些属性必须设置为[横幅广告的受支持广告大小](supported-ad-sizes-for-banner-ads.md)之一。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;adControl.Width = 728;
    ```

5.  确保 **AdControl** 已添加到父元素。 若要显示，**AdControl** 必须作为子控件添加到父控件（例如，**StackPanel** 或 **Grid**）。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    ContentPanel.Children.Add(adControl);
    ```

6.  检查 **Margin** 参数。 **AdControl** 必须在可视区域内。

7.  检查 **Visibility** 属性。 可选 **Visibility** 属性必须设置为 **Visible**。

    > [!div class="tabbedCodeSnippets"]
    ``` cs
    adControl = new AdControl();
    adControl.ApplicationId = "{ApplicationID}";
    adControl.AdUnitId = "{AdUnitID}";
    adControl.Height = 90;
    adControl.Width = 728;
    adControl.Visibility = System.Windows.Visibility.Visible;
    ```

8.  检查 **AdControl** 的父元素。 父元素必须处于活动状态并且可见。

9. **ApplicationId** 和 **AdUnitId** 的动态值不应在仿真器中测试。 若要确保 **AdControl** 正常运行，请使用 **ApplicationId** 和 **AdUnitId** 的[测试值](set-up-ad-units-in-your-app.md#test-ad-units)。

<span id="csharp-blackboxblinksdisappears"/>

### <a name="black-box-blinks-and-disappears"></a>黑盒闪烁和消失

1.  仔细检查上述 [AdControl 未显示](#csharp-adcontrolnotappearing)部分中的所有步骤。

2.  处理 **ErrorOccurred** 事件，并使用传递到事件处理程序的消息确定是否发生了错误以及引发了何种错误。 有关详细信息，请参阅 [XAML/C# 演练中的错误处理](error-handling-in-xamlc-walkthrough.md)。

    以下示例显示了实现错误调用所需的基本代码。 此 XAML 代码定义用来显示错误消息的 **TextBlock**。

    > [!div class="tabbedCodeSnippets"]
    ``` xml
    <TextBlock x:Name="TextBlock1" TextWrapping="Wrap" Width="500" Height="250" />
    ```

    此 C# 代码检索错误消息，并将其显示在 **TextBlock** 中。

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet2)]

    导致黑盒的最常见错误是“无广告可用”。 此错误意味着请求返回不了任何广告。

3.  **AdControl** 行为正常。 有时如果广告不刷新，相同的广告会连续出现多次。

<span id="csharp-adsnotrefreshing"/>

### <a name="ads-not-refreshing"></a>广告不刷新

1.  检查 **AdControl** 的 [IsAutoRefreshEnabled](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.isautorefreshenabled.aspx) 属性是否设置为 false。 默认情况下，此可选属性设置为 **true**。 在设置为 **false** 时，必须使用 **Refresh** 方法检索其他广告。

2.  检查 [Refresh](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.refresh.aspx) 方法的调用。 使用自动刷新（**IsAutoRefreshEnabled** 为 **true**）时，不能将 **Refresh** 用于检索其他广告。 使用手动刷新（**IsAutoRefreshEnabled** 为 **false**）时，根据设备的当前数据连接，只应至少在 30 到 60 秒后调用**Refresh**。

    以下示例展示了如何调用 **Refresh** 方法。

    > [!div class="tabbedCodeSnippets"]
    [!code-cs[AdControl](./code/AdvertisingSamples/AdControlSamples/cs/MiscellaneousSnippets.cs#Snippet3)]

3.  **AdControl** 行为正常。 有时如果广告不刷新，相同的广告会连续出现多次。

 

 
