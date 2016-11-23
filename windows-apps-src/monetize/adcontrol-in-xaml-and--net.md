---
author: mcleanbyron
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: "了解如何使用 AdControl 类在适用于 Windows10 (UWP)、Windows8.1 或 Windows Phone 8.1 的 XAML 应用中显示横幅广告。"
title: "XAML 和 .NET 中的 AdControl"
translationtype: Human Translation
ms.sourcegitcommit: 35f07c73a72e5242d59c6b45e6d5b4ac62f40741
ms.openlocfilehash: 0652bd1c3e52c9026b26e14b2475a4b34997ac91

---

# XAML 和 .NET 中的 AdControl




本演练介绍如何使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 类在适用于 Windows10 (UWP)、Windows8.1 或 Windows Phone 8.1 的 XAML 应用中显示横幅广告。 本演练不使用 **AdMediatorControl** 或广告中介。

有关演示如何使用 C# 和 C++ 将横幅广告添加到 XAML 应用的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

## 先决条件

* 对于 UWP 应用：使用 Visual Studio 2015 安装 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)。
* 对于 Windows8.1 或 Windows Phone 8.1 应用：使用 Visual Studio 2015 或 Visual Studio 2013 安装[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。

## 代码开发

1. 在 Visual Studio 中，打开项目或创建新项目。

2. 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft Advertising 库的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

1.  在“解决方案资源管理器”窗口中，右键单击“引用”，然后选择“添加引用...”

2.  在“引用管理器”中，根据你的项目类型选择以下引用之一：

    -   对于通用 Windows 平台 (UWP) 项目：展开“通用 Windows”、单击“扩展”，然后选中“适用于 XAML 的 Microsoft Advertising SDK”（版本 10.0）旁边的复选框。

    -   对于 Windows8.1 项目：展开“Windows8.1”、单击“扩展”，然后选中“适用于 Windows8.1 XAML 的广告中介 SDK”旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

    -   对于 Windows Phone 8.1 项目：展开“Windows Phone 8.1”、单击“扩展”，然后选中“适用于 Windows Phone 8.1 XAML 的广告中介 SDK”旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

  ![addreferences](images/13-a84c026e-b283-44f2-8816-f950a1ef89aa.png)

    > **注意** 此图像适用于生成 Windows10 UWP 项目的 Visual Studio 2015。 如果你正在生成 Windows8.1 或 Windows Phone 8.1 应用，或正在使用 Visual Studio 2013，你的屏幕看起来有所不同。

3.  在“引用管理器”中，单击“确定”。
4.  修改你要在其中嵌入广告的页面的 XAML，以包含 **Microsoft.Advertising.WinRT.UI** 命名空间。 例如，在由 Visual Studio 生成的默认示例应用中（即，在应用 MyAdFundedWindows10AppXAML 中），XAML 页面是 **MainPage.XAML**。

    由 Visual Studio 生成的 MainPage.xaml 文件的**页面**部分具有以下代码。

    ``` syntax
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

    ``` syntax
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

5.  在“网格”标记中，为 **AdControl** 添加代码。

    1.  将**页面**中的 [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) 和 [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) 属性分配给[测试模式值](test-mode-values.md)中提供的测试值。

        > **注意** 在提交应用之前，你需要将测试值替换为实时值。

    2.  调整控件的高度和宽度，以使其适应[横幅广告支持的广告大小](supported-ad-sizes-for-banner-ads.md)。

    完整的“网格”标记看起来像此代码。

    ``` syntax
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">

            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
    </Grid>
    ```

    MainPage.xaml 文件的完整代码应如下所示。

    ``` syntax
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
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
        </Grid>
    </Page>
    ```

6.  编译并运行应用以查看是否带有广告。

## 使用 Windows 开发人员中心发布带有实时广告的应用


1.  在开发人员中心仪表板中，转到应用的“盈利”&gt;“利用广告来盈利”页面，然后[创建独立的 Microsoft Advertising 单元](../publish/monetize-with-ads.md)。 对于广告单元类型，请指定“横幅”。 记下广告单元 ID 和应用程序 ID。

2.  在你的代码中，将测试广告单元值（**ApplicationId** 和 **AdUnitId**）替换为你在开发人员中心生成的实时值。

3.  使用开发人员中心仪表板[将应用提交](../publish/app-submissions.md)到应用商店。

4.  在开发人员中心仪表板中查看你的[广告性能报告](../publish/advertising-performance-report.md)。

## 注意

* C#：有关如何向 **AdControl** 事件分配事件处理程序的示例，请参阅 [XAML 属性示例](xaml-properties-example.md)。 有关介绍采用 C# 编写的事件处理程序的示例代码，请参阅 [C# 中的 AdControl 事件](adcontrol-events-in-c.md)。

* C++：当前版本的 Microsoft Advertising 库支持 C++。 **AdControl** 类采用本机 C++ 实现，并且不会加载 .NET CLR。 有关演示如何在 C++ 中使用 **AdControl** 的代码示例，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

* Visual Basic：有关如何向 **AdControl** 事件分配事件处理程序的示例，请参阅 [XAML 属性示例](xaml-properties-example.md)。

* 错误处理：若要了解如何处理错误，请参阅 [AdControl 错误处理](adcontrol-error-handling.md)。

## 相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)

 



<!--HONumber=Nov16_HO1-->


