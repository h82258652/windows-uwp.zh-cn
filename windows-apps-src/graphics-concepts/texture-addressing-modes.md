---
title: 纹理寻址模式
description: Direct3D 应用程序可以将纹理坐标分配至任何基元的任何顶点。
ms.assetid: 925E8F2E-43EC-404E-8870-03E39155F697
keywords:
- 纹理寻址模式
- Wrap 纹理寻址模式
- 镜像纹理寻址模式
- Clamp 纹理寻址模式
- 边框颜色纹理寻址模式
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 5e263876f414e5683ffc8a5645a12e5031b3d6fb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660182"
---
# <a name="texture-addressing-modes"></a>纹理寻址模式


Direct3D 应用程序可以将纹理坐标分配至任何基元的任何顶点。 通常，你分配至某个顶点的 u 纹理以及 v 纹理坐标的范围介于 0.0-1.0（包含这两个值）。 但如果在此范围之外分配纹理坐标，则可以创造某些特殊的纹理效果。 .

控制 Direct3D 纹理坐标为外部的用途\[0.0，1.0\]通过设置纹理寻址模式的范围。 例如，你可以通过应用程序设置纹理寻址模式，以使得纹理跨基元平铺。

借助 Direct3D，应用程序可以执行纹理环绕。 参阅[纹理环绕](texture-wrapping.md)。

启用纹理包装有效地使外部的纹理坐标\[0.0，1.0\]范围无效和行为的光栅化此类拖欠纹理坐标是不确定这种情况下。 当纹理环绕处于启用状态时，不能使用纹理寻址模式。 请注意，当纹理环绕处于启用状态时，你的应用程序不会指定低于 0.0 或高于 1.0 的纹理坐标。

## <a name="span-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspanspan-idsummaryofthetextureaddressingmodesspansummary-of-the-texture-addressing-modes"></a><span id="Summary_of_the_texture_addressing_modes"></span><span id="summary_of_the_texture_addressing_modes"></span><span id="SUMMARY_OF_THE_TEXTURE_ADDRESSING_MODES"></span>寻址模式的纹理的摘要


| 纹理寻址模式 | 描述                                                                                                                           |
|-------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| 环绕                    | 在每个整数点重复纹理。                                                                                        |
| Mirror                  | 以每个整数为界映射纹理。                                                                                        |
| Clamp                   | 你的纹理坐标到 clamps \[0.0，1.0\]范围;Clamp 模式将纹理应用一次，然后被抹去边缘像素的颜色。 |
| 边框颜色            | 对于超出 0.0-1.0（包括这两者）范围的任何纹理坐标，使用任意的*边框颜色*。                         |

 

## <a name="span-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanspan-idwraptextureaddressmodespanwrap-texture-address-mode"></a><span id="Wrap_texture_address_mode"></span><span id="wrap_texture_address_mode"></span><span id="WRAP_TEXTURE_ADDRESS_MODE"></span>包装纹理寻址模式


在 Wrap 纹理寻址模式下，Direct3D 可以在每个整数点重复纹理。

例如，假设应用程序创建平方基元并指定纹理坐标为 (0.0,0.0)、(0.0,3.0)、(3.0,3.0) 以及 (3.0,0.0)。 将纹理寻址模式设置为“Wrap”会使纹理在 u 和 v 方向应用三次，如下图所示。

![图示为在 u 和 v 方向环绕的正面纹理](images/wrap.png)

将此与下面的**镜像纹理寻址模式**比较。

## <a name="span-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanspan-idmirrortextureaddressmodespanmirror-texture-address-mode"></a><span id="Mirror_texture_address_mode"></span><span id="mirror_texture_address_mode"></span><span id="MIRROR_TEXTURE_ADDRESS_MODE"></span>镜像纹理寻址模式


在镜像纹理寻址模式下，Direct3D 可以每个整数为界映射纹理。

例如，假设应用程序创建平方基元并指定纹理坐标为 (0.0,0.0)、(0.0,3.0)、(3.0,3.0) 以及 (3.0,0.0)。 将纹理寻址模式设置为“镜像”会使纹理在 u 和 v 方向应用三次。 纹理应用到的所有其他行和列均为前一行或前一列的镜像，如下图所示。

![图示为采用 3x3 网格的镜像](images/mirror.png)

将此与前面的 **Wrap 纹理寻址模式**比较。

## <a name="span-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanspan-idclamptextureaddressmodespanclamp-texture-address-mode"></a><span id="Clamp_texture_address_mode"></span><span id="clamp_texture_address_mode"></span><span id="CLAMP_TEXTURE_ADDRESS_MODE"></span>将纹理寻址模式


将纹理地址模式将导致要固定其范围为你的纹理坐标的 Direct3D \[0.0，1.0\]范围;Clamp 模式将纹理应用一次，然后被抹去边缘像素的颜色。

例如，假设应用程序创建了一个平方基元，并将 (0.0,0.0)、(0.0,3.0)、(3.0,3.0) 以及 (3.0,0.0) 纹理坐标分配至该基元的顶点。 将纹理寻址模式设置为"Clamp"会使纹理应用一次。 列顶部和行结尾的像素颜色将分别延伸到基元的顶部和右侧。

下图所示为固定的纹理。

![图示为纹理以及一个固定的纹理](images/clamp.png)

## <a name="span-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanspan-idbordercolortextureaddressmodespanborder-color-texture-address-mode"></a><span id="Border_Color_texture_address_mode"></span><span id="border_color_texture_address_mode"></span><span id="BORDER_COLOR_TEXTURE_ADDRESS_MODE"></span>边框颜色纹理寻址模式


在边框颜色纹理寻址模式下，对于超出 0.0-1.0（包括这两者）范围的任何纹理坐标，Direct3D 可以使用任意颜色（称为*边框颜色*）。

在下图中，应用程序指定必须将纹理应用到使用红色边框的基元。

![图示为纹理以及使用红色边框的纹理](images/border.png)

## <a name="span-iddevicelimitationsspanspan-iddevicelimitationsspanspan-iddevicelimitationsspandevice-limitations"></a><span id="Device_Limitations"></span><span id="device_limitations"></span><span id="DEVICE_LIMITATIONS"></span>设备限制


尽管系统一般情况下支持超出 0.0-1.0（包括这两者）范围的纹理坐标，但硬件限制通常会影响纹理坐标可超出的范围。 当你检索设备功能时，呈现设备将显示对设备所允许的所有纹理坐标的限制。

例如，如果限值为 128，则输入的纹理坐标必须控制在 -128.0 至 +128.0 的范围内。 使超出该范围的纹理坐标通过顶点是无效的。 相同的限制适用于因纹理坐标自动生成以及纹理坐标变换而生成的纹理坐标。

纹理重复限制取决于按纹理坐标编索引的纹理的大小。 假设在此情况下，纹理大小为 32，设备允许的纹理坐标的范围为 512，则实际有效的纹理坐标范围应为 512/32 = 16，也就是说，该设备的纹理坐标范围必须介于 -16.0 至 +16.0。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理](textures.md)

 

 




