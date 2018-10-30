---
author: Xansky
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: 详细了解当前版本 Microsoft 广告 SDK 的已知问题。
title: 有关应用内广告的已知问题和疑难解答
ms.author: mhopkins
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, 广告, 已知问题, 疑难解答
ms.localizationpriority: medium
ms.openlocfilehash: 3adbc12b0e891461a97bb90575141517b280be76
ms.sourcegitcommit: 753e0a7160a88830d9908b446ef0907cc71c64e7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2018
ms.locfileid: "5760156"
---
# <a name="known-issues-and-troubleshooting-for-ads-in-apps"></a>有关应用内广告的已知问题和疑难解答

本主题列出了当前版本的 Microsoft 广告 SDK 的已知问题。 有关其他疑难解答指南，请参阅以下主题。

* [HTML 和 JavaScript 疑难解答指南](html-and-javascript-troubleshooting-guide.md)
* [XAML 和 C# 疑难解答指南](xaml-and-c-troubleshooting-guide.md)

## <a name="adcontrol-interface-unknown-in-xaml"></a>XAML 中的 AdControl 接口未知

适用于 [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) 的 XAML 标记可能会错误显示暗示接口未知的蓝色曲线。 这仅在面向 x86 时发生，并且可能会被省略。

## <a name="lasterror-from-previous-ad-request"></a>之前广告请求的 lastError

如果之前广告请求还留有 **lastError**，则在下一次广告调用期间，可能会引发该事件两次。 尽管还是会提出新的广告请求，而且可能也会产生有效广告，但此行为可能会引起混淆。

## <a name="interstitial-ads-and-navigation-buttons-on-phones"></a>手机上的间隙广告和导航按钮

在拥有软件**后退**、**开始**以及**搜索**按钮而非硬件按钮的手机（仿真器）上，倒计时器和单击间隙广告的按钮可能会被遮住。

## <a name="recently-created-ads-are-not-being-served-to-your-app"></a>未向你的应用投放最近创建的广告

如果你最近（少于一天）创建了广告，可能不会立即可用。 如果广告的编辑内容已经过批准，则在广告服务器已对其进行处理并且该广告作为库存可用时会立即投放。

## <a name="no-ads-are-shown-in-your-app"></a>你的应用中没有显示任何广告

你没有看到广告的原因有很多，其中包括网络错误。 其他原因可能包括：

* 在 Windows 开发人员中心中选择某个广告单元，其大小大于或小于应用代码中的 **AdControl** 的大小。

* 在运行动态应用时，如果将[测试模式值](set-up-ad-units-in-your-app.md#test-ad-units)用于广告单元 ID，则广告不会显示。

* 如果你在过去半小时创建了新的广告单元 ID，可能无法看到广告，直到服务器通过系统传播新数据为止。 之前显示了广告的现有 ID 应会立即显示广告。

如果你可以在应用中看到测试广告，则代码有效，并且能够显示广告。 如果遇到问题，请联系[产品支持人员](https://developer.microsoft.com/en-us/windows/support)。 在该页面上，选择**应用内广告**。

你还可在[论坛](http://go.microsoft.com/fwlink/p/?LinkId=401266)发布问题。

## <a name="test-ads-are-showing-in-your-app-instead-of-live-ads"></a>应用中显示的是测试广告而非实时广告

可以显示测试广告，即使你希望显示实时广告。 这可在以下方案中发生：

* 我们的广告平台无法验证或找到在应用商店中使用的动态应用程序 ID。 在此情况下，当用户创建了某个广告单元时，它的状态仍然可以为动态（非测试），但会在提出首个广告请求 6 个小时内移动到测试状态。 如果测试应用 10 天内没有提出请求，状态将改回为动态。

* 旁加载应用或在仿真器中运行的应用不会显示实时广告。

当实时广告单元服务测试广告时，该广告单元的状态会在 Windows 开发人员中心中显示**活动并服务测试广告**。 这当前不适用于手机应用。


<span id="reference_errors"/>

## <a name="reference-errors-caused-by-targeting-any-cpu-in-your-project"></a>项目中通过面向任何 CPU 引起的引用错误

在使用 Microsoft 广告 SDK 时，你无法在项目中面向**任何 CPU**。 如果你的项目面向**任何 CPU** 平台，可能会在添加类似于此引用的引用后看到警告。

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

有关最新的已知问题和发布与 Microsoft 广告 SDK 相关的问题的详细信息，请访问[论坛](http://go.microsoft.com/fwlink/p/?LinkId=401266)。

 

 
