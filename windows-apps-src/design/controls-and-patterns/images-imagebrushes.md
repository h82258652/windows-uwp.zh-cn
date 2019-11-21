---
Description: 了解如何将图像集成到应用中，包括如何使用以下两个主要 XAML 类的 API：Image 和 ImageBrush。
title: 图像和图像画笔
ms.assetid: CEA8780C-71A3-4168-A6E8-6361CDFB2FAF
label: Images and image brushes
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a07fc5c669d3c9cd374a4a031fba2ee6ef6d5756
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257280"
---
# <a name="images-and-image-brushes"></a>图像和图像画笔

要显示图像，可使用 **Image** 对象或 **ImageBrush** 对象。 Image 对象呈现图像，而 ImageBrush 对象使用图像绘制其他对象。 

> **重要的 API**：[Image 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)，[Source 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source)，[ImageBrush 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)，[ImageSource 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource)

## <a name="are-these-the-right-elements"></a>这些是正确的元素吗？
使用 **Image** 元素在应用中显示独立的图像。

使用 **ImageBrush** 将图像应用到另一个对象。 ImageBrush 的用途包括文本的装饰效果，或者控件或布局容器的背景。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 XAML 控件库应用，请单击此处<a href="xamlcontrolsgallery:/item/Image">打开此应用，了解 Image 的实际应用</a><strong style="font-weight: semi-bold"></strong>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-an-image"></a>创建图像

### <a name="image"></a>图像
本示例显示了如何使用 [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) 对象创建图像。


```XAML
<Image Width="200" Source="sunset.jpg" />
```

以下是呈现的 Image 对象。

![图像元素示例](images/Image_Licorice.jpg)

在该示例中，[Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) 属性用于指定要显示的图像的位置。 可以通过指定绝对 URL（例如 http://contoso.com/myPicture.jpg) ）或指定相对于你的应用包装结构的 URL 来设置 Source。 例如，我们将“licorice.jpg”图像文件放在项目的根目录文件夹中，并将包含图像文件的项目设置声明为内容。

### <a name="imagebrush"></a>ImageBrush

