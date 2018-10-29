---
author: Xansky
ms.assetid: adb2fa45-e18f-4254-bd8b-a749a386e3b4
description: 了解如何使用 AdControl 类在适用于 Windows10 (UWP) 的 JavaScript/HTML 应用中显示横幅广告。
title: HTML 5 和 JavaScript 中的 AdControl
ms.author: mhopkins
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 广告, AdControl, 广告控件, javascript, HTML
ms.localizationpriority: medium
ms.openlocfilehash: de3ffa9e82687e31c7f91548be953f224975d09a
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/29/2018
ms.locfileid: "5768993"
---
# <a name="adcontrol-in-html-5-and-javascript"></a>HTML 5 和 JavaScript 中的 AdControl

本演练介绍如何使用 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 类在适用于 Windows 10 的通用 Windows 平台 (UWP) JavaScript/HTML 应用中显示横幅广告。

有关演示如何将横幅广告添加到 JavaScript/HTML 应用的完整示例项目，请参阅 [GitHub 上的广告示例](http://aka.ms/githubads)。

## <a name="prerequisites"></a>先决条件

* 使用 Visual Studio 2015 或更高版本的 Visual Studio 安装 [Microsoft 广告 SDK](http://aka.ms/ads-sdk-uwp)。 有关安装说明，请参阅[此文章](install-the-microsoft-advertising-libraries.md)。

> [!NOTE]
> 如果已安装 Windows 10 SDK 版本 10.0.14393 （周年更新） 或更高版本的 Windows SDK，你还必须安装[WinJS](https://github.com/winjs/winjs)库。 此库过去包含在以前版本的 Windows SDK（适用于 Windows 10）中，但从 Windows 10 SDK 版本 10.0.14393（周年更新）开始，此库必须单独安装。 

## <a name="integrate-a-banner-ad-into-your-app"></a>在应用中集成横幅广告

1. 在 Visual Studio 中，打开项目或创建新项目。

    > [!NOTE]
    > 如果你使用现有项目，请打开项目中的 Package.appxmanifest 文件并确保已选择 **Internet（客户端）** 功能。 应用需要使用此功能接收测试广告和实时广告。

2. 如果你的项目面向**任何 CPU**，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 如果你的项目面向**任何 CPU**，你将无法在以下步骤中成功添加对 Microsoft Advertising 库的引用。 有关详细信息，请参阅[项目中由面向任何 CPU 引起的引用错误](known-issues-for-the-advertising-libraries.md#reference_errors)。

3. 在你的项目中添加对 Microsoft 广告 SDK 的引用：

    1. 在**解决方案资源管理器**窗口中，右键单击**引用**，然后选择**添加引用...**
    2.  在**引用管理器**中，展开**通用 Windows**、单击**扩展**，然后选中**适用于 JavaScript 的 Microsoft 广告 SDK**（版本 10.0）旁边的复选框。
    3.  在**引用管理器**中，单击“确定”。

6.  打开 index.html 文件（或其他适用于你项目的 html 文件）。

7.  在**&lt;标题&gt;** 部分中，在项目的 JavaScript 引用 default.css 和 main.js 之后，添加对 ad.js 的引用。

    ``` HTML
    <!-- Advertising required references -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
    ```

    > [!NOTE]
    > 在包含了 main.js; 之后，此行必须放置在 **&lt;head&gt;** 部分；否则，当你生成项目时将遇到错误。

8.  修改 default.html 文件（或其他适用于你项目的 html 文件）的 **&lt;body&gt;** 部分，以便包含 **AdControl** 的 **div**。 将 **AdControl** 的 **applicationId** 和 **adUnitId** 属性分配至[测试广告单元值](set-up-ad-units-in-your-app.md#test-ad-units)。 另外还要调整控件的**高度**和**宽度**，以使其适应[横幅广告支持的广告大小](supported-ad-sizes-for-banner-ads.md)。

    > [!NOTE]
    > 每个 **AdControl** 都有一个对应的*广告单元*，我们的服务使用该广告单元来为控件提供广告，每个广告单元都包含*单元 ID* 和*应用程序 ID*。 在这些步骤中，你将为控件分配测试广告单元 ID 和应用程序 ID 值。 这些测试值只能在应用的测试版本中使用。 在将应用发布到应用商店之前，你必须在 Windows 开发人员中心[将这些测试值替换为实时值](#release)。

    ``` HTML
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
        data-win-control="MicrosoftNSJS.Advertising.AdControl"
        data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    ```

9.  编译并运行应用以查看是否带有广告。

以下示例所示为简单应用的完整 index.html。

``` HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>AdControlExampleApp</title>
    <!-- WinJS references -->
    <link href="lib/winjs-4.0.1/css/ui-light.css" rel="stylesheet" />
    <script src="lib/winjs-4.0.1/js/base.js"></script>
    <script src="lib/winjs-4.0.1/js/ui.js"></script>
    <!-- AdControlExampleApp references -->
    <link href="css/default.css" rel="stylesheet" />
    <script src="js/main.js"></script>
    <!-- Required reference for AdControl -->
    <script src="//Microsoft.Advertising.JavaScript/ad.js"></script>
</head>
<body>
    <div id="myAd" style="position: absolute; top: 50px; left: 0px; width: 300px; height: 250px; z-index: 1"
      data-win-control="MicrosoftNSJS.Advertising.AdControl"
      data-win-options="{applicationId: '3f83fe91-d6be-434d-a0ae-7351c5a997f1', adUnitId: 'test'}">
    </div>
    <p>Content goes here</p>
</body>
</html>
```

### <a name="create-an-adcontrol-programmatically-in-javascript"></a>使用 Javascript 以编程方式创建 AdControl

上述步骤显示了如何在 HTML 标注中声明 **AdControl**。 或者，你可以使用 JavaScript 以编程方式创建 **AdControl**。 本示例假定在带有 ID **myAd** 的 HTML 中使用现有的 **div**。

> [!div class="tabbedCodeSnippets"]
[!code-javascript[AdControl](./code/AdvertisingSamples/AdControlSamples/js/main.js#DeclareAdControl)]

此示例假定你已经声明了名为 **myAdError**、**myAdRefreshed** 和 **myAdEngagedChanged** 的事件处理程序方法。

如果你使用此代码，并且没有看到广告，则可以尝试将 **position:relative** 的属性插入包含 **AdControl** 的 **div** 中。 这将替代 **IFrame** 的默认设置。 广告将正确显示，除非它们由于此属性的值而没有显示。 请注意，新的广告单元可能在长达 30 分钟内不可用。

> [!NOTE]
> 此示例中显示的 *applicationId* 和 *adUnitId* 值是[测试模式值](set-up-ad-units-in-your-app.md#test-ad-units)。 在提交应用之前，必须通过 Windows 开发人员中心[将测试值替换为实时值](set-up-ad-units-in-your-app.md#live-ad-units)。

<span id="release" />

## <a name="release-your-app-with-live-ads"></a>发布包含实时广告的应用

1. 确保在应用中对横幅广告的使用遵循我们的[横幅广告指南](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)。

1.  在开发人员中心仪表板中转到[应用内广告](../publish/in-app-ads.md)页面，然后[创建广告单元](set-up-ad-units-in-your-app.md#live-ad-units)。 对于广告单元类型，请指定**横幅**。 记下广告单元 ID 和应用程序 ID。
    > [!NOTE]
    > 测试广告单元和实时 UWP 广告单元的应用程序 ID 值采用不同的格式。 测试应用程序 ID 值为 GUID。 在仪表板中创建实时 UWP 广告单元时，该广告单元的应用程序 ID 值始终与应用的应用商店 ID（例如应用商店 ID 值类似于 9NBLGGH4R315）匹配。

2. 你可以选择通过配置[中介设置](../publish/in-app-ads.md#mediation)部分（位于[应用内广告](../publish/in-app-ads.md)页面上）的设置为 **AdControl** 启用广告中介。 广告中介显示来自多个广告网络（包括其他付费广告网络，如 Taboola 和 Smaato）的广告及 Microsoft 应用促销活动的广告，从而使你能够最大化你的广告收益和应用促销能力。

3.  在你的代码中，将测试广告单元值（**applicationId** 和 **adUnitId**）替换为你在开发人员中心生成的实时值。

4.  使用开发人员中心仪表板 [提交应用](../publish/app-submissions.md) 至应用商店。

5.  在开发人员中心仪表板中查看你的[广告性能报告](../publish/advertising-performance-report.md)。             

<span id="manage" />

## <a name="manage-ad-units-for-multiple-ad-controls-in-your-app"></a>管理你的应用中多个广告控件的广告单元

可在一个应用中使用多个 **AdControl** 对象（例如，应用中的每页可以托管不同的 **AdControl** 对象）。 在此情况下，我们建议你为每个控件分配不同的广告单元。 对每个控件使用不同的广告单元使你可以分别[配置中介设置](../publish/in-app-ads.md#mediation)并获取每个控件的独立[报告数据](../publish/advertising-performance-report.md)。 这还使我们的服务能够更好地优化我们为你的应用提供的广告。

> [!IMPORTANT]
> 每个广告单元都只能在一个应用中使用。 如果在多个应用中使用某个广告单元，将不为该广告单元提供广告。

## <a name="related-topics"></a>相关主题

* [横幅广告指南](ui-and-user-experience-guidelines.md#guidelines-for-banner-ads)
* [GitHub 上的广告示例](http://aka.ms/githubads)
* [为应用设置广告单元](set-up-ad-units-in-your-app.md)
* [JavaScript 演练中的错误处理](error-handling-in-javascript-walkthrough.md)
