---
author: jwmsft
ms.assetid: DE5B084C-DAC1-430B-A15B-5B3D5FB698F7
title: 优化动画、媒体和图像
description: 创建具备流畅动画、高帧速率和高性能媒体捕获与播放的通用 Windows 平台 (UWP) 应用。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 66704daaca67dae2ba4f5a3882f5885ff333d2ce
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/03/2018
ms.locfileid: "5990935"
---
# <a name="optimize-animations-media-and-images"></a>优化动画、媒体和图像


创建具备流畅动画、高帧速率和高性能媒体捕获与播放的通用 Windows 平台 (UWP) 应用。

## <a name="make-animations-smooth"></a>使动画流畅

UWP 应用的一个重要方面就是流畅的交互。 这包括“粘住你的手指”的触摸操纵、流畅的过渡和动画，以及提供输入反馈的小动画。 在 XAML 框架中，存在一个称为合成线程的线程，该线程专注于应用的可视元素的合成和动画。 因为合成线程独立于 UI 线程（运行框架和开发人员代码的线程），所以应用可以实现一致的帧速率和流畅的动画，而不管布局过程或扩展的计算多复杂。 本章节显示如何使用合成线程来使应用动画库保持流畅。 有关动画的详细信息，请参阅[动画概述](https://msdn.microsoft.com/library/windows/apps/Mt187350)。 若要了解如何在执行密集计算的同时增加应用的响应性，请参阅[保持 UI 线程有响应](keep-the-ui-thread-responsive.md)。

### <a name="use-independent-instead-of-dependent-animations"></a>使用独立动画而非从属动画

在创建独立动画时，可以从开始到结束计算独立动画，因为更改要进行动画处理的属性不影响场景中的其余对象。 因此，独立动画可以在合成线程上而不是在 UI 线程上运行。 这保证了它们可保持流畅，因为合成线程是以一致的节奏进行更新的。

所有这些类型的动画都保证是独立的：

-   使用关键帧的对象动画
-   零持续时间动画
-   [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/Hh759771) 和 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/Hh759772) 属性的动画
-   [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 属性的动画
-   针对 [**SolidColorBrush.Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 子属性时类型 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 的属性的动画
-   针对这些返回值类型的子属性时下列 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) 属性的动画：

    -   [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.rendertransform)
    -   [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d)
    -   [**投影**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection)
    -   [**剪辑**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.clip)

从属动画影响布局，因此没有来自 UI 线程的额外输入就无法进行计算。 从属动画包括对 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 和 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 等属性的修改。 默认情况下，从属动画不会运行，需要应用开发人员选择性加入。 启用后，如果 UI 线程保持解除阻止，那么从属动画会流畅地运行，但是如果框架或应用正在 UI 线程上执行许多其他工作，从属动画将开始抖动。

XAML 框架中的几乎所有动画默认都是独立的，但你可以采取某些操作来禁用此优化。 特别注意以下方案：

-   设置 [**EnableDependentAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210356) 属性以允许从属动画在 UI 线程上运行。 将这些动画转换为独立版本。 例如，动态显示对象的 [**ScaleTransform.ScaleX**](https://msdn.microsoft.com/library/windows/apps/BR242946) 和 [**ScaleTransform.ScaleY**](https://msdn.microsoft.com/library/windows/apps/BR242948) 而不是 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 和 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height)。 不要害怕去缩放图像和文本等对象。 仅当动态显示 [**ScaleTransform**](https://msdn.microsoft.com/library/windows/apps/BR242940) 时，框架才应用双线性缩放。 图像/文本将以最终大小重新进行光栅化，以确保它始终清晰。
-   进行逐帧更新，这些是有效的从属动画。 此操作的示例是在 [**CompositonTarget.Rendering**](https://msdn.microsoft.com/library/windows/apps/BR228127) 事件的处理程序中应用转换。
-   运行在 [**CacheMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.cachemode) 属性设置为 **BitmapCache** 的元素中视为独立的任何动画。 此类动画可视为从属动画，因为必须为每个帧对缓存重新进行光栅化。

### <a name="dont-animate-a-webview-or-mediaplayerelement"></a>不要对 WebView 或 MediaPlayerElement 进行动画处理

[**WebView**](https://msdn.microsoft.com/library/windows/apps/BR227702) 控件内的 Web 内容不是由 XAML 框架直接进行呈现的，并且它需要进行额外工作以与场景的剩余部分合成。 动态显示屏幕周围的控件时，此额外的工作会增加，并可能会引入同步问题（例如，HTML 内容可能不会与页面上的 XAML 内容的剩余部分同步移动）。 当需要对 **WebView** 控件进行动画处理时，请在动画的持续时间内使用 [**WebViewBrush**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.webviewbrush.aspx) 交换该控件。

对 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 进行动画处理似乎不是一个好方法。 除了有损性能，它还可能导致要播放的视频内容中出现断裂或其他痕迹。

> **注意** **MediaPlayerElement**本文章中的建议也适用于[**MediaElement**](https://msdn.microsoft.com/library/windows/apps/br242926)。 **MediaPlayerElement** 仅在 Windows 10 版本 1607 中可用，因此如果你要创建适用于以前版本的 Windows 的应用，则需要使用 **MediaElement**。

### <a name="use-infinite-animations-sparingly"></a>尽量少使用无限动画

大多数动画会在某段指定的时间内执行，但将 [**Timeline.Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 属性设置为“Forever”将允许动画无限期运行。 我们建议最大限度地减少使用无限动画，因为它们连续消耗 CPU 资源并妨碍 CPU 进入低功耗状态或空闲状态，从而使 CPU 更快地用尽电量。

为 [**CompositionTarget.Rendering**](https://msdn.microsoft.com/library/windows/apps/BR228127) 添加处理程序与运行某个无限动画相似。 通常，仅当有工作要做时 UI 线程才处于活动状态，但为此事件添加处理程序会强制它运行每一帧。 当不存在任何要处理的工作时，请移除处理程序，当再次需要处理程序时，重新注册处理程序即可。

### <a name="use-the-animation-library"></a>使用动画库

[**Windows.UI.Xaml.Media.Animation**](https://msdn.microsoft.com/library/windows/apps/BR243232) 命名空间包含一个高性能的、流畅的动画库，这些动画与其他 Windows 动画的外观一致。 相关类的名称中含有“主题”，并在[动画概述](https://msdn.microsoft.com/library/windows/apps/Mt187350)中进行了介绍。 此动画库支持许多常见的动画方案，例如创建应用的第一个视图的动画以及创建状态转换和内容转换。 我们建议尽可能使用此动画库以改进性能并提高 UWP UI 的一致性。

> **注意**的动画库无法创建所有可能属性的动画。 有关动画库不适用的 XAML 方案，请参阅[情节提要动画](https://msdn.microsoft.com/library/windows/apps/Mt187354)。


### <a name="animate-compositetransform3d-properties-independently"></a>独立播放 CompositeTransform3D 属性的动画

你可以独立创建每个 [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/Dn914714) 的属性的动画，以便仅应用你需要的动画。 有关示例和详细信息，请参阅 [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d)。 有关创建转换动画的详细信息，请参阅[情节提要动画](https://msdn.microsoft.com/library/windows/apps/Mt187354)和[关键帧和缓动函数动画](https://msdn.microsoft.com/library/windows/apps/Mt187352)。

## <a name="optimize-media-resources"></a>优化媒体资源

音频、视频和图像是多数应用使用的引人注目的内容形式。 随着媒体捕获速率的增大以及内容从标准清晰度变为高清晰度，存储、解码和播放此内容的资源需求数量将增加。 XAML 框架构建于添加到 UWP 媒体引擎的最新功能之上，因此应用将免费获得这些改进。 下面我们介绍一些其他技巧，使用这些技巧可以充分利用你的 UWP 应用中的媒体。

### <a name="release-media-streams"></a>发布媒体流

媒体文件是应用常用的最常见和最耗费的部分资源。 因为媒体文件资源可大幅增加应用的内存占用的大小，所以你必须记住在应用结束使用媒体文件时立即释放对媒体的处理。

例如，如果你的应用正在处理 [**RandomAccessStream**](/uwp/api/Windows.Storage.Streams.RandomAccessStream) 或 [**IInputStream**](https://msdn.microsoft.com/library/windows/apps/BR241718) 对象，请确保在应用结束使用对象时，对该对象调用结束方法来释放基本对象。

### <a name="display-full-screen-video-playback-when-possible"></a>尽可能显示全屏视频播放

在 UWP 应用中，始终使用 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 上的 [**IsFullWindow**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.isfullwindow.aspx) 属性来启用和禁用全屏呈现。 此操作可以确保在媒体播放期间使用系统级别优化。

当视频内容是唯一要呈现的内容时，XAML 框架可以优化视频内容的显示，从而提供使用更少能耗、产生更高帧速率的体验。 若要获得最有效的媒体播放，请将 **MediaPlayerElement** 的大小设置为屏幕的宽度和高度，并且不显示其他 XAML 元素

在 **MediaPlayerElement**（它占据屏幕的整个宽度和高度）上重叠 XAML 元素可能有正当理由，例如隐藏式字幕或瞬息传输控件。 确保在不需要这些元素来使媒体播放返回到其最有效的状态时隐藏它们（设置 `Visibility="Collapsed"`）。

### <a name="display-deactivation-and-conserving-power"></a>显示停用和节省电源

若要阻止在检测不到用户操作时停用屏幕（例如应用播放视频时），则可以调用 [**DisplayRequest.RequestActive**](https://msdn.microsoft.com/library/windows/apps/BR241818)。

若要节省电源并延长电池寿命，应调用 [**DisplayRequest.RequestRelease**](https://msdn.microsoft.com/library/windows/apps/BR241819) 以在不再需要显示请求时立即将其释放。

在以下情况中应该释放显示请求：

-   例如，由于用户操作、缓冲或有限带宽引起的调整需要暂停视频播放。
-   播放停止。 例如，视频播放完毕或完成演示文稿。
-   出现播放错误。 例如，存在网络连接问题或损坏的文件。

### <a name="put-other-elements-to-the-side-of-embedded-video"></a>将其他元素放置到嵌入式视频一侧

通常应用会提供一个嵌入视图，视频会在该视图中的某个页面内播放。 现在你明显丢失了全屏优化，因为 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 不是页面的大小，并且存在绘制的其他 XAML 对象。 谨防无意间通过在 **MediaPlayerElement** 周围绘制边框进入此模式。

当视频处于嵌入模式时，不要在其顶部绘制 XAML 元素。 如果绘制了，则会强制框架执行少量额外的工作来组合场景。 将传输控件放置到某个嵌入式媒体元素下面而不是放置在视频的上面，这是这种情况的优化的一个很好示例。 在此图像中，红色条指示一组传输控件（播放、暂停、停止等）。

![包含重叠元素的 MediaPlayerElement](images/videowithoverlay.png)

不要将这些控件置于非全屏显示的媒体上面。 而要将传输控件放置在呈现媒体的区域之外的某些地方。 在下一张图像中，控件放置在媒体的下面。

![包含相邻元素的 MediaPlayerElement](images/videowithneighbors.png)

### <a name="delay-setting-the-source-for-a-mediaplayerelement"></a>延迟设置 MediaPlayerElement 的源

媒体引擎是很耗费资源的对象，并且 XAML 框架会尽可能延迟加载 dll 和创建大型对象。 通过 [**Source**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.source.aspx) 属性设置 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 的源之后，会强制它执行此工作。 在用户真正准备好播放媒体时进行此设置将尽可能延迟大多数与 **MediaPlayerElement** 相关联的成本。

### <a name="set-mediaplayerelementpostersource"></a>设置 MediaPlayerElement.PosterSource

设置 [**MediaPlayerElement.PosterSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.postersource.aspx) 使 XAML 能够释放以其他方式使用的某些 GPU 资源。 此 API 允许应用使用尽可能少的内存。

### <a name="improve-media-scrubbing"></a>改进媒体清理

若要使媒体平台做出真正的响应，清理一直是一项艰巨的任务。 通常，人们通过更改滑块的值来执行此操作。 以下是关于如何使此操作尽可能有效的几个提示：

-   基于查询 [**MediaPlayerElement.MediaPlayer**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.mediaplayer.aspx) 上的 [**Position**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.position.aspx) 的计时器更新 [**Slider**](https://msdn.microsoft.com/library/windows/apps/BR209614) 的值。 确保为你的计时器使用合理的更新频率。 **Position** 属性在播放期间仅每 250 毫秒更新一次。
-   滑块上的步进频率的大小必须随视频的长度进行缩放。
-   当用户拖动滑块的缩略图时，请订阅到滑块上的 [**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx)、[**PointerMoved**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointermoved.aspx)、[**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerreleased.aspx) 事件以将 [**PlaybackRate**](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplaybacksession.playbackrate.aspx) 属性设置为 0。
-   在 [**PointerReleased**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerreleased.aspx) 事件处理程序中，手动将媒体位置设置为滑块位置值以在清理时实现最佳的缩略图对齐。

### <a name="match-video-resolution-to-device-resolution"></a>将视频分辨率与设备分辨率匹配

解码视频占用许多内存和 GPU 周期，因此请选择与显示视频所用的分辨率接近的视频格式。 如果它将要缩小到一个小得多的大小，则使用资源解码 1080 视频毫无意义。 许多应用不会将相同的视频解码为不同的分辨率，但如果可用，请使用与显示视频所用的分辨率接近的解码。

### <a name="choose-recommended-formats"></a>选择建议的格式

媒体格式选择是一个敏感的主题，且通常受到企业决策的驱动。 从 UWP 的性能方面考虑，我们建议使用 H.264 视频作为主要的视频格式，而将 AAC 和 MP3 作为首选的音频格式。 对于本地文件播放，MP4 是视频内容首选的文件容器。 H.264 解码是通过最新的图形硬件加速的。 另外，尽管可广泛获得 VC-1 解码的硬件加速，但对于市场上的大量图形硬件而言，加速在很多情况下仅局限于部分加速级别（IDCT 级别），而不是全速级别的硬件卸载（即 VLD 模式）。

如果你对视频内容生成过程具有完全的控制，那么你必须弄清楚如何在压缩效率和 GOP 结构之间保持良好的平衡。 使用 B 图片的相对更小的 GOP 大小可改进搜寻或诀窍模式的性能。

在包含短暂的低滞后音频效果时（如在游戏中），请使用带未压缩的 PCM 数据的 WAV 文件来减少压缩音频格式常见的处理开销。


## <a name="optimize-image-resources"></a>优化图像资源

### <a name="scale-images-to-the-appropriate-size"></a>将图像缩放到适当的大小

图像以非常高的分辨率捕获，这可能导致应用在解码图像数据时使用更多 CPU，并在从磁盘加载它后使用更多内存。 但是在内存中解码和保存高分辨率图像没有意义，只是将其显示为比其本机大小更小。 相反，使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 属性以将在屏幕上绘制它的相同大小创建该图像版本。

不要执行以下操作：

```xaml
<Image Source="ms-appx:///Assets/highresCar.jpg"
       Width="300" Height="200"/>    <!-- BAD CODE DO NOT USE.-->
```

而要这样做：

```xaml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg"
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

默认情况下，[**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 的单位是物理像素。 [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) 属性可用于更改此行为：将 **DecodePixelType** 设置为 **Logical** 会导致解码大小自动考虑到系统的当前比例系数，类似于其他 XAML 内容。 因此，举例来说，如果你希望 **DecodePixelWidth** 和 **DecodePixelHeight** 与显示图像所要使用的图像控件的 Height 和 Width 属性匹配，则通常适合将 **DecodePixelType** 设置为 **Logical**。 通过使用物理像素的默认行为，你必须自行考虑到系统的当前比例系数；并且你应该侦听缩放更改通知，以防用户更改其显示首选项。

如果将 DecodePixelWidth/Height 显式设置为大于将在屏幕上显示的图像，则应用将不必使用额外内存（最高为每像素 4 个字节），这对于大图像来说将很快成为昂贵的负担。 还将使用双线性缩放缩小该图像，这可能导致它在较大的比例系数中显得模糊。

如果 DecodePixelWidth/DecodePixelHeight 显式设置为小于将在屏幕上显示的图像，则它将缩小，并且可能显得像素化。

在某些无法事先确定合适解码大小的情况下，你应遵循 XAML 的自动正确大小解码，如果未指定显式的 DecodePixelWidth/DecodePixelHeight，则这将尽量尝试以合适大小解码该图像。

如果你事先知道图像内容的大小，则应设置显式的解码大小。 如果所提供的解码大小相对于其他 XAML 元素大小，你还应一起将 [**DecodePixelType**](https://msdn.microsoft.com/library/windows/apps/Dn298545) 设置为 **Logical**。 例如，如果你使用 Image.Width 和 Image.Heigh 显式设置内容大小，则可以将 DecodePixelType 设置为 DecodePixelType.Logical 以使用与图像控件相同的逻辑像素维度，然后显式使用 BitmapImage.DecodePixelWidth 和/或 BitmapImage.DecodePixelHeight 控制图像的大小以实现潜在较大的内存节省。

请注意，在确定解码内容的大小时，应考虑 Image.Stretch。

### <a name="right-sized-decoding"></a>正确大小的解码

如果你未设置显式的解码大小，则 XAML 将通过根据包含页面的初始布局将图像解码为它显示在屏幕上的相同大小，尽量尝试节省内存。 建议你在编写应用程序时尽量充分利用此功能。 如果满足任何以下条件，将禁用此功能。

-   在使用 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522) 或 [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx) 设置内容后，[**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) 连接到活动 XAML 树。
-   使用异步解码（如 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255)）解码图像。
-   通过在主机图像元素或画笔或者任何父元素上将 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 设置为 0 或将 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility) 设置为 **Collapsed** 来隐藏该图像。
-   图像控件或画笔使用 **None** 的 [**Stretch**](https://msdn.microsoft.com/library/windows/apps/BR242968)。
-   图像将用作 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756)。
-   `CacheMode="BitmapCache"` 在图像元素或任何父元素上设置。
-   图像画笔是非矩形（例如当应用到某个形状或文本时）。

在上述方案中，设置显式解码大小是实现内存节省的唯一方法。

在设置源之前，你应始终将 [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/BR243235) 附加到活动树。 每当在标记中指定图像元素或画笔时，将自动成为这种情况。 标题“活动树示例”下提供了相关示例。 在设置流来源时，你应该始终避免使用 [**SetSource**](https://msdn.microsoft.com/library/windows/apps/BR243255)，应改用 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522)。 而且最好在等待引发 [**ImageOpened**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.imageopened.aspx) 事件时避免隐藏图像内容（通过零透明度或折叠可见性）。 主观判断是否执行此操作：如果执行，你将不会从自动正确大小解码中获益。 如果你的应用最初必须隐藏图像内容，则它还应显式设置解码大小（如果可能）。

**活动树示例**

示例 1（良好）- 标记中指定的统一资源标识符 (URI)。

```xaml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

示例 2 标记 - 代码隐藏中指定的 URI。

```xaml
<Image x:Name="myImage"/>
```

示例 2 代码隐藏（良好）- 在设置 BitmapImage 的 UriSource 前将其连接到树。

```csharp
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

示例 2 代码隐藏 （不良）-将其连接到树前设置 BitmapImage 的 UriSource。

```csharp
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

### <a name="caching-optimizations"></a>缓存优化

缓存优化对使用 [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx) 从应用包或 Web 加载内容的图像有效。 URI 用于唯一标识基础内容，并且在内部，XAML 框架不会多次下载或解码该内容。 相反，它将使用缓存的软件或硬件资源多次显示该内容。

此优化的例外是以不同的分辨率多次显示该图像（可显式或通过自动正确大小解码指定）的情况。 每个缓存项目还存储图像的分辨率，并且如果 XAML 找不到源 URI 与所需的分辨率匹配的图像，则它将以该大小解码一个新版本。 但是，它不会再次下载编码的图像数据。

因此，你应该在从应用包加载图像时积极使用 [**UriSource**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imaging.bitmapimage.urisource.aspx)，而在不需要时避免使用文件流和 [**SetSourceAsync**](https://msdn.microsoft.com/library/windows/apps/JJ191522)。

### <a name="images-in-virtualized-panels-listview-for-instance"></a>虚拟化面板中的图像（例如 ListView）

如果从树中删除一个图像（因为应用显式删除了它，或因为它是一个现代虚拟化面板，并在滚动离开视图时隐式删除），则 XAML 将通过为该图像释放硬件资源来优化内存使用率，因为已不再需要它们。 内存不会立即释放，而是在框架更新期间释放，框架更新在图像元素不再存在于树中后立即发生。

因此，你应尽量使用现代虚拟化面板来托管图像内容的列表。

### <a name="software-rasterized-images"></a>软件栅格化图像

当图像用于非矩形画笔或 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756) 时，该图像将使用软件栅格化路径，这将完全不会缩放图像。 此外，它必须在软件和硬件内存中存储该图像的副本。 例如，如果一个图像用作椭圆形的画笔，则将在内部存储两次潜在较大的完整图像。 然后，在使用 **NineGrid** 或非矩形画笔时，你的应用应将其图像预先缩放为呈现时所要使用的相似大小。

### <a name="background-thread-image-loading"></a>后台线程图像加载

XAML 具有内部优化，使其可以将图像的内容异步解码到硬件中的图面，而无需软件内存中的中间图面。 这减少了峰值内存使用率和呈现延迟。 如果满足任何以下条件，将禁用此功能。

-   图像将用作 [**NineGrid**](https://msdn.microsoft.com/library/windows/apps/BR242756)。
-   `CacheMode="BitmapCache"` 在图像元素或任何父元素上设置。
-   图像画笔是非矩形（例如当应用到某个形状或文本时）。

### <a name="softwarebitmapsource"></a>SoftwareBitmapSource

[**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Dn997854) 类在不同的 WinRT 命名空间（如 [**BitmapDecoder**](https://msdn.microsoft.com/library/windows/apps/BR226176)、camera API 和 XAML）之间交换可互操作的未经压缩的图像。 此类可省去 [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/BR243259) 通常需要以及有助于减少峰值内存和源到屏幕延迟的额外副本。

还可配置提供源信息的 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Dn887358) 以使用自定义 [**IWICBitmap**](https://msdn.microsoft.com/library/windows/desktop/Ee719675) 提供可重新加载的备份存储，可允许应用在其认为合适时重新映射内存。 这是一个高级 C++ 用例。

你的应用应使用 [**SoftwareBitmap**](https://msdn.microsoft.com/library/windows/apps/Dn887358) 和 [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/Dn997854) 以与生成和使用图像的其他 WinRT API 互操作。 并且你的应用应在加载未经压缩的图像数据时使用 **SoftwareBitmapSource**，而不是使用 [**WriteableBitmap**](https://msdn.microsoft.com/library/windows/apps/BR243259)。

### <a name="use-getthumbnailasync-for-thumbnails"></a>为缩略图使用 GetThumbnailAsync

缩放图像的一个用例是创建缩略图。 尽管你可以使用 [**DecodePixelWidth**](https://msdn.microsoft.com/library/windows/apps/BR243243) 和 [**DecodePixelHeight**](https://msdn.microsoft.com/library/windows/apps/BR243241) 来提供较小版本的图像，但是 UWP 会提供更加高效的 API 来检索缩略图。 [**GetThumbnailAsync**](https://msdn.microsoft.com/library/windows/apps/BR227210) 为已对文件系统进行缓存的图像提供缩略图。 这将提供甚至比 XAML API 更好的性能，因为不需要打开或解码图像。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> FileOpenPicker picker = new FileOpenPicker();
> picker.FileTypeFilter.Add(".bmp");
> picker.FileTypeFilter.Add(".jpg");
> picker.FileTypeFilter.Add(".jpeg");
> picker.FileTypeFilter.Add(".png");
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary;
>
> StorageFile file = await picker.PickSingleFileAsync();
>
> StorageItemThumbnail fileThumbnail = await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64);
>
> BitmapImage bmp = new BitmapImage();
> bmp.SetSource(fileThumbnail);
>
> Image img = new Image();
> img.Source = bmp;
> ```
> ```vb
> Dim picker As New FileOpenPicker()
> picker.FileTypeFilter.Add(".bmp")
> picker.FileTypeFilter.Add(".jpg")
> picker.FileTypeFilter.Add(".jpeg")
> picker.FileTypeFilter.Add(".png")
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary
>
> Dim file As StorageFile = Await picker.PickSingleFileAsync()
>
> Dim fileThumbnail As StorageItemThumbnail = Await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64)
>
> Dim bmp As New BitmapImage()
> bmp.SetSource(fileThumbnail)
>
> Dim img As New Image()
> img.Source = bmp
> ```

### <a name="decode-images-once"></a>解码图像一次

要避免解码图像超过一次，请从 Uri 而不是使用内存流来分配 [**Image.Source**](https://msdn.microsoft.com/library/windows/apps/BR242760) 属性。 XAML 框架可以将多个位置中的同一 Uri 与一个已解码图像相关联，但是它无法对包含相同数据的多个内存流执行相同的操作，它可以为每个内存流创建一个不同的已解码图像。
