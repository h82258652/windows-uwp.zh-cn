---
author: drewbatgit
ms.assetid: E0189423-1DF3-4052-AB2E-846EA18254C4
description: "本主题介绍专用于视频捕获方案的效果。 其中包括视频防抖动效果。"
title: "视频捕获的效果"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 3af5ed7146f2420c2a6d3035c26290cbeaff8375

---

# 视频捕获的效果

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

本主题介绍专用于视频捕获方案的效果。 其中包括视频防抖动效果。

**注意**  
本文基于[使用 MediaCapture 捕获照片和视频](capture-photos-and-video-with-mediacapture.md)中讨论的概念和代码生成，详细介绍了实现基本照片和视频捕获的步骤。 建议你先熟悉该文中的基本媒体捕获模式，然后再转到更高级的捕获方案。 本文中的代码假设你的应用已有一个正确完成初始化的 MediaCapture 的实例。

## 视频防抖动效果

视频防抖动效果操作视频流的帧，以最大程度地减少由你的手握持捕获设备造成的抖动。 由于此技术会导致像素向右、向左、向上和向下移位，并且因为该效果无法获知视频帧之外的内容，因此将原始视频稍微裁剪以取得稳定视频。 提供实用工具函数以允许你调整视频编码设置，以便以最佳方式管理由该效果执行的裁剪。

在支持它的设备上，光学图像防抖动 (OIS) 通过机械地操作捕获设备来稳定视频，因此不需要裁剪该视频帧的边缘。 有关详细信息，请参阅[用于视频捕获的捕获设备控件](capture-device-controls-for-video-capture.md)。

### 设置你的应用以使用视频防抖动

除了基本媒体捕获所需的命名空间，使用视频防抖动效果还需要以下命名空间。

[!code-cs[VideoStabilizationEffectUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEffectUsing)]

声明成员变量以存储 [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760) 对象。 作为效果实现的一部分，你将修改用于编码已捕获视频的编码属性。 声明两个变量以存储初始输入和输出编码属性的备份副本，以便以后当禁用该效果时你可以还原它们。 最后，声明类型 [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026) 的成员变量，因为将从你的代码内的多个位置访问此对象。

[!code-cs[DeclareVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetDeclareVideoStabilizationEffect)]

在本文[使用 MediaCapture 捕获照片和视频](capture-photos-and-video-with-mediacapture.md)中所述的基本视频捕获实现中，媒体编码配置文件对象将分配给本地变量，因为不会在代码的其他位置使用它。 对于此方案，你应该将该对象分配给成员变量以便以后进行访问。

[!code-cs[EncodingProfileMember](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetEncodingProfileMember)]

### 初始化视频防抖动效果

在初始化 **MediaCapture** 对象后，创建 [**VideoStabilizationEffectDefinition**](https://msdn.microsoft.com/library/windows/apps/dn926762) 对象的新实例。 调用 [**MediaCapture.AddVideoEffectAsync**](https://msdn.microsoft.com/library/windows/apps/dn878035) 将该效果添加到视频管道，并检索 [**VideoStabilizationEffect**](https://msdn.microsoft.com/library/windows/apps/dn926760) 类的一个实例。 指定 [**MediaStreamType.VideoRecord**](https://msdn.microsoft.com/library/windows/apps/br226640) 以指示该效果应该应用到视频录制流。

注册 [**EnabledChanged**](https://msdn.microsoft.com/library/windows/apps/dn948982) 事件的事件处理程序并调用帮助程序方法 **SetUpVideoStabilizationRecommendationAsync**，两者将在本文稍后部分讨论。 最后，将该效果的 [**Enabled**](https://msdn.microsoft.com/library/windows/apps/dn926775) 属性设置为 True 以启用该效果。

[!code-cs[CreateVideoStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCreateVideoStabilizationEffect)]

### 使用建议的编码属性

如本文中前面所述，视频防抖动效果使用的技术必然导致通过稍微裁剪源视频来获得稳定视频。 在你的代码中定义以下帮助程序函数，以便调整视频编码属性以优化处理该效果的此限制。 使用该视频防抖动效果不需要此步骤，但如果你不执行此步骤，得到的视频将稍有变形，因此视觉保真度略低。

调用视频防抖动效果实例上的 [**GetRecommendedStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn948983)，从而传入 [**VideoDeviceController**](https://msdn.microsoft.com/library/windows/apps/br226825) 对象，这会向该效果通知当前输入流编码属性以及 **MediaEncodingProfile**，后者可让该效果得知当前输出编码属性。 此方法将返回包含新建议的输入和输出流编码属性的 [**VideoStreamConfiguration**](https://msdn.microsoft.com/library/windows/apps/dn926727) 对象。

建议的输入编码属性是（如果它受设备支持）比你提供的初始设置更高的分辨率，因此在应用效果的裁剪后，分辨率的损失最小。

调用 [**VideoDeviceController.SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895) 以设置新的编码属性。 在设置新的属性之前，使用成员变量来存储初始编码属性，以便当你禁用该效果时可以改回该设置。

如果视频防抖动效果必须裁剪输出视频，则建议的输出编码属性将是已裁剪的视频的大小。 这意味着输出分辨率将匹配已裁剪的视频大小。 如果未使用建议的输出属性，将放大视频以匹配初始输出大小，这将导致丢失视觉保真度。

设置 **MediaEncodingProfile** 对象的 [**Video**](https://msdn.microsoft.com/library/windows/apps/hh701124) 属性。 在设置新的属性之前，使用成员变量来存储初始编码属性，以便当你禁用该效果时可以改回该设置。

[!code-cs[SetUpVideoStabilizationRecommendationAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetSetUpVideoStabilizationRecommendationAsync)]

### 处理禁用的视频防抖动效果

如果像素吞吐量对于要处理的该效果太高，或如果检测到该效果运行缓慢，则系统可以自动禁用该视频防抖动效果。 如果发生这种情况，将引发 EnabledChanged 事件。 *sender* 参数中的 **VideoStabilizationEffect** 实例指示该效果的新状态：已启用或已禁用。 [**VideoStabilizationEffectEnabledChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/dn948979) 具有表明已启用或已禁用该效果原因的 [**VideoStabilizationEffectEnabledChangedReason**](https://msdn.microsoft.com/library/windows/apps/dn948981) 值。 请注意，如果你以编程方式启用或禁用该效果，在原因是 **Programmatic** 的情况下，也会引发此事件。

通常使用此事件来调节应用的 UI 以指示视频防抖动的当前状态。

[!code-cs[VideoStabilizationEnabledChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoStabilizationEnabledChanged)]

### 清理视频防抖动效果

若要清理视频防抖动效果，则调用 [**ClearEffectsAsync**](https://msdn.microsoft.com/library/windows/apps/br226592) 以清除视频管道中的所有效果。 如果包含初始编码属性的成员变量都不为 Null，可使用它们来还原编码属性。 最后，删除 **EnabledChanged** 事件处理程序并将该效果设置为 Null。

[!code-cs[CleanUpVisualStabilizationEffect](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCleanUpVisualStabilizationEffect)]

## 相关主题

* [使用 MediaCapture 捕获照片和视频](capture-photos-and-video-with-mediacapture.md)
 

 







<!--HONumber=Jun16_HO4-->


