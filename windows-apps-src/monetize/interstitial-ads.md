---
author: mcleanbyron
ms.assetid: 1f970d38-2338-470e-b5ba-811402752fc4
description: "了解如何使用 Microsoft Store Services SDK 中的 Microsoft Advertising 库，将间隙广告纳入到 Windows10、Windows8.1 或 Windows Phone 8.1 应用中。"
title: "间隙广告"
translationtype: Human Translation
ms.sourcegitcommit: 8574695fe12042e44831227f81e1f6ea45e9c0da
ms.openlocfilehash: fdc9bddafc7b80f66bb160183a6c416a8573883a

---

# 间隙广告




本演练演示了如何使用 Microsoft Store Services SDK 中的 Microsoft Advertising 库，将间隙广告纳入到 Windows10、Windows8.1 或 Windows Phone 8.1 应用中。

有关演示如何使用 C# 和 C++ 向 JavaScript/HTML 应用和 XAML 应用添加间隙广告的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

<span id="whatareinterstitialads10"/>
## 什么是间隙广告？

与横幅广告不同，间隙广告（或称*间隙*）会显示在应用的整个屏幕上。 游戏中经常会使用两种基本形式。

* 通过 *Paywall* 广告，用户必须在某段固定间隔时间后观看广告。 例如在转换游戏关卡之间：

    ![whatisaninterstitial](images/13-ed0a333b-0fc8-4ca9-a4c8-11e8b4392831.png)

