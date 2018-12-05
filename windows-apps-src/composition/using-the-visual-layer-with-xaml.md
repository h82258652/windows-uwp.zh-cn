---
ms.assetid: b7a4ac8a-d91e-461b-a060-cc6fcea8e778
title: 将可视化层与 XAML 结合使用
description: 了解将可视化层 API 与现有 XAML 内容结合使用以创建高级动画和效果的技术。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ae9bc0f6d53181a88b02ecda19b3aed745febe40
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8736785"
---
# <a name="using-the-visual-layer-with-xaml"></a>将可视化层与 XAML 结合使用

使用可视化层功能的大多数应用都将使用 XAML 定义主要 UI 内容。 在 Windows 10 周年更新中，XAML 框架和可视化层中有新功能，可使结合这两种技术来创建精美的用户体验变得更简单。
XAML 和可视化层互操作功能可用于创建单独使用 XAML API 时无法实现的高级动画和效果。 这包括：

- 画笔效果（如模糊和毛玻璃）
- 动态照明效果
- 滚动驱动的动画和视差
- 自动布局动画
- 像素完美投影

这些效果和动画可应用于现有 XAML 内容，因此你无需大幅重构 XAML 应用即可充分利用新功能。
布局动画、阴影和模糊效果将在下面的秘诀部分进行介绍。 有关代码示例实现视差，请参阅 [ParallaxingListItems 示例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)。 [WindowsUIDevLabs 存储库](https://github.com/Microsoft/WindowsUIDevLabs)还有用于实现动画、阴影和效果的其他示例。

## <a name="the-xamlcompositionbrushbase-class"></a>XamlCompositionBrushBase 类

**XamlCompositionBrush** 为使用 **CompositionBrush** 绘制某个区域 XAML 画笔提供基类。 这可以用于方便地向 XAML UI 元素应用合成效果（如模糊或毛玻璃）。

有关将画笔与 XAML UI 结合使用的详细信息，请参阅[**画笔**](/windows/uwp/design/style/brushes#xamlcompositionbrushbase)部分。

有关代码示例，请参阅 [**XamlCompositionBrushBase**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamlcompositionbrushbase) 的参考页面。

## <a name="the-xamllight-class"></a>XamlLight 类

**XamlLight** 为使用 **CompositionLight** 对某个区域动态照明的 XAML 照明效果提供基类。

有关使用光（包括对 XAML UI 元素进行照明）的详细信息，请参阅[**照明**](xaml-lighting.md)部分。

有关代码示例，请参阅 [**XamlLight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.xamllight) 的参考页面。

## <a name="the-elementcompositionpreview-class"></a>ElementCompositionPreview 类

[**ElementCompositionPreview**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.aspx) 是一个静态类，可提供 XAML 和可视化层互操作功能。 有关可视化层及其功能的概述，请参阅[可视化层](https://msdn.microsoft.com/windows/uwp/graphics/visual-layer)。 **ElementCompositionPreview** 类提供以下方法：

-   [**GetElementVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx)：获取用于呈现此元素的“handout”视觉对象
-   [**SetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.setelementchildvisual.aspx)：将“handin”视觉对象设置为此元素的可视化树的最后一个子项。 此视觉对象将在元素其余部分上进行绘制。 
-   [**GetElementChildVisual**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx)：检索使用 **SetElementChildVisual** 设置的视觉对象
-   [**GetScrollViewerManipulationPropertySet**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual.aspx)：获取可用于基于 **ScrollViewer** 中的滚动偏移创建 60fps 动画的对象。

## <a name="remarks-on-elementcompositionpreviewgetelementvisual"></a>关于 ElementCompositionPreview.GetElementVisual 的备注

**ElementCompositionPreview.GetElementVisual** 返回用于呈现给定 **UIElement** 的“handout”视觉对象。 诸如 **Visual.Opacity**、**Visual.Offset** 和 **Visual.Size** 之类的属性由 XAML 框架基于 UIElement 的状态设置。 这将支持隐式重新定位动画等技术（请参阅*秘诀*）。

请注意，由于**偏移**和**大小**根据 XAML 框架布局设置，因此开发人员应在修改或对这些属性进行动画处理时小心。 开发人员应仅在元素的左上角在布局中与其父项的左上角位置相同时修改偏移或对其进行动画处理。 通常不应该修改大小，但访问属性可能很有用。 例如，下面的投影和毛玻璃示例使用 handout 视觉对象的大小作为对动画的输入。

作为附加警告，handout 视觉对象的更新属性将不会反映在相应的 UIElement 中。 因此，例如，将 **UIElement.Opacity** 设置为 0.5 会将相应的 handout 视觉对象的不透明度设置为 0.5。 但是，将 handout 视觉对象的 Visual 的**不透明度**设置为 0.5 会导致内容以 50% 的不透明度显示，但不会更改相应 UIElement 的 Opacity 属性的值。

### <a name="example-of-offset-animation"></a>**Offset** 动画的示例

#### <a name="incorrect"></a>错误

```xaml
<Border>
      <Image x:Name="MyImage" Margin="5" />
</Border>
```

```csharp
// Doesn’t work because Image has a margin!
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

#### <a name="correct"></a>正确

```xaml
<Border>
    <Canvas Margin="5">
        <Image x:Name="MyImage" />
    </Canvas>
</Border>
```

```csharp
// This works because the Canvas parent doesn’t generate a layout offset.
ElementCompositionPreview.GetElementVisual(MyImage).StartAnimation("Offset", parallaxAnimation);
```

## <a name="the-elementcompositionpreviewsetelementchildvisual-method"></a>**ElementCompositionPreview.SetElementChildVisual** 方法

**ElementCompositionPreview.SetElementChildVisual** 允许开发人员提供将显示为元素的可视化树一部分的“handin”视觉对象。 这允许开发人员创建一个“复合岛”，在其内，基于视觉对象的内容可以显示在 XAML UI 内。 开发人员应谨慎使用此技术，因为基于视觉对象的内容将不具有与 XAML 内容相同的辅助功能和用户体验保证。 因此，通常建议，仅在有必要实现自定义效果（如下面的秘诀部分中所找到的效果）时使用此技术。

## <a name="getalphamask-methods"></a>**GetAlphaMask** 方法

[**Image**](https://msdn.microsoft.com/library/windows/apps/br242752)、[**TextBlock**](https://msdn.microsoft.com/library/windows/apps/br209652) 和 [**Shape**](/uwp/api/Windows.UI.Xaml.Shapes.Shape) 各自实现一个称为 **GetAlphaMask** 的方法，该方法返回一个 **CompositionBrush**，用于表示带有元素形状的灰度图像。 此 **CompositionBrush** 可充当复合 **DropShadow** 的输入，因此阴影可以反映元素的形状，而不是矩形。 这将为文本、带有 alpha 的图像和形状启用像素完美、基于轮廓的阴影。 有关此 API 的示例，请参阅下面的*投影*。

## <a name="recipes"></a>秘诀

### <a name="reposition-animation"></a>重新定位动画

使用复合隐式动画，开发人员可以在元素的布局中对相对于其父项的更改自动进行动画处理。 例如，如果你更改下面的按钮的**边距**，它将自动动画处理到它的新布局位置。

#### <a name="implementation-overview"></a>实现概述

1. 获取目标元素的 handout **视觉对象**
1. 创建可对 **Offset** 属性中的更改自动进行动画处理的 **ImplicitAnimationCollection**
1. 将 **ImplicitAnimationCollection** 与后备视觉对象关联

```xaml
<Button x:Name="RepositionTarget" Content="Click Me" />
```

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeRepositionAnimation(RepositionTarget);
}

private void InitializeRepositionAnimation(UIElement repositionTarget)
{
    var targetVisual = ElementCompositionPreview.GetElementVisual(repositionTarget);
    Compositor compositor = targetVisual.Compositor;

    // Create an animation to animate targetVisual's Offset property to its final value
    var repositionAnimation = compositor.CreateVector3KeyFrameAnimation();
    repositionAnimation.Duration = TimeSpan.FromSeconds(0.66);
    repositionAnimation.Target = "Offset";
    repositionAnimation.InsertExpressionKeyFrame(1.0f, "this.FinalValue");

    // Run this animation when the Offset Property is changed
    var repositionAnimations = compositor.CreateImplicitAnimationCollection();
    repositionAnimations["Offset"] = repositionAnimation;

    targetVisual.ImplicitAnimations = repositionAnimations;
}
```

### <a name="drop-shadow"></a>投影

将像素完美投影应用到 **UIElement**，例如包含图片的**椭圆**。 由于阴影需要由应用创建的 **SpriteVisual**，因此我们需要使用 **ElementCompositionPreview.SetElementChildVisual** 创建一个“主机”元素，该元素将包含 **SpriteVisual**。

#### <a name="implementation-overview"></a>实现概述

1. 获取主机元素的 handout **视觉对象**
2. 创建一个 Windows.UI.Composition **DropShadow**
3. 配置 **DropShadow** 以通过蒙板从目标元素中获取其形状
    - **DropShadow** 默认为矩形，因此如果目标是矩形，则这不是必需的
4. 将阴影附加到新的 **SpriteVisual**，并将该 **SpriteVisual** 设置为主机元素的子元素
5. 使用 **ExpressionAnimation** 将 **SpriteVisual** 的大小绑定到主机的大小

```xaml
<Grid Width="200" Height="200">
    <Canvas x:Name="ShadowHost" />
    <Ellipse x:Name="CircleImage">
        <Ellipse.Fill>
            <ImageBrush ImageSource="Assets/Images/2.jpg" Stretch="UniformToFill" />
        </Ellipse.Fill>
    </Ellipse>
</Grid>
```

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost, CircleImage);
}

private void InitializeDropShadow(UIElement shadowHost, Shape shadowTarget)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(shadowHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a drop shadow
    var dropShadow = compositor.CreateDropShadow();
    dropShadow.Color = Color.FromArgb(255, 75, 75, 80);
    dropShadow.BlurRadius = 15.0f;
    dropShadow.Offset = new Vector3(2.5f, 2.5f, 0.0f);
    // Associate the shape of the shadow with the shape of the target element
    dropShadow.Mask = shadowTarget.GetAlphaMask();

    // Create a Visual to hold the shadow
    var shadowVisual = compositor.CreateSpriteVisual();
    shadowVisual.Shadow = dropShadow;

    // Add the shadow as a child of the host in the visual tree
   ElementCompositionPreview.SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    shadowVisual.StartAnimation("Size", bindSizeAnimation);
}
```

下面两个列表显示了采用相同 XAML 结构的旧版 C&#35; 代码的 [C++/WinRT](https://aka.ms/cppwinrt) 和 [C++/CX](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx) 等效内容。

```cppwinrt
#include <winrt/Windows.UI.Composition.h>
#include <winrt/Windows.UI.Xaml.h>
#include <winrt/Windows.UI.Xaml.Hosting.h>
#include <winrt/Windows.UI.Xaml.Shapes.h>
...
MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost(), CircleImage());
}

