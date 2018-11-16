---
author: Xansky
ms.assetid: cf0d2709-21a1-4d56-9341-d4897e405f5d
description: 了解如何在应用中捕获 AdControl 错误。
title: XAML/C# 演练中的错误处理
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
keywords: windows 10, uwp, 广告, 投放广告, 错误处理, XAML, c#
ms.localizationpriority: medium
ms.openlocfilehash: be101f5ec189d822bc9704b435f4a098b61f57ac
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6973039"
---
# <a name="error-handling-in-xamlc-walkthrough"></a>XAML/C# 演练中的错误处理

本演练介绍如何在应用中捕获与广告相关的错误。 本演练使用 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 显示横幅广告，但其中的普通概念也适用于间隙广告和本机广告。

这些示例假定你拥有一个 XAML/C# 应用，它包含一个 **AdControl**。 有关演示如何向你的应用添加 **AdControl** 的分步说明，请参阅 [XAML 和 .NET 中的 AdControl](adcontrol-in-xaml-and--net.md)。 

1.  在 MainPage.xaml 文件中，找到 **AdControl** 的定义。 该代码如下所示。
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300" />
    ```

2.   After the **Width** property, but before the closing tag, assign a name of an error event handler to the [ErrorOccurred](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.erroroccurred) event. 在本演练中，错误事件处理程序的名称为 **OnAdError**。
    ``` xml
    <UI:AdControl
      ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
      AdUnitId="test"
      HorizontalAlignment="Left"
      Height="250"
      Margin="10,10,0,0"
      VerticalAlignment="Top"
      Width="300"
      ErrorOccurred="OnAdError"/>
    ```

3.  若要生成运行时错误，请创建具有不同应用程序 ID 的第二个 **AdControl**。 因为应用中的所有 **AdControl** 对象必须使用相同的应用程序 ID，所以创建具有不同应用程序 ID 的其他 **AdControl** 会引发错误。

    仅在 MainPage.xaml 中的第一个 **AdControl** 后面，定义第二个 **AdControl**，然后将 [ApplicationId](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol.applicationid) 属性设置为零 (“0”)。
    ``` xml
    <UI:AdControl
        ApplicationId="0"
        AdUnitId="test"
        HorizontalAlignment="Left"
        Height="250"
        Margin="10,265,0,0"
        VerticalAlignment="Top"
        Width="300"
        ErrorOccurred="OnAdError" />
    ```

4.  在 MainPage.xaml.cs 中，将以下 **OnAdError** 事件处理程序添加到 **MainPage** 类。 此事件处理程序会将信息写入 Visual Studio **输出**窗口。
    ``` csharp
    private void OnAdError(object sender, AdErrorEventArgs e)
    {
        System.Diagnostics.Debug.WriteLine("AdControl error (" + ((AdControl)sender).Name +
            "): " + e.ErrorMessage + " ErrorCode: " + e.ErrorCode.ToString());
    }
    ```

4.  生成并运行该项目。 在应用运行后，你将在 Visual Studio 的**输出**窗口中看到与以下内容类似的消息。
    ```
    AdControl error (): MicrosoftAdvertising.Shared.AdException: all ad requests must use the same application ID within a single application (0, d25517cb-12d4-4699-8bdc-52040c712cab) ErrorCode: ClientConfiguration
    ```

## <a name="related-topics"></a>相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)
