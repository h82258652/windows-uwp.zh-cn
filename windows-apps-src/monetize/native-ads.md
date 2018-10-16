---
author: Xansky
description: 了解如何向 UWP 应用中添加本机广告。
title: 本机广告
ms.author: mhopkins
ms.date: 05/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, 广告, 广告控件, 本机广告
ms.localizationpriority: medium
ms.openlocfilehash: 4a529cc360d6a357cf4ff6aa36370c4339b37ed6
ms.sourcegitcommit: 106aec1e59ba41aae2ac00f909b81bf7121a6ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/15/2018
ms.locfileid: "4614300"
---
# <a name="native-ads"></a>本机广告

本机广告是基于组件的广告格式，其中每一个广告创意元素（如标题、图像、说明和行动号召文字）都作为单独的元素提供给你。 你可以使用自己的字体、颜色、动画和其他 UI 组件将这些元素集成到你的应用中，从而组合成一种与你的应用和谐共融的用户体验，同时从广告中获得较高收益。

对于广告商而言，本机广告提供了高绩效的展示位置，因为广告体验紧密集成在应用中，用户倾向于与此类广告进行更多的互动。

> [!NOTE]
> 目前，只有 Windows 10 上基于 XAML 的 UWP 应用支持本机广告。 我们打算在未来的 Microsoft 广告 SDK 版本中支持使用 HTML 和 JavaScript 编写的 UWP 应用。

## <a name="prerequisites"></a>先决条件

* 使用 Visual Studio 2015 或更高版本的 Visual Studio 安装 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp)。 有关安装说明，请参阅[此文章](install-the-microsoft-advertising-libraries.md)。

## <a name="integrate-a-native-ad-into-your-app"></a>在应用中集成本机广告

请按照以下说明在应用中集成本机广告，并确认你的本机广告实现显示了测试广告。

1. 在 Visual Studio 中，打开项目或创建新项目。
    > [!NOTE]
    > 如果你使用现有项目，请打开项目中的 Package.appxmanifest 文件并确保已选择 **Internet（客户端）** 功能。 应用需要使用此功能接收测试广告和实时广告。

2. 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft 广告 SDK 的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 在你的项目中添加对 Microsoft 广告 SDK 的引用：

    1. 在**解决方案资源管理器**窗口中，右键单击**引用**，然后选择**添加引用...**
    2.  在**引用管理器**中，展开**通用 Windows**、单击**扩展**，然后选中**适用于 XAML 的 Microsoft 广告 SDK**（版本 10.0）旁边的复选框。
    3.  在**引用管理器**中，单击“确定”。

4. 在应用的相应代码文件中（例如，在 MainPage.xaml.cs 或部分其他页面的代码文件中）添加以下命名空间引用。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Namespaces)]

5.  在应用的相应位置（例如，在 ```MainPage``` 或部分其他页面）声明 [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2) 对象和几个字符串字段，这些字段代表本机广告的应用程序 ID 和广告单元 ID。 以下代码示例将 `myAppId` 和 `myAdUnitId` 字段分配给本机广告的[测试值](set-up-ad-units-in-your-app.md#test-ad-units)。
    > [!NOTE]
    > 每个 **NativeAdsManagerV2** 都有一个对应的*广告单元*，我们的服务使用该广告单元来为本机广告控件提供广告，每个广告单元都包含*广告单元 ID* 和*应用程序 ID*。 在这些步骤中，你将为控件分配测试广告单元 ID 和应用程序 ID 值。 这些测试值只能在应用的测试版本中使用。 在将应用发布到 Microsoft Store 之前，必须在 Windows 开发人员中心[将这些测试值替换为实时值](#release)。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#Variables)]

6.  In code that runs on startup (for example, in the constructor for the page), instantiate the **NativeAdsManagerV2** object and wire up event handlers for the **AdReady** and **ErrorOccurred** events of the object.

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ConfigureNativeAd)]

7.  准备好显示本机广告后，调用 **RequestAd** 方法抓取广告。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#RequestAd)]

8.  为应用准备好本机广告后，系统将调用 [AdReady](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.adready) 事件处理程序，并向 *e* 参数传递代表此本机广告的 [NativeAdV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadv2) 对象。 使用 **NativeAdV2** 属性获取本机广告的每个元素，在页面上显示这些元素。 此外，请务必调用 **RegisterAdContainer** 方法注册用作本机广告容器的 UI 元素；要正确跟踪广告曝光数和点击数，就必须执行此操作。
    > [!NOTE]
    > 本机广告的某些元素是必需的，并且必须始终在应用中显示。 有关详细信息，请参阅我们的[本机广告指南](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)。

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

9.  Define an event handler for the **ErrorOccurred** event to handle errors related to the native ad. 下面的示例在测试期间将错误信息写入 Visual Studio **输出**窗口。

    [!code-cs[NativeAd](./code/AdvertisingSamples/NativeAdSamples/cs/MainPage.xaml.cs#ErrorOccurred)]

10.  编译并运行应用以查看是否带有测试广告。

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>发布包含实时广告的应用

确认你的本机广告实现能够成功显示测试广告后，请按照以下说明配置应用来显示真实的广告，并将更新后的应用提交到应用商店。

1.  请确保你的本机广告实现遵守[本机广告指南](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)。

2.  在开发人员中心仪表板中转到[应用内广告](../publish/in-app-ads.md)页面，然后[创建广告单元](set-up-ad-units-in-your-app.md#live-ad-units)。 对于广告单元类型，请指定**本机**。 记下广告单元 ID 和应用程序 ID。
    > [!NOTE]
    > 测试广告单元和实时 UWP 广告单元的应用程序 ID 值采用不同的格式。 测试应用程序 ID 值为 GUID。 在仪表板中创建实时 UWP 广告单元时，该广告单元的应用程序 ID 值始终与应用的应用商店 ID（例如应用商店 ID 值类似于 9NBLGGH4R315）匹配。

3. 你可以选择通过配置[中介设置](../publish/in-app-ads.md#mediation)部分（位于[应用内广告](../publish/in-app-ads.md)页面上）的设置为本机广告启用广告中介。 广告中介能够显示多个广告网络的广告，让你最大程度地增加广告收益，并充分利用应用促销功能。

4.  在代码中将测试广告单元值（即 [NativeAdsManagerV2](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativeadsmanagerv2.-ctor) 构造函数的 *applicationId* 和 *adUnitId* 参数）替换为在开发人员中心生成的实时值。

5.  使用开发人员中心仪表板[提交应用](../publish/app-submissions.md)至应用商店。

6.  在开发人员中心仪表板中查看你的[广告性能报告](../publish/advertising-performance-report.md)。

## <a name="manage-ad-units-for-multiple-native-ads-in-your-app"></a>管理你的应用中多个本机广告的广告单元

可以在单个应用中使用多个本机广告展示位置。 在此情况下，我们建议为每个本机广告展示位置分配不同的广告单元。 对每个本机广告使用不同的广告单元可以分别[配置中介设置](../publish/in-app-ads.md#mediation)并获取每个控件的独立[报告数据](../publish/advertising-performance-report.md)。 这还使我们的服务能够更好地优化我们为你的应用提供的广告。

> [!IMPORTANT]
> 每个广告单元都只能在一个应用中使用。 如果在多个应用中使用某个广告单元，将不为该广告单元提供广告。

## <a name="related-topics"></a>相关主题

* [本机广告指南](ui-and-user-experience-guidelines.md#guidelines-for-native-ads)
* [应用内广告](../publish/in-app-ads.md)
* [为应用设置广告单元](set-up-ad-units-in-your-app.md)
