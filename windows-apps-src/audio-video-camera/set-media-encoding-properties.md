---
ms.assetid: 09BA9250-A476-4803-910E-52F0A51704B1
description: 本文介绍如何使用 IMediaEncodingProperties 界面设置相机预览流以及已捕获照片和视频的分辨率和帧速率。
title: 为 MediaCapture 设置格式、分辨率和帧速率
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f81ab1ef635bf4cfb20c289d6998c242f7aa47fc
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8929186"
---
# <a name="set-format-resolution-and-frame-rate-for-mediacapture"></a>为 MediaCapture 设置格式、分辨率和帧速率



本文向你演示如何使用 [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) 界面设置相机预览流以及已捕获照片和视频的分辨率和帧速率。 还将演示如何确保预览流的纵横比与已捕获媒体的纵横比相匹配。

相机配置文件提供发现和设置相机流属性的更高级方法，但并非所有设备均支持这些文件。 有关详细信息，请参阅[相机配置文件](camera-profiles.md)。

本文中的代码源自 [CameraResolution 示例](http://go.microsoft.com/fwlink/p/?LinkId=624252&clcid=0x409)。 你可以下载该示例以查看上下文中使用的代码，或将该示例用作你自己的应用的起始点。

> [!NOTE] 
> 本文以[使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)中讨论的概念和代码为基础，该文章介绍了实现基本照片和视频捕获的步骤。 建议你先熟悉该文中的基本媒体捕获模式，然后再转到更高级的捕获方案。 本文中的代码假设你的应用已有一个正确完成初始化的 MediaCapture 的实例。

## <a name="a-media-encoding-properties-helper-class"></a>媒体编码属性帮助程序类

创建包装 [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) 界面功能的简单帮助程序类使选择一组符合特定标准的编码属性变得更简单。 此帮助程序类因具有以下编码属性功能的行为而极为有用：

**警告** [**VideoDeviceController.GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994)方法采用[**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640)枚举，如**VideoRecord**或**照片**、 的成员，并返回的任一[**列表ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993)或[**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217)对象可传达流编码设置，如捕获的照片或视频的分辨率。 调用 **GetAvailableMediaStreamProperties** 的结果可能包括 **ImageEncodingProperties** 或 **VideoEncodingProperties**，不论指定的 **MediaStreamType** 值是什么。 出于此原因，在尝试访问任何属性值之前，你都应始终查看每个返回的值的类型并将其转换到相应类型。

以下定义的帮助程序类为 [**ImageEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh700993) 或 [**VideoEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701217) 处理类型检查和转换，因此你的应用代码无需分辨这两种类型。 除此之外，此帮助程序类还会公开属性的纵横比属性、帧速率（仅适用于视频编码属性）以及使在应用 UI 中显示编码属性变得更简单的友好名称。

必须将 [**Windows.Media.MediaProperties**](https://msdn.microsoft.com/library/windows/apps/hh701296) 命名空间包含在帮助程序类的源文件中。

[!code-cs[MediaEncodingPropertiesUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMediaEncodingPropertiesUsing)]

[!code-cs[StreamPropertiesHelper](./code/BasicMediaCaptureWin10/cs/StreamPropertiesHelper.cs#SnippetStreamPropertiesHelper)]

## <a name="determine-if-the-preview-and-capture-streams-are-independent"></a>确定预览流和捕获流是否独立。

在某些设备上，相同的硬件引脚可同时用于预览流和捕获流。 在这些设备上，设置某个流的编码属性也将设置其他流的编码属性。 在将不同的硬件引脚用于捕获和预览的设备上，可单独为每个流设置属性。 使用以下代码确定预览流和捕获流是否独立。 你应该根据此测试结果，将 UI 调整为独立启用或禁用流设置。

[!code-cs[CheckIfStreamsAreIdentical](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCheckIfStreamsAreIdentical)]

## <a name="get-a-list-of-available-stream-properties"></a>获取可用流属性列表

通过获取应用 [MediaCapture](https://msdn.microsoft.com/library/windows/apps/br226825) 对象的 [**VideoDeviceController**](capture-photos-and-video-with-mediacapture.md)，然后调用 [**GetAvailableMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211994) 并在 [**MediaStreamType**](https://msdn.microsoft.com/library/windows/apps/br226640) 值的 **VideoPreview**、**VideoRecord** 或 **Photo** 的其中一项属性中传递，获取捕获设备的可用流属性列表。 在此示例中，Linq 语法用于为从 **GetAvailableMediaStreamProperties** 返回的每个 [**IMediaEncodingProperties**](https://msdn.microsoft.com/library/windows/apps/hh701011) 值创建 **StreamPropertiesHelper** 对象（在本文前文中定义）列表。 该示例首先使用 Linq 扩展方法先基于分辨率随后基于帧速率为返回的属性排序。

如果你的应用具有特定分辨率或帧速率要求，你可以按编程方式选择一组媒体编码属性。 典型的相机应用将公开 UI 中的可用属性列表，并允许用户选择所需设置。 在列表中为 **StreamPropertiesHelper** 对象列表中的每个项创建 **ComboBoxItem**。 内容设置为帮助程序类返回的友好名称，而标记设置为帮助程序类本身，以便可以在稍后用于检索相关联的编码属性。 然后将每个 **ComboBoxItem** 添加到传递到该方法中的 **ComboBox**。

[!code-cs[PopulateStreamPropertiesUI](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPopulateStreamPropertiesUI)]

## <a name="set-the-desired-stream-properties"></a>设置所需的流属性

通过调用 [**SetMediaStreamPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh700895)、传递指示是应设置照片属性、视频属性还是预览属性的 **MediaStreamType**，告知视频设备控制器使用所需的编码属性。 本示例在用户选择使用 **PopulateStreamPropertiesUI** 帮助程序方法填充的 **ComboBox** 对象之一中的项目时，设置所请求的编码属性。

[!code-cs[PreviewSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPreviewSettingsChanged)]

[!code-cs[PhotoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhotoSettingsChanged)]

[!code-cs[VideoSettingsChanged](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetVideoSettingsChanged)]

## <a name="match-the-aspect-ratio-of-the-preview-and-capture-streams"></a>匹配预览流和捕获流的纵横比

典型相机应用会为用户提供用于选择视频或照片捕获分辨率的 UI，但会以编程方式设置预览分辨率。 有以下几种不同的策略可用于选择最适合你的应用的预览流分辨率：

-   选择最高可用的预览分辨率，从而让 UI 框架对预览执行任何必要的缩放。

-   选择最接近捕获分辨率的预览分辨率，以便预览向最终捕获的媒体显示最接近的表示形式。

-   选择最接近 [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) 的大小的预览分辨率，以便仅必要的像素通过预览流管道。

**重要提示**有可能某些设备上，设置不同的纵横比相机的预览流和捕获流。 匹配失误引起的帧裁剪可能导致内容显示在预览中不可见的捕获媒体中，这会导致消极的用户体验。 强烈建议你为预览流和捕获流在容差较小的窗口中使用相同的纵横比。 只要纵横比密切匹配，为捕获和预览启用完全不同的分辨率便不会出现什么问题。


为确保照片或视频捕获流与预览流的纵横比相匹配，此示例调用 [**VideoDeviceController.GetMediaStreamProperties**](https://msdn.microsoft.com/library/windows/apps/br211995) 并传递 **VideoPreview** 枚举值以请求预览流的当前流属性。 接下来定义纵横比容差较小的窗口，以便我们可以包括可能不同于预览流（只要相近即可）的纵横比。 接下来，Linq 扩展方法用于选择纵横比处于预览流的定义容差范围内的 **StreamPropertiesHelper** 对象。

[!code-cs[MatchPreviewAspectRatio](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetMatchPreviewAspectRatio)]

 

 




