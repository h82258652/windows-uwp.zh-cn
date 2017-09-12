---
author: mcleanbyron
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: "详细了解当前版本 Microsoft Advertising 库的已知问题。"
title: "Advertising 库的已知问题"
ms.author: mcleans
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 广告, 已知问题"
ms.openlocfilehash: b18c4568770afb70bcca991c79d59a9912981705
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2017
---
# <a name="known-issues-for-the-advertising-libraries"></a>Advertising 库的已知问题




本主题列出了 Microsoft Store Services SDK（适用于 UWP 应用）以及适用于 Windows 和 Windows Phone 8.x 的 Microsoft 广告 SDK（适用于 Windows 8.1 和 Windows Phone 8.x 应用）中当前版本的 Microsoft Advertising 库的已知问题。

## <a name="windows-phone-8x-silverlight-projects"></a>Windows Phone 8.x Silverlight 项目

适用于 Windows 和 Windows Phone 8.x 的 Microsoft 广告 SDK 有限支持 Windows Phone 8.x Silverlight 项目。 有关详细信息，请参阅[用于 Windows Phone 8.x Silverlight 项目的广告支持](adcontrol-in-windows-phone-silverlight.md#silverlight_support)。

若要获取适用于 Windows Phone 8.x Silverlight 项目的 Microsoft Advertising 程序集，请安装[适用于 Windows 和 Windows Phone 8.x 的 Microsoft Advertising SDK](http://aka.ms/store-8-sdk)、在 Visual Studio 中打开你的项目，然后转到**项目** > **添加连接的服务** > **广告中介**即可自动下载程序集。 在执行此操作之后，如果你不想要使用广告中介，可以将广告中介引用从项目中删除。 有关详细信息，请参阅 [Windows Phone Silverlight 中的 AdControl](adcontrol-in-windows-phone-silverlight.md)。

## <a name="adcontrol-interface-unknown-in-xaml"></a>XAML 中的 AdControl 接口未知

适用于 [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) 的 XAML 标记可能会错误显示暗示接口未知的蓝色曲线。 这仅在面向 x86 时发生，并且可能会被省略。

## <a name="lasterror-from-previous-ad-request"></a>之前广告请求的 lastError

如果之前广告请求还留有 **lastError**，则在下一次广告调用期间，可能会引发该事件两次。 尽管还是会提出新的广告请求，而且可能也会产生有效广告，但此行为可能会引起混淆。

## <a name="interstitial-ads-and-navigation-buttons-on-phones"></a>手机上的间隙广告和导航按钮

在拥有软件**后退**、**开始**以及**搜索**按钮而非硬件按钮的手机（仿真器）上，倒计时器和单击间隙广告的按钮可能会被遮住。

## <a name="recently-created-ads-are-not-being-served-to-your-app"></a>未向你的应用投放最近创建的广告

如果你最近（少于一天）创建了广告，可能不会立即可用。 如果广告的编辑内容已经过批准，则在广告服务器已对其进行处理并且该广告作为库存可用时会立即投放。

## <a name="no-ads-are-shown-in-your-app"></a>你的应用中没有显示任何广告

你没有看到广告的原因有很多，其中包括网络错误。 其他原因可能包括：

* 在 Windows 开发人员中心中选择某个广告单元，其大小大于或小于应用代码中的 **AdControl** 的大小。

* 在运行动态应用时，如果将[测试模式值](test-mode-values.md)用于广告单元 ID，则广告不会显示。

* 如果你在过去半小时创建了新的广告单元 ID，可能无法看到广告，直到服务器通过系统传播新数据为止。 之前显示了广告的现有 ID 应会立即显示广告。

如果你可以在应用中看到测试广告，则代码有效，并且能够显示广告。 如果你遇到问题，请联系[产品支持人员](https://go.microsoft.com/fwlink/p/?LinkId=331508)。 在该页面上，选择**应用内广告**。

你还可在[论坛](http://go.microsoft.com/fwlink/p/?LinkId=401266)发布问题。

## <a name="test-ads-are-showing-in-your-app-instead-of-live-ads"></a>应用中显示的是测试广告而非实时广告

可以显示测试广告，即使你希望显示实时广告。 这可在以下方案中发生：

* 我们的广告平台无法验证或找到在应用商店中使用的动态应用程序 ID。 在此情况下，当用户创建了某个广告单元时，它的状态仍然可以为动态（非测试），但会在提出首个广告请求 6 个小时内移动到测试状态。 如果测试应用 10 天内没有提出请求，状态将改回为动态。

* 旁加载应用或在仿真器中运行的应用不会显示实时广告。

当实时广告单元服务测试广告时，该广告单元的状态会在 Windows 开发人员中心中显示**活动并服务测试广告**。 这当前不适用于手机应用。

## <a name="obsolete-test-values-for-ad-unit-id-and-application-id-no-longer-working"></a>广告单元 ID 和应用程序 ID 的过时测试值不再有用。

Windows Phone Silverlight 应用的以下测试值已过时，并且不再有用。 如果你拥有使用这些测试值的现有项目，请将项目更新为使用[测试模式值](test-mode-values.md)中提供的值。

| 应用程序 ID  |  广告单元 ID    |
|-----------------|----------------|
| test_client     |  Image320_50   |
| test_client     |  Image300_50   |
| test_client     |  TextAd   |
| test_client     |  Image480_80   |

<span id="reference_errors"/>
## <a name="reference-errors-caused-by-targeting-any-cpu-in-your-project"></a>项目中通过面向任何 CPU 引起的引用错误

在使用 Microsoft Advertising 库时，你无法在项目中面向**任何 CPU**。 如果你的项目面向**任何 CPU** 平台，可能在添加类似于此引用的引用后会看到警告。

![referenceerror\-solutionexplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

若要删除此警告，请更新你的项目以使用特定于体系结构的生成输出（例如，**x86**）。 使用**配置管理器**以设置适用于调试和版本配置的平台目标。

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

在你为应用商店提交创建应用包（如下图所示）时，请确保包含计划面向的体系结构。 如果计划在 x64 操作系统上运行 x86 版本，可以选择跳过 x64。

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## <a name="z-order-in-javascripthtml-apps"></a>JavaScript/HTML 应用中的 Z 顺序

JavaScript/HTML 应用不得将元素放入 Z 顺序的保留 MAX-10 范围。 唯一的例外是中断覆盖层，例如 Skype 应用的入站呼叫通知。

<span id="bkmk-ui"/>
## <a name="do-not-use-borders"></a>不要使用边框

设置由 **AdControl** 从父类继承的边框相关的属性将引起广告位置出错。

## <a name="more-information"></a>详细信息


有关最新的已知问题和发布与 Microsoft Advertising 库相关的问题的详细信息，请访问[论坛](http://go.microsoft.com/fwlink/p/?LinkId=401266)。

## <a name="support"></a>支持


若要联系产品支持人员询问有关 Microsoft Advertising 库的问题，请访问[支持页面](https://go.microsoft.com/fwlink/p/?LinkId=331508)，然后选择**应用内广告**。

 

 
