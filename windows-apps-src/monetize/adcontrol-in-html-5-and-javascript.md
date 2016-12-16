---
author: mcleanbyron
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: "了解如何使用 AdControl 类在适用于 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 JavaScript/HTML 应用中显示横幅广告。"
title: "HTML 5 和 JavaScript 中的 AdControl"
translationtype: Human Translation
ms.sourcegitcommit: 2f0835638f330de0ac2d17dae28347686cc7ed97
ms.openlocfilehash: 501edf178ecccf8a6b62d4602837dbbdf820d744

---

# HTML 5 和 JavaScript 中的 AdControl




本演练介绍如何使用 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 类在适用于 Windows 10 (UWP)、Windows 8.1 或 Windows Phone 8.1 的 JavaScript/HTML 应用中显示横幅广告。 本演练不使用 **AdMediatorControl** 或广告中介。

有关演示如何将横幅广告添加到 JavaScript/HTML 应用的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

## 先决条件


* 对于 UWP 应用：使用 Visual Studio 2015 安装 [Microsoft Store Services SDK](http://aka.ms/store-em-sdk)。
* 对于 Windows 8.1 或 Windows Phone 8.1 应用：使用 Visual Studio 2015 或 Visual Studio 2013 安装[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)。

> **注意** 如果你已使用 Visual Studio 2015 安装了 Windows 10 Anniversary SDK Preview Build 14295 或更高版本，还必须安装 WinJS 库。 此库过去包含在以前版本的 Windows SDK（适用于 Windows 10）中，但从 Windows 10 Anniversary SDK Preview Build 14295 开始，此库必须单独安装。 若要安装 WinJS，请参阅[获取 WinJS](http://try.buildwinjs.com/download/GetWinJS/)。

## 代码开发

1. 在 Visual Studio 中，打开项目或创建新项目。

2. 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft Advertising 库的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

3.  在“解决方案资源管理器”****窗口中，右键单击“引用”****，然后选择“添加引用...”****

4.  在“引用管理器”****中，根据你的项目类型选择以下引用之一：

    -   对于通用 Windows 平台 (UWP) 项目：展开“通用 Windows”****、单击“扩展”****，然后选中“适用于 JavaScript 的 Microsoft Advertising SDK”****（版本 10.0）旁边的复选框。

    -   对于 Windows 8.1 项目：展开“Windows 8.1”****、单击“扩展”****，然后选中“适用于 Windows 8.1 Native (JS) 的 Microsoft Advertising SDK”****旁边的复选框。

    -   对于 Windows 8.1 项目：展开“Windows Phone 8.1”****、单击“扩展”****，然后选中“适用于 Windows Phone 8.1 Native (JS) 的 Microsoft Advertising SDK”****旁边的复选框。

    ![javascriptaddreference](images/13-f7f6d6a6-161e-4f17-995d-1236d0b5d9f2.png)

    > **注意** 此图像适用于生成 Windows 10 UWP 项目的 Visual Studio 2015。 如果你正在生成 Windows 8.1 或 Windows Phone 8.1 应用，或正在使用 Visual Studio 2013，你的屏幕看起来有所不同。

5.  在“引用管理器”****中，单击“确定”。

6.  打开 default.html 文件（或其他适用于你的项目的 html 文件）。

7.  在**&lt;标题&gt;**部分中，在项目的 JavaScript 引用 default.css 和 default.js 之后，添加对 ad.js 的引用。

    在 UWP 项目中，添加以下代码。

    ``` syntax
    <!-- Microsoft advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    在 Windows 8.1 或 Windows Phone 8.1 项目中，添加以下代码。

    ``` syntax
    <!-- Microsoft advertising required references -->
    <script src="/MSAdvertisingJS/ads/ad.js"></script>
    ```

    > **注意**  在包含了 default.js 之后，此行必须放置在 **&lt;head&gt;** 部分；否则，在生成项目时会遇到错误。

8.  修改 default.html 文件（或其他适用于你的项目的 html 文件）的 **&lt;body&gt;** 部分，以便包含 **AdControl** 的 div。 将 **AdControl** 中的 **applicationId** 和 **adUnitId** 属性分配给[测试模式值](test-mode-values.md)中提供的测试值，然后调整控件的高度和宽度，以使其适应[横幅广告支持的广告大小](supported-ad-sizes-for-banner-ads.md)。

    > **注意**  在提交应用之前，你需要将这些测试 **applicationId** 和 **adUnitId** 值替换为实时值。

    ``` syntax
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
          data-win-control="MicrosoftNSJS.Advertising.AdControl"
          data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    ```

9.  编译并运行应用以查看是否带有广告。

## 使用 Windows 开发人员中心发布带有实时广告的应用


1.  在开发人员中心仪表板中，转到应用的“盈利”****&gt;“利用广告来盈利”****页面，然后[创建独立的 Microsoft Advertising 单元](../publish/monetize-with-ads.md)。 对于广告单元类型，请指定“横幅”****。 记下广告单元 ID 和应用程序 ID。

2.  在你的代码中，将测试广告单元值（**applicationId** 和 **adUnitId**）替换为你在开发人员中心生成的实时值。

3.  使用开发人员中心仪表板[将应用提交](../publish/app-submissions.md)到应用商店。

4.  在开发人员中心仪表板中查看你的[广告性能报告](../publish/advertising-performance-report.md)。

## 示例 UWP 项目的完整 default.html


``` syntax
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>My_Windows_10_Ad_Funded_JavaScript_App</title>

    <!-- WinJS references -->
    <link href="//Microsoft.WinJS.2.0.Preview/css/ui-dark.css" rel="stylesheet" />
    <script src="//Microsoft.WinJS.2.0.Preview/js/base.js"></script>
    <script src="//Microsoft.WinJS.2.0.Preview/js/ui.js"></script>

    <!-- My_Windows_10_Ad_Funded_JavaScript_App references -->
    <link href="/css/default.css" rel="stylesheet" />
    <script src="/js/default.js"></script>

    <!-- Microsoft advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: '10865270'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

## 相关主题

* [GitHub 上的广告示例](http://aka.ms/githubads)
 

 



<!--HONumber=Sep16_HO2-->


