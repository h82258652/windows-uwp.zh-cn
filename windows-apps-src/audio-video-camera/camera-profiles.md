---
author: drewbatgit
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: 本文讨论了如何使用相机配置文件来发现和管理不同视频捕获设备的功能。 这包括如下任务：选择支持特定分辨率或帧速率的配置文件、选择支持同时访问多台相机的配置文件，以及选择支持 HDR 的配置文件。
title: 通过相机配置文件发现和选择相机功能
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1af7453e8bfea973a67dc878438050accc01fb4c
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/21/2018
ms.locfileid: "7445383"
---
# <a name="discover-and-select-camera-capabilities-with-camera-profiles"></a>通过相机配置文件发现和选择相机功能



本文讨论了如何使用相机配置文件来发现和管理不同视频捕获设备的功能。 这包括如下任务：选择支持特定分辨率或帧速率的配置文件、选择支持同时访问多台相机的配置文件，以及选择支持 HDR 的配置文件。

> [!NOTE] 
> 本文以[使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)中讨论的概念和代码为基础，该文章介绍了实现基本照片和视频捕获的步骤。 建议你先熟悉该文中的基本媒体捕获模式，然后再转到更高级的捕获方案。 本文中的代码假设你的应用已有一个正确完成初始化的 MediaCapture 的实例。

 

## <a name="about-camera-profiles"></a>关于相机配置文件

不同设备上的相机支持不同的功能，包括一组支持的捕获分辨率、视频捕获的帧速率，以及是否支持 HDR 或可变帧速率捕获。 通用 Windows 平台 (UWP) 媒体捕获框架将此功能集存储在 [**MediaCaptureVideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695) 中。 由 [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694) 对象表示的相机配置文件具有三组媒体描述；一组用于照片捕获，一组用于视频捕获，另一组用于视频预览。

在初始化你的 [MediaCapture](capture-photos-and-video-with-mediacapture.md) 对象之前，你可以在当前设备上查询捕获设备，以查看支持哪些配置文件。 当你选择某个受支持的配置文件时，你知道捕获设备支持该配置文件的媒体说明中的所有功能。 这样就无需采用试错方法来确定特定设备支持哪些功能组合。

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

本文中的代码示例将这一最简单的初始化过程替换为支持各种功能的相机配置文件的发现过程，后者随后用于初始化媒体捕获设备。

## <a name="find-a-video-device-that-supports-camera-profiles"></a>查找支持相机配置文件的视频设备

