---
author: jwmsft
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
title: "合成动画"
description: "许多合成对象和效果属性均可使用关键帧和表达式动画设置动画，以便 UI 元素的属性可以随时间变化或基于计算发生变化。"
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: e7d0f5c3fc0d1414dc1b4f714683494fcffd4f51
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2017
---
# <a name="composition-animations"></a>合成动画

Windows.UI.Composition API 允许你在统一的 API 层中创建、设置动画、转换和操作合成器对象。 合成动画提供一种在应用程序 UI 中运行动画的强大且有效的方法。 它们从一开始就设计为确保你的动画可以独立于 UI 线程并以 60 FPS 的频率运行，并让你不仅可以使用时间，而且还可以使用输入和其他属性来灵活生成令人惊叹的体验以驱动动画。

本文概述了允许你设置合成对象属性的动画的可用功能。 我们假设你熟悉可视化层结构的基础知识。 有关详细信息，请参阅[合成视觉对象](./composition-visual-tree.md)。

合成动画有两种类型：**关键帧动画**和**表达式动画**。

![动画类型](./images/composition-animation-types.png)

## <a name="types-of-composition-animations"></a>合成动画类型

**关键帧动画**提供传统的时间驱动、*逐帧*动画体验。 你可以显式定义描述动画属性在动画时间线中的特定点处需要具有的值的*控制点*。 更重要的是，你能够使用缓动函数（或称为插入程序）来介绍如何在这些控制点之间过渡。

**隐式动画**是一种动画类型，让你可以定义可重用的单个动画，或者定义独立于核心应用逻辑的一系列动画。 隐式动画让你可以创建动画*模板*，并将它们与触发器连接。 这些触发器是由显式分配引起的属性更改。 你可以将模板定义为单个动画或动画组。 动画组是可通过显式方式或者使用触发器一起启动的动画模板集合。 借助隐式动画，用户可以在每次希望更新属性值和查看其是否设置动画时，无需创建显式 KeyFrameAnimations。

**表达式动画**是在 Windows 10 11 月更新（版本 10586）的可视化层中引入的动画类型。
表达式动画背后的理念是，你可以在[可视化](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx)属性与将在每个帧中进行评估和更新的离散值之间创建数学关系。 你可以在合成对象或属性集中引用属性、使用数学函数帮助程序，甚至引用输入以派生这些数学关系。 表达式使 Windows 平台支持视差和粘滞标头等体验并使之流畅展开。

## <a name="why-composition-animations"></a>为什么使用合成动画？

**性能** 在生成通用 Windows 应用程序时，大多数代码在 UI 线程上运行。
为了确保动画可以在不同设备类别上流畅运行，系统会执行动画计算，并处理独立线程以维持 60 FPS 的频率。
这意味着，你可以依靠系统提供流畅的动画，同时应用程序可以执行高级用户体验的其他复杂操作。

**可能性** 可视化层中的合成动画的目标是方便创建优美的 UI。 我们希望为你提供不同类型的动画，以便你提供出色的想法。

**模板化** 可视层化中的所有合成动画都是模板，这意味着你可以在多个对象上使用动画，而无需创建单独的动画。
这允许你使用相同的动画，并调整属性或参数以满足某些其他需求，无需担心阻碍以前的动画使用。

