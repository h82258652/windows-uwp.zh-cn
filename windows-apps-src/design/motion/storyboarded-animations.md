---
ms.assetid: 0CBCEEA0-2B0E-44A1-A09A-F7A939632F3A
title: 情节提要动画
description: 情节提要动画不仅仅是视觉动画。
ms.date: 07/13/2018
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 212ef252e7d123ebf457a6584f77addb04fdfb2c
ms.sourcegitcommit: a5f5bd724e65ce4a62d28dfd9080afb127886d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/06/2019
ms.locfileid: "9059988"
---
# <a name="storyboarded-animations"></a>情节提要动画

情节提要动画不仅仅是视觉动画。 情节提要动画是更改作为时间函数的依赖属性的值的一种方法。 可能需要该动画库外部的情节提要动画的主要原因之一在于，需要定义控件的视觉状态并将其作为控件模板或页面定义的一部分。

## <a name="differences-with-silverlight-and-wpf"></a>与 Silverlight 和 WPF 之间的区别

如果熟悉 Microsoft Silverlight 或 Windows Presentation Foundation (WPF)，请阅读本节内容；否则，可以跳过本节。

一般来说，在 Windows 运行时应用中创建情节提要动画与使用 Silverlight 或 WPF 创建动画类似。 但是也有一些重要的差异：