在搜索受支持的相机配置文件之前，你应查找支持使用相机配置文件的捕获设备。 在下面示例中定义的 **GetVideoProfileSupportedDeviceIdAsync** 帮助程序方法使用 [**DeviceInformaion.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432) 方法来检索所有可用视频捕获设备的列表。 它将循环访问列表中的所有设备，同时为每台设备调用静态方法 [**IsVideoProfileSupported**](https://msdn.microsoft.com/library/windows/apps/dn926714)，以查看设备是否支持视频配置文件。 还有，每台设备的 [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906) 属性允许你指定希望相机位于设备正面还是背面。

如果在指定的面板中发现支持相机配置文件的设备，将返回包含设备 ID 字符串的 [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437) 值。

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

如果从 **GetVideoProfileSupportedDeviceIdAsync** 帮助程序方法返回的设备 ID 为 null 或是一个空字符串，则指定面板上不存在支持相机配置文件的设备。 在此情况下，应在不使用配置文件的情况下初始化媒体捕获设备。

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## <a name="select-a-profile-based-on-supported-resolution-and-frame-rate"></a>基于受支持的分辨率和帧速率选择配置文件

若要选择具有特定功能（如具有达到特定分辨率和帧速率的功能）的配置文件，首先应调用上面定义的帮助程序方法，以获取支持使用相机配置文件的捕获设备的 ID。

创建新的 [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573) 对象，传入选定的设备 ID。 接下来，调用静态方法 [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708)，获取设备支持的所有相机配置文件的列表。

此示例使用包含在 using **System.Linq** 命名空间中的 Linq 查询方法，来选择包含 [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) 对象的配置文件，其中的 [**Width**](https://msdn.microsoft.com/library/windows/apps/dn926700)、[**Height**](https://msdn.microsoft.com/library/windows/apps/dn926697) 和 [**FrameRate**](https://msdn.microsoft.com/library/windows/apps/dn926696) 属性与请求的值相匹配。 如果找到匹配项，则 **MediaCaptureInitializationSettings** 的 [**VideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926679) 和 [**RecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926678) 将设置为从 Linq 查询返回的匿名类型中的值。 如果未找到匹配项，则使用默认配置文件。

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

使用你所需的相机配置文件填充 **MediaCaptureInitializationSettings** 后，你只需对你的媒体捕获对象调用 [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) 即可将其配置为所需的配置文件。

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## <a name="use-media-frame-source-groups-to-get-profiles"></a>使用媒体帧源组来获取配置文件

自 Windows 10 版本 1803 起，可在初始化 **MediaCapture** 对象之前，通过特定功能使用 [**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup) 类获取相机配置文件。 借助帧源组，设备制造商可将传感器组或捕获功能作为单个虚拟设备。 这样一来，即可实现计算摄影方案（如同时使用深度和彩色相机），也可以选择简单捕获方案的相机配置文件。 若要详细了解如何使用 **MediaFrameSourceGroup**，请参阅[使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)。

以下示例方法演示了如何使用 **MediaFrameSourceGroup** 对象查找支持已知视频配置文件的相机配置文件，例如支持 HDR 或可变照片序列的相机配置文件。 首先，通过调用 [**MediaFrameSourceGroup.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.FindAllAsync) 获取当前设备上可用的所有媒体帧源组的列表。 循环访问每个源组，并通过调用 [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) 获取当前源组中支持特定配置文件（本例中为带 WCG 照片的 HDR）的所有视频配置文件的列表。 如果找到符合条件的配置文件，则新建一个 **MediaCaptureInitializationSettings** 对象，将 **VideoProfile** 设置为选择配置文件并将 **VideoDeviceId** 设置为当前媒体帧源组的 **Id** 属性。 这样一来，你可以将 **KnownVideoProfile.HdrWithWcgVideo** 值代入此方法来获取支持 HDR 视频的媒体捕获设置。 通过代入 **KnownVideoProfile.VariablePhotoSequence** 获取支持可变照片序列的设置。

 [!code-cs[FindKnownVideoProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindKnownVideoProfile)]

## <a name="use-known-profiles-to-find-a-profile-that-supports-hdr-video-legacy-technique"></a>使用已知的配置文件来查找支持 HDR 视频的配置文件（旧技术）

> [!NOTE] 
> 自 Windows 10 版本 1803 起弃用本节中介绍的 API。 详情请参阅上一节：**使用媒体帧源组获取配置文件**。

在选择支持 HDR 的配置文件时，最初的操作与其他方案类似。 创建**MediaCaptureInitializationSettings**和一个字符串，用于保留捕获设备 id。 添加一个将跟踪是否支持 HDR 视频的布尔变量。

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

使用上面定义的 **GetVideoProfileSupportedDeviceIdAsync** 帮助程序方法，获取支持相机配置文件的捕获设备的设备 ID。

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

静态方法 [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) 将返回受指定设备（按指定的 [**KnownVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn948843) 值进行分类）支持的相机配置文件。 在此方案中，指定 **VideoRecording** 值是为了将返回的相机配置文件限制为支持视频录制的配置文件。

循环访问返回的相机配置文件列表。 对于每个相机配置文件，循环访问配置文件中的每个 [**VideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695)，同时检查 [**IsHdrVideoSupported**](https://msdn.microsoft.com/library/windows/apps/dn926698) 属性是否为 true。 找到合适的媒体描述后，中断循环并将配置文件和描述对象分配给 **MediaCaptureInitializationSettings** 对象。

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## <a name="determine-if-a-device-supports-simultaneous-photo-and-video-capture"></a>确定设备是否支持同时进行照片和视频捕获

许多设备支持同时捕获照片和视频。 若要确定捕获设备是否支持此功能，请调用 [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) 以获取设备支持的所有相机配置文件。 使用链接查询来查找至少具有一个同时用于 [**SupportedPhotoMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926703) 和 [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) 的条目的配置文件，这表示该配置文件支持同时捕获。

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

可优化此查询，查找支持特定分辨率或支持除同时录制视频功能外的其他功能的配置文件。 你还可以使用 [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) 并指定 [**BalancedVideoAndPhoto**](https://msdn.microsoft.com/library/windows/apps/dn948843) 值，以检索支持同时捕获的配置文件，但查询所有配置文件将提供更完整的结果。

## <a name="related-topics"></a>相关主题

* [相机](camera.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