int32_t MyProperty();
void MyProperty(int32_t value);

void InitializeDropShadow(Windows::UI::Xaml::UIElement const& shadowHost, Windows::UI::Xaml::Shapes::Shape const& shadowTarget)
{
    auto hostVisual{ Windows::UI::Xaml::Hosting::ElementCompositionPreview::GetElementVisual(shadowHost) };
    auto compositor{ hostVisual.Compositor() };

    // Create a drop shadow
    auto dropShadow{ compositor.CreateDropShadow() };
    dropShadow.Color(Windows::UI::ColorHelper::FromArgb(255, 75, 75, 80));
    dropShadow.BlurRadius(15.0f);
    dropShadow.Offset(Windows::Foundation::Numerics::float3{ 2.5f, 2.5f, 0.0f });
    // Associate the shape of the shadow with the shape of the target element
    dropShadow.Mask(shadowTarget.GetAlphaMask());

    // Create a Visual to hold the shadow
    auto shadowVisual = compositor.CreateSpriteVisual();
    shadowVisual.Shadow(dropShadow);

    // Add the shadow as a child of the host in the visual tree
    Windows::UI::Xaml::Hosting::ElementCompositionPreview::SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    auto bindSizeAnimation{ compositor.CreateExpressionAnimation(L"hostVisual.Size") };
    bindSizeAnimation.SetReferenceParameter(L"hostVisual", hostVisual);

    shadowVisual.StartAnimation(L"Size", bindSizeAnimation);
}
```

```cpp
#include "WindowsNumerics.h"