可以针对[表达式动画](https://channel9.msdn.com/events/Build/2016/P486)、[交互式体验](https://channel9.msdn.com/Events/Build/2016/P405)、[隐式动画](https://channel9.msdn.com/events/Build/2016/P484)和[连接的动画](https://channel9.msdn.com/events/Build/2016/P485)查看我们的 //BUILD 会话，了解一些示例是可能的。

还可以查看 [Composition GitHub](http://go.microsoft.com/fwlink/?LinkID=789439)，以获取有关如何使用 API 的示例和操作中 API 的高保真度示例。

## <a name="what-can-you-animate-with-composition-animations"></a>使用合成动画可以设置什么内容的动画？

合成动画可应用于合成对象的大多数属性，例如 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 和 **InsetClip**。
还可以将合成动画应用到合成效果和属性集。 **在选择设置动画的内容时，请记下类型，这可用于确定所构造的关键帧动画的类型或者是表达式动画必须解析为的类型。**

### <a name="visual"></a>可视

|可进行动画设置的视觉属性|类型|
|------|------|
|AnchorPoint|Vector2|
|CenterPoint|Vector3|
|Offset|Vector3|
|Opacity|Scalar|
|Orientation|Quaternion|
|RotationAngle|Scalar|
|RotationAngleInDegrees|Scalar|
|RotationAxis|Vector3|
|Scale|Vector3|
|Size|Vector2|
|TransformMatrix*|Matrix4x4|
*如果你想要将整个 TransformMatrix 属性作为 Matrix4x4 进行动画设置，需要使用表达式动画才能达到此目的。
否则，你可以定向矩阵的个别单元，并在该处使用关键帧或表达式动画。

### <a name="insetclip"></a>InsetClip

|可进行动画设置的 InsetClip 属性|类型|
|-------------------------------|-------|
|BottomInset|Scalar|
|LeftInset|Scalar|
|RightInset|Scalar|
|TopInset|Scalar|

## <a name="visual-sub-channel-properties"></a>可视化子通道属性

除了能够设置 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 属性的动画外，你还可以针对动画定向这些属性的*子通道*组件。
例如，假设你只想设置 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 X Offset 的动画，而非整个 Offset。
动画可以定向 Vector3 Offset 属性，或 Offset 属性的 Scalar X 组件。
除了能够定向属性的单个子通道组件，还可以定向多个组件。
例如，可以定向 Scale 的 X 和 Y 组件。

|可进行动画处理的可视化子通道属性|类型|
|----------------------------------------|------|
|AnchorPoint.x、y|Scalar|
|AnchorPoint.xy|Vector2|
|CenterPoint.x、y、z|Scalar|
|CenterPoint.xy、xz、yz|Vector2|
|Offset.x、y、z|Scalar|
|Offset.xy、xz、yz|Vector2|
|RotationAxis.x、y、z|Scalar|
|RotationAxis.xy、xz、yz|Vector2|
|Scale.x、y、z|Scalar|
|Scale.xy、xz、yz|Vector2|
|Size.x、y|Scalar|
|Size.xy|Vector2|
|TransformMatrix._11 ... TransformMatrix._NN|Scalar|
|TransformMatrix._11_12 ... TransformMatrix._NN_NN|Vector2|
|TransformMatrix._11_12_13 ... TransformMatrix._NN_NN_NN|Vector3|
|TransformMatrix._11_12_13_14|Vector4|
|Color*|Colors (Windows.UI)|

*设置 Brush 属性的 Color 子通道的动画会稍有不同。 需要将 StartAnimation() 附加到 Visual.Brush，并声明该属性以在参数中动画设置为“Color”。
（有关设置颜色动画的更多详细信息将在后面部分中讨论）

## <a name="property-sets-and-effects"></a>属性集和效果

除了设置 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 和 InsetClip 属性的动画之外，还可以设置 PropertySet 或 Effect 中的属性的动画。
对于属性集，定义某个属性并将其存储在合成属性集中，该属性在以后可以是动画的目标（也可以同时在另一个动画中引用）。 这将在接下来的部分中进行详细介绍。

对于效果，可以使用合成效果 API 定义图形效果（查看此处的[效果概述](./composition-effects.md)）。
除了定义效果之外，还可以设置 Effect 的属性值的动画。
通过在 Sprite Visuals 上定向 Brush 属性的属性组件来完成此操作。

## <a name="quick-formula-getting-started-with-composition-animations"></a>快速公式：合成动画入门

在深入了解关于如何构造和使用不同类型的动画前，下面是有关如何汇集合成动画的快速高级公式。

1. 获取合成器。 这可以从创建动画的页面或 FrameworkElement 获取。
1. 为你的动画创建新对象，动画可以是关键帧动画和表达式动画。
    * 对于关键帧动画，请确保创建可匹配要设置动画的属性类型的关键帧动画类型。
    * 表达式动画仅有一种类型。
1. 定义动画的内容，插入关键帧或定义表达式字符串。
    * 对于关键帧动画，请确保关键帧的值与想要设置动画的属性具有相同类型。
    * 对于表达式动画，请确保表达式字符串将解析为与想要设置动画的属性具有相同类型。
1. 在想要设置其属性的动画的 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 上启动动画，调用 StartAnimation 并包括为参数：想要设置动画的属性的名称（使用字符串形式）以及动画对象。

```cs
// KeyFrame Animation Example to target Opacity property
// Step 1 - Get the compositor
_compositor = ElementCompositionPreview.GetElementVisual(this).Compositor;

// Step 2 - Create your animation object
var animation = _compositor.CreateScalarKeyFrameAnimation();

// Step 3 - Define Content
animation.Duration = TimeSpan.FromSeconds(1);
animation.InsertKeyFrame(1f, 0.2f);

// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation(nameof(Visual.Opacity), animation);

// Expression Animation Example to target Opacity property
// Step 2 - Create your animation object
var expression = _compositor.CreateExpressionAnimation();
// Step 3 - Define Content (you can also define the string as part of the expression object
// declaration)
expression.Expression = "targetVisual.Offset.X / windowWidth";
expression.SetReferenceParameter("targetVisual", _target);
expression.SetScalarParameter("windowWidth", _xSizeWindow);
// Step 4 - Attach animation to Visual property and start animation
_targetVisual.StartAnimation("Opacity", expression);

```

## <a name="using-keyframe-animations"></a>使用关键帧动画

关键帧动画是基于时间的动画，此类动画使用一个或多个关键帧来指定已设置动画的值应该如何随时间变化。
帧表示标记或控制点，允许你定义已设置动画的值在特定时间应该是什么值。

### <a name="creating-your-animation-and-defining-keyframes"></a>创建你的动画和定义关键帧

若要构造关键帧动画，请使用合成器对象（与你希望设置动画的属性类型相关）的构造函数方法。
不同种类的关键帧动画有：

* ColorKeyFrameAnimation
* QuaternionKeyFrameAnimation
* ScalarKeyFrameAnimation
* Vector2KeyFrameAnimation
* Vector3KeyFrameAnimation
* Vector4KeyFrameAnimation

创建 Vector3 关键帧动画的示例：

```cs
var animation = _compositor.CreateVector3KeyFrameAnimation();
```

每个关键帧动画通过插入定义两个组件（第三个组件可选）的个别关键帧分段构造。

* 时间：介于 0.0 和 1.0 之间关键帧的标准化进度状态。
* 值：设置动画值在时间状态下的特定值。
* （可选）缓动函数：用于描述上一个关键帧与当前关键帧之间的内插的函数（在本主题的后面部分进行讨论）。

在动画的中间点插入关键帧的示例：

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

**注意：**当使用关键帧动画设置颜色的动画时，还需要记住其他一些事项：

1. 需要将 StartAnimation 附加到 Visual.Brush，而非附加到 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx)，并且 **Color** 是你想要设置动画的属性参数。
1. 关键帧的“value”组件由 Windows.UI 命名空间的 Colors 对象定义。
1. 通过设置 InterpolationColorSpace 属性，可以选择定义内插将完成的颜色空间。 可能值的包括：
    * CompositionColorSpace.Rgb
    * CompositionColorSpace.Hsl

## <a name="keyframe-animation-properties"></a>关键帧动画属性

在定义了关键帧动画和个别关键帧后，你可以为动画定义多个属性：

* DelayTime – 在 StartAnimation() 调用后到动画启动前这段时间。
* Duration – 动画的时长。
* IterationBehavior – 动画的计数或无限重复行为。
* IterationCount – 关键帧动画将重复的有限次数。
* KeyFrame Count – 特定关键帧动画中的关键帧读数。
* StopBehavior – 用于在调用 StopAnimation 后指定设置动画的属性值的行为。
* Direction – 指定动画的播放方向。

将动画的持续时间设置为 5 秒的示例：

```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

## <a name="easing-functions"></a>缓动函数

缓动函数 (CompositionEasingFunction) 指示从上一关键帧值到当前关键帧值的中间值进度。 如果你未针对关键帧提供缓动函数，将使用默认曲线。

支持的缓动函数有三种类型：

* 线性
* 三次方贝塞尔
* 步骤

三次方贝塞尔是经常用来描述可缩放的平滑曲线的参数函数。 在与合成关键帧动画一起使用时，你将定义属于 Vector2 对象的两个控制点。 这些控制点用于定义曲线的形状。 建议使用类似站点（例如[此站点](http://cubic-bezier.com/#0,-0.01,.48,.99)）来可视化两个控制点构造三次方贝塞尔曲线的方式。

若要创建缓动函数，请使用合成器对象的构造函数方法。 下面的两个示例演示创建线性缓动函数和基本的三次方贝塞尔缓动函数。

```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
var step = _compositor.CreateStepEasingFunction();
```

若要将缓动函数添加到关键帧，只需在插入到动画时将该函数从第三个参数添加到关键帧即可。

使用关键帧添加 easeIn 缓动函数的示例：

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

## <a name="starting-and-stopping-keyframe-animations"></a>启动和停止关键帧动画

在定义了动画和关键帧后，你可以随时连接动画。 在启动动画时，指定要设置动画的 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 和目标属性以及动画的引用。
通过调用 StartAnimation() 函数执行此操作。 请记住，在属性上调用 StartAnimation() 将断开连接并删除任何之前运行的动画。
**注意：**选择用来设置动画的属性的引用采用字符串形式。

根据 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 Offset 属性设置并启动动画的示例：

```cs
targetVisual.StartAnimation("Offset", animation);
```

如果想要定向子通道属性，需要将该子通道添加到定义想要设置动画的属性的字符串。
在以上示例中，语法将更改为 StartAnimation("Offset.X, animation2)，其中 animation2 是 ScalarKeyFrameAnimation。

在启动动画后，仍然能够在动画完成前阻止它。 通过使用 StopAnimation() 函数完成此操作。
根据 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 Offset 属性停止动画的示例：

```cs
targetVisual.StopAnimation("Offset");
```

在显式停止动画时，你还可以定义它的行为。 若要执行此操作，请定义动画的 Stop Behavior 属性。 有以下三种选项：

* LeaveCurrentValue：动画将已设置动画的属性值标记为动画的最后一个计算的值。
* SetToFinalValue：动画将已设置动画的属性值标记为最后一个关键帧的值。
* SetToInitialValue：动画将已设置动画的属性值标记为第一个关键帧的值。

设置关键帧动画的 StopBehavior 属性的示例：

```cs
animation.StopBehavior = AnimationStopBehavior.LeaveCurrentValue;
```

## <a name="animation-completion-events"></a>动画完成事件

通过使用关键帧动画，你可以在完成精选动画（或动画组）时使用动画批来进行聚合。
仅可以批处理关键帧动画完成事件。 表达式动画没有一个确切终点，因此它们不会引发完成事件。
如果表达式动画在批中启动，该动画将会像预期那样执行，并且不会影响引发批的时间。

当批内的所有动画都完成时，将引发批完成事件。
引发批的事件所需的时间取决于该批中时长最长的动画或延迟最为严重的动画。
在你需要了解选定的动画组将于何时完成以便计划一些其他工作时，聚合结束状态非常有用。

批在引发完成事件后释放。 还可以随时调用 Dispose() 来尽早释放资源。
如果批处理的动画结束较早，并且你不希望继续完成事件，你可能会想要手动释放批对象。
如果动画已中断或取消，将会引发完成事件，并且该事件会计入设置它的批。
这显示在 [Windows/Composition GitHub](http://go.microsoft.com/fwlink/p/?LinkId=789439) 上的 Animation_Batch SDK 示例中。

## <a name="scoped-batches"></a>Scoped 批

若要聚合特定动画组或定向单个动画的完成事件，请创建 Scoped 批。

```cs
CompositionScopedBatch myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
```

创建 Scoped 批后，所有启动的动画均将聚合，直至使用 Suspend 或 End 函数显式暂停或停止该批。

调用 Suspend 函数将停止聚合动画结束状态，直到调用 Resume。 这将使你能够显式从给定批中排除内容。

在下面的示例中，定向 VisualA 的 Offset 属性的动画不会包括在批中：

```cs
myScopedBatch.Suspend();
VisualA.StartAnimation("Offset", myAnimation);
myScopeBatch.Resume();
```

为了完成批操作，必须调用 End()。 如果不进行 End 调用，该批将保持打开永久收集对象。

下面的代码片段和图表显示了批如何聚合动画来跟踪结束状态的示例。
请注意，在此示例中，动画 1、3 和 4 会使此批处理跟踪结束状态，但动画 2 不会。

```cs
myScopedBatch.End();
CompositionScopedBatch myScopedBatch =  _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
// Start Animation1
[…]
myScopedBatch.Suspend();
// Start Animation2
[…]
myScopedBatch.Resume();
// Start Animation3
[…]
// Start Animation4
[…]
myScopedBatch.End();
```

![限定范围的批处理包含动画 1、动画 3和动画 4，而动画 2 不在限定范围的批处理中](./images/composition-scopedbatch.png)

## <a name="batching-a-single-animations-completion-event"></a>批处理单个动画的完成事件

如果你想要了解单个动画何时结束，需要创建正好包括你定向的动画的 Scoped 批。 例如：

```cs
CompositionScopedBatch myScopedBatch =  _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation("Opacity", myAnimation);
myScopedBatch.End();
```

## <a name="retrieving-a-batchs-completion-event"></a>检索批的完成事件

在批处理一个动画或多个动画时，你将按照相同方法检索批的完成事件。
需要为定向批的 Completed 事件注册事件处理方法。

```cs
myScopedBatch.Completed += OnBatchCompleted;
```

## <a name="batch-states"></a>批状态

你可以使用两种属性来确定现有批的状态：IsActive 和 IsEnded。

如果定向的批对聚合动画打开，IsActive 属性将返回 true。 批暂停或结束时，IsActive 将返回 false。

当你无法向该特定批添加动画时，IsEnded 属性将返回 true。 当你为某个特定批显式调用 End() 时，批将结束。

## <a name="using-expression-animations"></a>使用表达式动画

表达式动画是在 Windows 10 11 月更新 (10586) 中引入的新型动画。
在高级别上，表达式动画基于离散值和其他合成对象属性的引用之间的数学等式/关系。
与使用插入程序函数（三次方贝塞尔、Quad、Quintic 等）描述值如何随时间更改的关键帧动画相比，表达式动画使用数学等式来定义已设置动画的值如何计算每个帧。
请务必指出，表达式动画没有定义的持续时间。启动后，它们将运行并使用数学等式确定设置动画的属性的值，直到显式停止它们为止。

**那么表达式动画为什么有用呢？**

表达式动画的实际功能源于它们能够创建包含其他对象的参数或属性引用的数学关系。
这意味着你可以拥有引用其他合成对象、本地变量甚至是合成属性集中的共享值上的属性值。
由于此引用模型，并且计算每个帧的等式，所以如果定义等式的值发生更改，等式的输出也会更改。
这将超越值必须是离散值并且必须预定义的传统关键帧动画而开放更大的可能性。
例如，使用表达式动画可以轻松描述粘滞标头和视差等体验。

**注意：**我们使用“表达式”或“表达式字符串”作为定义表达式动画对象的数学等式的引用。

## <a name="creating-and-attaching-your-expression-animation"></a>创建和附加表达式动画

在跳转到创建表达式动画的语法前，需要提及一些核心原则：

* 表达式动画使用定义的数学等式来确定每个帧设置动画的属性的值。
* 数学等式作为字符串输入到表达式中。
* 数学等式的输出必须解析为与计划设置动画的属性具有相同类型。 如果它们不匹配，你将在计算表达式时收到错误。 如果等式解析为 Nan (number/0)，系统会使用之前计算的最后一个值。
* 表达式动画具有*无限生存期*，即它们会继续运行到停止为止。

若要创建你的表达式动画，只需使用合成对象的构造函数，可在其中定义数学表达式。

下面是定义将两个 Scalar 值加在一起的非常基本的表达式的构造函数示例（我们将在下一节深入研究更复杂的表达式）：

```cs
var expression = _compositor.CreateExpressionAnimation("0.2 + 0.3");
```

与关键帧动画相似，在定义了表达式动画后，你需要将其附加到 Visual 并声明希望动画来设置动画的属性。
下面我们继续操作上述示例，并将表达式动画附加到 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 Opacity 属性（Scalar 类型）：

```cs
targetVisual.StartAnimation("Opacity", expression);
```

## <a name="components-of-your-expression-string"></a>表达式字符串的组件

前一部分中的示例展示了两个加在一起的简单 Scalar 值。
虽然这是表达式的有效示例，但它没有完全展示使用表达式的潜在用途。
上述示例有一点值得注意的是，因为这些值是离散值，所以每一帧的等式会解析为 0.5，并且在动画的生存期内永远不会更改。
表达式的真正潜力源自定义值可以周期性或一直更改的数学关系。

让我们了解一下可以组成这些类型的表达式的不同部分。

### <a name="operators-precedence-and-associativity"></a>运算符、优先级和结合性

表达式字符串支持使用可以描述等式不同组件之间的数学关系的典型运算符：

|类别| 运算符|
|--------|-----------|
|一元| -|
|乘法|* /|
|加法|+ -|
|Mod| %|

同样，在计算表达式时，它将遵循运算符优先级和结合性，如 C# 语言规范中所定义。
换言之，它会遵循运算的基本顺序。

在下面的示例中，在计算时，根据运算顺序将首先解析括号，然后再解析等式的其余部分：

```cs
"(5.0 * (72.4 – 36.0) + 5.0" // (5.0 * 36.4 + 5) -> (182 + 5) -> 187
```

### <a name="property-parameters"></a>属性参数

属性参数是表达式动画最强大的组件之一。
在表达式字符串中，你可以引用其他对象（例如 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx)、合成属性集或其他 C# 对象）中的属性值。

若要在表达式字符串中使用这些属性，只需将引用定义为表达式动画的参数。
通过将在表达式中使用的字符串映射到实际对象可完成此操作。 这允许系统在计算等式时知道要检查什么内容才能计算值。
有不同类型的参数与你想要包括在等式中的对象类型相关：

|类型|创建参数的函数|
|----|------------------------------|
|Scalar|SetScalarParameter(String ref, Scalar obj)|
|Vector|SetVector2Parameter(String ref, Vector2 obj)<br/>SetVector3Parameter(String ref, Vector3 obj)<br/>SetVector4Parameter(String ref, Vector4 obj)|
|Matrix|SetMatrix3x2Parameter(String ref, Matrix3x2 obj)<br/>SetMatrix4x4Parameter(String ref, Matrix4x4 obj)|
|Quaternion|SetQuaternionParameter(String ref, Quaternion obj)|
|Color|SetColorParameter(String ref, Color obj)|
|CompositionObject|SetReferenceParameter(String ref, Composition object obj)|
|布尔型| SetBooleanParameter(String ref, Boolean obj)|

在下面的示例中，我们将创建引用其他两个合成[可视化](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx)和基本 System.Numerics Vector3 对象的 Offset 的表达式动画。

```cs
var commonOffset = new Vector3(25.0, 17.0, 10.0);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + additionalOffset");
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetVector3Parameter("additionalOffset", commonOffset);
```

此外，你可以使用与上述相同的模型从表达式引用属性集中的值。
合成属性集是动画用于存储数据的有用方法，并且可用于创建可共享、可重复使用、不依赖于任何其他合成对象的生存期的数据。
属性集值可在与其他属性引用类似的表达式中引用。 （属性集将在后文中详细讨论）

我们可以直接修改上述示例，以便使用属性集来定义 commonOffset 而非本地变量：

```cs
_sharedProperties = _compositor.CreatePropertySet();
_sharedProperties.InsertVector3("commonOffset", offset);
var expression = _compositor.CreateExpressionAnimation("SomeOffset / ParentOffset + sharedProperties.commonOffset");
expression.SetVector3Parameter("SomeOffset", childVisual.Offset);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
```

最后，在引用其他对象的属性时，还可以在表达式字符串中引用子通道属性，或将其作为引用参数的一部分进行引用。

在下面的示例中，我们引用两个 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 Offset 属性的 x 子通道：一个位于表达式字符串本身中，另一个在创建参数引用时生成。
请注意，在引用 Offset 的 X 组件时，我们会更改 Scalar 参数的参数类型，而非之前示例中所示的 Vector3：

```cs
var expression = _compositor.CreateExpressionAnimation("xOffset/ ParentOffset.X");
expression.SetScalarParameter("xOffset", childVisual.Offset.X);
expression.SetVector3Parameter("ParentOffset", parentVisual.Offset);
```

### <a name="expression-helper-functions-and-constructors"></a>表达式帮助程序函数和构造函数

除了能够访问 Operator 和 Property 参数，你还可以利用数学函数列表以用于它们的表达式。
提供这些函数以在与对 System.Numerics 对象处理方式相似的不同类型上执行计算和操作。

下面的示例创建了定向利用 Clamp 帮助程序函数的 Scalar 的表达式：

```cs
var expression = _compositor.CreateExpressionAnimation("Clamp((scroller.Offset.y * -1.0) – container.Offset.y, 0, container.Size.y – header.Size.y)"
```

除了帮助程序函数外，你还可以使用内置于表达式字符串中的 Constructor 方法，它们根据提供的参数生成该类型的实例。

下面的示例创建了在表达式字符串中定义新的 Vector3 的表达式：

```cs
var expression = _compositor.CreateExpressionAnimation("Offset / Vector3(targetX, targetY, targetZ");
```

你可以在附录部分查找帮助程序函数和构造函数的完整扩展列表，或者在下表中找到每种类型的扩展列表：

* [Scalar](#scalar)
* [Vector2](#vector2)
* [Vector3](#vector3)
* [Matrix3x2](#matrix3x2)
* [Matrix4x4](#matrix4x4)
* [Quaternion](#quaternion)
* [Color](#color)

### <a name="expression-keywords"></a>表达式关键字

在计算表达式字符串时，你可以利用处理方式不同的特殊“关键字”。
由于它们被认为是“关键字”，所以无法用作属性引用的字符串参数部分。

|关键字|描述|
|-------|--------------|
|This.StartingValue| 提供对要设置动画的属性的初始起始值的引用。|
|This.CurrentValue|提供对属性的当前“已知”值的引用|
|Pi| 提供对 PI 值的关键字引用。|

下面的示例展示如何使用 this.StartingValue 关键字：

```cs
var expression = _compositor.CreateExpressionAnimation("this.StartingValue + delta");
```

### <a name="expressions-with-conditionals"></a>使用条件的表达式

除了支持使用运算符、属性引用、函数和构造函数的数学关系，你还可以创建包含三元运算符的表达式：

```cs
(condition ? ifTrue_expression : ifFalse_expression)
```

条件语句可以让你根据特定条件编写表达式，系统将使用不同的数学关系来计算设置动画属性的值。
三元运算符可以嵌套为 true 或 false 语句的表达式。

以下条件运算符在条件语句中受支持：

* 等于 (==)
* 不等于 (!=)
* 小于 (&lt;)
* 小于或等于 (&lt;=)
* 大于 (&gt;)
* 大于或等于 (&gt;=)

支持将以下连词作为条件语句中的运算符或函数：

* Not: ! / Not(bool1)
* And: &amp;&amp; / And(bool1, bool2)
* Or: || / Or(bool1, bool2)

下面是使用条件的表达式动画示例。

```cs
var expression = _compositor.CreateExpressionAnimation("target.Offset.x > 50 ? 0.0f + (target.Offset.x / parent.Offset.x) : 1.0f");
```

## <a name="expression-keyframes"></a>表达式关键帧

在本文档的前面部分中，我们介绍了如何创建关键帧动画、表达式动画，以及可用于组成表达式字符串的全部不同部分。 如果你想获取表达式动画的功能，但又想拥有关键帧动画提供的时间内插，该怎么办？
答案就是使用表达式关键帧！

你可以使值成为表达式字符串，而不用为关键帧动画中的每个控制点定义离散值。
在这种情况下，系统会使用表达式字符串计算时间线给定点处的设置动画属性的值。
然后，系统仅会内插入此值，就像在正常的关键帧动画中那样。

你无需创建特殊动画来使用表达式关键帧，只需将 ExpressionKeyFrame 插入标准的关键帧动画，提供作为值的时间和表达式字符串。 下面的示例展示了将表达式字符串用作关键帧之一的值：

```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, "VisualBOffset.X / VisualAOffset.Y");
animation.InsertKeyFrame(1.00f, 0.8f);
```

## <a name="expression-sample"></a>表达式示例

下面的代码显示了为从滚动查看器提取输入值的基本视差体验而设置表达式动画的示例。

```cs
// Get scrollviewer
ScrollViewer myScrollViewer = ThumbnailList.GetFirstDescendantOfType<ScrollViewer>();
_scrollProperties = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);

