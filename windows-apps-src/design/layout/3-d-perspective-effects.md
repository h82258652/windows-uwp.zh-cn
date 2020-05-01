---
ms.assetid: 90F07341-01F4-4205-8161-92DD2EB49860
title: XAML UI 的 3D 透视效果
description: 你可以使用透视变换将 3D 效果应用到 Windows 运行时应用中的内容。 例如，你可以创建旋转的对象接近和远离你的视觉效果，如此处所示。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8db56882833b9d3bd8a6d2932d04e07a72b205e2
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "66365253"
---
# <a name="3-d-perspective-effects-for-xaml-ui"></a>XAML UI 的 3D 透视效果


你可以使用透视变换将 3D 效果应用到 Windows 运行时应用中的内容。 例如，你可以创建旋转的对象接近和远离你的视觉效果，如此处所示。

![具有射影转换特性的图像](images/3dsimple.png)

射影转换的另一个常规用途是相互安排对象，从而创造三维效果，如下所述。

![堆叠对象以创造三维效果](images/3dstacking.png)

除了创造静态三维效果外，你还可以利用射影转换特性的动画效果，从而创造动态的三维效果。

你只看到射影转换对图像试用，但实际上，你也可以将这些效果应用到所有 [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement)（包括控件）。 例如，你可以将三维效果应用到控件的整个容器，例如：

![应用到元素容器的三维效果](images/skewedstackpanel.png)

下面是用于创建此示例的 XAML 代码：

```xml
<StackPanel Margin="35" Background="Gray">    
    <StackPanel.Projection>        
        <PlaneProjection RotationX="-35" RotationY="-35" RotationZ="15"  />    
    </StackPanel.Projection>    
    <TextBlock Margin="10">Type Something Below</TextBlock>    
    <TextBox Margin="10"></TextBox>    
    <Button Margin="10" Content="Click" Width="100" />
</StackPanel>
```

在本文档中，我们重点讲述 [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) 的属性，用于在三维空间内旋转和移动对象。 下一个示例可以让你体验这些属性，并观看这些属性在对象上产生的效果。

## <a name="planeprojection-class"></a>PlaneProjection 类

