---
title: 用于游戏编程的 Visual Studio 工具
description: 概要介绍了 Visual Studio 中提供的 DirectX 特定工具。
ms.assetid: 43137bfc-7876-70e0-515c-4722f68bd064
---

# 用于游戏编程的 Visual Studio 工具


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**摘要**

-   [从模板创建 DirectX 游戏项目](user-interface.md)
-   用于 DirectX 游戏编程的 Visual Studio 工具


如果使用 Visual Studio Ultimate 开发 DirectX 应用，那么可以使用附加的工具来创建、编辑、预览以及导出图像、模型和着色器资源。 还有一些工具，可以用来在生成时转换资源以及调试 DirectX 图形代码。

本主题概要介绍了这些图形工具。

## 图像编辑器


使用图像编辑器可处理 DirectX 使用的各种丰富纹理和图像格式。 图像编辑器支持以下格式。

-   .png
-   .jpg、jpeg、.jpe、.jfif
-   .dds
-   .gif
-   .bmp
-   .dib
-   .tif、.tiff
-   .tga

创建[生成自定义文件](#custom)以在生成时将这些内容转换为 .dds 文件。

有关详细信息，请参阅[使用纹理和图像](https://msdn.microsoft.com/library/windows/apps/hh873119.aspx)。

> **注意** 图像编辑器并非为了取代功能全面的图像编辑应用，而是适合于很多简单的查看和编辑方案。

 

## 模型编辑器


可以使用模型编辑器从头开始创建基本的 3D 模型，或者使用功能全面的 3D 建模工具查看和修改更复杂的 3D 模型。 模型编辑器支持 DirectX 应用开发中使用的多种 3D 模型格式。 可以创建[生成自定义文件](#custom)以在生成时将这些格式转换为 .cmo 文件。

-   .fbx
-   .dae
-   .obj

下面是编辑器中应用了照明的模型屏幕截图。

![茶壶](images/modeleditor.png)

有关详细信息，请参阅[使用 3D 模型](https://msdn.microsoft.com/library/windows/apps/hh873114.aspx)。

> **注意** 模型编辑器并非为了取代功能全面的模型编辑应用，而是适合于很多简单的查看和编辑方案。

 

## 着色器设计器


使用着色器设计器为你的游戏或应用创建自定义视觉效果（即使你不了解 HLSL 编程）。

创建一个着色器视觉图。 每个节点都显示该操作的输出预览。 下面是对球形预览应用兰伯特照明的示例。

![着色器视觉图](images/shaderdesigner.png)

使用着色器编辑器设计、编辑着色器，并采用 .dgsl 格式保存着色器。 它还导出以下格式。

-   .hlsl（源代码）
-   .cso（字节码）
-   .h（HLSL 字节码数组）

创建[生成自定义文件](#custom)以在生成时将这些格式转换为 .cso 文件。

下面是着色器编辑器导出的部分 HLSL 代码。 这仅是兰伯特照明节点的代码。

```hlsl
//
// Lambert lighting function
//
float3 LambertLighting(
    float3 lightNormal,
    float3 surfaceNormal,
    float3 materialAmbient,
    float3 lightAmbient,
    float3 lightColor,
    float3 pixelColor
    )
{
    // Compute the amount of contribution per light.
    float diffuseAmount = saturate(dot(lightNormal, surfaceNormal));
    float3 diffuse = diffuseAmount * lightColor * pixelColor;

    // Combine ambient with diffuse.
    return saturate((materialAmbient * lightAmbient) + diffuse);
}
```

有关详细信息，请参阅[使用着色器](https://msdn.microsoft.com/library/windows/apps/hh873117.aspx)。

## 为 3D 资源生成自定义


你可以向项目中添加生成自定义以便 Visual Studio 将资源转换为可以使用的格式。 在此之后，你可以将资源加载到应用中 并通过创建和填充 DirectX 资源来使用它们，就像在任何其他 DirectX 应用中一样。

若要添加生成自定义，请在“解决方案资源管理器”****中右键单击该项目，然后选择“生成自定义...”****。 可以向项目中添加以下类型的生成自定义。

-   图像内容管道获取图像文件作为输入，并输出 DirectDraw 表面 (.dds) 文件。
-   网格内容管道获取网格文件（如 .fbx）并输出 .cmo 网格文件。
-   着色器内容管道从 Visual Studio 着色器编辑器中获取着色器视觉图 (.dgsl) 并输出编译的着色器输出 (.cso) 文件。

有关详细信息，请参阅[在游戏或应用中使用 3D 资源](https://msdn.microsoft.com/library/windows/apps/hh972446.aspx)。

## 调试 DirectX 图形


Visual Studio 提供特定于图形的调试工具。 可使用这些工具来调试如下内容：

-   图形管道。
-   事件调用堆栈。
-   对象表。
-   设备状态。
-   着色器缺陷。
-   常量缓冲区和参数未初始化或不正确。
-   DirectX 版本兼容性。
-   有限的 Direct2D 支持。
-   操作系统和 SDK 要求。

有关详细信息，请参阅[调试 DirectX 图形](https://msdn.microsoft.com/library/windows/apps/hh315751.aspx)。

> **注意** 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

 

 






<!--HONumber=Mar16_HO1-->


