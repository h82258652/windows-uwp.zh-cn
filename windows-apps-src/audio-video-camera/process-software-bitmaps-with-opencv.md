---
author: laurenhughes
ms.assetid: ''
description: 本文介绍了如何通过开放源计算机视觉库 (OpenCV) 使用 SoftwareBitmap 类。
title: 通过 OpenCV 处理位图
ms.author: lahugh
ms.date: 03/19/2018
ms.topic: article
keywords: windows 10, uwp, opencv, softwarebitmap
ms.localizationpriority: medium
ms.openlocfilehash: b9f1f2050590267d0a98779eba11bbe0b363da0c
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2018
ms.locfileid: "5860204"
---
# <a name="process-bitmaps-with-opencv"></a>通过 OpenCV 处理位图

本文介绍了如何通过开放源计算机视觉库 (OpenCV) 使用 **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** 类，该类被许多不同的 UWP API 用于表示图像。开放源计算机视觉库 (OpenCV) 是一种开放源本机代码库，可提供多种用于处理算法的图像。 

本文中的示例向你想演示了如何创建一个可通过 UWP 应用使用的本机代码 Windows 运行时组件，包括使用 C# 创建的应用。 此帮助程序组件将公开单个方法，即**模糊**，它将使用 OpenCV 的模糊图像处理功能。 此组件使用专用方法获取 OpenCV 库可直接使用的基础图像数据缓冲区的指针，从而能够更简单地扩展帮助程序组件，以实现其他 OpenCV 处理功能。 

