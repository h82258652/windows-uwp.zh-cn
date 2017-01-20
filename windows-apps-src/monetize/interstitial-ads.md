---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "了解如何使用 Microsoft Store Services SDK 中的 Microsoft Advertising 库，将间隙广告纳入到 Windows 10、Windows 8.1 或 Windows Phone 8.1 应用中。"
title: "间隙广告"
translationtype: Human Translation
ms.sourcegitcommit: 2b5dbf872dd7aad48373f6a6df3dffbcbaee8090
ms.openlocfilehash: fae0fc57eca3477bf46a6f3ac43ec35781241a6e

---

# <a name="interstitial-ads"></a>间隙广告

本演练演示了如何使用 Microsoft Store Services SDK 中的 Microsoft Advertising 库，将间隙广告纳入到 Windows 10、Windows 8.1 或 Windows Phone 8.1 应用中。

有关演示如何使用 C# 和 C++ 向 JavaScript/HTML 应用和 XAML 应用添加间隙广告的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

<span id="whatareinterstitialads10"/>
## <a name="what-are-interstitial-ads"></a>什么是间隙广告？

与横幅广告不同，间隙广告（或称*间隙*）会显示在应用的整个屏幕上。 游戏中经常会使用两种基本形式。

* 通过 *Paywall* 广告，用户必须在某段固定间隔时间后观看广告。 例如在转换游戏关卡之间：

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* 通过 *Rewards Based* 广告，用户显式寻找某些好处（例如完成关卡的提示或额外时间），并通过应用的用户界面来初始化视频广告。

    请务必注意，此 SDK 除在视频播放时外不会处理任何用户界面。 在考虑如何在应用中集成间隙广告时，可就有关处理方法、避免内容的指南参考[间隙最佳做法](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

## <a name="build-an-app-with-interstitial-ads"></a>使用间隙广告生成应用

若要在应用中显示间隙广告，请遵循针对项目类型的说明：

* [XAML/.NET](#interstitialadsxaml10)
* [HTML/JavaScript](#interstitialadshtml10)
* [C++（DirectX 互操作）](#interstitialadsdirectx10)

<span/>
### <a name="prerequisites"></a>先决条件

* 对于 UWP 应用：使用 Visual Studio 2015 安装 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)。
* 对于 Windows 8.1 或 Windows Phone 8.1 应用：使用 Visual Studio 2015 或 Visual Studio 2013 安装[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。

<span id="interstitialadsxaml10"/>
###<a name="xamlnet"></a>XAML/.NET

本部分提供 C# 示例，但 Visual Basic 和 C++ 也受 XAML/.NET 项目支持。 有关完整的 C# 代码示例，请参阅 [C# 中的间隙广告示例代码](interstitial-ad-sample-code-in-c.md)。

1. 在 Visual Studio 中打开你的项目。

2. 在**引用管理器**中，根据你的项目类型选择以下引用之一：

  * 对于通用 Windows 平台 (UWP) 项目：展开“通用 Windows”、单击“扩展”，然后选中“适用于 XAML 的 Microsoft Advertising SDK”（版本 10.0）旁边的复选框。

  * 对于 Windows 8.1 项目：展开“Windows 8.1”、单击“扩展”，然后选中“适用于 Windows 8.1 XAML 的广告中介 SDK”旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

  * 对于 Windows Phone 8.1 项目：展开“Windows Phone 8.1”、单击“扩展”，然后选中“适用于 Windows Phone 8.1 XAML 的广告中介 SDK”旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

3.  在应用的相应代码文件中（例如，在 MainPage.xaml.cs 或部分其他页面的代码文件中）添加以下命名空间引用。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet1)]

4.  在应用的相应位置（例如，在 ```MainPage``` 或部分其他页面）声明 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 对象和几个字符串字段，这些字段代表间隙广告的应用程序 ID 和广告单元 ID。 以下代码示例将 `myAppId` 和 `myAdUnitId` 字段分配给[测试模式值](test-mode-values.md)中提供的测试值。 这些值仅用于测试；你必须在发布应用前使用 Windows 开发人员中心中的实时值替换它们。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet2)]

