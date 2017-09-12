---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "了解如何使用 Microsoft Advertising 库，将间隙广告纳入到 Windows10、Windows8.1 或 Windows Phone 8.1 应用中。"
title: "间隙广告"
ms.author: mcleans
ms.date: 06/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 广告, ad control, interstitial"
ms.openlocfilehash: 16cb78d9419d54a1bd56fb855ca1fad6fedf665a
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2017
---
# <a name="interstitial-ads"></a>间隙广告

本操作实例介绍如何将间隙广告包含在 Windows 10 的通用 Windows 平台 (UWP) 应用和游戏以及 Windows 8.1 或 Windows Phone 8.1 应用中。 有关演示如何使用 C# 和 C++ 向 JavaScript/HTML 应用和 XAML 应用添加间隙广告的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

<span id="whatareinterstitialads10"/>
## <a name="what-are-interstitial-ads"></a>什么是间隙广告？

与局限于应用或游戏中的部分 UI 的标准横幅广告不同的是，间隙广告是在整个屏幕上显示。 游戏中经常会使用两种基本形式。

* 通过 *Paywall* 广告，用户必须在某段固定间隔时间后观看广告。 例如在转换游戏关卡之间：

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* 通过*基于奖励*的广告，用户显式寻找某些好处（例如完成关卡的提示或额外时间），并通过应用的用户界面来初始化广告。

我们提供两种可用于应用和游戏的间隙广告：

* **间隙视频广告**：它们可用于 Windows 10 的通用 Windows 平台 (UWP) 应用和 Windows 8.1 或 Windows Phone 8.1 的应用。

* **间隙横幅广告**：它们仅可用于 Windows 10 的 UWP 应用。

> [!NOTE]
> 间隙广告的 API 不会处理任何用户界面，播放视频时除外。 在考虑如何在应用中集成间隙广告时，可就有关处理方法、避免内容的指南参考[间隙最佳做法](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

## <a name="build-an-app-with-interstitial-ads"></a>使用间隙广告生成应用

若要在应用中显示间隙广告，请遵循针对项目类型的说明：

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++（DirectX 互操作）](#interstitialadsdirectx10)

<span/>
### <a name="prerequisites"></a>先决条件

* 对于 UWP 应用，请安装 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp) 与 Visual Studio 2015 或更高版本。