通过使用 [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 来设置 UIElement 的 [**Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection) 属性，你可以对任何 [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) 应用 3D 效果。 **PlaneProjection** 定义在空间内呈现转换的方式。 下一个示例显示了一个简单情况。

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35"   />
    </Image.Projection>
</Image>
```

下图表明将图像呈现为什么。 x 轴、y 轴和 z 轴显示为红线。 图像使用 [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) 属性沿着 x 轴向后旋转了 35 度。

![RotateX 减去 35 度。](images/3drotatexminus35.png)

[  **RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) 属性围绕旋转中心的 y 轴转动。

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35"   />
    </Image.Projection>
</Image>
```

![RotateY 减去 35 度。](images/3drotateyminus35.png)

[  **RotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationz) 属性围绕旋转中心（与对象面板垂直的一条线）的 z 轴转动。

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationZ="-45"/>
    </Image.Projection>
</Image>
```

![RotateZ 减去 45 度。](images/3drotatezminus35.png)

旋转属性可以指定正值，也可以指定负值，因此可以向两个方向旋转。 绝对值可以大于 360，这样将绕对象旋转多于一周。

你可以使用 [**CenterOfRotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx)、[**CenterOfRotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy) 和 [**CenterOfRotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationz) 属性移动旋转中心的位置。 在默认情况下，旋转的轴正好位于对象的中心，所以对象会绕着中心旋转。 但是，如果你将旋转中心移动到对象的外侧边，它将会绕着该侧边进行旋转。 **CenterOfRotationX** 和 **CenterOfRotationY** 的默认值为 0.5，而 **CenterOfRotationZ** 的默认值为 0。 对于 **CenterOfRotationX** 和 **CenterOfRotationY**，0 和 1 之间的值设置对象中某些位置的透视点。 0 值表示对象的一条边，1 表示相对的边。 允许使用超出此范围的值，并且会相应地移动旋转中心。 由于旋转中心的 z 轴穿过对象面板的中心，所以，如果你使用负值，则可以将旋转中心移到对象后；如果你使用正值，则可以将旋转中心移到对象前（面向你的方向）。

[**CenterOfRotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx) 沿着对象的 x 轴平行移动旋转中心，而 [**CenterOfRotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy) 则沿着对象的 y 轴移动旋转中心。 下图演示了为 **CenterOfRotationY** 使用不同的值。

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.5"（默认值）**

![CenterOfRotationY 等于 0.5](images/3drotatexminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationX="-35" CenterOfRotationY="0.1"/>
    </Image.Projection>
</Image>
```

**CenterOfRotationY = "0.1"**

![CenterOfRotationY 等于 0.1](images/3dcenterofrotationy0point1.png)

注意将 [**CenterOfRotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationy) 属性设置为默认值 0.5 时，图像围绕中心旋转的方式，以及将其设置为 0.1 时，图像围绕接近上边的位置旋转的方式。 如果将 [**CenterOfRotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationx) 属性改成 [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) 属性旋转对象的位置，你会看到类似的行为。

```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.5"（默认值）**

![CenterOfRotationX 等于 0.5](images/3drotateyminus35.png)
```xml
<Image Source="kid.png">
    <Image.Projection>
        <PlaneProjection RotationY="-35" CenterOfRotationX="0.5" />
    </Image.Projection>
</Image>
```

**CenterOfRotationX = "0.9"（右边）**

![CenterOfRotationX 等于 0.9](images/3dcenterofrotationx0point9.png)

使用 [**CenterOfRotationZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.centerofrotationz) 可将旋转中心置于对象面板的上面或下面。 这样，你可以让对象围绕一个点旋转，这就类似于行星绕着恒星公转一样。

## <a name="positioning-an-object"></a>定位对象

至此，你了解了如何在一个空间内转动对象。 通过以下属性，你可以相对于其他对象定位一个空间内的旋转对象。

-   [**LocalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx) 沿着旋转对象面板的 x 轴移动对象。
-   [**LocalOffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) 沿着旋转对象面板的 y 轴移动对象。
-   [**LocalOffsetZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz) 沿着旋转对象面板的 z 轴移动对象。
-   [**GlobalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx) 沿着屏幕的 x 轴移动对象。
-   [**GlobalOffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsety) 沿着屏幕的 y 轴移动对象。
-   [**GlobalOffsetZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetz) 沿着屏幕的 z 轴移动对象。

**局部偏移**

[  **LocalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx)、[**LocalOffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) 和 [**LocalOffsetZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz) 属性可以在对象旋转后，沿着对象面板的相应轴转换对象。 因此，对象的旋转决定对象的转换方向。 为了介绍此概念，下一个示例使 **LocalOffsetX** 从 0 转到 400 度，使 [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) 从 0 转到 65 度。

注意，在上述示例中，对象沿着自己的 x 轴移动。 在动画最开始的位置，当 [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) 值接近于零（与屏幕平行）时，对象沿着屏幕 x 轴方向移动，但是随着对象向你的方向旋转，对象会沿着面向你的对象面板的 x 轴移动。 另一方面，如果将 **RotationY** 属性转动到 -65 度，则该对象会朝远离你的方向呈曲线移动。

[**LocalOffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsety) 与 [**LocalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetx) 的工作原理相似，只不过该属性是沿着纵轴移动，所以更改 [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) 会影响 **LocalOffsetY** 移动对象的方向。 在下一示例中，**LocalOffsetY** 从 0 转动到 400 度，而 **RotationX** 从 0 转动到 65 度。

[**LocalOffsetZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.localoffsetz) 以垂直于对象平面的方向转换对象，就好像从对象后面直接通过平面中心向你画一条垂线一样。 为了介绍 **LocalOffsetZ** 的工作原理，下一个示例使 **LocalOffsetZ** 从 0 转到 400 度，使 [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) 从 0 转到 65 度。

在动画的开始位置，当 [**RotationX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationx) 值接近于零（与屏幕平行）时，对象直接向你移动，但是随着对象的表面向下旋转，该对象会向下移动。

**全局偏移**

[  **GlobalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx)、[**GlobalOffsetY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsety) 和 [**GlobalOffsetZ**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetz) 属性沿着相对屏幕的轴转换对象。 也就是说，与局部偏移属性不同，对象沿着移动的轴与应用到对象的任何旋转无关。 如果你只想让对象沿着屏幕的 x-、y- 或 z- 轴移动，而不考虑应用到对象的旋转，那么，这些属性会非常有用。

下一个示例将 [**GlobalOffsetX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.globaloffsetx) 从 0 转动到 400 度，而将 [**RotationY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.planeprojection.rotationy) 从 0 转动到 65 度。

注意，在此示例中，对象在旋转时不会改变转向。 这是因为该对象沿着屏幕的 x 轴移动，与对象的旋转无关。

## <a name="positioning-an-object"></a>定位对象

对于比 [**PlaneProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Matrix3DProjection) 更复杂的半 3D 方案，你可以使用 [**Matrix3DProjection**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D.Matrix3D) 和 [**Matrix3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.PlaneProjection) 类型。 **Matrix3DProjection** 可为你提供应用于任何 [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) 的完全 3D 转换矩阵，从而使你可以将任何模型转换矩阵和透视矩阵应用到元素。 请记住，这些 API 非常小，因此如果要使用它们，你需要编写正确创建 3D 转换矩阵的代码。 因此，对于简单的 3D 方案，使用 **PlaneProjection** 更容易。
