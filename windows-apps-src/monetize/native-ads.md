---
author: mcleanbyron
description: "了解如何向 UWP 应用中添加本机广告。"
title: "本机广告"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, uwp, 广告, 广告控件, 本机广告"
ms.openlocfilehash: 47a69e48f04c670a462c34083af1117d2c7908d8
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2017
---
# <a name="native-ads"></a>本机广告

本机广告是基于组件的广告格式，其中每一个广告创意元素（如标题、图像、说明和行动号召文字）都作为单独的元素提供给你。 你可以使用自己的字体、颜色、动画和其他 UI 组件将这些元素集成到你的应用中，从而组合成一种与你的应用和谐共融的用户体验，同时从广告中获得较高收益。

对于广告商而言，本机广告提供了高绩效的展示位置，因为广告体验紧密集成在应用中，用户倾向于与此类广告进行更多的互动。

> [!NOTE]
> 要向应用商店中的公共版本应用提供本机广告，你必须在开发人员中心仪表板的**通过广告盈利**页面中创建 **Native** 广告单元。 目前只有参与试用计划的部分开发人员能够创建 **Native** 广告单元，但我们打算尽快向所有开发人员提供此功能。 如果你有兴趣参加我们的试用计划，请通过 aiacare@microsoft.com 联系我们。

> [!NOTE]
> 目前，只有 Windows 10 上基于 XAML 的 UWP 应用支持本机广告。 我们打算在未来的 Microsoft 广告 SDK 版本中支持使用 HTML 和 JavaScript 编写的 UWP 应用。

## <a name="prerequisites"></a>先决条件

* 安装 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp)（10.0.4 或更高版本）及 Visual Studio 2015 或更高版本的 Visual Studio。 有关安装说明，请参阅[此文章](install-the-microsoft-advertising-libraries.md)。 你可以通过 MSI 安装程序在开发计算机上安装此 SDK，也可以通过 NuGet 程序包安装此 SDK 以用于特定项目。

## <a name="integrate-a-native-ad-into-your-app"></a>在应用中集成本机广告

请按照以下说明在应用中集成本机广告，并确认你的本机广告实现显示了测试广告。

1. 在 Visual Studio 中，打开项目或创建新项目。

2. 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft 广告 SDK 的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

3.  在**解决方案资源管理器**窗口中，右键单击**引用**，然后选择**添加引用...**

4.  在**引用管理器**中，展开**通用 Windows**、单击**扩展**，然后选中**适用于 XAML 的 Microsoft 广告 SDK**（版本 10.0）旁边的复选框。

5.  在**引用管理器**中，单击“确定”。

6. 在应用的相应代码文件中（例如，在 MainPage.xaml.cs 或部分其他页面的代码文件中）添加以下命名空间引用。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

7.  在应用的相应位置（例如，在 ```MainPage``` 或部分其他页面）声明 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.aspx) 对象和几个字符串字段，这些字段代表本机广告的应用程序 ID 和广告单元 ID。 以下代码示例将 `myAppId` 和 `myAdUnitId` 字段分配给本机广告的[测试值](test-mode-values.md)。

    > [!NOTE]
    > 每个 **NativeAdsManager** 都有一个对应的*广告单元*，我们的服务使用该广告单元来为本机广告控件提供广告，每个广告单元都包含*单元 ID* 和*应用程序 ID*。 在这些步骤中，你将为控件分配测试广告单元 ID 和应用程序 ID 值。 这些测试值只能在应用的测试版本中使用。 在将应用发布到应用商店之前，你必须在 Windows 开发人员中心[将这些测试值替换为实时值](#release)。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

8.  在启动时运行的代码中（例如，在页面的构造函数中）实例化 **NativeAdsManager** 对象，并为对象的 [AdReady](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.adready.aspx) 和 [ErrorOccurred](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.erroroccurred.aspx) 事件连接事件处理程序。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

9.  准备好显示本机广告后，调用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.requestad.aspx) 方法抓取广告。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