5.  在启动时运行的代码中（例如，在页面的构造函数中）实例化 **InterstitialAd** 对象，并安装对象事件的事件处理程序。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet3)]

6.  在你需要间隙广告之前大约 30-60 秒，请使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法预取广告。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet4)]

6.  此时在你想要显示广告的代码中，确认是否已准备好显示 **InterstitialAd**，然后使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法显示它。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet5)]

7.  定义 **InterstitialAd** 对象的事件处理程序。

  > [!div class="tabbedCodeSnippets"]
  [!code-cs[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cs/MainPage.xaml.cs#Snippet6)]

8.  生成并测试你的应用，以确认它会显示测试广告。

<span id="interstitialadshtml10"/>
###<a name="htmljavascript"></a>HTML/JavaScript

以下说明假设你已在 Visual Studio 2015 中为 JavaScript 创建了通用 Windows 项目，并且面向特定的 CPU。 有关完整的代码示例，请参阅 [JavaScript 中的间隙广告示例代码](interstitial-ad-sample-code-in-javascript.md)。

1. 在 Visual Studio 中打开你的项目。

2.  在**引用管理器**中，根据你的项目类型选择以下引用之一：

  * 对于通用 Windows 平台 (UWP) 项目：展开“通用 Windows”****、单击“扩展”****，然后选中“适用于 JavaScript 的 Microsoft Advertising SDK”****（版本 10.0）旁边的复选框。

  * 对于 Windows 8.1 项目：展开“Windows 8.1”****、单击“扩展”****，然后选中“适用于 Windows 8.1 Native (JS) 的 Microsoft Advertising SDK”****旁边的复选框。

  * 对于 Windows 8.1 项目：展开“Windows Phone 8.1”****、单击“扩展”****，然后选中“适用于 Windows Phone 8.1 Native (JS) 的 Microsoft Advertising SDK”****旁边的复选框。

3.  在项目的 HTML 文件的**&lt;标题&gt;**部分中，在项目的 JavaScript 引用 default.css 和 default.js 之后，添加对 ad.js 的引用。 在 UWP 项目中，添加以下引用。

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
  ```

  在 Windows 8.1 或 Windows Phone 8.1 项目中，添加以下引用。

  > [!div class="tabbedCodeSnippets"]
  ``` html
  <script src="/MSAdvertisingJS/ads/ad.js"></script>
  ```

4.  在项目的 .js 文件中声明 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 对象和几个字段，这些字段包含间隙广告的应用程序 ID 和广告单元 ID。 以下代码示例将 `applicationId` 和 `adUnitId` 字段分配给[测试模式值](test-mode-values.md)中提供的测试值。 这些值仅用于测试；你必须在发布应用前使用 Windows 开发人员中心中的实时值替换它们。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet1)]

5.  在启动时运行的代码中（例如，在页面的构造函数中）实例化 **InterstitialAd** 对象，并安装对象的事件处理程序。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet2)]

5. 在你需要间隙广告之前大约 30-60 秒，请使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法预取广告。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/script.js#Snippet3)]

6.  此时在你想要显示广告的代码中，确认是否已准备好显示 **InterstitialAd**，然后使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法显示它。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet4)]

7.  定义 **InterstitialAd** 对象的事件处理程序。

  > [!div class="tabbedCodeSnippets"]
  [!code-javascript[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/js/samples.js#Snippet5)]

9.  生成并测试你的应用，以确认它会显示测试广告。

<span id="interstitialadsdirectx10"/>
###<a name="c-directx-interop"></a>C++（DirectX 互操作）

此示例假设你已在 Visual Studio 2015 中创建了 C++ **DirectX 和 XAML 应用（通用 Windows）**项目，并且面向特定的 CPU 体系结构。
 
1. 在 Visual Studio 中打开你的项目。

1.  在**引用管理器**中，根据你的项目类型选择以下引用之一：

  * 对于通用 Windows 平台 (UWP) 项目：展开“通用 Windows”、单击“扩展”，然后选中“适用于 XAML 的 Microsoft Advertising SDK”（版本 10.0）旁边的复选框。

  * 对于 Windows 8.1 项目：展开“Windows 8.1”、单击“扩展”，然后选中“适用于 Windows 8.1 XAML 的广告中介 SDK”旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

  * 对于 Windows Phone 8.1 项目：展开“Windows Phone 8.1”、单击“扩展”，然后选中“适用于 Windows Phone 8.1 XAML 的广告中介 SDK”旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

2.  在应用的相应头文件（例如，DirectXPage.xaml.h）中，声明 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) 对象和相关的事件处理程序方法。  

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet1)]

3.  在相同的头文件中声明几个字符串字段，这些字段代表间隙广告的应用程序 ID 和广告单元 ID。 以下代码示例将 `myAppId` 和 `myAdUnitId` 字段分配给[测试模式值](test-mode-values.md)中提供的测试值。 这些值仅用于测试；你必须在发布应用前使用 Windows 开发人员中心中的实时值替换它们。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.h#Snippet2)]

4.  在想要添加代码以显示间隙广告的 .cpp 文件中，添加以下命名空间引用。 以下示例假设你要将代码添加到应用的 DirectXPage.xaml.cpp 文件中。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet3)]

6.  在启动时运行的代码中（例如，在页面的构造函数中）实例化 **InterstitialAd** 对象，并安装对象事件的事件处理程序。 在以下示例中，```InterstitialAdSamplesCpp``` 是项目的命名空间；请根据需要针对代码更改此名称。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet4)]

7. 在你需要间隙广告之前大约 30-60 秒，请使用 [RequestAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.requestad.aspx) 方法预取广告。 这样就允许在应该显示广告之前有足够的时间请求和准备广告。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet5)]

7.  此时在你想要显示广告的代码中，确认是否已准备好显示 **InterstitialAd**，然后使用 [Show](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.show.aspx) 方法显示它。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet6)]

8.  定义 **InterstitialAd** 对象的事件处理程序。

  > [!div class="tabbedCodeSnippets"]
  [!code-cpp[InterstitialAd](./code/AdvertisingSamples/InterstitialAdSamples/cpp/DirectXPage.xaml.cpp#Snippet7)]

9. 生成并测试你的应用，以确认它会显示测试广告。

<span/>
### <a name="release-your-app-with-live-ads-using-windows-dev-center"></a>使用 Windows 开发人员中心发布带有实时广告的应用

1.  在开发人员中心仪表板中，转到应用的“盈利”****&gt;“利用广告来盈利”****页面，然后“创建广告单元”[](../publish/monetize-with-ads.md)。 对于广告单元类型，指定“视频间隙”****。 记下广告单元 ID 和应用程序 ID。

2.  在你的代码中，将测试广告单元值替换为你在开发人员中心生成的实时值。

3.  使用 Windows 开发人员中心仪表板[提交应用](../publish/app-submissions.md)到应用商店。

4.  在开发人员中心仪表板中查看[广告性能报告](../publish/advertising-performance-report.md)。

<span id="interstitialbestpractices10"/>
## <a name="interstitial-best-practices-and-policies"></a>间隙最佳做法和策略


有关如何有效使用间隙广告和必须遵循的策略的详细信息，请参阅[间隙最佳做法和策略](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

<span id="targetplatform10"/>
## <a name="remove-reference-errors-target-a-specific-cpu-platform-xaml-and-html"></a>删除引用错误：面向特定的 CPU 平台（XAML 和 HTML）


在使用 Microsoft Advertising 库时，你无法在项目中面向**任何 CPU**。 如果你的项目面向**任何 CPU** 平台，在你添加对 Microsoft Advertising 库的引用后，可能会在项目中看到一条警告。 若要删除此警告，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 有关详细信息，请参阅[已知问题](known-issues-for-the-advertising-libraries.md)。

## <a name="related-topics"></a>相关主题


* [C 中的间隙广告示例代码#](interstitial-ad-sample-code-in-c.md)
* [JavaScript 中的间隙广告示例代码](interstitial-ad-sample-code-in-javascript.md)
* [GitHub 上的广告示例](http://aka.ms/githubads)

 

 



<!--HONumber=Dec16_HO2-->