-   情节提要动画不是为 UI 创建视觉动画的唯一方法，也不一定是应用开发人员这样做的最简便方法。 不使用情节提要动画，它通常是使用主题动画和过渡动画的更好的设计做法。 这可以快速创建推荐的 UI 动画，而无需使用复杂的动画属性目标定位。 有关详细信息，请参阅[动画概述](xaml-animation.md)。
-   在 Windows 运行时中，许多 XAML 控件包括主题动画和过渡动画，作为其内置行为的一部分。 大多数情况下，WPF 和 Silverlight 控件没有默认的动画行为。
-   如果动画系统确定自定义动画可能会在 UI 中引发不良性能，则并非所有你创建的自定义动画可以在 Windows 运行时应用中默认运行。 系统确定可能会产生性能影响的动画称为*从属动画*。 此动画是从属动画的原因是：动画的时钟直接根据 UI 线程运行，同时它也是活动用户输入和其他更新尝试将运行时更改应用到 UI 的位置。 在某些情况下，占用 UI 线程上大量系统资源的从属动画使应用看似无响应。 如果你的动画导致布局更改或可能影响 UI 线程的性能，则通常需要显式启用动画以观察其运行情况。 这种情况下，特定动画类的 **EnableDependentAnimation** 属性就可以派上用场了。 有关详细信息，请参阅[从属动画和独立动画](./storyboarded-animations.md#dependent-and-independent-animations)。
-   目前 Windows 运行时中不支持自定义缓动函数。

## <a name="defining-storyboarded-animations"></a>定义情节提要动画

情节提要动画是更改作为时间函数的依赖属性的值的一种方法。 创建动画的属性并不始终是直接影响你的应用的 UI 的属性。 但由于 XAML 可以定义应用的 UI，因此通常它是你要创建动画的 UI 相关的属性。 例如，你可以创建 [**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) 的角度或按钮的背景颜色值的动画。

你可能定义情节提要动画的一个主要原因是你是否是控件创作者或正在重新创建控件模板，并且正在定义视觉状态。 有关详细信息，请参阅[视觉状态的情节提要动画](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)。

无论你正在定义视觉状态还是应用的自定义动画，本主题中介绍的情节提要动画的概念和 API 基本上都适用于二者任一。

为了创建动画，通过情节提要动画确定目标的属性必须是*依赖属性*。 依赖属性是 Windows 运行时 XAML 实现的关键特征。 最常见 UI 元素的可写属性通常作为依赖属性实现，以便你可以为其创建动画、应用数据绑定值或应用 [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849) 和确定具有 [**Setter**](https://msdn.microsoft.com/library/windows/apps/BR208817) 的属性为目标。 有关依赖属性工作原理的详细信息，请参阅[依赖属性概述](https://msdn.microsoft.com/library/windows/apps/Mt185583)。

大多数情况下，通过编写 XAML 来定义情节提要动画。 如果你使用了工具，例如 Microsoft Visual Studio，则该工具将会为你生成 XAML。 也可以使用代码定义情节提要动画，但这种方法并不常见。

让我们来看个简单示例。 在此 XAML 示例中，在特定的 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 对象上为 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 属性创建动画。

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>

  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

### <a name="identifying-the-object-to-animate"></a>确定要实现动画的对象

在前面的示例中，情节提要为 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 的 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 属性创建了动画。 你没有在对象本身上声明动画。 而是在情节提要的动画定义内声明了动画。 情节提要通常在 XAML 中定义，不在要创建动画的对象的 XAML UI 定义的紧邻位置。 而它们通常会设置为 XAML 资源。

若要将动画连接到目标，则按其标识的编程名称来引用目标。 你应始终在 XAML UI 定义中应用 [x:Name 属性](https://msdn.microsoft.com/library/windows/apps/Mt204788)以命名要创建动画的对象。 然后，通过在动画定义中设置 [**Storyboard.TargetName**](https://msdn.microsoft.com/library/windows/apps/Hh759823) 确定要创建动画的对象目标。 对于 **Storyboard.TargetName** 的值，使用目标对象的名称字符串，它是你之前设置的名称并在其他位置具有 x:Name 属性。

### <a name="targeting-the-dependency-property-to-animate"></a>确定要创建动画的依赖属性目标

在动画中为 [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824) 设置一个值。 这将确定要为哪个目标对象的特定属性创建动画。

有时，你需要确定不是目标对象的直接属性的属性目标，而它可能嵌套在对象属性关系的更深位置。 你通常需要执行此操作以便深入到构成对象和属性值的集中，直至你可以引用可创建动画的属性类型为止（[**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)、[**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870)、[**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723)）。 此概念称为*间接目标*，采用此方式确定属性目标的语法称为*属性路径*。

下面提供了一个示例。 情节提要动画的一个常见方案是更改部分应用 UI 或控件的颜色，以便表明控件处于特定状态。 说明你希望为 [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) 的 [**Foreground**](https://msdn.microsoft.com/library/windows/apps/BR209665) 创建动画，以便它从红色变为绿色。 你预期包括 [**ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066)，这非常正确。 但是，UI 元素上的任何影响对象颜色属性的类型实际上都不是 [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723)， 而是类型 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush)。 因此，你实际上要创建动画的目标是 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962) 类的 [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 属性，它通常是用于与颜色相关的这些 UI 属性的 **Brush** 派生的类型。 下面是形成动画的属性目标的属性路径的示例：

```xaml
<Storyboard x:Name="myStoryboard">
  <ColorAnimation
    Storyboard.TargetName="tb1"
    Storyboard.TargetProperty="(TextBlock.Foreground).(SolidColorBrush.Color)"
    From="Red" To="Green"/>
</Storyboard>
```

下面介绍如何根据其组成考虑此语法：

- 每组 () 括号包含一个属性名称。
- 在属性名称内，有一个点，该点分隔开类型名称和属性名称，以使你标识的属性清晰明确。
- 中间的点（即，不在括号内的点）是一个步骤。 这是按语法进行解释的，意思是，获取第一个属性（它是一个对象）的值，进入其对象模型，然后确定第一个属性值的特定子属性目标。

下面是动画目标方案的列表，你可能从中使用间接属性目标，以及与你将使用的语法类似的某些属性路径字符串：

- 当应用到 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) 时，创建 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 的 [**X**](https://msdn.microsoft.com/library/windows/apps/BR243029) 值的动画： `(UIElement.RenderTransform).(TranslateTransform.X)`
- 当应用到 [**Fill**](/uwp/api/Windows.UI.Xaml.Shapes.Shape.Fill) 时，在 [**LinearGradientBrush**](https://msdn.microsoft.com/library/windows/apps/BR210108) 的 [**GradientStop**](https://msdn.microsoft.com/library/windows/apps/BR210078) 中创建 [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 的动画： `(Shape.Fill).(GradientBrush.GradientStops)[0].(GradientStop.Color)`
- 当应用到 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) 时，创建 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 的 [**X**](https://msdn.microsoft.com/library/windows/apps/BR243029) 值的动画（它是 [**TransformGroup**](https://msdn.microsoft.com/library/windows/apps/BR243022) 中四种转换之一）：`(UIElement.RenderTransform).(TransformGroup.Children)[3].(TranslateTransform.X)`

你将发现某些示例使用方括号来包含数字。 这是一种索引器。 它指示它前面的属性名称具有用作值的一个集合，并且你需要该集合中的一项（按照从零开始的索引标识）。

你还可以为 XAML 附加的属性创建动画。 始终在括号内包含附加属性的全称，例如 `(Canvas.Left)`。 有关详细信息，请参阅[创建 XAML 附加属性的动画](./storyboarded-animations.md#animating-xaml-attached-properties)。

有关如何使用属性的间接目标的属性路径来实现动画的详细信息，请参阅 [Property-path 语法](https://msdn.microsoft.com/library/windows/apps/Mt185586)或 [**Storyboard.TargetProperty 附加属性**](https://msdn.microsoft.com/library/windows/apps/Hh759824)。

### <a name="animation-types"></a>动画类型

Windows 运行时动画系统具有情节提要动画可以应用于的三种特定类型：

-   [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)，可使用任意 [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) 为其创建动画
-   [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870)，可使用任意 [**PointAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210346) 为其创建动画
-   [**Color**](https://msdn.microsoft.com/library/windows/apps/Hh673723)，可使用任意 [**ColorAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243066) 为其创建动画

对于对象引用值，还存在通用化 [**Object**](https://msdn.microsoft.com/library/windows/apps/xaml/system.object.aspx) 动画类型，我们将在后面部分对此进行讨论。

### <a name="specifying-the-animated-values"></a>指定动画化的值

到目前为止，我们已经向你介绍了如何确定要创建动画的对象和属性目标，但尚未介绍动画运行时会对属性值执行的操作。

我们介绍的动画类型有时也称为 **From**/**To**/**By** 动画。 这表示，动画使用一个或多个来自动画定义的以下输入来随时间更改属性的值：

-   值从 **From** 值开始。 如果你没有指定 **From** 值，则起始值是动画运行前那个时刻动画化属性具有的任意值。 它可能是一个默认值，样式或模板中的值，或是 XAML UI 定义或应用代码特别应用的值。
-   在动画结尾处，值为 **To** 值。
-   或者，若要指定与起始值相对的结束值，请设置 **By** 属性。 最好是设置此值，而非 **To** 属性。
-   如果你没有指定 **To** 值或 **By** 值，则结束值是动画运行前那个时刻动画化属性具有的任意值。 在这种情况下，最好有 **From** 值，因为如果不指定该值，则动画将不会更改值；其起始值和结束值是同一个值。
-   动画通常具有至少其中一个 **From**、**By** 或 **To**，但绝不会同时有三个值。

让我们重新复习下以前的 XAML 示例，再次看看 **From** 和 **To** 值，以及 **Duration**。 本例创建 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 属性的动画，并且 **Opacity** 的属性类型为 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)。 因此，此处要使用的动画为 [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136)。

`From="1.0" To="0.0"` 指定动画何时运行，[**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 属性在值 1 处开始并且创建动画直至 0 处。 换句话说，根据这些 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx) 值对 **Opacity** 属性产生作用，此动画将对象从不透明开始，然后逐渐变成透明形式。

```xaml
...
<Storyboard x:Name="myStoryboard">
  <DoubleAnimation
    Storyboard.TargetName="MyAnimatedRectangle"
    Storyboard.TargetProperty="Opacity"
    From="1.0" To="0.0" Duration="0:0:1"/>
</Storyboard>
...
```

`Duration="0:0:1"` 指定动画持续的时间，即矩形淡化的速度。 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 属性采用以下形式指定：*hours*:*minutes*:*seconds*。 此示例中持续时间为一秒。

有关 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) 值和 XAML 语法的详细信息，请参阅 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)。

> [!NOTE]
> 对于我们展示的示例，如果你确定要创建动画对象的起始状态的 [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 始终等于 1（无论通过默认值还是显式设置），则可以忽略 **From** 值，动画可使用隐式起始值，并且结果相同。

### <a name="fromtoby-are-nullable"></a>From/To/By 可以为空

我们之前提到过，你可以忽略 **From**、**To** 或 **By**，并因此使用当前非动画化的值作为缺失值的替代值。 动画的 **From**、**To** 或 **By** 属性的类型不能猜测。 例如，[**DoubleAnimation.To**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.doubleanimation.easingfunction.aspx) 属性的类型不是 [**Double**](https://msdn.microsoft.com/library/windows/apps/xaml/system.double.aspx)。 而是 **Double** 的 [**Nullable**](https://msdn.microsoft.com/library/windows/apps/xaml/b3h38hb0.aspx)。 并且其默认值为 **null**，而不是 0。 该 **null** 值是动画系统区别你没有特别为 **From**、**To** 或 **By** 属性设置值的方式。 VisualC + + 组件扩展 (C + + CX) 不具有**Nullable**类型，因此它改为使用[**IReference**](https://msdn.microsoft.com/library/windows/apps/BR225864) 。

### <a name="other-properties-of-an-animation"></a>动画的其他属性

本节中接下来介绍的属性全部是可选的，因为它们具有适合于大部分动画的默认值。

### **<a name="autoreverse"></a>AutoReverse**

如果你没有在动画中指定 [**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202) 或 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211)，则该动画将运行一次，并且运行持续时间为 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 中指定的值。

[**AutoReverse**](https://msdn.microsoft.com/library/windows/apps/BR243202) 属性指定在时间线达到其 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 的结尾处后是否反向播放。 如果将其设置为 **true**，则动画在达到其声明的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 的结尾处后反向播放，从其结束值 (**To**) 起更改值直至返回其起始值 (**From**)。 这意味着，动画有效运行时间是其 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 的两倍。

### **<a name="repeatbehavior"></a>RepeatBehavior**

[**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) 属性指定时间线播放的次数，或是时间线应在其范围内重复的较长持续时间。 默认情况下，时间线具有“1x”的迭代计数，这表示它播放其 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 的一次倍数，并且不再重复。

你可能会使动画运行多个迭代。 例如，值“3x”将使动画运行三次。 或者，可以为 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) 指定不同的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377)。 该 **Duration** 应长于动画本身的 **Duration** 方才有效。 例如，对于具有“0:0:2”的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 的动画，如果你指定“0:0:10”的 **RepeatBehavior**，则该动画重复五次。 如果这些数不能被整除，动画将在到达 **RepeatBehavior** 时间时被截断，此时动画可能正在播放中。 最后，你可以指定特殊值“永远”，这将使动画无限期运行直至被故意停止。

有关 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411) 值和 XAML 语法的详细信息，请参阅 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR210411)。

### **<a name="fillbehaviorstop"></a>FillBehavior="Stop"**

默认情况下，当动画结束时，即使在超过其持续时间后，动画将属性值保留为最终 **To** 或 **By** 修改的值。 但是，如果你将 [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209) 属性的值设置为 [**FillBehavior.Stop**](https://msdn.microsoft.com/library/windows/apps/BR210306)，则动画化值的值将还原为应用动画前的任意值，或者更精确些还原为按照依赖属性系统（有关此区别的详细信息，请参阅[依赖属性概述](https://msdn.microsoft.com/library/windows/apps/Mt185583)）确定的当前有效值。

### **<a name="begintime"></a>BeginTime**

默认情况下，动画的 [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204) 为“0:0:0”，因此动画在其包含的 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 运行后立刻开始运行。 如果 **Storyboard** 包含多个动画并且你希望错开其他动画与初始动画的起始时间，或者希望有意创建短时延迟，则可以更改此值。

### **<a name="speedratio"></a>SpeedRatio**

如果在一个 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 中有多个动画，则可以更改一个或多个动画相对于 **Storyboard** 的时间比。 这是父 **Storyboard**，最终控制 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) 时间在动画运行期间的消逝方式。 此属性不常用。 有关详细信息，请参阅 [**SpeedRatio**](https://msdn.microsoft.com/library/windows/apps/BR243213)。

## <a name="defining-more-than-one-animation-in-a-storyboard"></a>在 **Storyboard** 中定义多个动画

[**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 的内容可以是多个动画定义。 如果将相关的动画应用到同一目标对象的两个属性，则可以具有多个动画。 例如，你可以同时更改用作 UI 元素的 [**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980) 的 [**TranslateTransform**](https://msdn.microsoft.com/library/windows/apps/BR243027) 的 [**TranslateX**](https://msdn.microsoft.com/library/windows/apps/BR228122) 和 [**TranslateY**](https://msdn.microsoft.com/library/windows/apps/BR228124) 属性；这将使元素沿对角线转换。 你需要两个不同的动画才能完成该操作，但你可能希望两个动画属于同一 **Storyboard**，因为你始终希望这两个动画同时运行。

动画无需是同一类型，或确定同一对象目标。 他们可以有不同的持续时间，并且无需共享任何属性值。

当父 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 运行时，其中的每个动画都将运行。

实际上，[**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 类具有许多与动画类型具有的相同的动画属性，因为二者共享 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 基类。 因此，**Storyboard** 可以具有 [**RepeatBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243211) 或 [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204)。 你通常不会在 **Storyboard** 上设置它们，但除非你希望所有包含的动画都具有该行为。 一般来说，与在 **Storyboard** 上设置一样，任何 **Timeline** 属性都会应用到其所有子动画。 如果未设置，则 **Storyboard** 具有通过包含的动画的最长 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR242377) 值计算而来的隐式持续时间。 如果 **Storyboard** 上显式设置的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 少于其子动画之一的持续时间，则将导致动画被截断，而这通常不是期望的效果。

情节提要无法包含尝试确定同一对象上的同一属性目标和创建其动画的两个动画。 如果你尝试这样做，则将在情节提要尝试运行时遇到运行时错误。 即使在动画在时间上没有重叠的情况下，此限制也适用，因为 [**BeginTime**](https://msdn.microsoft.com/library/windows/apps/BR243204) 值和持续时间明显不同。 如果你确实希望对一个情节提要中的同一属性应用更复杂的动画时间线，则实现方法为使用关键帧动画。 请参阅[关键帧和缓动函数动画](key-frame-and-easing-function-animations.md)。

如果这些输入来自多个情节提要，则动画系统可以将多个动画应用到一个属性的值中。 专门将此行为用于同时运行的情节提要并不常见。 但是，应用于控件属性的应用定义的动画可以修改之前作为该控件的视觉状态模型一部分运行的动画的 **HoldEnd** 值。

## <a name="defining-a-storyboard-as-a-resource"></a>将情节提要定义为资源

[**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 是放入动画对象的容器。 通常会在页面级别的 [**Resources**](https://msdn.microsoft.com/library/windows/apps/BR208740) 或 [**Application.Resources**](https://msdn.microsoft.com/library/windows/apps/BR242338) 中将 **Storyboard** 定义为要创建动画的对象可用的资源。

下一示例介绍如何将上一示例 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 包含在页面级别的 [**Resources**](https://msdn.microsoft.com/library/windows/apps/BR208740) 定义中，其中 **Storyboard** 为根 [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503) 的键控资源。 请注意 [x:Name 属性](https://msdn.microsoft.com/library/windows/apps/Mt204788)。 此属性是你定义 **Storyboard** 的变量名称的方式，以便以后 XAML 中的其他元素以及代码可以参考 **Storyboard**。

```xaml
<Page ...>
  <Page.Resources>
    <!-- Storyboard resource: Animates a rectangle's opacity. -->
    <Storyboard x:Name="myStoryboard">
      <DoubleAnimation
        Storyboard.TargetName="MyAnimatedRectangle"
        Storyboard.TargetProperty="Opacity"
        From="1.0" To="0.0" Duration="0:0:1"/>
    </Storyboard>
  </Page.Resources>
  <!--Page root element, UI definition-->
  <Grid>
    <Rectangle x:Name="MyAnimatedRectangle"
      Width="300" Height="200" Fill="Blue"/>
  </Grid>
</Page>
```

在 XAML 文件（例如 page.xaml 或 app.xaml）的 XAML 根上定义资源是在 XAML 中组织键控资源的常用做法。 还可以将资源分为单独的文件，然后将其合并到应用或页面中。 有关详细信息，请参阅 [ResourceDictionary 和 XAML 资源参考](https://msdn.microsoft.com/library/windows/apps/Mt187273)。

> [!NOTE]
> Windows 运行时 XAML 支持使用 [x:Key 属性](https://msdn.microsoft.com/library/windows/apps/Mt204787)或 [x:Name 属性](https://msdn.microsoft.com/library/windows/apps/Mt204788)标识资源。 使用 x:Name 属性对于 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 更常见，因为你最终会需要按变量名称引用它，以便你可以调用其 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 方法和运行动画。 如果你确实要使用 [x:Key 属性](https://msdn.microsoft.com/library/windows/apps/Mt204787)，则需要使用 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 方法（例如 [**Item**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.resourcedictionary.item) 索引器）来作为键控资源检索它，然后将检索的对象转换为 **Storyboard** 以使用 **Storyboard** 方法。

### <a name="storyboards-for-visual-states"></a>视觉状态的情节提要

在为控件的可视外观声明视觉状态动画时，你还可将动画放在 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 单元中。 在此情况下，你定义的 **Storyboard** 元素位于 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) 容器中，此容器嵌套在 [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849)（作为键控资源的 **Style**）中更深层的位置。 在此情况下，不需要对 **Storyboard** 使用键或名称，因为它是一种 **VisualState**，其中包含 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager) 可以调用的目标名称。 控件的样式通常分为单独的 XAML [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/BR208794) 文件，而不是放在特定页面或应用的 **Resources** 集合中。 有关详细信息，请参阅[视觉状态的情节提要动画](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)。

## <a name="dependent-and-independent-animations"></a>从属动画和独立动画

在此处，我们需要引入某些有关动画系统工作原理的重要知识点。 特别是，动画在基础方面与 Windows 运行时应用在屏幕上呈现的方式以及该呈现使用处理线程的方式的相互影响。 Windows 运行时应用始终具有一个主 UI 线程，并且此线程负责使用当前信息更新屏幕。 另外，Windows 运行时应用具有一个合成线程，该线程用于预先计算布局然后紧接着显示布局。 创建 UI 动画时可能会为 UI 线程带来大量的工作。 系统必须在两次刷新之间的相当短的时间间隔内重新绘制屏幕的大量区域。 对于捕捉动画化属性的最新属性值，这是必需的。 如果你不多加注意，则动画可能会使 UI 的响应速度下降，或者将影响也位于同一 UI 线程上的其他应用功能的性能。

确定具有降低 UI 线程性能的某些风险的各种动画被称为*从属动画*。 不受此风险限制的动画为*独立动画*。 从属动画和独立动画之间的区别并不仅仅由动画类型（[**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) 等）确定，如我们之前所述。 相反，它由创建动画的特定属性以及其他因素（例如，控件的继承和组合）确定。 在某些情况下，虽然动画确实更改了 UI，但该动画只对 UI 线程产生最小的影响，并且可以转而由合成线程作为独立动画进行处理。

如果某个动画具有以下特征的任意特征，则该动画为独立动画：

-   动画的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 为 0 秒（请参阅“警告”）
-   动画以 [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) 为目标
-   动画以下列 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) 属性的子属性值为目标：[**Transform3D**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.transform3d.aspx)、[**RenderTransform**](https://msdn.microsoft.com/library/windows/apps/BR208980)、[**Projection**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.projection.aspx)、[**Clip**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.clip)
-   动画以 [**Canvas.Left**](https://msdn.microsoft.com/library/windows/apps/Hh759771) 或 [**Canvas.Top**](https://msdn.microsoft.com/library/windows/apps/Hh759772) 为目标
-   动画确定 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 值为目标并使用 [**SolidColorBrush**](https://msdn.microsoft.com/library/windows/apps/BR242962)，为其 [**Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color) 创建动画
-   动画以 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) 为目标

> [!WARNING]
> 要让动画被视为独立动画，必须显式设置 `Duration="0"`。 例如，如果从此 XAML 中删除 `Duration="0"`，动画将被视为从属动画，即使框架的 [**KeyTime**](https://msdn.microsoft.com/library/windows/apps/BR243169) 是“0:0:0”。

```xaml
<Storyboard>
  <DoubleAnimationUsingKeyFrames
    Duration="0"
    Storyboard.TargetName="Button2"
    Storyboard.TargetProperty="Width">
    <DiscreteDoubleKeyFrame KeyTime="0:0:0" Value="200"/>
  </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

如果你的动画不符合这些条件，则它可能是从属动画。 默认情况下，动画系统不运行从属动画。 因此在开发和测试过程中，你甚至不会看到动画运行。 你仍可以使用此动画，但必须专门启用每个此类从属动画。 若要启用你的动画，请将动画对象的 **EnableDependentAnimation** 设置为 **true**。 （代表动画的每个 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 子类都具有属性的不同实现方式，但它们全部称为 `EnableDependentAnimation`。）

对于启用从属动画，应用开发人员要承担的要求为，动画系统的设计意识以及开发经验。 我们希望开发人员要了解动画对 UI 的响应性方面的性能成本。 性能较低的动画在全面的应用中很难隔离和调试。 因此，最好仅打开你确实要用于应用的 UI 体验的从属动画。 我们不希望由于使用大量循环的装饰性动画而导致容易损失应用的性能。 有关动画的性能使用技巧的详细信息，请参阅[优化动画和媒体](https://msdn.microsoft.com/library/windows/apps/Mt204774)。

作为一名应用开发人员，你还可以选择应用应用级设置，该设置始终禁用从属动画，甚至禁用其中 **EnableDependentAnimation** 为 **true** 的那些动画。 请参阅 [**Timeline.AllowDependentAnimations**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.allowdependentanimations)。

> [!TIP]
> 如果你使用动画窗格在 Blend for Visual Studio 2017，你尝试将从属动画应用到视觉状态属性时，将在设计器中显示警告。 错误列表的生成输出中，将不显示警告。 如果你手动编辑 XAML，设计器不会显示一条警告。 在运行时在调试时，输出窗格的调试输出将显示一条警告的动画不独立，将跳过。


## <a name="starting-and-controlling-an-animation"></a>启动动画和控制动画

到目前为止，我们向你展示的所有内容实际上都不会导致动画运行或应用！ 在动画启动和运行前，动画在 XAML 中声明的值更改都是潜在的并且尚未发生。 你必须采用某些与应用生存期或用户体验相关的方法来显式启动动画。 最简单的方法是，你可以通过在作为该动画的父动画的 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 上调用 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 方法来启动一个动画。 你不能直接通过 XAML 调用方法，因此无论你采用哪种方法启动动画，你都将通过代码来完成。 它将是页面的代码隐藏或是应用的组件，或许是控件的逻辑（如果你定义自定义控件类）。

通常来说，你将调用 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 并仅仅让动画运行直至其持续时间结束。 但是，你也可以使用 [**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx)、[**Resume**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.resume.aspx) 和 [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop) 方法在运行时控制 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)，以及用于更先进的动画控制方案的其他 API。

当你在包含无限重复 (`RepeatBehavior="Forever"`) 的动画的情节提要上调用 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 时，该动画将一直运行，除非停止加载包含该动画的页面或者专门调用 [**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx) 或 [**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop)。

### <a name="starting-an-animation-from-app-code"></a>通过应用代码启动动画

你可以自动启动动画，也可以通过响应用户操作来启动动画。 对于自动方案，通常使用对象生存期事件（例如 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723)）来用作动画触发器。 **Loaded** 事件是用于此方法的适合事件，因为此时 UI 已准备好交互，并且由于另一部分的 UI 仍处于加载状态因此动画的开始部分不会被截断。

在此示例中，[**PointerPressed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed) 事件附加到矩形，因此当用户单击矩形时，动画即开始。

```xaml
<Rectangle PointerPressed="Rectangle_Tapped"
  x:Name="MyAnimatedRectangle"
  Width="300" Height="200" Fill="Blue"/>
```

事件处理程序通过使用 **Storyboard** 的 [**Begin**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.begin) 方法来启动 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490)（动画）。

```csharp
myStoryboard.Begin();
```

```cppwinrt
myStoryboard().Begin();
```

```cpp
myStoryboard->Begin();
```

```vb
myStoryBoard.Begin()
```

如果你希望在动画应用完值之后运行其他逻辑，你可以处理 [**Completed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.timeline.completed.aspx) 事件。 同样，要对属性系统/动画交互进行故障排除，[**GetAnimationBaseValue**](https://msdn.microsoft.com/library/windows/apps/BR242358) 方法可能十分有用。

> [!TIP]
> 在你为应用方案编写代码（其中会从应用代码启动动画）时，你可能希望再次查看动画或转换是否已存在于 UI 方案的动画库中。 库动画使得所有 Windows 运行时应用间的 UI 体验更加一致，并且更易于使用。

 

### <a name="animations-for-visual-states"></a>视觉状态的动画

用于定义控件的视觉状态的 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 的运行行为不同于应用直接运行情节提要的方式。 当应用到 XAML 中的视觉状态时，**Storyboard** 是包含的 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/BR209007) 的元素，并且可以通过使用 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager) API 将该状态作为整体进行控制。 当控件使用包含的 **VisualState** 时，其中的任何动画都将根据其动画值和 [**Timeline**](https://msdn.microsoft.com/library/windows/apps/BR210517) 属性运行。 有关详细信息，请参阅[视觉状态的情节提要](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)。 对于视觉状态，显示的 [**FillBehavior**](https://msdn.microsoft.com/library/windows/apps/BR243209) 是不同的。 如果视觉状态更改为另一个状态，则之前视觉状态及其动画应用的所有属性更改都将被取消，即使在新的视觉状态没有专门将新的动画应用到属性的情况下也是如此。

### <a name="storyboard-and-eventtrigger"></a>**Storyboard** 和 **EventTrigger**

存在启动动画的一种方法，该方法可以在 XAML 中完全声明。 但是，此技术目前不再被广泛使用。 它是来自 WPF 和早期版本的 Silverlight（在 [**VisualStateManager**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visualstatemanager) 支持之前）中的旧语法。 鉴于导入/兼容性原因，此 [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390) 语法在 Windows 运行时 XAML 中仍然有效，但仅用于基于 [**FrameworkElement.Loaded**](https://msdn.microsoft.com/library/windows/apps/BR208723) 事件的触发行为；尝试触发其他事件将引发异常或无法编译。 有关详细信息，请参阅 [**EventTrigger**](https://msdn.microsoft.com/library/windows/apps/BR242390) 或 [**BeginStoryboard**](https://msdn.microsoft.com/library/windows/apps/BR243053)。

## <a name="animating-xaml-attached-properties"></a>创建 XAML 附加属性的动画

这不是一个常见情形，但你可以将动画值应用到 XAML 附加属性。 有关哪些附加属性及其工作原理的详细信息，请参阅[附加属性概述](https://msdn.microsoft.com/library/windows/apps/Mt185579)。 确定附加属性目标需要括号中包含属性名的 [Property-path语法](https://msdn.microsoft.com/library/windows/apps/Mt185586)。 你可以通过使用应用不连续整数值的 [**ObjectAnimationUsingKeyFrames**](https://msdn.microsoft.com/library/windows/apps/BR210320) 创建内置附加属性的动画，如 [**Canvas.ZIndex**](https://msdn.microsoft.com/library/windows/apps/Hh759773)。 不过，Windows 运行时 XAML 实现的现有局限性是无法创建自定义附加属性的动画。

## <a name="more-animation-types-and-next-steps-for-learning-about-animating-your-ui"></a>更多动画类型，以及了解有关创建 UI 动画的后续步骤

到目前为止，我们已经介绍了在两个值之间创建动画的自定义动画，然后在动画运行期间根据需要采用线性方式插入值。 它们被称为 **From**/**To**/**By** 动画。 但还有另一种动画类型可以帮助你声明介于起始和结束之间的中间值。 它们被称为*关键帧动画*。 还有一种方法可以在 **From**/**To**/**By** 动画或关键帧动画上改变插入逻辑。 这将涉及缓动函数的应用。 有关这些概念的详细信息，请参阅[关键帧和缓动函数动画](key-frame-and-easing-function-animations.md)。

## <a name="related-topics"></a>相关主题

* [Property-path 语法](https://msdn.microsoft.com/library/windows/apps/Mt185586)
* [依赖属性概述](https://msdn.microsoft.com/library/windows/apps/Mt185583)
* [关键帧以及缓动函数动画](key-frame-and-easing-function-animations.md)
* [视觉状态的情节提要动画](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)
* [控件模板](https://msdn.microsoft.com/library/windows/apps/Mt210948)
* [**情节提要**](https://msdn.microsoft.com/library/windows/apps/BR210490)
* [**Storyboard.TargetProperty**](https://msdn.microsoft.com/library/windows/apps/Hh759824)
 

 