10.  为你的应用准备好本机广告后，系统将调用你的 **AdReady** 事件处理程序，并向 *e* 参数传递代表此本机广告的 [NativeAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.aspx) 对象。 使用 **NativeAd** 属性获取本机广告的每个元素，在你的页面上显示这些元素。 此外，请务必调用 [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) 方法注册用作本机广告容器的 UI 元素；要正确跟踪广告曝光数和点击数，就必须执行此操作。
  > [!NOTE]
  > 本机广告的某些元素是必需的，并且必须始终在应用中显示。 有关详细信息，请参阅[要求和指南](#requirements-and-guidelines)。

    例如，假设你的应用包含具有以下 **StackPanel** 的 ```MainPage```（或一些其他页面）。 此 **StackPanel** 包含一系列显示本机广告的不同元素的控件，包括标题、描述、图像、*赞助商*文本，以及用于显示*行动号召*文本的按钮。

    ``` xml
    <StackPanel x:Name="NativeAdContainer" Background="#555555" Width="Auto" Height="Auto"
                Orientation="Vertical">
        <Image x:Name="AdIconImage" HorizontalAlignment="Left" VerticalAlignment="Center"
               Margin="20,20,20,20"/>
        <TextBlock x:Name="TitleTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
               Text="The ad title will go here" FontSize="24" Foreground="White" Margin="20,0,0,10"/>
        <TextBlock x:Name="DescriptionTextBlock" HorizontalAlignment="Left" VerticalAlignment="Center"
                   Foreground="White" TextWrapping="Wrap" Text="The ad description will go here"
                   Margin="20,0,0,0" Visibility="Collapsed"/>
        <Image x:Name="MainImageImage" HorizontalAlignment="Left"
               VerticalAlignment="Center" Margin="20,20,20,20" Visibility="Collapsed"/>
        <Button x:Name="CallToActionButton" Background="Gray" Foreground="White"
                HorizontalAlignment="Left" VerticalAlignment="Center" Width="Auto" Height="Auto"
                Content="The call to action text will go here" Margin="20,20,20,20"
                Visibility="Collapsed"/>
        <StackPanel x:Name="SponsoredByStackPanel" Orientation="Horizontal" Margin="20,20,20,20">
            <TextBlock x:Name="SponsoredByTextBlock" Text="The ad sponsored by text will go here"
                       FontSize="24" Foreground="White" Margin="20,0,0,0" HorizontalAlignment="Left"
                       VerticalAlignment="Center" Visibility="Collapsed"/>
            <Image x:Name="IconImageImage" Margin="40,20,20,20" HorizontalAlignment="Left"
                   VerticalAlignment="Center" Visibility="Collapsed"/>
        </StackPanel>
    </StackPanel>
    ```

    下面的代码示例显示了一个 **AdReady** 事件处理程序，它在 **StackPanel** 的控件中显示本机广告的各个元素，然后调用 **RegisterAdContainer** 方法注册 **StackPanel**。 此代码假设它从包含 **StackPanel** 的页面的代码隐藏文件运行。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#AdReady)]

11.  为 **ErrorOccurred** 事件定义一个事件处理程序，以处理与本机广告相关的错误。 下面的示例在测试期间将错误信息写入 Visual Studio **输出**窗口。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

12.  编译并运行应用以查看是否带有测试广告。

<span id="release" />
## <a name="release-your-app-with-live-ads"></a>发布包含实时广告的应用

确认你的本机广告实现能够成功显示测试广告后，请按照以下说明配置应用来显示真实的广告，并将更新后的应用提交到应用商店。

1.  请确保你的本机广告实现遵守本机广告[要求和指南](#requirements-and-guidelines)。

2.  在开发人员中心仪表板中，转到应用的[通过广告盈利](../publish/monetize-with-ads.md)页面，然后[创建广告单元](../monetize/set-up-ad-units-in-your-app.md)。 对于广告单元类型，请指定 **Native**。 记下广告单元 ID 和应用程序 ID。
    > [!IMPORTANT]
    > 每个广告单元都只能在一个应用中使用。 如果在多个应用中使用某个广告单元，将不为该广告单元提供广告。

3. 你可以选择性地通过配置[广告中介](../publish/monetize-with-ads.md#mediation)部分（位于[通过广告盈利](../publish/monetize-with-ads.md)页面上）的设置来为本机广告启用广告中介。 广告中介能够显示多个广告网络的广告，让你最大程度地增加广告收益，并充分利用应用促销功能。

4.  在你的代码中，将测试广告单元值（即 [NativeAdsManager](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativeadsmanager.nativeadsmanager.aspx) 构造函数的 *applicationId* 和 *adUnitId* 参数）替换为你在开发人员中心生成的实时值。

5.  使用开发人员中心仪表板[提交应用](../publish/app-submissions.md)至应用商店。

6.  在开发人员中心仪表板中查看你的[广告性能报告](../publish/advertising-performance-report.md)。

<span id="requirements-and-guidelines" />
## <a name="requirements-and-guidelines"></a>要求和指南

本机广告让你能够良好地控制向用户展示广告内容的方式。 遵守下述要求和指南有助于确保将广告商的信息传达给用户，同时也可帮助避免为用户提供混乱的本机广告体验。

### <a name="register-the-container-for-your-native-ad"></a>为本机广告注册容器

你必须在代码中调用 **NativeAd** 对象的 [RegisterAdContainer](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.registeradcontainer.aspx) 方法注册用作本机广告容器的 UI 元素，并有选择地注册你希望注册为广告的可点击目标的任何特定控件。 要正确跟踪广告曝光数和点击数，就必须执行此操作。

**RegisterAdContainer** 方法有两个重载：

* 如果你希望所有单个本机广告元素的整个容器变成可点击状态，请调用 [RegisterAdContainer(FrameworkElement)](https://msdn.microsoft.com/library/windows/apps/mt809188.aspx) 方法并向方法传递容器控件。 例如，你在各个独立控件中显示所有本机广告元素，所有这些控件都托管在 **StackPanel** 中，你希望整个 **StackPanel** 变成可点击状态，请向此方法传递 **StackPanel**。

* 如果你只想让特定本机广告元素变成可点击状态，请调用 [RegisterAdContainer(FrameworkElement, IVector(FrameworkElement))](https://msdn.microsoft.com/library/windows/apps/mt809189.aspx) 方法。 只有你传递给第二个参数的控件会变成可点击状态。

### <a name="required-native-ad-elements"></a>必需的本机广告元素

在你的本机广告设计中，你必须始终向用户显示以下本机广告元素。 若不包含这些元素，你的广告单元的业绩会很差，收益率也很低。

1. 始终显示本机广告的标题（由 **NativeAd** 对象的 [Title](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.title.aspx) 属性提供）。 提供足以显示至少 25 个字符的空间。 如果标题较长，请使用省略号替换其他文字。
2. 始终显示以下元素中的至少一种，以帮助区分本机广告体验与你的应用体验，清楚地表明此内容由广告商提供：
  * 可区分的*广告*图标（由 **NativeAd** 对象的 [AdIcon](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.adicon.aspx) 属性提供）。 此图标由 Microsoft 提供。
  * *赞助商*文本（由 **NativeAd** 对象的 [SponsoredBy](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.sponsoredby.aspx) 属性提供）。 此文本由广告商提供。
  * 除了*赞助商*文本以外，你也可以选择显示一些其他的文本，以帮助区分本机广告体验与你的应用体验，例如“赞助内容”、“促销内容”、“推荐内容”等。

### <a name="user-experience"></a>用户体验

本机广告应与你的应用的其余部分清晰分隔，并且周围有防止意外点击的空间。 使用边框、不同背景或其他 UI 将广告内容与应用的其余部分隔开。 请记住，从长远来看，意外点击广告不会给你的广告收益或最终用户体验带来任何好处。

### <a name="description"></a>说明

如果你选择显示广告说明（由 **NativeAd** 对象的 [Description](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.description.aspx) 属性提供），请提供足以显示至少 75 个字符的空间。 我们建议你使用动画显示广告描述的完整内容。

### <a name="call-to-action"></a>行动号召

*行动号召*文本（由 **NativeAd** 对象的 [CallToAction](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.nativead.calltoaction.aspx) 属性提供）是广告的重要组成部分。 如果你选择显示此文本，请遵循以下准则：

* 始终在可点击的控件（例如按钮或超链接）上向用户显示*行动号召*文本。
* 始终显示完整的*行动号召*文本。
* 确保*行动号召*文本与广告商的其他促销文本分隔开。

### <a name="learn-and-optimize"></a>学习和优化

我们建议你为应用中每个不同的本机广告展示位置创建和使用不同的广告单元。 这样，你就可以获取每个本机广告展示位置的单独报告数据，并根据这些数据做出更改，从而优化每个本机广告展示位置的业绩。

## <a name="related-topics"></a>相关主题

* [通过广告盈利](../publish/monetize-with-ads.md)
* [为应用设置广告单元](../monetize/set-up-ad-units-in-your-app.md)