* 对于 Windows8.1 或 Windows Phone 8.1 应用：使用 Visual Studio 2015 或 Visual Studio 2013 安装[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。

<span id="interstitialadsxaml10"/>
### <a name="xamlnet"></a>XAML/.NET

本部分提供 C# 示例，但 Visual Basic 和 C++ 也受 XAML/.NET 项目支持。 有关完整的 C# 代码示例，请参阅 [C# 中的间隙广告示例代码](interstitial-ad-sample-code-in-c.md)。

1. 在 Visual Studio 中打开你的项目。

2. 在**引用管理器**中，根据你的项目类型选择以下引用之一：

  * 对于通用 Windows 平台 (UWP) 项目：展开**通用 Windows**、单击**扩展**，然后选中**适用于 XAML 的 Microsoft Advertising SDK**（版本 10.0）旁边的复选框。

  * 对于 Windows8.1 项目：展开**Windows8.1**、单击**扩展**，然后选中**适用于 Windows8.1 XAML 的广告中介 SDK** 旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

  * 对于 Windows Phone 8.1 项目：展开**Windows Phone 8.1**、单击**扩展**，然后选中**适用于 Windows Phone 8.1 XAML 的广告中介 SDK** 旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

3.  在应用的相应代码文件中（例如，在 MainPage.xaml.cs 或部分其他页面的代码文件中）添加以下命名空间引用。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  在应用的相应位置（例如，在 ```MainPage``` 或部分其他页面）声明 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 对象和几个字符串字段，这些字段代表间隙广告的应用程序 ID 和广告单元 ID。 以下代码示例将 `myAppId` 和 `myAdUnitId` 字段分配给[测试模式值](test-mode-values.md)中提供的间隙视频广告测试值。

  > [!NOTE]
  > 每个 **InterstitialAd** 都有一个对应的*广告单元*，我们的服务使用该广告单元来为控件提供广告，每个广告单元都包含*单元 ID* 和*应用程序 ID*。 在这些步骤中，你将为控件分配测试广告单元 ID 和应用程序 ID 值。 这些测试值只能在应用的测试版本中使用。 在将应用发布到应用商店之前，你必须在 Windows 开发人员中心[将这些测试值替换为实时值](#release)。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  在启动时运行的代码中（例如，在页面的构造函数中）实例化 **InterstitialAd** 对象，并安装对象事件的事件处理程序。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  如果你需要显示*间隙视频*广告：在需要广告之前大约 30-60 秒，请使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法预取广告。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **AdType.Video**。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

  如果你需要显示*间隙横幅*广告（仅适用于 UWP 应用），在需要广告之前大约 5-8 秒，请使用[RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法预取广告。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **AdType.Video**。

  ```csharp
  myInterstitialAd.RequestAd(AdType.Display, myAppId, myAdUnitId);
  ```

6.  此时在你想要显示间隙视频或间隙横幅广告的代码中，确认是否已准备好显示 **InterstitialAd**，然后使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法显示它。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  定义 **InterstitialAd** 对象的事件处理程序。

  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  生成并测试你的应用，以确认它会显示测试广告。

<span id="interstitialadshtml10"/>
### <a name="htmljavascript"></a>HTML/JavaScript

以下说明假设你已在 Visual Studio 中为 JavaScript 创建了通用 Windows 项目，并且面向特定的 CPU。 有关完整的代码示例，请参阅 [JavaScript 中的间隙广告示例代码](interstitial-ad-sample-code-in-javascript.md)。

1. 在 Visual Studio 中打开你的项目。

2.  在**引用管理器**中，根据你的项目类型选择以下引用之一：

  * 对于通用 Windows 平台 (UWP) 项目：展开**通用 Windows**、单击**扩展**，然后选中**适用于 JavaScript 的 Microsoft Advertising SDK**（版本 10.0）旁边的复选框。

  * 对于 Windows 8.1 项目：展开 **Windows 8.1**、单击**扩展**，然后选中**适用于 Windows 8.1 Native (JS) 的 Microsoft Advertising SDK** 旁边的复选框。

  * 对于 Windows 8.1 项目：展开 **Windows Phone 8.1**、单击**扩展**，然后选中**适用于 Windows Phone 8.1 Native (JS) 的 Microsoft Advertising SDK** 旁边的复选框。

3.  在项目的 HTML 文件的**&lt;标题&gt;**部分中，在项目的 JavaScript 引用 default.css 和 default.js 之后，添加对 ad.js 的引用。 在 UWP 项目中，添加以下引用。

  ``` HTML
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  在 Windows 8.1 或 Windows Phone 8.1 项目中，添加以下引用。

  ``` HTML
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

4.  在项目的 .js 文件中声明 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 对象和几个字段，这些字段包含间隙广告的应用程序 ID 和广告单元 ID。 以下代码示例将 `applicationId` 和 `adUnitId` 字段分配给[测试模式值](test-mode-values.md)中提供的间隙视频广告测试值。

  > [!NOTE]
  > 每个 **InterstitialAd** 都有一个对应的*广告单元*，我们的服务使用该广告单元来为控件提供广告，每个广告单元都包含*单元 ID* 和*应用程序 ID*。 在这些步骤中，你将为控件分配测试广告单元 ID 和应用程序 ID 值。 这些测试值只能在应用的测试版本中使用。 在将应用发布到应用商店之前，你必须在 Windows 开发人员中心[将这些测试值替换为实时值](#release)。

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  在启动时运行的代码中（例如，在页面的构造函数中）实例化 **InterstitialAd** 对象，并安装对象的事件处理程序。

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. 如果你需要显示*间隙视频*广告：在需要广告之前大约 30-60 秒，请使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法预取广告。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **InterstitialAdType.video**。

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

  如果你需要显示*间隙横幅*广告（仅适用于 UWP 应用），在需要广告之前大约 5-8 秒，请使用[RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法预取广告。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **InterstitialAdType.display**。

  ```js
  if (interstitialAd) {
      interstitialAd.requestAd(MicrosoftNSJS.Advertising.InterstitialAdType.display, applicationId, adUnitId);
  }
  ```

6.  此时在你想要显示广告的代码中，确认是否已准备好显示 **InterstitialAd**，然后使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法显示它。

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  定义 **InterstitialAd** 对象的事件处理程序。

  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  生成并测试你的应用，以确认它会显示测试广告。

<span id="interstitialadsdirectx10"/>
### <a name="c-directx-interop"></a>C++（DirectX 互操作）

此示例假设你已在 Visual Studio 中创建了 C++ **DirectX 和 XAML 应用（通用 Windows）**项目，并且面向特定的 CPU 体系结构。
 
1. 在 Visual Studio 中打开你的项目。

1.  在**引用管理器**中，根据你的项目类型选择以下引用之一：

  * 对于通用 Windows 平台 (UWP) 项目：展开**通用 Windows**、单击**扩展**，然后选中**适用于 XAML 的 Microsoft Advertising SDK**（版本 10.0）旁边的复选框。

  * 对于 Windows8.1 项目：展开**Windows8.1**、单击**扩展**，然后选中**适用于 Windows8.1 XAML 的广告中介 SDK** 旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

  * 对于 Windows Phone 8.1 项目：展开**Windows Phone 8.1**、单击**扩展**，然后选中**适用于 Windows Phone 8.1 XAML 的广告中介 SDK** 旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

2.  在应用的相应头文件（例如，DirectXPage.xaml.h）中，声明 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 对象和相关的事件处理程序方法。  

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  在相同的头文件中声明几个字符串字段，这些字段代表间隙广告的应用程序 ID 和广告单元 ID。 以下代码示例将 `myAppId` 和 `myAdUnitId` 字段分配给[测试模式值](test-mode-values.md)中提供的间隙视频广告测试值。

  > [!NOTE]
  > 每个 **InterstitialAd** 都有一个对应的*广告单元*，我们的服务使用该广告单元来为控件提供广告，每个广告单元都包含*单元 ID* 和*应用程序 ID*。 在这些步骤中，你将为控件分配测试广告单元 ID 和应用程序 ID 值。 这些测试值只能在应用的测试版本中使用。 在将应用发布到应用商店之前，你必须在 Windows 开发人员中心[将这些测试值替换为实时值](#release)。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  在想要添加代码以显示间隙广告的 .cpp 文件中，添加以下命名空间引用。 以下示例假设你要将代码添加到应用的 DirectXPage.xaml.cpp 文件中。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  在启动时运行的代码中（例如，在页面的构造函数中）实例化 **InterstitialAd** 对象，并安装对象事件的事件处理程序。 在以下示例中，```InterstitialAdSamplesCpp``` 是项目的命名空间；请根据需要针对代码更改此名称。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. 如果你需要显示*间隙视频*广告：在需要间隙广告之前大约 30-60 秒，请使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法预取广告。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **AdType::Video**。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

  如果你需要显示*间隙横幅*广告（仅适用于 UWP 应用），在需要广告之前大约 5-8 秒，请使用[RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法预取广告。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。 请务必将广告类型指定为 **AdType::Display**。

  ```cpp
  m_interstitialAd->RequestAd(AdType::Display, myAppId, myAdUnitId);
  ```

7.  此时在你想要显示广告的代码中，确认是否已准备好显示 **InterstitialAd**，然后使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法显示它。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  定义 **InterstitialAd** 对象的事件处理程序。

  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. 生成并测试你的应用，以确认它会显示测试广告。

<span id="release" />
## <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>使用 Windows 开发人员中心发布带有实时广告的应用

1.  在开发人员中心仪表板中，转到应用的[通过广告盈利](../publish/monetize-with-ads.md)页面，然后[创建广告单元](../monetize/set-up-ad-units-in-your-app.md)。 对于广告单元类型，请选择**视频间隙**或**横幅间隙**，具体取决于你将要显示的间隙广告类型。 记下广告单元 ID 和应用程序 ID。

2. 如果你的应用是适用于 Windows 10 的 UWP 应用，则可以为 **InterstitialAd** 选择性启用广告中介，方法是配置[通过广告盈利](../publish/monetize-with-ads.md)页面的[广告中介](../publish/monetize-with-ads.md#mediation)部分的设置。 广告中介显示来自多个广告网络（包括其他付费广告网络，如 Taboola 和 Smaato）的广告及 Microsoft 应用促销活动的广告，从而使你能够最大化你的广告收益和应用促销能力。

3.  在你的代码中，将测试广告单元值替换为你在开发人员中心生成的实时值。

4.  使用 Windows 开发人员中心仪表板[提交应用](../publish/app-submissions.md)到应用商店。

5.  在开发人员中心仪表板中查看你的[广告性能报告](../publish/advertising-performance-report.md)。

<span id="manage" />
## <a name="manage-ad-units-for-multiple-interstitial-ad-controls-in-your-app"></a>管理你的应用中多个间隙广告控件的广告单元

你可以在单个应用中使用多个 **InterstitialAd** 控件。 在此情况下，我们建议你为每个控件分配不同的广告单元。 对每个控件使用不同的广告单元使你可以分别[配置中介设置](../publish/monetize-with-ads.md#mediation)并获取每个控件的独立[报告数据](../publish/advertising-performance-report.md)。 这还使我们的服务能够更好地优化我们为你的应用提供的广告。

> [!IMPORTANT]
> 每个广告单元都只能在一个应用中使用。 如果在多个应用中使用某个广告单元，将不为该广告单元提供广告。

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>间隙最佳做法和策略


有关如何有效使用间隙广告和必须遵循的策略的详细信息，请参阅[间隙最佳做法和策略](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

<span id="targetplatform10"/>
## <a name="remove-reference-errors-target-a-specific-cpu-platform-xaml-and-html"></a>删除引用错误：面向特定的 CPU 平台（XAML 和 HTML）


在使用 Microsoft Advertising 库时，你无法在项目中面向**任何 CPU**。 如果你的项目面向**任何 CPU** 平台，在你添加对 Microsoft Advertising 库的引用后，可能会在项目中看到一条警告。 若要删除此警告，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 有关详细信息，请参阅[已知问题](known-issues-for-the-advertising-libraries.md)。

## <a name="related-topics"></a>相关主题


* [C# 间隙广告示例代码](interstitial-ad-sample-code-in-c.md)
* [JavaScript 间隙广告示例代码](interstitial-ad-sample-code-in-javascript.md)
* [GitHub 上的广告示例](http://aka.ms/githubads)
* [为应用设置广告单元](../monetize/set-up-ad-units-in-your-app.md)
