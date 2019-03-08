---
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: 了解如何使用 Microsoft 广告 SDK 将间隙广告纳入到面向 Windows 10 的 UWP 应用。
title: 间隙广告
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 广告, ad control, interstitial
ms.localizationpriority: medium
ms.openlocfilehash: 9abf761aa141ef3d0c19d6d5401b6815542d4172
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603912"
---
# <a name="interstitial-ads"></a>间隙广告

本演练介绍如何将间隙广告纳入面向 Windows 10 的通用 Windows 平台 (UWP) 应用和游戏中。 有关演示如何使用 C# 和 C++ 向 JavaScript/HTML 应用和 XAML 应用添加间隙广告的完整示例项目，请参阅 [GitHub 上的广告示例](https://aka.ms/githubads)。

<span id="whatareinterstitialads10"/>

## <a name="what-are-interstitial-ads"></a>什么是间隙广告？

与局限于应用或游戏中的部分 UI 的标准横幅广告不同的是，间隙广告是在整个屏幕上显示。 游戏中经常会使用两种基本形式。

* 通过 *Paywall* 广告，用户必须在某段固定间隔时间后观看广告。 例如在转换游戏关卡之间：

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* 通过*基于奖励*的广告，用户显式寻找某些好处（例如完成关卡的提示或额外时间），并通过应用的用户界面来初始化广告。

我们提供两种可在应用和游戏中使用的间隙广告：**间隙视频广告**和**间隙横幅广告**。

