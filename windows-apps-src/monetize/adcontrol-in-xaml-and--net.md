---
author: mcleanbyron
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: "了解如何使用 AdControl 类在适用于 Windows10 (UWP)、Windows8.1 或 Windows Phone 8.1 的 XAML 应用中显示横幅广告。"
title: "XAML 和 .NET 中的 AdControl"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 广告, AdControl, 广告控件, XAML, .net, 演练"
ms.openlocfilehash: be273ca4c17edb4affa5e0abb4b3317b03893280
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2017
---
# <a name="adcontrol-in-xaml-and-net"></a>XAML 和 .NET 中的 AdControl


本演练介绍如何使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 类在适用于 Windows10 (UWP)、Windows8.1 或 Windows Phone 8.1 的 XAML 应用中显示横幅广告。

有关演示如何使用 C# 和 C++ 将横幅广告添加到 XAML 应用的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

## <a name="prerequisites"></a>先决条件

* 对于 UWP 应用，请使用 Visual Studio 2015 安装 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp) 或更高版本。
* 对于 Windows8.1 或 Windows Phone 8.1 应用：使用 Visual Studio 2015 或 Visual Studio 2013 安装[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。

## <a name="code-development"></a>代码开发

1. 在 Visual Studio 中，打开项目或创建新项目。

2. 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft Advertising 库的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

1.  在**解决方案资源管理器**窗口中，右键单击**引用**，然后选择**添加引用...**

2.  在**引用管理器**中，根据你的项目类型选择以下引用之一：

    -   对于通用 Windows 平台 (UWP) 项目：展开**通用 Windows**、单击**扩展**，然后选中**适用于 XAML 的 Microsoft Advertising SDK**（版本 10.0）旁边的复选框。

    -   对于 Windows8.1 项目：展开**Windows8.1**、单击**扩展**，然后选中**适用于 Windows8.1 XAML 的广告中介 SDK** 旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

    -   对于 Windows Phone 8.1 项目：展开**Windows Phone 8.1**、单击**扩展**，然后选中**适用于 Windows Phone 8.1 XAML 的广告中介 SDK** 旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

    ![addreferences](images/13-a84c026e-b283-44f2-8816-f950a1ef89aa.png)

3.  在**引用管理器**中，单击“确定”。

4.  修改你要在其中嵌入广告的页面的 XAML，以包含 **Microsoft.Advertising.WinRT.UI** 命名空间。 例如，在由 Visual Studio 生成的默认示例应用中（即，在应用 MyAdFundedWindows10AppXAML 中），XAML 页面是 **MainPage.XAML**。

    由 Visual Studio 生成的 MainPage.xaml 文件的**页面**部分具有以下代码。

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

    添加命名空间引用 **Microsoft.Advertising.WinRT.UI**，以使 MainPage.xaml 文件的**页面**部分具有以下代码。

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      </Grid>
    </Page>
    ```

5. 在**网格**标记中，为 **AdControl** 添加代码。 将**页面**中的 [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) 和 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 属性分配给[测试模式值](test-mode-values.md)中提供的测试值。 另外还要调整控件的高度和宽度，以使其适应[横幅广告支持的广告大小](supported-ad-sizes-for-banner-ads.md)。

    > [!NOTE]
    > 每个 **AdControl** 都有一个对应的*广告单元*，我们的服务使用该广告单元来为控件提供广告，每个广告单元都包含*单元 ID* 和*应用程序 ID*。 在这些步骤中，你将为控件分配测试广告单元 ID 和应用程序 ID 值。 这些测试值只能在应用的测试版本中使用。 在将应用发布到应用商店之前，你必须在 Windows 开发人员中心[将这些测试值替换为实时值](#release)。

    完整的**网格**标记应类似如下代码。

    ``` xml
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
            AdUnitId="test"
            HorizontalAlignment="Left"
            Height="250"
            VerticalAlignment="Top"
            Width="300"/>
    </Grid>
    ```

    MainPage.xaml 文件的完整代码应如下所示。

    ``` xml
    <Page
      x:Class="MyAdFundedWindows10AppXAML.MainPage"
      xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
      xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
      xmlns:local="using:MyAdFundedWindows10AppXAML"
      xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
      xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
      xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
      mc:Ignorable="d">
      <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                  AdUnitId="test"
                  HorizontalAlignment="Left"
                  Height="250"
                  VerticalAlignment="Top"
                  Width="300"/>
      </Grid>
    </Page>
    ```

6.  编译并运行应用以查看是否带有广告。

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>使用 Windows 开发人员中心发布带有实时广告的应用

1.  在开发人员中心仪表板中，转到应用的[通过广告盈利](../publish/monetize-with-ads.md)页面，然后[创建广告单元](../monetize/set-up-ad-units-in-your-app.md)。 对于广告单元类型，请指定**横幅**。 记下广告单元 ID 和应用程序 ID。

2. 如果你的应用是适用于 Windows 10 的 UWP 应用，则可以为 **AdControl** 选择性启用广告中介，方法是配置[通过广告盈利](../publish/monetize-with-ads.md)页面的[广告中介](../publish/monetize-with-ads.md#mediation)部分的设置。 广告中介显示来自多个广告网络（包括其他付费广告网络，如 Taboola 和 Smaato）的广告及 Microsoft 应用促销活动的广告，从而使你能够最大化你的广告收益和应用促销能力。

3.  在你的代码中，将测试广告单元值（**ApplicationId** 和 **AdUnitId**）替换为你在开发人员中心生成的实时值。

4.  使用开发人员中心仪表板 [提交应用](../publish/app-submissions.md) 至应用商店。

5.  在开发人员中心仪表板中查看你的[广告性能报告](../publish/advertising-performance-report.md)。

<span id="manage" />
## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>管理你的应用中多个广告控件的广告单元

可在一个应用中使用多个 **AdControl** 对象（例如，应用中的每页可以托管不同的 **AdControl** 对象）。 在此情况下，我们建议你为每个控件分配不同的广告单元。 对每个控件使用不同的广告单元使你可以分别[配置中介设置](../publish/monetize-with-ads.md#mediation)并获取每个控件的离散[报告数据](../publish/advertising-performance-report.md)。 这还使我们的服务能够更好地优化我们为你的应用提供的广告。

> [!IMPORTANT]
> 每个广告单元都只能在一个应用中使用。 如果在多个应用中使用某个广告单元，将不为该广告单元提供广告。

## <a name="notes"></a>备注

* C#：有关如何向 **AdControl** 事件分配事件处理程序的示例，请参阅 [XAML 属性示例](xaml-properties-example.md)。 有关介绍采用 C# 编写的事件处理程序的示例代码，请参阅 [C# 中的 AdControl 事件](adcontrol-events-in-c.md)。

* C++：当前版本的 Microsoft Advertising 库支持 C++。 **AdControl** 类采用本机 C++ 实现，并且不会加载 .NET CLR。 有关演示如何在 C++ 中使用 **AdControl** 的代码示例，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

* Visual Basic：有关如何向 **AdControl** 事件分配事件处理程序的示例，请参阅 [XAML 属性示例](xaml-properties-example.md)。

* 错误处理：若要了解如何处理错误，请参阅 [AdControl 错误处理](adcontrol-error-handling.md)。

## <a name="related-topics"></a>相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)
* [为应用设置广告单元](../monetize/set-up-ad-units-in-your-app.md)
