---
author: drewbatgit
ms.assetid: 708170E1-777A-4E4A-9F77-5AB28B88B107
description: "本文介绍如何使用手动设备控件实现增强的视频捕获方案，包括 HDR 视频和曝光优先级。"
title: "用于视频捕获的手动相机控件"
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: cd2adcffa233b76563e47f93f298cf954154adef
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manual-camera-controls-for-video-capture"></a>用于视频捕获的手动相机控件

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


本文介绍如何使用手动设备控件实现增强的视频捕获方案，包括 HDR 视频和曝光优先级。

本文中讨论的视频设备控件全部使用相同模式添加到你的应用中。 首先，检查运行你的应用的当前设备是否支持该控件。 如果控件受支持，则为控件设置所需模式。 通常，如果特定控件在当前设备上不受支持，你应禁用或隐藏允许用户启用该功能的 UI 元素。

本文中讨论的所有设备控件 API 都是 [**Windows.Media.Devices**](https://msdn.microsoft.com/library/windows/apps/br206902) 命名空间的成员。

[!code-cs[VideoControllersUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoControllersUsing)]

> [!NOTE] 
> 本文以[使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)中讨论的概念和代码为基础，该文章介绍了实现基本照片和视频捕获的步骤。 我们建议你先熟悉该文中的基本媒体捕获模式，然后再转到更高级的捕获方案。 本文中的代码假设你的应用已有一个正确完成初始化的 MediaCapture 的实例。

## <a name="hdr-video"></a>HDR 视频

高动态范围 (HDR) 视频功能将 HDR 处理应用到捕获设备的视频流。 通过选择 [**HdrVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926682) 属性来确定 HDR 视频是否受支持。

HDR 视频控件支持以下三种模式：开、关和自动。这意味着设备以动态方式确定 HDR 视频处理是否会改进媒体捕获；如果会改进，则启用 HDR 视频。 若要确定特定模式在当前设备上是否受支持，请检查以查看 [**HdrVideoControl.SupportedModes**](https://msdn.microsoft.com/library/windows/apps/dn926683) 集合是否包含所需模式。

通过将 [**HdrVideoControl.Mode**](https://msdn.microsoft.com/library/windows/apps/dn926681) 设置为所需模式，启用或禁用 HDR 视频处理。

[!code-cs[SetHdrVideoMode](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetHdrVideoMode)]

## <a name="exposure-priority"></a>曝光优先级

启用 [**ExposurePriorityVideoControl**](https://msdn.microsoft.com/library/windows/apps/dn926644) 时，将评估来自捕获设备的视频帧以确定视频是否正在捕获光线较暗的场景。 如果是，该控件将降低已捕获视频的帧速率，以便增加每个帧的曝光时间并改进已捕获视频的视觉质量。

通过检查 [**ExposurePriorityVideoControl.Supported**](https://msdn.microsoft.com/library/windows/apps/dn926647) 属性，确定曝光优先级控件在当前设备上是否受支持。

通过将 [**ExposurePriorityVideoControl.Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926646) 设置为所需模式，启用或禁用曝光优先级控件。

[!code-cs[EnableExposurePriority](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEnableExposurePriority)]

## <a name="related-topics"></a>相关主题

* [相机](camera.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




