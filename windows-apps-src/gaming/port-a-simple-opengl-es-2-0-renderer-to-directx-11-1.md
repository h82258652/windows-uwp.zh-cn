---
author: mtoepke
title: 将简单的 OpenGL ES 2.0 呈现器移植到 Direct3D 11
description: 作为首个移植练习，我们将从最基本的内容开始：将旋转且顶点着色的立方体的简单呈现器从 OpenGL ES 2.0 移植到 Direct3D 中，以便它与 Visual Studio 2015 中的 DirectX 11 应用（通用 Windows）模板相匹配。
ms.assetid: e7f6fa41-ab05-8a1e-a154-704834e72e6d
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, opengl, direct3d 11, 移植
ms.localizationpriority: medium
ms.openlocfilehash: e7541a8f54f64197c17acea5f1737e36b0e6f670
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5556861"
---
# <a name="port-a-simple-opengl-es-20-renderer-to-direct3d-11"></a>将简单的 OpenGL ES 2.0 呈现器移植到 Direct3D 11



对于本次移植练习，我们将从最基本的内容开始：将旋转且顶点着色的立方体的简单呈现器从 OpenGL ES 2.0 移植到 Direct3D 中，以便它与 Visual Studio 2015 中的 DirectX 11 应用（通用 Windows）模板相匹配。 当我们逐步完成此移植过程时，你将学习到以下内容：

-   如何将一组简单的顶点着色器移植到 Direct3D 输入缓冲区
-   如何将 uniform 和 attribute 移植到常量缓冲区
-   如何配置 Direct3D 着色器对象
-   如何在 Direct3D 着色器开发中使用基本的 HLSL 语义
-   如何将非常简单的 GLSL 移植到 HLSL

本主题会在创建新的 DirectX 11 项目之后开始。 若要了解如何创建新的 DirectX 11 项目， 请阅读[创建新的用于通用 Windows 平台 (UWP) 的 DirectX 11 项目](user-interface.md)。

通过上面任一链接创建的项目准备了 [Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476345) 基础结构的所有代码， 你可以直接执行将呈现器从 Open GL ES 2.0 移植到 Direct3D 11 的过程。

本主题介绍了执行以下同一基本图形任务的两种代码路径：在窗口中显示一个旋转的顶点着色立方体。 在这两种情况下，该代码涉及以下流程：

1.  从硬编码数据创建立方体网格。 此网格表示为顶点的列表，每个顶点拥有一个位置、一个正常矢量和一个颜色矢量。 此网格放入顶点缓冲区供着色管道进行处理。
2.  创建着色器对象以处理立方体网格。 有两个着色器：拥有用于光栅化的顶点的顶点着色器，以及在光栅化后为立方体的单个像素着色的片段（像素）着色器。 这些像素将写入呈现目标以供显示。
3.  顶点和片段着色器中生成分别生成用于顶点和像素处理的着色语言。
4.  在屏幕上显示呈现后的立方体。

![简单的 OpenGL 立方体](images/simple-opengl-cube.png)

完成本操作实例之后，你应该会熟悉 Open GL ES 2.0 和 Direct3D 11 之间的下列基本差别：

-   顶点缓冲区和顶点数据的表示。
-   创建和配置着色器的过程。
-   着色语言以及着色器对象的输入和输出。
-   屏幕绘制行为。

在本操作实例中，我们谈及了一个简单且通用的 OpenGL 呈现器结构，该结构的定义如下：

``` syntax
typedef struct 
{
    GLfloat pos[3];        
    GLfloat rgba[4];
} Vertex;

typedef struct
{
  // Integer handle to the shader program object.
  GLuint programObject;

  // The vertex and index buffers
  GLuint vertexBuffer;
  GLuint indexBuffer;

  // Handle to the location of model-view-projection matrix uniform
  GLint  mvpLoc; 
   
  // Vertex and index data
  Vertex  *vertices;
  GLuint   *vertexIndices;
  int       numIndices;

  // Rotation angle used for animation
  GLfloat   angle;

  GLfloat  mvpMatrix[4][4]; // the model-view-projection matrix itself
} Renderer;
```

该结构具有一个实例，并且包含用于呈现非常简单的顶点作色网格所需的所有组件。

> **注意**本主题中的所有 OpenGL ES 2.0 代码都基于 Khronos Group，提供 Windows API 实现和使用 Windows C 编程语法。

 

## <a name="what-you-need-to-know"></a>你需要了解的内容


### <a name="technologies"></a>技术

-   [Microsoft Visual C++](http://msdn.microsoft.com/library/vstudio/60k1461a.aspx)
-   OpenGL ES 2.0

### <a name="prerequisites"></a>先决条件

-   可选。 查看[将 EGL 代码移植到 DXGI 和 Direct3D](moving-from-egl-to-dxgi.md)。 阅读本主题以便更好地了解 DirectX 提供的图形接口。

## <a name="span-idkeylinksstepsheadingspansteps"></a><span id="keylinks_steps_heading"></span>步骤


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="port-the-shader-config.md">移植着色器对象</a></p></td>
<td align="left"><p>移植 OpenGL ES 2.0 中的简单呈现器时，第一步是在 Direct3D 11 中设置等效的顶点着色器和片段着色器对象，并且确保在编译之后主程序能够与着色器对象进行通信。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="port-the-vertex-buffers-and-data-config.md">移植顶点缓冲区和数据</a></p></td>
<td align="left"><p>在此步骤中，你将定义将包含网格的顶点缓冲区以及允许着色器按照指定的顺序遍历顶点的索引缓冲区。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="port-the-glsl.md">移植 GLSL</a></p></td>
<td align="left"><p>转到创建和配置缓冲区及着色器对象的代码之后，应该将这些着色器中的代码从 OpenGL ES 2.0 的 GL 着色器语言 (GLSL) 移植到 Direct3D 11 的高级着色器语言 (HLSL)。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="draw-to-the-screen.md">绘制到屏幕</a></p></td>
<td align="left"><p>最终，我们会移植可将旋转立方体绘制到屏幕的代码。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idadditionalresourcesspanadditional-resources"></a><span id="additional_resources"></span>其他资源


-   [为 UWP DirectX 游戏开发准备开发人员环境](prepare-your-dev-environment-for-windows-store-directx-game-development.md)
-   [创建针对 UWP 的新的 DirectX 11 项目](user-interface.md)
-   [将 OpenGL ES 2.0 概念和基础结构映射到 Direct3D 11](map-concepts-and-infrastructure.md)

 

 