// Setup the expression
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 *  ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5   * ItemHeight))";
```

## <a name="animating-with-property-sets"></a>使用属性集设置动画

合成属性集可让你存储可跨多个动画共享的值，并且不会依赖另一个合成对象的生存期。
属性集在存储常用值，然后再在动画中引用它们时非常有用。
你还可以根据应用程序逻辑使用属性集存储数据以驱动表达式。

若要创建属性集，请使用合成器对象的构造函数方法：

```cs
_sharedProperties = _compositor.CreatePropertySet();
```

在创建属性集后，你可以向其中添加属性和值：

```cs
_sharedProperties.InsertVector3("NewOffset", offset);
```

与我们之前看到的情况相似，我们可以在表达式动画中引用此属性集值：

```cs
var expression = _compositor.CreateExpressionAnimation("this.target.Offset + sharedProperties.NewOffset");
expression.SetReferenceParameter("sharedProperties", _sharedProperties);
targetVisual.StartAnimation("Offset", expression);
```

还可以设置属性集值的动画。 通过将动画附加到 PropertySet 对象，然后再在字符串中引用该属性名称来完成此操作。
在下面，我们使用关键帧动画在属性集中设置 NewOffset 属性的动画。

```cs
var keyFrameAnimation = _compositor.CreateVector3KeyFrameAnimation()
keyFrameAnimation.InsertKeyFrame(0.5f, new Vector3(25.0f, 68.0f, 0.0f);
keyFrameAnimation.InsertKeyFrame(1.0f, new Vector3(89.0f, 125.0f, 0.0f);
_sharedProperties.StartAnimation("NewOffset", keyFrameAnimation);
```

你可能想知道此代码是否在应用中执行以及表达式动画附加到的已设置动画的属性值会发生什么。
在这种情况下，表达式最初输出到某个值，但在关键帧动画开始设置属性集中属性的动画后，表达式值也会更新，因为将计算每个帧的等式。 这就是带有表达式和关键帧动画的属性集的美妙之处！

## <a name="manipulationpropertyset"></a>ManipulationPropertySet

除了利用合成属性集之外，开发人员也可以访问 ManipulationPropertySet，该属性集允许访问 XAML ScrollViewer 之外的属性。 然后可以在表达式动画中使用并引用这些属性，以支持视差和粘滞标头等体验。 注意：你可以从任何 XAML 控件（ListView、GridView 等）中获取具有可滚动内容的 ScrollViewer，然后使用该 ScrollViewer 获取这些可滚动控件的 ManipulationPropertySet。

在你的表达式中，可以引用滚动查看器的以下属性：

|Property| Type|
|--------|-----|
|Translation| Vector3|
|Pan| Vector3|
|Scale| Vector3|
|CenterPoint| Vector3|
|Matrix| Matrix4x4|

若要获取对 ManipulationPropertySet 的引用，请利用 ElementCompositionPreview 的 GetScrollViewerManipulationPropertySet 方法。

```csharp
CompositionPropertySet manipPropSet = ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScroller);
```

拥有对该属性集的引用后，可以引用在属性集中找到的滚动查看器的属性。 第一步是创建引用参数。

```csharp
ExpressionAnimation exp = compositor.CreateExpressionAnimation();
exp.SetReferenceParameter("ScrollManipulation", manipPropSet);
```

在设置引用参数后，可以在表达式中引用 ManipulationPropertySet 属性。

```csharp
exp.Expression = “ScrollManipulation.Translation.Y / ScrollBounds”;
_target.StartAnimation(“Opacity”, exp);
```

## <a name="using-implicit-animations"></a>使用隐式动画

动画是向用户介绍行为的绝佳方法。 有多种方法可以设置内容的动画，但到目前为止所讨论的所有方法都要求显式*启动*动画。 尽管这使你可以实现对定义动画开始时间的完全控制，但每次更改属性值时，将使得管理变得很困难（如果需要动画）。 在应用程序已分离了基于应用“逻辑”（该逻辑用于定义应用的核心组件和基础结构）定义动画的应用“个性化”时，通常会出现这种情况。 隐式动画提供了一种更轻松、更简洁的方式来定义独立于核心应用逻辑的动画。 可以连接这些动画，以便通过特定的属性更改触发器运行。

### <a name="setting-up-your-implicitanimationcollection"></a>设置 ImplicitAnimationCollection

隐式动画由其他 **CompositionAnimation** 对象（**KeyFrameAnimation** 或 **ExpressionAnimation**）定义。 **ImplicitAnimationCollection** 表示 **CompositionAnimation** 对象集，它们将在到达属性更改*触发器*时启动。 请注意，在定义动画时，确保设置 **Target** 属性，这将定义动画在启动后将定向的 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 属性。 **Target** 属性只能是可进行动画设置的 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 属性。
在下面的代码片段中，创建了单个 **Vector3KeyFrameAnimation** 并将其定义为 **ImplicitAnimationCollection** 的一部分。 然后 **ImplicitAnimationCollection** 将附加到 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 **ImplicitAnimation** 属性，这样一来，在到达触发器时，动画便会启动。

```csharp
Vector3KeyFrameAnimation animation = _compositor.CreateVector3KeyFrameAnimation();
animation.DelayTime =  TimeSpan.FromMilliseconds(index);
animation.InsertExpressionKeyFrame(1.0f, "this.FinalValue");
animation.Target = "Offset";
ImplicitAnimationCollection implicitAnimationCollection = compositor.CreateImplicitAnimationCollection();

visual.ImplicitAnimations = implicitAnimationCollection;
```

### <a name="triggering-when-the-implicitanimation-starts"></a>当 ImplicitAnimation 启动时触发

触发器是用于描述动画将何时隐式启动的术语。 当前，触发器定义为 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 上的任何动画属性的变化，当显式对该属性进行设置时将出现这些变化。 例如，通过将 **Offset** 触发器放在 **ImplicitAnimationCollection** 上并将动画与该触发器关联，已定向 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 **Offset** 更新将使用该集合中的动画，对它的新值进行动画设置。
通过上面的示例，我们添加这一附加行来将触发器设置为目标 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的 **Offset** 属性。

```csharp
implicitAnimationCollection["Offset"] = animation;
```

请注意，**ImplicitAnimationCollection** 可具有多个触发器。 这意味着，对于不同属性的变化，隐式动画或动画组可进行启动。 在上面的示例中，开发人员可以为其他属性（例如 Opacity）添加触发器。

### <a name="thisfinalvalue"></a>this.FinalValue

在第一个隐式示例中，我们将 ExpressionKeyFrame 用于“1.0”关键帧，并为它指定了 **this.FinalValue** 的表达式。 **this.FinalValue** 是表达式语言中的保留关键字，用于提供隐式动画的区分行为。 **this.FinalValue** 将 API 属性上设置的值绑定到了动画。 这将有助于创建真实的模板。 **this.FinalValue** 在显式动画中不适用，由于 API 属性立即设定；而对于隐式动画，它将出现延迟。

## <a name="using-animation-groups"></a>使用动画组

**CompositionAnimationGroup** 为你对动画列表进行分组提供了简单的方法，它可以与隐式或显式动画一起使用。

### <a name="creating-and-populating-animation-groups"></a>创建和填充动画组

Compositor 对象的 **CreateAnimationGroup** 方法允许你创建动画组：

```sharp
CompositionAnimationGroup animationGroup = _compositor.CreateAnimationGroup();
animationGroup.Add(animationA);
animationGroup.Add(animationB);
```

一旦创建了组，便可以将各个动画添加到动画组。 请记住无需显式启动各个动画，它们在以下情况下都将全部启动：当针对显式方案调用 **StartAnimationGroup** 时，或者在某一隐式动画中到达触发器时。
请注意，确保添加到组的动画都已定义 **Target** 属性。 这将定义设置动画的目标 [Visual](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.visual.aspx) 的属性。

### <a name="using-animation-groups-with-implicit-animations"></a>将动画组用于隐式动画

你可以创建隐式动画，以便在到达触发器时，将启动一组动画组形式的动画。 在此情况下，将动画组定义为在到达触发器时启动的动画集。

```csharp
implicitAnimationCollection["Offset"] = animationGroup;
```

### <a name="using-animation-groups-with-explicit-animations"></a>将动画组用于显式动画

你可以创建显式动画，以便在调用 **StartAnimationGroup** 时，将启动已添加的各个动画。 请注意，在此 **StartAnimation** 调用中，当各个动画可以定向不同的属性时，不存在组的定向属性。 请确保已设置每个动画的目标属性。

```csharp
visual.StartAnimationGroup(AnimationGroup);
```

### <a name="e2e-sample"></a>E2E 示例

此示例显示了在设置新值时以隐式方式对 Offset 属性进行动画设置。

```csharp
class PropertyAnimation
{
    PropertyAnimation(Compositor compositor, SpriteVisual heroVisual, SpriteVisual listVisual)
    {
      // Define ImplicitAnimationCollection
        ImplicitAnimationCollection implicitAnimations =
        compositor.CreateImplicitAnimationCollection();

        // Trigger animation when the “Offset” property changes.
        implicitAnimations["Offset"] = CreateAnimation(compositor);

        // Assign ImplicitAnimations to a visual. Unlike Visual.Children,
        // ImplicitAnimations can be shared by multiple visuals so that they
        // share the same implicit animation behavior (same as Visual.Clip).
        heroVisual.ImplicitAnimations = implicitAnimations;

        // ImplicitAnimations can be shared among visuals
        listVisual.ImplicitAnimations = implicitAnimations;

        listVisual.Offset = new Vector3(20f, 20f, 20f);
    }

    Vector3KeyFrameAnimation CreateAnimation(Compositor compositor)
    {
        Vector3KeyFrameAnimation animation = compositor.CreateVector3KeyFrameAnimation();
        animation.InsertExpressionKeyFrame(0f, "this.StartingValue");
        animation.InsertExpressionKeyFrame(1f, "this.FinalValue");
        animation.Target = “Offset”;
        animation.Duration = TimeSpan.FromSeconds(0.25);
        return animation;
    }
}
```

## <a name="appendix"></a>附录

### <a name="expression-functions-by-structure-type"></a>按结构类型划分的表达式函数

### <a name="scalar"></a>Scalar

|函数和构造函数运算| 描述|
|-----------------------------------|--------------|
|Abs(Float value)| 返回表示浮点参数绝对值的 Float|
|Clamp(Float value, Float min, Float max)| 返回大于最小值且小于最大值或等于最小值（如果值小于最小值）或最大值（如果值大于最大值）的浮点值|
|Max (Float value1, Float value2)| 返回 value1 和 value2 之间的较大浮点数。|
|Min (Float value1, Float value2)| 返回 value1 和 value2 之间的较小浮点数。|
|Lerp(Float value1, Float value2, Float progress)| 返回表示根据进度计算的两个 Scalar 值之间的线性内插计算的浮点数（注意：进度在 0.0 和 1.0 之间）|
|Slerp(Float value1, Float value2, Float progress)| 返回表示根据进度计算的两个 Float 值之间的球面内插的 Float（注意：进度在 0.0 和 1.0 之间）|
|Mod(Float value1, Float value2)| 返回 value1 和 value2 拆分后生成的 Float 剩余数|
|Ceil(Float value)| 返回舍入到下一个较大整数的 Float 参数|
|Floor(Float value)| 返回舍入到下一个较小整数的 Float 参数|
|Sqrt(Float value)| 返回 Float 参数的平方根|
|Square(Float value)| 返回 Float 参数的平方|
|Sin(Float value1)| 返回 Float 参数的 Sin|
|Asin(Float value2)| 返回 Float 参数的 ArcSin|
|Cos(Float value1)| 返回 Float 参数的 Cos|
|ACos(Float value2)| 返回 Float 参数的 ArcCos|
|Tan(Float value1)| 返回 Float 参数的 Tan|
|ATan(Float value2)| 返回 Float 参数的 ArcTan|
|Round(Float value)| 返回舍入到最近整数的 Float 参数|
|Log10(Float value)| 返回 Float 参数的 Log（基数 10）结果|
|Ln(Float value)| 返回 Float 参数的自然对数结果|
|Pow(Float value, Float power)| 返回提升到特定幂的 Float 参数的结果|
|ToDegrees(Float radians)| 返回换算为度的 Float 参数|
|ToRadians(Float degrees)| 返回换算为弧度的 Float 参数|

### <a name="vector2"></a>Vector2

|函数和构造函数运算|描述|
|-----------------------------------|--------------|
|Abs (Vector2 value)|返回绝对值应用到每个组件的 Vector2|
|Clamp (Vector2 value1, Vector2 min, Vector2 max)|返回包含各个组件的受限值的 Vector2|
|Max (Vector2 value1, Vector2 value2)|返回在 value1 和 value2 的每个相应组件上执行 Max 的 Vector2|
|Min (Vector2 value1, Vector2 value2)|返回在 value1 和 value2 的每个相应组件上执行 Min 的 Vector2|
|Scale(Vector2 value, Float factor)|返回每个矢量乘以比例因子的组件的 Vector2。|
|Transform(Vector2 value, Matrix3x2 matrix)|返回在 Vector2 和 Matrix3x2 之间进行线性转换（又称为矢量乘以矩阵）后生成的 Vector2。|
|Lerp(Vector2 value1, Vector2 value2, Float progress)|返回表示根据进度计算的两个 Vector2 值之间的线性内插计算的 Vector2（注意：进度在 0.0 和 1.0 之间）|
|Length(Vector2 value)|返回表示 Vector2 长度/幅值的 Float 值|
|LengthSquared(Vector2)|返回表示 Vector2 长度/幅值平方的 Float 值|
|Distance(Vector2 value1, Vector2 value2)|返回表示两个 Vector2 值之间距离的 Float 值|
|DistanceSquared(Vector2 value1, Vector2 value2)|返回表示两个 Vector2 值之间距离的平方的 Float 值|
|Normalize(Vector2 value)|返回表示所有组件均已标准化的参数的单元矢量的 Vector2|
|Vector2(Float x, Float y)|使用两个 Float 参数构造 Vector2|

### <a name="vector3"></a>Vector3

|函数和构造函数运算|描述|
|-----------------------------------|--------------|
|Abs (Vector3 value)|返回绝对值应用到每个组件的 Vector3|
|Clamp (Vector3 value1, Vector3 min, Vector3 max)|返回包含各个组件的受限值的 Vector3|
|Max (Vector3 value1, Vector3 value2)|返回在 value1 和 value2 的每个相应组件上执行 Max 的 Vector3|
|Min (Vector3 value1, Vector3 value2)|返回在 value1 和 value2 的每个相应组件上执行 Min 的 Vector3|
|Scale(Vector3 value, Float factor)|返回每个矢量乘以比例因子的组件的 Vector3。|
|Lerp(Vector3 value1, Vector3 value2, Float progress)|返回表示根据进度计算的两个 Vector3 值之间的线性内插计算的 Vector3（注意：进度在 0.0 和 1.0 之间）|
|Length(Vector3 value)|返回表示 Vector3 长度/幅值的 Float 值|
|LengthSquared(Vector3)|返回表示 Vector3 长度/幅值平方的 Float 值|
|Distance(Vector3 value1, Vector3 value2)|返回表示两个 Vector3 值之间距离的 Float 值|
|DistanceSquared(Vector3 value1, Vector3 value2)|返回表示两个 Vector3 值之间距离的平方的 Float 值|
|Normalize(Vector3 value)|返回表示所有组件均已标准化的参数的单元矢量的 Vector3|
|Vector3(Float x, Float y, Float z)|使用三个 Float 参数构造 Vector3|

### <a name="vector4"></a>Vector4

|函数和构造函数运算|描述|
|-----------------------------------|--------------|
|Abs (Vector4 value)|返回绝对值应用到每个组件的 Vector3|
|Clamp (Vector4 value1, Vector4 min, Vector4 max)|返回包含各个组件的受限值的 Vector4|
|Max (Vector4 value1 Vector4 value2)|返回在 value1 和 value2 的每个相应组件上执行 Max 的 Vector4|
|Min (Vector4 value1 Vector4 value2)|返回在 value1 和 value2 的每个相应组件上执行 Min 的 Vector4|
|Scale(Vector3 value, Float factor)|返回每个矢量乘以比例因子的组件的 Vector3。|
|Transform(Vector4 value, Matrix4x4 matrix)|返回在 Vector4 和 Matrix4x4 之间进行线性转换（又称为矢量乘以矩阵）后生成的 Vector4。|
|Lerp(Vector4 value1, Vector4 value2, Float progress)|返回表示根据进度计算的两个 Vector4 值之间的线性内插计算的 Vector4（注意：进度在 0.0 和 1.0 之间）|
|Length(Vector4 value)|返回表示 Vector4 长度/幅值的 Float 值|
|LengthSquared(Vector4)|返回表示 Vector4 长度/幅值平方的 Float 值|
|Distance(Vector4 value1, Vector4 value2)|返回表示两个 Vector4 值之间距离的 Float 值|
|DistanceSquared(Vector4 value1, Vector4 value2)|返回表示两个 Vector4 值之间距离的平方的 Float 值|
|Normalize(Vector4 value)|返回表示所有组件均已标准化的参数的单元矢量的 Vector4|
|Vector4(Float x, Float y, Float z, Float w)|使用四个 Float 参数构造 Vector4|

### <a name="matrix3x2"></a>Matrix3x2

|函数和构造函数运算|   描述|
|-----------------------------------|--------------|
|Scale(Matrix3x2 value, Float factor)|  返回每个矩阵乘以比例因子的组件的 Matrix3x2。|
|Inverse(Matrix 3x2 value)| 返回表示倒数矩阵的 Matrix3x2 对象|
|Matrix3x2(Float M11, Float M12, Float M21, Float M22, Float M31, Float M32)|   使用 6 个 Float 参数构造 Matrix3x2|
|Matrix3x2.CreateFromScale(Vector2 scale)|  使用表示比例的 Vector2 构造 Matrix3x2<br/>\[scale.X, 0.0<br/> 0.0, scale.Y<br/> 0.0, 0.0 \]|
|Matrix3x2.CreateFromTranslation(Vector2 translation)|  使用表示转换的 Vector2 构造 Matrix3x2<br/>\[1.0, 0.0,<br/> 0.0, 1.0,<br/> translation.X, translation.Y\]|
|Matrix3x2.CreateSkew(Float x, Float y, Vector2 centerpoint)| 使用表示倾斜的两个 Float 和一个 Vector2 构造 Matrix3x2<br/>\[1.0, Tan(y),<br/>Tan(x), 1.0,<br/>-centerpoint.Y * Tan(x), -centerpoint.X * Tan(y)\]|
|Matrix3x2.CreateRotation(Float radians)| 使用以弧度表示的旋转构造 Matrix3x2<br/>\[Cos(radians), Sin(radians),<br/>-Sin(radians), Cos(radians),<br/>0.0, 0.0 \]|
|Matrix3x2.CreateTranslation(Vector2 translation)| 与 CreateFromTranslation 相同|
|Matrix3x2.CreateScale(Vector2 scale)| 与 CreateFromScale 相同|


### <a name="matrix4x4"></a>Matrix4x4

|函数和构造函数运算|   描述|
|-----------------------------------|--------------|
|Scale(Matrix4x4 value, Float factor)|  返回每个矩阵乘以比例因子的组件的 Matrix 4x4。|
|Inverse(Matrix4x4)|    返回表示倒数矩阵的 Matrix4x4 对象|
|Matrix4x4(Float M11, Float M12, Float M13, Float M14,<br/>Float M21, Float M22, Float M23, Float M24,<br/>    Float M31, Float M32, Float M33, Float M34,<br/>    Float M41, Float M42, Float M43, Float M44)| 使用 16 个 Float 参数构造 Matrix4x4|
|Matrix4x4.CreateFromScale(Vector3 scale)|  使用表示比例的 Vector3 构造 Matrix4x4<br/>\[scale.X, 0.0, 0.0, 0.0,<br/> 0.0, scale.Y, 0.0, 0.0,<br/> 0.0, 0.0, scale.Z, 0.0,<br/> 0.0, 0.0, 0.0, 1.0\]|
|Matrix4x4.CreateFromTranslation(Vector3 translation)|  使用表示转换的 Vector3 构造 Matrix4x4<br/>\[1.0, 0.0, 0.0, 0.0,<br/> 0.0, 1.0, 0.0, 0.0,<br/> 0.0, 0.0, 1.0, 0.0,<br/> translation.X, translation.Y, translation.Z, 1.0\]|
|Matrix4x4.CreateFromAxisAngle(Vector3 axis, Float angle)|  使用 Vector3 轴和表示角度的 Float 构造 Matrix4x4|
|Matrix4x4(Matrix3x2 matrix)| 使用 Matrix3x2 构造 Matrix4x4<br/>\[matrix.11, matrix.12, 0, 0,<br/>matrix.21, matrix.22, 0, 0,<br/>0, 0, 1, 0,<br/>matrix.31, matrix.32, 0, 1\]|
|Matrix4x4.CreateTranslation(Vector3 translation)| 与 CreateFromTranslation 相同|
|Matrix4x4.CreateScale(Vector3 scale)| 与 CreateFromScale 相同|


### <a name="quaternion"></a>四元

|函数和构造函数运算|   描述|
|-----------------------------------|--------------|
|Slerp(Quaternion value1, Quaternion value2, Float progress)|   返回表示根据进度计算的两个 Quaternion 值之间的球面内插的 Quaternion（注意：进度在 0.0 和 1.0 之间）|
|Concatenate(Quaternion value1 Quaternion value2)|  返回表示两个 Quaternion 串联的 Quaternion（又称为表示两个单个旋转组合的 Quaternion）|
|Length(Quaternion value)|  返回表示 Quaternion 长度/幅值的 Float 值。|
|LengthSquared(Quaternion)| 返回表示 Quaternion 长度/幅值平方的 Float 值|
|Normalize(Quaternion value)|   返回组件已标准化的 Quaternion|
|Quaternion.CreateFromAxisAngle(Vector3 axis, Scalar angle)|    使用 Vector3 轴和表示角度的 Scalar 构造 Quaternion|
|Quaternion(Float x, Float y, Float z, Float w)|    使用四个 Float 值构造 Quaternion|

### <a name="color"></a>Color

|函数和构造函数运算|   描述|
|-----------------------------------|--------------|
|ColorLerp(Color colorTo, Color colorFrom, Float progress)| 返回表示根据给定进度计算的两个颜色对象之间的线性内插值的 Color 对象。 （注意：进度在 0.0 和 1.0 之间）|
|ColorLerpRGB(Color colorTo, Color colorFrom, Float progress)|  返回表示根据 RGB 颜色空间中的给定进度计算的两个对象之间的线性内插值的 Color 对象。|
|ColorLerpHSL(Color colorTo, Color colorFrom, Float progress)|  返回表示根据 HSL 颜色空间中的给定进度计算的两个对象之间的线性内插值的 Color 对象。|
|ColorArgb(Float a, Float r, Float g, Float b)| 构造表示由 ARGB 组件定义的 Color 的对象|
|ColorHsl(Float h, Float s, Float l)|   构造表示由 HSL 组件定义的 Color 的对象（注意：色调在 0 到 2pi 之间定义）|

