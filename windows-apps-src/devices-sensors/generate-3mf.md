---
author: PatrickFarley
Description: Describes the structure of the 3D Manufacturing Format file type and how it can be created and manipulated with the Windows.Graphics.Printing3D API.
MS-HAID: dev\_devices\_sensors.generate\_3mf
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 生成 3MF 程序包
ms.assetid: 968d918e-ec02-42b5-b50f-7c175cc7921b
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 02eb6442b4769e92bec2b41ed9ab0e91a6a98a7f
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/11/2018
ms.locfileid: "4529080"
---
# <a name="generate-a-3mf-package"></a>生成 3MF 程序包

**重要的 API**

-   [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.aspx)

本指南介绍了 3D 制造格式文档的结构以及如何使用 [**Windows.Graphics.Printing3D**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.aspx) API 创建和操作该文档。

## <a name="what-is-3mf"></a>什么是 3MF？

3D 制造格式是有关出于制造目的（3D 打印）使用 XML 描述 3D 模型的外观和结构的一组约定。 它定义一组部件（有些必选，有些可选）及其关系，目的是向 3D 制造设备提供所有必要的信息。 符合 3D 制造格式的数据集可以另存为带有 .3mf 扩展名的文件。

在 Windows 10 中，**Windows.Graphics.Printing3D** 命名空间中的 [**Printing3D3MFPackage**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3d3mfpackage.aspx) 类类似于单个 .3mf 文件，并且其他类映射到该文件中的特定 XML 元素。 本指南介绍如何以编程方式创建和设置 3MF 文档的每个主要部分、如何利用 3MF Materials Extension 以及最后如何将 **Printing3D3MFPackage** 对象转换并另存为 .3mf 文件。 有关 3MF 的标准或 3MF Materials Extension 的详细信息，请参阅 [3MF 规范](http://3mf.io/what-is-3mf/3mf-specification/)。

<!-- >**Note** This guide describes how to construct a 3MF document from scratch. If you wish to make changes to an already existing 3MF document provided in the form of a .3mf file, you simply need to convert it to a **Printing3D3MFPackage** and alter the contained classes/properties in the same way (see [link]) below). -->


## <a name="core-classes-in-the-3mf-structure"></a>3MF 结构中的核心类

**Printing3D3MFPackage** 类表示完整的 3MF 文档，并且 3MF 文档的核心是其模型部分，由 [**Printing3DModel**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dmodel.aspx) 类表示。 我们希望指定的有关 3D 模型的大部分信息都将通过设置 **Printing3DModel** 类的属性和它们的基础类的属性来存储。

[!code-cs[InitClasses](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetInitClasses)]

<!-- >**Note** We do not yet associate the **Printing3D3MFPackage** with its corresponding **Printing3DModel** object. Only after fleshing out the **Printing3DModel** with all of the information we wish to specify will we make that association (see [link]). -->

## <a name="metadata"></a>Metadata

