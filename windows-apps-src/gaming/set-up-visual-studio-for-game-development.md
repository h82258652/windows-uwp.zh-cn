---
title: 用于游戏编程的 Visual Studio 工具
description: 概要介绍了 Visual Studio 中提供的 DirectX 特定工具。
ms.assetid: 43137bfc-7876-70e0-515c-4722f68bd064
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, visual studio, 工具, directx
ms.localizationpriority: medium
ms.openlocfilehash: f4ff471ae4c45761a1c1dad5819e97c8245746a1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368135"
---
# <a name="visual-studio-tools-for-game-programming"></a>用于游戏编程的 Visual Studio 工具



**摘要**

-   [从模板创建 DirectX 游戏项目](user-interface.md)
-   用于 DirectX 游戏编程的 Visual Studio 工具


如果使用 Visual Studio Ultimate 开发 DirectX 应用，那么可以使用附加的工具来创建、编辑、预览以及导出图像、模型和着色器资源。 还有一些工具，可以用来在生成时转换资源以及调试 DirectX 图形代码。

本主题概要介绍了这些图形工具。

## <a name="image-editor"></a>图像编辑器


使用图像编辑器可处理 DirectX 使用的各种丰富纹理和图像格式。 图像编辑器支持以下格式。

-   .png
-   .jpg、jpeg、.jpe、.jfif
-   .dds
-   .gif
-   .bmp
-   .dib
-   .tif、.tiff
-   .tga

创建[生成自定义文件](#build-customizations-for-3d-assets)以在生成时将这些内容转换为 .dds 文件。

有关详细信息，请参阅[使用纹理和图像](https://docs.microsoft.com/visualstudio/designers/working-with-textures-and-images?view=vs-2015)。

> **请注意**  使用图像编辑器不是为了替代完整功能图像编辑应用，但适用于许多简单查看和编辑方案。

 

## <a name="model-editor"></a>模型编辑器


可以使用模型编辑器从头开始创建基本的 3D 模型，或者使用功能全面的 3D 建模工具查看和修改更复杂的 3D 模型。 模型编辑器支持 DirectX 应用开发中使用的多种 3D 模型格式。 可以创建[生成自定义文件](#build-customizations-for-3d-assets)以在生成时将这些格式转换为 .cmo 文件。

-   .fbx
-   .dae
-   .obj

下面是编辑器中应用了照明的模型屏幕截图。

![茶壶](images/modeleditor.png)

有关详细信息，请参阅[使用 3D 模型](https://docs.microsoft.com/visualstudio/designers/working-with-3-d-models?view=vs-2015)。

> **请注意**  使用模型编辑器不是为了替代完整的功能模型编辑应用，但适用于许多简单查看和编辑方案。

 

## <a name="shader-designer"></a>着色器设计器


使用着色器设计器为你的游戏或应用创建自定义视觉效果（即使你不了解 HLSL 编程）。

创建一个着色器视觉图。 每个节点都显示该操作的输出预览。 下面是对球形预览应用兰伯特照明的示例。

![着色器视觉图](images/shaderdesigner.png)

使用着色器编辑器设计、编辑着色器，并采用 .dgsl 格式保存着色器。 它还导出以下格式。

-   .hlsl（源代码）
-   .cso（字节码）
-   .h（HLSL 字节码数组）

创建[生成自定义文件](#build-customizations-for-3d-assets)以在生成时将这些格式转换为 .cso 文件。

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

有关详细信息，请参阅[使用着色器](https://docs.microsoft.com/visualstudio/designers/working-with-shaders?view=vs-2015)。

## <a name="build-customizations-for-3d-assets"></a>为 3D 资源生成自定义


你可以向项目中添加生成自定义以便 Visual Studio 将资源转换为可以使用的格式。 在此之后，你可以将资源加载到应用中 并通过创建和填充 DirectX 资源来使用它们，就像在任何其他 DirectX 应用中一样。

若要添加生成自定义，请在“解决方案资源管理器”  中右键单击该项目，然后选择“生成自定义...”  。可以向项目中添加以下类型的生成自定义。

-   图像内容管道获取图像文件作为输入，并输出 DirectDraw 表面 (.dds) 文件。
-   网格内容管道获取网格文件（如 .fbx）并输出 .cmo 网格文件。
-   着色器内容管道从 Visual Studio 着色器编辑器中获取着色器视觉图 (.dgsl) 并输出编译的着色器输出 (.cso) 文件。

有关详细信息，请参阅[在游戏或应用中使用 3D 资源](https://docs.microsoft.com/visualstudio/designers/using-3-d-assets-in-your-game-or-app?view=vs-2015)。

## <a name="debugging-directx-graphics"></a>调试 DirectX 图形


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

有关详细信息，请参阅[调试 DirectX 图形](https://docs.microsoft.com/visualstudio/debugger/visual-studio-graphics-diagnostics?view=vs-2015)。


 

 

 