MainPage::MainPage()
{
    InitializeComponent();
    InitializeDropShadow(ShadowHost, CircleImage);
}

void MainPage::InitializeDropShadow(Windows::UI::Xaml::UIElement^ shadowHost, Windows::UI::Xaml::Shapes::Shape^ shadowTarget)
{
    auto hostVisual = Windows::UI::Xaml::Hosting::ElementCompositionPreview::GetElementVisual(shadowHost);
    auto compositor = hostVisual->Compositor;

    // Create a drop shadow
    auto dropShadow = compositor->CreateDropShadow();
    dropShadow->Color = Windows::UI::ColorHelper::FromArgb(255, 75, 75, 80);
    dropShadow->BlurRadius = 15.0f;
    dropShadow->Offset = Windows::Foundation::Numerics::float3(2.5f, 2.5f, 0.0f);
    // Associate the shape of the shadow with the shape of the target element
    dropShadow->Mask = shadowTarget->GetAlphaMask();

    // Create a Visual to hold the shadow
    auto shadowVisual = compositor->CreateSpriteVisual();
    shadowVisual->Shadow = dropShadow;

    // Add the shadow as a child of the host in the visual tree
    Windows::UI::Xaml::Hosting::ElementCompositionPreview::SetElementChildVisual(shadowHost, shadowVisual);

    // Make sure size of shadow host and shadow visual always stay in sync
    auto bindSizeAnimation = compositor->CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation->SetReferenceParameter("hostVisual", hostVisual);

    shadowVisual->StartAnimation("Size", bindSizeAnimation);
}
```

### <a name="frosted-glass"></a>毛玻璃

创建使背景内容模糊和带色彩的效果。 请注意，开发人员需要安装 Win2D NuGet 程序包才能使用效果。 有关安装说明，请参阅 [Win2D 主页](http://microsoft.github.io/Win2D/html/Introduction.htm)。

#### <a name="implementation-overview"></a>实现概述

1.  获取主机元素的 handout **视觉对象**
2.  使用 Win2D 和 **CompositionEffectSourceParameter** 创建模糊效果树
3.  基于效果树创建 **CompositionEffectBrush**
4.  将 **CompositionEffectBrush** 的输入设置为 **CompositionBackdropBrush**，从而允许将效果应用到 **SpriteVisual** 背后的内容
5.  将 **CompositionEffectBrush** 设置为新 **SpriteVisual** 的内容，并将该 **SpriteVisual** 设置为主机元素的子元素。 可以选择使用 XamlCompositionBrushBase。
6.  使用 **ExpressionAnimation** 将 **SpriteVisual** 的大小绑定到主机的大小

```xaml
<Grid Width="300" Height="300" Grid.Column="1">
    <Image
        Source="Assets/Images/2.jpg"
        Width="200"
        Height="200" />
    <Canvas
        x:Name="GlassHost"
        Width="150"
        Height="300"
        HorizontalAlignment="Right" />