> [!NOTE]
> 间隙广告的 API 不会处理任何用户界面，播放视频时除外。 在考虑如何在应用中集成间隙广告时，可就有关处理方法、避免内容的指南参考[间隙最佳做法](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

## <a name="prerequisites"></a>必备条件

* 使用 Visual Studio 2015 或更高版本的 Visual Studio 安装 [Microsoft 广告 SDK](https://aka.ms/ads-sdk-uwp)。 有关安装说明，请参阅[此文章](install-the-microsoft-advertising-libraries.md)。

## <a name="integrate-an-interstitial-ad-into-your-app"></a>在应用中集成间隙广告

若要在应用中显示间隙广告，请遵循针对项目类型的说明：

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C + + （DirectX 互操作）](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>

### <a name="xamlnet"></a>XAML/.NET

本部分提供 C# 示例，但 Visual Basic 和 C++ 也受 XAML/.NET 项目支持。 有关完整的 C# 代码示例，请参阅 [C# 中的间隙广告示例代码](interstitial-ad-sample-code-in-c.md)。

1. 在 Visual Studio 中打开你的项目。
    > [!NOTE]
    > 如果你使用现有项目，请打开项目中的 Package.appxmanifest 文件并确保已选择 **Internet（客户端）** 功能。 应用需要使用此功能接收测试广告和实时广告。

2. 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft Advertising 库的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 在你的项目中添加对 Microsoft 广告 SDK 的引用：

    1. 在“解决方案资源管理器”窗口中，右键单击“引用”，然后选择“添加引用...”
    2.  在“引用管理器”中，展开“通用 Windows”、单击“扩展”，然后选中“适用于 XAML 的 Microsoft Advertising SDK”（版本 10.0）旁边的复选框。
    3.  在“引用管理器”中，单击“确定”。

3.  在应用的相应代码文件中（例如，在 MainPage.xaml.cs 或部分其他页面的代码文件中）添加以下命名空间引用。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  在应用的相应位置（例如，在 ```MainPage``` 或部分其他页面）声明 [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 对象和几个字符串字段，这些字段代表间隙广告的应用程序 ID 和广告单元 ID。 以下代码示例将 `myAppId` 和 `myAdUnitId` 字段分配给间隙广告的[测试值](set-up-ad-units-in-your-app.md#test-ad-units)。

    > [!NOTE]
    > 每个 **InterstitialAd** 都有一个对应的*广告单元*，我们的服务使用该广告单元来为控件提供广告，每个广告单元都包含*单元 ID* 和*应用程序 ID*。 在这些步骤中，你将为控件分配测试广告单元 ID 和应用程序 ID 值。 这些测试值只能在应用的测试版本中使用。 您的应用程序发布到应用商店之前，必须[替换为这些测试的值与实时值](#release)从合作伙伴中心。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  在启动时运行的代码中（例如，在页面的构造函数中）实例化 **InterstitialAd** 对象，并安装对象事件的事件处理程序。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  如果你想要显示*插播式视频*ad:大约 30-60 秒之后需要 ad，使用[RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)方法来预提取 ad。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **AdType.Video**。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

    如果你想要显示*插播式横幅*ad:大约 5 到 8 秒之前需要 ad，使用[RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)方法来预提取 ad。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **AdType.Video**。

    ```csharp
    myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
    ```

6.  此时在你想要显示间隙视频或间隙横幅广告的代码中，确认是否已准备好显示 **InterstitialAd**，然后使用 [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 方法显示它。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  定义 **InterstitialAd** 对象的事件处理程序。

    [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  生成并测试你的应用，以确认它会显示测试广告。

<span id="interstitialadshtml10"/>

### <a name="htmljavascript"></a>HTML/JavaScript

以下说明假设你已在 Visual Studio 中为 JavaScript 创建了通用 Windows 项目，并且面向特定的 CPU。 有关完整的代码示例，请参阅 [JavaScript 中的间隙广告示例代码](interstitial-ad-sample-code-in-javascript.md)。

1. 在 Visual Studio 中打开你的项目。

2. 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft Advertising 库的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 在你的项目中添加对 Microsoft 广告 SDK 的引用：

    1. 在“解决方案资源管理器”窗口中，右键单击“引用”，然后选择“添加引用...”
    2.  在“引用管理器”中，展开“通用 Windows”、单击“扩展”，然后选中“适用于 JavaScript 的 Microsoft Advertising SDK”（版本 10.0）旁边的复选框。
    3.  在“引用管理器”中，单击“确定”。

3.  在项目的 HTML 文件的**&lt;标题&gt;** 部分中，在项目的 JavaScript 引用 default.css 和 default.js 之后，添加对 ad.js 的引用。

    ``` HTML
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  在项目的 .js 文件中声明 [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 对象和几个字段，这些字段包含间隙广告的应用程序 ID 和广告单元 ID。 以下代码示例将 `applicationId` 和 `adUnitId` 字段分配给间隙广告的[测试值](set-up-ad-units-in-your-app.md#test-ad-units)。

    > [!NOTE]
    > 每个 **InterstitialAd** 都有一个对应的*广告单元*，我们的服务使用该广告单元来为控件提供广告，每个广告单元都包含*单元 ID* 和*应用程序 ID*。 在这些步骤中，你将为控件分配测试广告单元 ID 和应用程序 ID 值。 这些测试值只能在应用的测试版本中使用。 您的应用程序发布到应用商店之前，必须[替换为这些测试的值与实时值](#release)从合作伙伴中心。

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  在启动时运行的代码中（例如，在页面的构造函数中）实例化 **InterstitialAd** 对象，并安装对象的事件处理程序。

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. 如果你想要显示*插播式视频*ad:大约 30-60 秒之后需要 ad，使用[RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)方法来预提取 ad。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **InterstitialAdType.video**。

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

    如果你想要显示*插播式横幅*ad:大约 5 到 8 秒之前需要 ad，使用[RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)方法来预提取 ad。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **InterstitialAdType.display**。

    ```js
    if (interstitialAd) {
        interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
    }
    ```

6.  此时在你想要显示广告的代码中，确认是否已准备好显示 **InterstitialAd**，然后使用 [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 方法显示它。

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  定义 **InterstitialAd** 对象的事件处理程序。

    [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  生成并测试你的应用，以确认它会显示测试广告。

<span id="interstitialadsdirectx10"/>

### <a name="c-directx-interop"></a>C++（DirectX 互操作）

此示例假设你已在 Visual Studio 中创建了 C++ **DirectX 和 XAML 应用（通用 Windows）** 项目，并且面向特定的 CPU 体系结构。
 
1. 在 Visual Studio 中打开你的项目。

3. 在你的项目中添加对 Microsoft 广告 SDK 的引用：

    1. 在“解决方案资源管理器”窗口中，右键单击“引用”，然后选择“添加引用...”
    2.  在“引用管理器”中，展开“通用 Windows”、单击“扩展”，然后选中“适用于 XAML 的 Microsoft Advertising SDK”（版本 10.0）旁边的复选框。
    3.  在“引用管理器”中，单击“确定”。

2.  在应用的相应头文件（例如，DirectXPage.xaml.h）中，声明 [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) 对象和相关的事件处理程序方法。  

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  在相同的头文件中声明几个字符串字段，这些字段代表间隙广告的应用程序 ID 和广告单元 ID。 以下代码示例将 `myAppId` 和 `myAdUnitId` 字段分配给间隙广告的[测试值](set-up-ad-units-in-your-app.md#test-ad-units)。

    > [!NOTE]
    > 每个 **InterstitialAd** 都有一个对应的*广告单元*，我们的服务使用该广告单元来为控件提供广告，每个广告单元都包含*单元 ID* 和*应用程序 ID*。 在这些步骤中，你将为控件分配测试广告单元 ID 和应用程序 ID 值。 这些测试值只能在应用的测试版本中使用。 您的应用程序发布到应用商店之前，必须[替换为这些测试的值与实时值](#release)从合作伙伴中心。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  在想要添加代码以显示间隙广告的 .cpp 文件中，添加以下命名空间引用。 以下示例假设你要将代码添加到应用的 DirectXPage.xaml.cpp 文件中。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  在启动时运行的代码中（例如，在页面的构造函数中）实例化 **InterstitialAd** 对象，并安装对象事件的事件处理程序。 在以下示例中，```InterstitialAdSamplesCpp``` 是项目的命名空间；请根据需要针对代码更改此名称。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. 如果你想要显示*插播式视频*ad:在你需要间隙广告之前大约 30-60 秒，请使用 [RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad) 方法预取广告。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **AdType::Video**。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

    如果你想要显示*插播式横幅*ad:大约 5 到 8 秒之前需要 ad，使用[RequestAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.requestad)方法来预提取 ad。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **AdType::Display**。

    ```cpp
    m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
    ```

7.  此时在你想要显示广告的代码中，确认是否已准备好显示 **InterstitialAd**，然后使用 [Show](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad.show) 方法显示它。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  定义 **InterstitialAd** 对象的事件处理程序。

    [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. 生成并测试你的应用，以确认它会显示测试广告。

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>发布包含实时广告的应用

1. 确保在应用中对间隙广告的使用遵循我们的[间隙广告指南](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

2.  在合作伙伴中心，请转到[应用内广告](../publish/in-app-ads.md)页并[创建广告单元](set-up-ad-units-in-your-app.md#live-ad-units)。 对于广告单元类型，请选择**视频间隙**或**横幅间隙**，具体取决于你将要显示的间隙广告类型。 记下广告单元 ID 和应用程序 ID。
    > [!NOTE]
    > 测试广告单元和实时 UWP 广告单元的应用程序 ID 值采用不同的格式。 测试应用程序 ID 值为 GUID。 当在合作伙伴中心创建实时 UWP ad 单元时，ad 单元的应用程序 ID 值将始终匹配您的应用程序 （示例 Store ID 值看上去像 9NBLGGH4R315） Store ID。

3. 你可以选择性地通过配置[中介设置](../publish/in-app-ads.md#mediation)部分（位于[应用内广告](../publish/in-app-ads.md)页面上）中的设置为**间隙广告**启用广告中介。 广告中介显示来自多个广告网络（包括其他付费广告网络，如 Taboola 和 Smaato）的广告及 Microsoft 应用促销活动的广告，从而使你能够最大化你的广告收益和应用促销能力。

4.  在代码中，将测试 ad 单元值替换在合作伙伴中心生成的实时值。

5.  [将应用提交](../publish/app-submissions.md)到使用合作伙伴中心在存储区。

6.  查看你[广告性能报告](../publish/advertising-performance-report.md)在合作伙伴中心。

<span id="manage" />

## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>管理你的应用中多个间隙广告控件的广告单元

你可以在单个应用中使用多个 **InterstitialAd** 控件。 在此情况下，我们建议你为每个控件分配不同的广告单元。 对每个控件使用不同的广告单元使你可以分别[配置中介设置](../publish/in-app-ads.md#mediation)并获取每个控件的独立[报告数据](../publish/advertising-performance-report.md)。 这还使我们的服务能够更好地优化我们为应用提供的广告。

> [!IMPORTANT]
> 每个广告单元都只能在一个应用中使用。 如果在多个应用中使用某个广告单元，将不为该广告单元提供广告。

## <a name="related-topics"></a>相关主题

* [插播式广告的准则](ui-and-user-experience-guidelines.md#interstitialbestpractices10)
* [插播式广告中的代码示例C#](interstitial-ad-sample-code-in-c.md)
* [在 JavaScript 中的插播式广告示例代码](interstitial-ad-sample-code-in-javascript.md)
* [GitHub 上的广告示例](https://aka.ms/githubads)
* [设置 ad 单位为你的应用](set-up-ad-units-in-your-app.md)