借助 [ImageBrush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush) 对象，你可使用图像来绘制获取 [Brush](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Brush) 对象的区域。 例如，你可将 ImageBrush 用于 [Ellipse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Shapes.Ellipse) 的 [Fill](https://docs.microsoft.com/uwp/api/windows.ui.xaml.shapes.shape.fill) 属性或 [Canvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Canvas) 的 [Background](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background) 属性的值。

接来下的示例显示如何使用 ImageBrush 绘制 Ellipse。

```XAML
<Ellipse Height="200" Width="300">
   <Ellipse.Fill>
     <ImageBrush ImageSource="sunset.jpg" />
   </Ellipse.Fill>
</Ellipse>
```

以下是通过 ImageBrush 绘制的 Ellipse。

![ImageBrush 绘制的椭圆。](images/Image_ImageBrush_Ellipse.jpg)

### <a name="stretch-an-image"></a>拉伸图像

如果你没有设置 Image 的 [Width](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.width) 或 [Height](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.height) 值，将以 Source 指定的图像的尺寸显示   。 设置 **Width** 和 **Height** 可创建显示图像所在的封闭矩形区域。 你可通过使用 [Stretch](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.stretch) 属性指定图像填充此封闭区域的方式。 Stretch 属性接受 [Stretch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) 枚举定义的以下值：

-   **无**：该图像未拉伸以适合输出尺寸。 注意此 Stretch 设置：如果源图像大于封闭区域，你的图像将被剪切，但通常不符合预期的，因为你对视口没有任何控制权，就像你处理精修的 [Clip](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) 一样。
-   **Uniform**：缩放图像以适应输出尺寸。 但是会保留内容的纵横比。 这是默认值。
-   **UniformToFill**：缩放图像以完全填满输出区域，但保留其原始的纵横比。
-   **填充**：缩放图像以适应输出尺寸。 因为内容的高度和宽度是独立缩放的，所以可能不能保留图像的原始纵横比。 即，图像可能为了完全填满输出区域而失真。

![拉伸设置示例。](images/Image_Stretch.jpg)

### <a name="crop-an-image"></a>裁剪图像

你可使用 [Clip](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip) 属性从图像输出中剪切一个区域。 你可将 Clip 属性设置为 [Geometry](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Geometry)。 目前不支持非矩形剪裁。

接下来的示例显示如何将 [RectangleGeometry](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RectangleGeometry) 用作图像的剪裁区域。 在该示例中，我们定义高度为 200 的 **Image** 对象。 **RectangleGeometry** 定义将显示图像区域的矩形。 [Rect](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.rectanglegeometry.rect) 属性设置为 25,25,100,150，即将矩形开始位置定义为“25,25”，将宽度和高度分别定义为 100 和 150。 仅显示矩形区域内的图像部分。

```xaml
<Image Source="sunset.jpg" Height="200">
    <Image.Clip>
        <RectangleGeometry Rect="25,25,100,150" />
    </Image.Clip>
</Image>
```

下面是黑色背景上的裁剪图像。

![RectangleGeometry 剪切的图像对象。](images/Image_Cropped.jpg)

### <a name="apply-an-opacity"></a>应用不透明度

你可将 [Opacity](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.opacity) 应用至图像中，以便图像显示为半透明。 不透明度值的范围为 0.0 到 1.0，其中 1.0 为完全不透明，而 0.0 为完全透明。 此示例显示了如何将 0.5 的不透明度应用到图像。

```xaml
<Image Height="200" Source="sunset.jpg" Opacity="0.5" />
```

以下是呈现的图像，不透明度为 0.5，透过部分不透明的黑色背景。

![不透明度为 0.5 的图像对象。](images/Image_Opacity.jpg)

### <a name="image-file-formats"></a>图像文件格式

**Image** 和 **ImageBrush** 可显示以下图像文件格式：

-   联合图像专家组 (JPEG)
-   可移植网络图形 (PNG)
-   位图 (BMP)
-   图形交换格式 (GIF)
-   标记图像文件格式 (TIFF)
-   JPEG XR
-   图标 (ICO)

[Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)、[BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) 和 [BitmapSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource) 的 API 不包含任何用于编码和解码媒体格式的专用方法。 所有的编码和解码操作都是内置的，并且至多将编码和解码方面展现为加载事件的事件数据的一部分。 如果你需要对图像编码或解码执行任何特殊工作（你可能会在你的应用执行图像转换或操作时使用该工作），那么你应该使用在 [Windows.Graphics.Imaging](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging) 命名空间中提供的 API。 这些 API 也受 Windows 中的 Windows 图像处理组件 (WIC) 支持。

从 Windows 10 版本 1607 开始，**Image** 元素支持动态 GIF 图像。 当你使用 **BitmapImage** 作为图像 **Source** 时，可以访问 BitmapImage API 来控制动态 GIF 图像的播放。 有关详细信息，请参阅 [BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) 类页面上的“备注”。

> 注意：当你的应用针对&nbsp;Windows&nbsp;10 版本 1607 编译并且正在版本 1607（或更高版本）上运行时，动态 GIF 支持可用  。 当应用针对以前版本编译或在以前版本上运行时，将显示 GIF 的第一帧，但不对其进行动画处理。

有关应用资源和如何在应用中打包图像源的详细信息，请参阅[定义应用资源](https://docs.microsoft.com/previous-versions/windows/apps/hh965321(v=win.10))。

### <a name="writeablebitmap"></a>WriteableBitmap

[WriteableBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) 提供的 [BitmapSource](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapSource) 是可修改的，并且不使用 WIC 中的基于文件的基础解码。 你可动态更改图像，然后重新呈现更新的图像。 若要定义 WriteableBitmap 的缓冲区内容，请使用 [PixelBuffer](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.writeablebitmap.pixelbuffer) 属性来访问该缓冲区并使用流或语言特定的缓冲区类型来填充它  。 有关示例代码，请参阅 [WriteableBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap)。

### <a name="rendertargetbitmap"></a>RenderTargetBitmap

[RenderTargetBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap) 类可从运行的应用中捕获 XAML UI 树，然后代表位图图像源。 捕获后，该图像源可以应用到应用的其他部分，由用户另存为资源或应用数据，或者用于其他方案。 一个非常有用的方案是为导航方案创建 XAML 页面的运行时缩略图，如提供 [Hub](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Hub) 控件中的图像链接。 **RenderTargetBitmap** 对于捕获图像中出现的内容确实具有某些限制。 有关详细信息，请参阅 [RenderTargetBitmap](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.RenderTargetBitmap) 的 API 参考主题。

### <a name="image-sources-and-scaling"></a>图像源和缩放

你应该以多个推荐大小创建图像源，以确保你的应用在 Windows 进行缩放时具有良好的外观。 指定 **Image** 的 **Source** 时，你可以使用将为当前缩放自动引用正确资源的命名约定。 有关命名约定规范和详细信息，请参阅[快速入门：使用文件或图像资源](https://docs.microsoft.com/previous-versions/windows/apps/hh965325(v=win.10))。

有关如何设计缩放的详细信息，请参阅[布局和缩放的 UX 指南](https://developer.microsoft.com/windows/apps/design)。

### <a name="image-and-imagebrush-in-code"></a>代码中的 Image 和 ImageBrush

使用 XAML 指定 Image 和 ImageBrush 比使用代码指定更为典型。 这是因为这些元素通常是设计工具的输出，它们作为 XAML UI 定义的一部分。

如果要使用代码定义一个 Image 或 ImageBrush，请使用默认的构造函数，然后设置相关的源属性（[Image.Source](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) 或 [ImageBrush.ImageSource](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imagebrush.imagesource)）。 在你使用代码设置源属性时，它们需要 [BitmapImage](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage)（而非 URI）。 如果源是一个流，请使用 [SetSourceAsync](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) 方法来初始化该值。 如果源是一个 URI（其中包含应用中使用 ms-appx 或 ms-resource 方案的内容），请使用获取 URI 的 [BitmapImage](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage) 构造函数   。 如果在检索或解码图像资源时存在任何计时问题，而你可能在图像资源可用前需要使用替代内容用以显示，则还可以考虑处理 [ImageOpened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.imageopened) 事件。 有关示例代码，请参阅 [XAML 图像示例](https://code.msdn.microsoft.com/windowsapps/0f5d56ae-5e57-48e1-9cd9-993115b027b9)。

> [!NOTE]
> 如果你使用代码建立图像，可以使用自动处理来访问具有当前规模和区域性限定符的非限定资源，或者可以使用具有区域性和规模限定符的 [ResourceManager](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceManager) 和 [ResourceMap](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap) 来直接获取资源。 有关详细信息，请参阅[资源管理系统](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

-   [音频、视频和相机](https://docs.microsoft.com/windows/uwp/audio-video-camera/index)
-   [Image 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image)
-   [ImageBrush 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ImageBrush)