3MF 文档的模型部分可以将元数据以存储在 **Metadata** 属性中的字符串的键/值对的形式保存。 存在大量预定义的元数据名称，但其他对可以添加为扩展名的一部分（在 [3MF 规范](http://3mf.io/what-is-3mf/3mf-specification/)中有更详细的介绍）。 由程序包的接收器（一种 3D 制造设备）来确定是否以及如何处理元数据，但最好是在 3MF 程序包中包含尽可能多的基本信息：

[!code-cs[Metadata](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMetadata)]

## <a name="mesh-data"></a>网格数据

在本指南的上下文中，网格是从单个顶点集构造的三维几何图形（尽管它无需显示为单个顶点）。 网格部件由 [**Printing3DMesh**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dmesh.aspx) 类表示。 一个有效的网格对象必须包含有关其所有顶点以及存在于特定顶点集之间的所有三角形面的位置的信息。

以下方法将顶点添加到网格，然后提供它们在 3D 空间中的相应位置：

[!code-cs[Vertices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetVertices)]

下一个方法定义在这些顶点上绘制的所有三角形：

[!code-cs[TriangleIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTriangleIndices)]

> [!NOTE]
> 所有三角形都必须以逆时针方向的顺序（当从网格对象外部查看三角形时）定义其索引，以便使它们的面法线矢量指向外部。

当 Printing3DMesh 对象包含有效的顶点和三角形集时，应该随后将其添加到模型的 **Meshes** 属性。 程序包中的所有 **Printing3DMesh** 对象都必须存储在 **Printing3DModel** 类的 **Meshes** 属性下。

[!code-cs[MeshAdd](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMeshAdd)]


## <a name="create-materials"></a>创建材料


3D 模型可以保留多个材料的数据。 此约定旨在充分利用可在单个打印作业中使用多个材料的 3D 制造设备。 还有多种*类型*的材料组，每个都能够支持大量不同的单独材料。 每个材料组都必须有唯一的引用 ID 号，并且该组内的每个材料都必须有唯一的 ID。

然后，模型内的不同网格对象可以引用这些材料。 此外，每个网格上的个别三角形可以指定不同的材料。 更进一步，甚至可以在单个三角形内表示不同的材料，其中每个三角形顶点都分配有不同的材料，并将面材料计算为顶点之间的渐变。

本指南将先介绍如何在各自的材料组内创建不同类型的材料，然后将它们存储为模型对象上的资源。 然后，我们会了解将不同的材料分配到个别网格和个别三角形。

### <a name="base-materials"></a>基本材料

默认材料类型为**基本材料**，该类型具有**颜色材料**值（如下所述）和旨在指定要使用的材料*类型*的名称属性。

[!code-cs[BaseMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetBaseMaterialGroup)]

> [!NOTE]
> 3D 制造设备将确定哪些可用物理材料映射到存储在 3MF 中的哪些虚拟材料元素。 材料映射并不一定是 1:1：如果 3D 打印机只使用一种材料，无论向哪些对象或面分配了不同的材料，它都将以该材料打印整个模型。

### <a name="color-materials"></a>颜色材料

**颜色材料**类似于**基本材料**，但它们不包含名称。 因此，它们不会提供有关计算机应使用哪些材料类型的说明。 它们仅保留颜色数据，并且让计算机选择材料类型（计算机随后可能提示用户进行选择）。 在以下代码中，独立使用上一方法中的 `colrMat` 对象。

[!code-cs[ColorMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetColorMaterialGroup)]

### <a name="composite-materials"></a>复合材料

**复合材料**仅指示制造设备使用不同**基本材料**的统一组合。 每个**复合材料组**都必须仅引用一个要从中抽取成分的**基本材料组**。 此外，此组内要提供的**基本材料**必须在**材料索引**列表中列出，以供每个**复合材料**在指定比率（每个**复合材料**只是**基本材料**的比率）时引用。

[!code-cs[CompositeMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetCompositeMaterialGroup)]

### <a name="texture-coordinate-materials"></a>纹理坐标材料

3MF 支持使用 2D 图形为 3D 模型的图面上色。 通过此方式，该模型可以在每个三角形面上传达更多的颜色数据（与每个三角形顶点只有一个颜色值相反）。 和**颜色材料**一样，纹理坐标材料仅传达颜色数据。 若要使用 2D 纹理，必须先声明纹理资源：

[!code-cs[TextureResource](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTextureResource)]

> [!NOTE]
> 纹理数据属于 3MF 程序包本身，而不属于程序包内的模型部件。

接下来，我们必须填写 **Texture3Coord 材料**。 其中每一个都引用一个纹理资源，并在图像上指定一个特定的点（在 UV 坐标中）。

[!code-cs[Texture2CoordMaterialGroup](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetTexture2CoordMaterialGroup)]

## <a name="map-materials-to-faces"></a>将材料映射到面

为了指示哪些材料映射到每个三角形上的哪些顶点，我们必须在模型的网格对象上执行其他一些工作（如果模型包括多个网格，则每个网格都必须分别分配它们的材料）。 如上所述，材料按顶点、按三角形分配。 请参考下面的代码以查看如何输入和解释此信息。

[!code-cs[MaterialIndices](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetMaterialIndices)]

## <a name="components-and-build"></a>组件和版本

组件结构允许用户在可打印的 3D 模型中放置多个网格对象。 [**Printing3DComponent**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dcomponent.aspx) 对象包含单个网格和对其他组件的引用列表。 这实际上是 [**Printing3DComponentWithMatrix**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing3d.printing3dcomponentwithmatrix.aspx) 对象的列表。 每个 **Printing3DComponentWithMatrix** 对象都包含一个 **Printing3DComponent**，重要的是，还包含一个适用于所谓的 **Printing3DComponent** 的网格和包含组件的转换矩阵。

例如，汽车的模型可能由承载车身网格的“车身”**Printing3DComponent** 组成。 然后，“车身”组件可能包含对四个不同的 **Printing3DComponentWithMatrix** 对象的引用，这些对象全都引用带有“车轮”网格的相同 **Printing3DComponent** 并包含四个不同的转换矩阵（将车轮映射到车身上的四个不同位置）。 在此方案中，“车身”网格和“车轮”网格各自只需存储一次，即使最终产品总共会展示五个网格。

所有 **Printing3DComponent** 组件都必须在模型的 **Components** 属性中直接引用。 要在打印作业中使用的单个特定组件存储在 **Build** 属性中。

[!code-cs[Components](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetComponents)]

## <a name="save-package"></a>保存程序包
现在我们具有包含已定义材料和组件的模型，我们可以将其保存到程序包。

[!code-cs[SavePackage](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSavePackage)]

我们可以从此处在应用内启动打印作业（请参阅[从应用进行 3D 打印](https://msdn.microsoft.com/library/windows/apps/mt204541.aspx)），或者将此 **Printing3D3MFPackage** 另存为 .3mf 文件。

以下方法选取已完成的 **Printing3D3MFPackage** 并将其数据保存到.3mf 文件。

[!code-cs[SaveTo3mf](./code/3dprinthowto/cs/Generate3MFMethods.cs#SnippetSaveTo3mf)]

## <a name="related-topics"></a>相关主题

[从应用进行 3D 打印](https://msdn.microsoft.com/windows/uwp/devices-sensors/3d-print-from-app)  
[3D 打印 UWP 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/3DPrinting)
 

 

 