* 有关使用 **SoftwareBitmap** 的介绍，请参阅[创建、编辑和保存位图图像](imaging.md)。 
* 若要了解如何使用 OpenCV 库，请转至 [http://opencv.org](http://opencv.org)。
* 若要了解如何结合使用本文中所示的 OpenCV 帮助程序组件和 **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** 对来自相机的帧进行实时图像处理，请参阅[通过 MediaFrameReader 使用 OpenCV](use-opencv-with-mediaframereader.md)。
* 有关实现不同效果的完整示例代码，请参阅 Windows 通用示例 GitHub 存储库中的[相机帧 + OpenCV 示例](https://go.microsoft.com/fwlink/?linkid=854003)。

> [!NOTE] 
> 本文中详述的 OpenCVHelper 组件采用的技术要求要处理的图像数据驻留在 CPU 内存中，而不是 GPU 内存中。 因此，对于允许你请求图像内存位置的 API，例如 **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)** 类，你应指定 CPU 内存。

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>为 OpenCV 互操作创建一个帮助程序 Windows 运行时组件

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. 向你的解决方案中添加新的本机代码 Windows 运行时组件项目

1. 在“解决方案资源管理器”中右键单击你的解决方案并选择**添加->新建项目**即可在 Visual Studio 中为解决方案添加新项目。 
2. 在 **Visual C++** 类别下，选择 **Windows 运行时组件（通用 Windows）**。 在本示例中，将项目命名为“OpenCVBridge”并单击**确定**。 
3. 在**新建 Windows 通用项目**对话框中，为你的应用选择目标和最低操作系统版本，然后单击**确定**。
4. 在“解决方案资源管理器”中右键单击自动生成的文件 Class1.cpp 并选择**删除**，弹出确认对话框后，选择**删除**。 然后删除 Class1.h 头文件。
5. 右键单击 OpenCVBridge 项目图标并选择**添加->类...**。在**添加类**对话框的**类名称**字段中输入“OpenCVHelper”并单击**确定**。 代码将在后续步骤中添加到已创建的类文件中。

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. 将 OpenCV NuGet 包添加到你的组件项目

1. 在解决方案资源管理器中右键单击 OpenCVBridge 项目图标，然后选择**管理 NuGet 包...**
2. 当 NuGet 包管理器对话框打开后，选择**浏览**选项卡并在搜索框中键入“OpenCV.Win”。
3. 选择“OpenCV.Win.Core”并单击**安装**。 在**预览**对话框中，单击**确定**。
4. 使用相同的步骤安装“OpenCV.Win.ImgProc”包。

> [!NOTE]
> OpenCV.Win.Core 和 OpenCV.Win.ImgProc 不会定期更新，但仍建议安装这两个包，用于创建 OpenCVHelper，如本页所述。

### <a name="3-implement-the-opencvhelper-class"></a>3. 实现 OpenCVHelper 类

将以下代码粘帖到 OpenCVHelper.h 头文件中。 此代码包含已安装的 *Core* 和 *ImgProc* 包的 OpenCV 头文件，并声明三种将在后续步骤中展示的方法。

[!code-cpp[OpenCVHelperHeader](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h#SnippetOpenCVHelperHeader)]

删除 OpenCVHelper.cpp 文件的已有内容，然后添加以下 include 指令。 

[!code-cpp[OpenCVHelperInclude](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperInclude)]

在 include 指令后，添加以下 **using** 指令。 

[!code-cpp[OpenCVHelperUsing](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperUsing)]

接下来，将方法 **GetPointerToPixelData** 添加到 OpenCVHelper.cpp。 此方法采用 **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)**，并且通过一系列转换可获得像素数据的 COM 接口表示，通过它我们可以以 **char** 阵列的形式获取基础数据缓冲区的指针。 

首先通过调用 **[LockBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)** 获得包含像素数据的 **[BitmapBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer)**，请求读取/写入缓冲区，使 OpenCV 库能够修改此像素数据。  将通过调用 **[CreateReference](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** 来获取 **[IMemoryBufferReference](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference)** 对象。 接着，**IMemoryBufferByteAccess** 界面将投影为 **IInspectable**（所有 Windows 运行时类的基本界面），并且将通过调用 **[QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521(v=vs.85).aspx)** 来获取 **[IMemoryBufferByteAccess](https://msdn.microsoft.com/library/mt297505(v=vs.85).aspx)** COM 界面，它使我们能够以 **char** 阵列的形式获取像素数据缓冲区。 最后，填充 **char** 阵列，方法是调用 **[IMemoryBufferByteAccess::GetBuffer](https://msdn.microsoft.com/library/mt297506(v=vs.85).aspx)**。 如果此方法中的任何转换步骤失败，方法会返回 **false**，表明无法继续进行后续处理。

[!code-cpp[OpenCVHelperGetPointerToPixelData](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperGetPointerToPixelData)]

接着，添加方法 **TryConvert**，如下所示。 此方法采用 **SoftwareBitmap** 并尝试将其转换为 **Mat** 对象，后者是 OpenCV 用于表示图像数据缓冲区的矩阵对象。 此方法通过调用前面定义的 **GetPointerToPixelData** 方法来获取像素数据缓冲区的 **char** 阵列表示。 如果成功，将会调用 **Mat** 类的构造函数，以传递从源 **SoftwareBitmap** 对象获得的像素宽度和高度。 

> [!NOTE] 
> 本示例指定 CV_8UC4 常量作为创建的 **Mat** 对象的像素格式。 这意味着传递至此方法的 **SoftwareBitmap** 必须具有带预乘 alpha 的 **[BitmapPixelFormat](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** property value of  **[BGRA8](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)**（与 CV_8UC4 等效）来运行此示例。

此方法将会返回所创建的 **Mat** 对象的浅拷贝，以便在 **SoftwareBitmap** 引用的同一数据像素数据缓冲区（而不是此缓冲区的副本）上继续后续处理。

[!code-cpp[OpenCVHelperTryConvert](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperTryConvert)]

最后，此示例帮助程序类实现单个图像处理方法，即**模糊**，该方法只需使用前面定义的 **TryConvert** 方法即可检索表示模糊操作的源位图和目标位图的 **Mat** 对象，然后调用 OpenCV ImgProc 库中的**模糊**方法。 其他**模糊**参数用于指定 X 和 Y 方向的模糊效果的大小（程度）。

[!code-cpp[OpenCVHelperBlur](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperBlur)]


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>有关使用帮助程序组件的简单 SoftwareBitmap OpenCV 示例
现在已创建了 OpenCVBridge 组件，我们就可以创建一个使用 OpenCV **模糊**方法修改 **SoftwareBitmap** 的简单 C# 应用。 若要从 UWP 应用访问 Windows 运行时组件，你必须先为组件添加引用。 在“解决方案资源管理器”中，右键单击 UWP 应用项目下的**引用**节点并选择**添加引用...**。在“引用管理器”中，选择**项目->解决方案**。 选中 OpenCVBridge 项目旁边的框并单击**确定**。

以下示例代码使用户能够选择一个图像文件，然后使用**[BitmapDecoder](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder)** 创建图像的 **SoftwareBitmap** 表示。 有关使用 **SoftwareBitmap** 的详细信息，请参阅[创建、编辑和保存位图图像](https://docs.microsoft.com/windows/uwp/audio-video-camera/imaging)。

如本文前面所述，**OpenCVHelper** 类要求需要解码的所有提供 **SoftwareBitmap** 图像均使用带预乘 alpha 值的 BGRA8 像素格式，因此，如果图像不是采用此格式，则示例代码将调用 **[Convert](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** 来将图像转换为预期格式。

接着，将会创建 **SoftwareBitmap**，以用作模糊操作的目标。 输入图像属性用作构造函数的参数，以创建具有匹配格式的位图。

将创建新的 **OpenCVHelper** 示例，并调用**模糊**方法，以传递源位图和目标位图。 最后，将创建 **SoftwareBitmapSource**，以将输出图像分配给 XAML **Image** 控件。


[!code-cs[OpenCVBlur](./code/ImagingWin10/cs/MainPage.OpenCV.xaml.cs#SnippetOpenCVBlur)]

## <a name="related-topics"></a>相关主题

* [BitmapEncoder 选项参考](bitmapencoder-options-reference.md)
* [图像元数据](image-metadata.md)
 

 




