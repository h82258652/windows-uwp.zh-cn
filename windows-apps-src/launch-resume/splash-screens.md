---
author: PatrickFarley
title: 初始屏幕
description: 此部分介绍如何设置和配置应用的初始屏幕。
ms.assetid: 6b954bb3-e5b0-46d1-8afc-fb805536cf6d
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 45609a0feb244f746fb8dfbf3dee0dacbe541fc6
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5559257"
---
# <a name="splash-screens"></a>初始屏幕

所有 UWP 应用都必须包含初始屏幕，即图像与背景色的结合，两者均可自定义。

当用户启动应用时，将立即显示你的初始屏幕。 这可以在初始化应用资源的同时向用户提供即时反馈。 应用准备好交互后，会立刻取消初始屏幕。

设计良好的初始屏幕可以使你的应用更加吸引人。 下面是一个简单易懂的初始屏幕：

![从初始屏幕示例中捕获初始屏幕 75% 比例的屏幕。](images/regularsplashscreen.png)

此初始屏幕通过组合绿色背景色与透明背景 PNG 图像进行创建。

具有背景色的简单图像看起来不错，不管应用运行在哪个设备。 只有背景的大小会更改以补偿各种屏幕大小。 你的图像会始终保持完整。

此外，你可以使用 [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) 类来自定义你的应用的启动体验。 你可以对你创建的延长初始屏幕进行适当定位，以使你的应用具有更多时间来完成诸如准备应用 UI 或完成网络操作等附加任务。 你还可以使用 **SplashScreen** 类以在初始屏幕消失时通知你，以便你可以开始进入动画。

| 主题 | 说明 |
|-------|-------------|
| [添加初始屏幕](add-a-splash-screen.md) | 设置你的应用的初始屏幕图像和背景色。 |
| [延长显示初始屏幕的时间](create-a-customized-splash-screen.md) | 通过为你的应用创建延长的初始屏幕，延长显示初始屏幕的时间。 此延长的屏幕将模仿你的应用启动时显示的初始屏幕，并且可以进行自定义。 |