</Grid>
```

```csharp
public MainPage()
{
    InitializeComponent();
    InitializeFrostedGlass(GlassHost);
}

private void InitializeFrostedGlass(UIElement glassHost)
{
    Visual hostVisual = ElementCompositionPreview.GetElementVisual(glassHost);
    Compositor compositor = hostVisual.Compositor;

    // Create a glass effect, requires Win2D NuGet package
    var glassEffect = new GaussianBlurEffect
    { 
        BlurAmount = 15.0f,
        BorderMode = EffectBorderMode.Hard,
        Source = new ArithmeticCompositeEffect
        {
            MultiplyAmount = 0,
            Source1Amount = 0.5f,
            Source2Amount = 0.5f,
            Source1 = new CompositionEffectSourceParameter("backdropBrush"),
            Source2 = new ColorSourceEffect
            {
                Color = Color.FromArgb(255, 245, 245, 245)
            }
        }
    };

    //  Create an instance of the effect and set its source to a CompositionBackdropBrush
    var effectFactory = compositor.CreateEffectFactory(glassEffect);
    var backdropBrush = compositor.CreateBackdropBrush();
    var effectBrush = effectFactory.CreateBrush();

    effectBrush.SetSourceParameter("backdropBrush", backdropBrush);

    // Create a Visual to contain the frosted glass effect
    var glassVisual = compositor.CreateSpriteVisual();
    glassVisual.Brush = effectBrush;

    // Add the blur as a child of the host in the visual tree
    ElementCompositionPreview.SetElementChildVisual(glassHost, glassVisual);

    // Make sure size of glass host and glass visual always stay in sync
    var bindSizeAnimation = compositor.CreateExpressionAnimation("hostVisual.Size");
    bindSizeAnimation.SetReferenceParameter("hostVisual", hostVisual);

    glassVisual.StartAnimation("Size", bindSizeAnimation);
}
```

## <a name="additional-resources"></a>其他资源

- [可视化层概述](https://msdn.microsoft.com/windows/uwp/composition/visual-layer)
- [**ElementCompositionPreview** 类](https://msdn.microsoft.com/library/windows/apps/mt608976)
- [WindowsUIDevLabs GitHub](https://github.com/microsoft/windowsuidevlabs) 中的高级 UI 和复合示例
- [BasicXamlInterop 示例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/BasicXamlInterop)
- [ParallaxingListItems 示例](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2010586/ParallaxingListItems)