* 通过 *Rewards Based* 广告，用户显式寻找某些好处（例如完成关卡的提示或额外时间），并通过应用的用户界面来初始化视频广告。

    请务必注意，此 SDK 除在视频播放时外不会处理任何用户界面。 在考虑如何在应用中集成间隙广告时，可就有关处理方法、避免内容的指南参考[间隙最佳做法](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

## 使用间隙广告生成应用


### 先决条件

* 对于 UWP 应用：使用 Visual Studio 2015 安装 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)。
* 对于 Windows8.1 或 Windows Phone 8.1 应用：使用 Visual Studio 2015 或 Visual Studio 2013 安装[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。

### 代码开发

* [适用于 XAML/.NET 应用的步骤](#interstitialadsxaml10)
* [适用于 HTML/JavaScript 的步骤](#interstitialadshtml10)
* [适用于 C++（DirectX 互操作）的步骤](#interstitialadsdirectx10)

<span id="interstitialadsxaml10"/>
### 间隙广告 (XAML/.NET)

> **注意** 本部分提供 C# 示例，但 Visual Basic 和 C++ 也受支持。
 
1. 在 Visual Studio 中打开你的项目。
2. 在“引用管理器”中，根据你的项目类型选择以下引用之一：

    -   对于通用 Windows 平台 (UWP) 项目：展开“通用 Windows”、单击“扩展”，然后选中“适用于 XAML 的 Microsoft Advertising SDK”（版本 10.0）旁边的复选框。

    -   对于 Windows8.1 项目：展开“Windows8.1”、单击“扩展”，然后选中“适用于 Windows8.1 XAML 的广告中介 SDK”旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

    -   对于 Windows Phone 8.1 项目：展开“Windows Phone 8.1”、单击“扩展”，然后选中“适用于 Windows Phone 8.1 XAML 的广告中介 SDK”旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

3.  在应用代码中，包含以下命名空间引用。

    ``` syntax
    using Microsoft.Advertising.WinRT.UI;
    ```

4.  声明 `MyAppId` 和 `MyAdUnitId` 属性。

    ``` syntax
    var MyAppId = "<your app id for windows>";
    var MyAdUnitId = "<your adunit for windows";

    // if your code is in a universal solution and resides in the shared project
    // you can opt to use #if WINDOWS_APP or WINDOWS_PHONE_APP to override with different
    // identifiers for each
#if WINDOWS_PHONE_APP
    MyAppId = "<your app id for phone>";
    MyAdUnitId = "<your adunit for phone>";
#endif
    ```

    > **注意** 在提交应用之前，你需要将测试值替换为实时值。

5.  实例化 [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx)、绑定所有事件处理程序，并请求某个广告。

    ``` syntax
    // instantiate an InterstitialAd
    InterstitialAd MyVideoAd = new InterstitialAd();

    // wire up all 4 events, see below for function templates
    MyVideoAd.AdReady += MyVideoAd_AdReady;
    MyVideoAd.ErrorOccurred += MyVideoAd_ErrorOccurred;
    MyVideoAd.Completed += MyVideoAd_Completed;
    MyVideoAd.Cancelled += MyVideoAd_Cancelled;

    // pre-fetch an ad 30-60 seconds before you need it
    MyVideoAd.RequestAd(AdType.Video, MyAppId, MyAdUnitId);
    ```

6.  在代码中显示广告的地方确保广告已准备就绪，然后再展示广告。

    ``` syntax
    if ((InterstitialAdState.Ready) == (MyVideoAd.State))
    {
      MyVideoAd.Show();
    }
    ```

7.  定义事件，并为之编码。

    ``` syntax
    void MyVideoAd_AdReady(object sender, object e)
    {
      // code
    }

    void MyVideoAd_ErrorOccurred(object sender, AdErrorEventArgs e)
    {
      // code
    }

    void MyVideoAd_Completed(object sender, object e)
    {  
      // code
    }

    void MyVideoAd_Cancelled(object sender, object e)
    {
      // code
    }
    ```

8.  将 `MyAppId` 属性分配到[测试模式值](test-mode-values.md)中提供的测试值。 此值仅用于测试；你可以在发布应用前使用实时值替换。

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  将 `MyAdUnitId` 属性分配到[测试模式值](test-mode-values.md)中提供的测试值。 此值仅用于测试；你可以在发布应用前使用实时值替换。

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

10.  生成并测试你的应用，以确认它会显示测试广告。

<span id="interstitialadshtml10"/>
### 间隙广告 (HTML/JavaScript)

此示例假设你已在 Visual Studio 2015 中为 JavaScript 创建了通用应用项目，并且面向特定的 CPU。

1. 在 Visual Studio 中打开你的项目。
2.  在“引用管理器”中，根据你的项目类型选择以下引用之一：

    -   对于通用 Windows 平台 (UWP) 项目：展开“通用 Windows”、单击“扩展”，然后选中“适用于 JavaScript 的 Microsoft Advertising SDK”（版本 10.0）旁边的复选框。

    -   对于 Windows8.1 项目：展开“Windows8.1”、单击“扩展”，然后选中“适用于 Windows8.1 Native (JS) 的 Microsoft Advertising SDK”旁边的复选框。

    -   对于 Windows8.1 项目：展开“Windows Phone 8.1”、单击“扩展”，然后选中“适用于 Windows Phone 8.1 本机 (JS) 的 Microsoft Advertising SDK”旁边的复选框。

3.  在 HTML 中，包括以下脚本引用。

    ``` syntax
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

4.  声明 `myAppId` 和 `myAdUnitId` 属性。

    ``` syntax
    <script>
      var myAppId = "<your app id>";
      var myAdUnitId = "<your adunit id>";
    </script>
    ```

5.  实例化 **InterstitialAd**、绑定所有事件处理程序，并请求某个广告。

    ``` syntax
    // instantiate an InterstitialAd
    window.interstitialAd = new MicrosoftNSJS.Advertising.InterstitialAd();

    // wire up all 4 events, see below for function templates
    window.interstitialAd.onAdReady = readyHandler;
    window.interstitialAd.onErrorOccurred = errorHandler;
    window.interstitialAd.onCompleted = completeHandler;
    window.interstitialAd.onCancelled = cancelHandler;

    // pre-fetch an ad 30-60 seconds before you need it
    var myAdType = MicrosoftNSJS.Advertising.InterstitialAdType.video;
    window.interstitialAd.requestAd(myAdType, myAppId, myAdUnitId);
    ```

6.  在代码中显示广告的地方确保广告已准备就绪，然后再展示广告。

    ``` syntax
    if ((MicrosoftNSJS.Advertising.InterstitialAdState.ready) == (window.interstitialAd.state)) {
             window.interstitialAd.show();
    }
    ```

7.  定义事件，并为之编码。

    ``` syntax
    function readyHandler(sender) {
      // code
    }

    function errorHandler(sender, args) {
      // code
    }

    function completeHandler(sender) {
      // code
    }

    function cancelHandler(sender) {
      // code
    }
    ```

7.  将 `MyAppId` 属性分配到[测试模式值](test-mode-values.md)中提供的测试值。 此值仅用于测试；你可以在发布应用前使用实时值替换。

    ``` syntax
    var MyAppId = "d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

8.  将 `MyAdUnitId` 属性分配到[测试模式值](test-mode-values.md)中提供的测试值。 此值仅用于测试；你可以在发布应用前使用实时值替换。

    ``` syntax
    var MyAdUnitId = "11389925";
    ```

9.  生成并测试你的应用，以确认它会显示测试广告。

<span id="interstitialadsdirectx10"/>
### 间隙广告（C++ 和带有 XAML 互操作的 DirectX）

此示例假设你已在 Visual Studio 2015 中为 XAML 创建了通用应用项目，并且面向特定的 CPU 体系结构。

> **重要提示** 根据 DirectX 的要求使用 C++ 编写此代码。

 
1. 在 Visual Studio 中打开你的项目。
1.  在“引用管理器”中，根据你的项目类型选择以下引用之一：

    -   对于通用 Windows 平台 (UWP) 项目：展开“通用 Windows”、单击“扩展”，然后选中“适用于 XAML 的 Microsoft Advertising SDK”（版本 10.0）旁边的复选框。

    -   对于 Windows8.1 项目：展开“Windows8.1”、单击“扩展”，然后选中“适用于 Windows8.1 XAML 的广告中介 SDK”旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

    -   对于 Windows Phone 8.1 项目：展开“Windows Phone 8.1”、单击“扩展”，然后选中“适用于 Windows Phone 8.1 XAML 的广告中介 SDK”旁边的复选框。 此选项会将 Microsoft Advertising 库和广告中介库都添加到你的项目，但是你可以忽略广告中介库。

2.  在应用的相应标头文件中，声明间隙广告对象和相关的属性/方法。

    ``` syntax
    Microsoft::Advertising::WinRT::UI::InterstitialAd^ m_ia;
    void OnAdReady(Object^ sender, Object^ args);
    void OnAdCompleted(Object^ sender, Object^ args);
    void OnAdCancelled(Object^ sender, Object^ args);
    void OnAdError (Object^ sender,  Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args);
    ```

3.  声明 `AppId` 和 `AdUnitId` 属性。

    ``` syntax
    #if WINDOWS_PHONE_APP
    static Platform::String^ IA_APPID = L"<your app id for phone>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for phone>";
    #endif

    #if WINDOWS_APP
    static Platform::String^ IA_APPID = L"<your app id for windows>";
    static Platform::String^ IA_ADUNITID = L"<your ad unit for windows>";
    #endif
    ```

4.  在 .cpp 文件中，添加命名空间引用。

    ``` syntax
    using namespace Microsoft::Advertising::WinRT::UI;
    ```

5.  实例化 **InterstitialAd**、绑定所有事件处理程序，并请求某个广告。

    ``` syntax
    // Instantiate an InterstitialAd.
    m_ia = ref new InterstitialAd();

    // Wire up all 4 events, see below for function templates.            
    m_ia->AdReady += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdReady);
    m_ia->Completed += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCompleted);
    m_ia->Cancelled += ref new Windows::Foundation::EventHandler<Platform::Object ^>
        (this, &Simple3DGameXaml::DirectXPage::OnAdCancelled);
    m_ia->ErrorOccurred += ref new
        Windows::Foundation::EventHandler<Microsoft::Advertising::WinRT::UI::AdErrorEventArgs ^>
            (this, &Simple3DGameXaml::DirectXPage::OnAdError);

    // Pre-fetch an ad 30-60 seconds before you need it.
    m_ia->RequestAd(AdType::Video, IA_APPID, IA_ADUNITID);
    ```

6.  在代码中显示广告的地方确保广告已准备就绪，然后再展示广告。

    ``` syntax
    if ((InterstitialAdState::Ready == m_ia->State))
    {
        m_ia->Show();
    }
    ```

7.  定义事件，并为之编码。

    ``` syntax
    void DirectXPage::OnAdReady(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCompleted(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdCancelled(Object^ sender, Object^ args)
    {
      // code
    }

    void DirectXPage::OnAdError
      (Object^ sender, Microsoft::Advertising::WinRT::UI::AdErrorEventArgs^ args)
    {
      // code
    }
    ```

8.  将 `AppId` 属性分配到[测试模式值](test-mode-values.md)中提供的测试值。 此值仅用于测试；你可以在发布应用前使用实时值替换。

    ``` syntax
    static Platform::String^ IA_APPID = L"d25517cb-12d4-4699-8bdc-52040c712cab";
    ```

9.  将 `AdUnitId` 属性分配到[测试模式值](test-mode-values.md)中提供的测试值。 此值仅用于测试；你可以在发布应用前使用实时值替换。

    ``` syntax
    static Platform::String^ IA_ADUNITID = L"11389925";
    ```

10. 生成并测试你的应用，以确认它会显示测试广告。

### 使用 Windows 开发人员中心发布带有实时广告的应用

1.  在开发人员中心仪表板中，转到应用的“盈利”&gt;“利用广告来盈利”页面，然后[创建独立的 Microsoft Advertising 单元](../publish/monetize-with-ads.md)。 对于广告单元类型，指定“视频间隙”。 记下广告单元 ID 和应用程序 ID。

2.  在你的代码中，将测试广告单元值替换为你在开发人员中心生成的实时值。

3.  使用 Windows 开发人员中心仪表板[将应用提交](../publish/app-submissions.md)到应用商店。

4.  在开发人员中心仪表板中查看[广告性能报告](../publish/advertising-performance-report.md)。

<span id="interstitialbestpractices10"/>
## 间隙最佳做法和策略


有关如何有效使用间隙广告和必须遵循的策略的详细信息，请参阅[间隙最佳做法和策略](ui-and-user-experience-guidelines.md#interstitialbestpractices10)。

<span id="targetplatform10"/>
## 删除引用错误：面向特定的 CPU 平台（XAML 和 HTML）


在使用 Microsoft Advertising 库时，你无法在项目中面向**任何 CPU**。 如果你的项目面向**任何 CPU** 平台，在你添加对 Microsoft Advertising 库的引用后，可能会在项目中看到一条警告。 若要删除此警告，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 有关详细信息，请参阅[已知问题](known-issues-for-the-advertising-libraries.md)。

## 相关主题


* [C 中的间隙广告示例代码#](interstitial-ad-sample-code-in-c.md)
* [JavaScript 中的间隙广告示例代码](interstitial-ad-sample-code-in-javascript.md)
* [GitHub 上的广告示例](http://aka.ms/githubads)

 

 



<!--HONumber=Nov16_HO1-->


