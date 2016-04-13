---
ms.assetid: 386faf59-8f22-2e7c-abc9-d04216e78894
合成动画
许多合成对象和效果属性均可使用关键帧和表达式动画设置动画，以便 UI 元素的属性可以随时间变化或基于计算发生变化。
---
# 合成动画

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

许多合成对象和效果属性均可使用关键帧和表达式动画设置动画，以便 UI 元素的属性可以随时间变化或基于计算发生变化。 有两种类型的动画：由 [**KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706830) 类表示的关键帧动画和 由 [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817) 类表示的表达式动画。

-   [动画的属性](./composition-animation.md#animatable_properties)
-   [关键帧动画](./composition-animation.md#keyframe-animations)
    -   [创建你的动画和关键帧](./composition-animation.md#creating-your-animation-and-Keyframes)
    -   [关键帧属性](./composition-animation.md#keyframe-properties)
    -   [缓动函数](./composition-animation.md#easing-functions)
    -   [启动和停止关键帧动画](./composition-animation.md#starting-and-stopping-keyframe-animations)
    -   [动画完成事件](./composition-animation.md#animation-completion-events)
-   [表达式动画](./composition-animation.md#expression-animations)
    -   [创建你的表达式](./composition-animation.md#creating-your-expression)
    -   [属性集](./composition-animation.md#property-sets)
    -   [表达式关键帧](./composition-animation.md#expression_keyframes)

## 动画的属性

以下属性可通过将其与关键帧动画或表达式动画关联来设置动画：

-   CompositionColorBrush.Color
-   InsetClip.BottomInset
-   InsetClip.LeftInset
-   InsetClip.RightInset
-   InsetClip.TopInset
-   Visual.AnchorPoint
-   Visual.CenterPoint
-   Visual.Offset
-   Visual.Opacity
-   Visual.Orientation
-   Visual.RotationAngle
-   Visual.RotationAxis
-   Visual.Size
-   Visual.TransformMatrix
-   CompositionPropertySet

## 关键帧动画

关键帧动画是基于时间的动画，此类动画使用一个或多个关键帧来指定已设置动画的值应该如何随时间变化。 帧表示标记，允许你定义已设置动画的值在特定时间应该是什么值。

### 创建你的动画和关键帧

要构建关键帧动画，请使用 Compositor 类（与你希望设置动画的属性结构类型相关）的构造函数方法之一。

-   [**CreateColorKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt589424)
-   [**CreateQuaternionKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt517858)
-   [**CreateScalarKeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706803)
-   [**CreateVector2KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706806)
-   [**CreateVector3KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706807)
-   [**CreateVector4KeyFrameAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706808)

例如，以下代码段将创建 Vector3 关键帧动画：

```cs
var animation = _compositor.CreateVector3KeyFrameAnimation();
```

通过调用 KeyFrameAnimation 的 InsertKeyFrame 方法，然后指定两个组件并且可有选择地指定第三个组件，来创建每个关键帧：

-   时间：介于 0.0 和 1.0 之间关键帧的标准化进度状态
-   值：设置动画值在时间状态下的特定值
-   （可选）缓动函数：用于描述上一关键帧与当前关键帧之间的内插的函数（稍后将作介绍）

例如，以下代码段在将在动画中间触发的 Vector3KeyFrameAnimation 中插入了一个关键帧：

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f));
```

### 关键帧属性

在定义了关键帧动画和各个关键帧后，你可以为动画定义多个属性：

-   DelayTime – 在 StartAnimation() 调用后到动画启动前这段时间
-   Duration – 动画的时长
-   IterationBehavior – 动画的计数或无限重复行为
-   IterationCount – 关键帧动画将重复的有限次数
-   KeyFrame Count – 特定 KeyFrameAnimation 中的关键帧数目
-   StopBehavior – 用于在调用 StopAnimation 后指定设置动画的属性值的行为。

例如，以下代码段将动画时长设置为五秒：

```cs
animation.Duration = TimeSpan.FromSeconds(5);
```

### 缓动函数

关键帧缓动函数 CompositionEasingFunction 指示从上一关键帧值到当前关键帧值的中间值进度。 如果你未针对关键帧提供缓动函数，将使用默认曲线。

支持的缓动函数有两种类型：

-   线性 ([**LinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706839))
-   三次方贝塞尔 ([**CubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706812))

要创建缓动函数，请使用 [**Compositor**](https://msdn.microsoft.com/library/windows/apps/Dn706761) 类的 [**CreateLinearEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706801) 或 [**CreateCubicBezierEasingFunction**](https://msdn.microsoft.com/library/windows/apps/Dn706791) 方法：

```cs
var linear = _compositor.CreateLinearEasingFunction();
var easeIn = _compositor.CreateCubicBezierEasingFunction(new Vector2(0.5f, 0.0f), new Vector2(1.0f, 1.0f));
```

注意：此处提供了一个很有用的工具，可用于为三次方贝塞尔缓动函数创建点。

如果要将缓动函数添加到关键帧中，只需在该函数插入到动画时将其从第三个参数添加到关键帧即可。

```cs
animation.InsertKeyFrame(0.5f, new Vector3(50.0f, 80.0f, 0.0f), easeIn);
```

### 启动和停止关键帧动画

在定义动画、关键帧和属性后，你可以随时将动画连接到你想要设置动画的属性。 你可以将动画连接到你计划设置动画的属性，方法为在该属性所属的 [**Visual**](https://msdn.microsoft.com/library/windows/apps/Dn706858) 对象上 调用 [**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840)。

常规语法和示例如下：

```cs
targetVisual.StartAnimation(“Offset”, animation);
```

在启动动画后，仍然能够使其停止和断开连接。 这将通过使用 [**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841) 方法和指定需要停止设置动画的属性来实现。

例如：

```cs
targetVisual.StopAnimation(“Offset”);
```

### 动画完成事件

与表达式动画不同，关键帧动画定义了结尾部分，这是有条件的。 开发人员可以确定成组的或单个关键帧动画何时使用批完成操作。 可根据批对象类型创建或检索批，它们可用于聚合动画的结束状态。 Scoped 批将在检索提交批时进行创建，这些不同批的使用将在后面进行介绍。

当批内的所有动画都完成时，将引发批完成事件。 引发批的完成事件所需的时间取决于该批中时长最长的动画或 延迟最为严重的动画。 在你需要了解选定的动画组将于何时完成以便计划其他工作时，聚合结束状态非常有用。

### Scoped 批

若要聚合特定动画组或单个动画，首先需要通过调用 [**CreateScopedBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589425) 来创建 Scoped 批。

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
```

创建 Scoped 批后，所有启动的动画均将聚合，直至使用 [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) 或 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) 显式暂停或停止该批。

调用 [**Suspend**](https://msdn.microsoft.com/library/windows/apps/Mt590848) 将停止聚合动画结束状态， 直到调用 [**Resume**](https://msdn.microsoft.com/library/windows/apps/Mt590847)。 这将使你能够显式从给定批中排除内容。

```cs
myScopedBatch.Suspend();
```

要完成批操作，必须调用 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844)。 如果不进行 End 调用，该批将保持打开永久收集对象。

```cs
myScopedBatch.End();
```

如果你想要了解单个动画什么时候结束，则需要创建 Scoped 批、启动动画并停止批。

例如：

```cs
myScopedBatch = _compositor.CreateScopedBatch(CompositionBatchTypes.Animation);
Visual.StartAnimation(“Opacity”, myAnimation);
myScopedBatch.End();
```

### Commit 批

Commit 批是提交周期内隐式创建的批。 当前 Commit 批可以通过在提交周期内随时调用 [**GetCommitBatch**](https://msdn.microsoft.com/library/windows/apps/Mt589430) 进行检索。 提交周期定义为从合成器更新的这段时间。 更新将排队等待，直到系统准备好处理所做的更改并将位绘制到屏幕。 标题无法控制何时进行提交。 在提交周期内 Commit 批将聚合所有对象，这将在 GetCommitBatch 调用后执行。 每个提交周期内只有一个 Commit 批，且无需在 Commit 批上显式调用 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844)。

```cs
myCommitBatch = _compositor.GetCommitBatch(CompositionBatchTypes.Animation);
```

### 批状态

若要确定某个批是否对聚合动画打开，你可以使用 **IsActive** 属性。 如果 **IsEnded** 属性返回 true，你将无法向该特定批添加动画。 当 Scoped 批已通过调用 [**End**](https://msdn.microsoft.com/library/windows/apps/Mt590844) 方法显式终止时，这些批将处于“终止”状态。 当前提交周期结束时，Commit 批将终止。

## 表达式动画

表达式动画是将数学表达式用于指定已设置动画的值应如何针对每个帧进行计算的动画。 表达式语言本身可以从其他合成对象引用属性。 与关键帧动画不同，表达式动画并非基于时间的动画，它们可针对每个帧进行处理（如有必要）。 在描述可以由合成引擎处理的更复杂的用户体验（例如粘滞标头和视差）时，表达式非常有用。

### 创建你的表达式

若要创建你的表达式，请在 Compositor 对象上调用 [**CreateExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt187002) 并 指定你想要使用的表达式：

```cs
var expression = _compositor.CreateExpressionAnimation(“INSERT_EXPRESSION_STRING”)
```

### 运算符、优先级和结合性

表达式语言支持以下运算符，并遵循运算符优先级和结合性，如 C# 语言规范中所定义：

-   一元 (-)
-   乘法 (\* /)
-   加法 (- +)

表达式语言还支持逐分量数学运算的概念。 这些成员访问 (x.y) 运算符仅在“基元”类型（矢量和矩阵）上受支持。 例如：

```cs
Offset.x + 5.0
```

### 属性参数

表达式语言的功能强大的组件之一是能够在表达式中将参数用作变量。 表达式字符串可以包含计算时可替换为常量值的参数或者可引用其他对象属性值的参数。 例如：

```cs
ChildVisual.Offset.X / ParentVisual.Offset.Y
```

在此示例中，ChildOffset 和 ParentOffset 是用于表示两种视觉对象的偏移属性的参数。 使用 [**CompositionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706708) 类的 **Set\*Parameter** 方法定义参数：

-   若要创建一个矢量参数，你可以使用 [**SetVector2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector2parameter.aspx)、[**SetVector3Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setvector3parameter.aspx) 或 [**SetVector4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setvector4parameter)。
-   若要创建一个矩阵参数，你可以使用 [**SetMatrix3x2Parameter**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix3x2parameter.aspx) 或 [**SetMatrix4x4Parameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setmatrix4x4parameter)。
-   若要创建一个标量参数，你可以使用 [**SetScalarParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setscalarparameter)。
-   若要创建一个颜色参数，你可以使用 [**SetColorParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setcolorparameter)。
-   若要创建一个四元数参数，你可以使用 [**SetQuaternionParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setquaternionparameter)。
-   若要创建一个引用参数 ，你可以使用 [**SetReferenceParameter**](https://msdn.microsoft.com/library/windows/apps/windows.ui.composition.compositionanimation.setreferenceparameter)。

在上述表达式字符串中，我们需要创建两个参数来定义两个视觉对象：

```cs
Expression.SetReferenceParameter(“ChildVisual”, childVisual);
Expression.SetReferenceParameter(“ParentVisual”, parentVisual);
```

### 表达式 Helper 函数

除了具有对运算符和属性参数的访问权限外，你还具有对表达式语言的完整Helper 函数库的访问权限。 下面列出了几个 Helper 函数，不过你可以在 [**ExpressionAnimation**](https://msdn.microsoft.com/library/windows/apps/Dn706817) 类的备注部分中找到 Helper 函数的完整列表：

-   Max (Scalar value1, Scalar value2)
-   Scale (Vector3 value, Scalar factor)
-   Transform(Vector2 value, Matrix 3x2 matrix)

下面是一个使用 Clamp Helper 函数的更复杂的表达式字符串示例：

```cs
Clamp((scroller.Offset.y * -1.0) – container.Offset.y, 0, container.Size.y – header.Size.y)
```

### 启动和停止表达式动画

若要启动和停止表达式动画，请使用与处理关键帧动画时相同的函数（[**StartAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590840)、[**StopAnimation**](https://msdn.microsoft.com/library/windows/apps/Mt590841)）。 注意：表达式动画将继续运行，直到使用 **StopAnimation** 使其停止为止：与关键帧动画不同，它们的时长不限。

### 属性集

除了能够引用其他合成对象的属性外， 你还可以创建和存储你自己的可跨多个动画使用的属性值。 属性集由 [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772) 类表示。 借助属性集，你可以创建具有某个值的属性并稍后在表达式中引用它，甚至可以连接它作为关键帧动画的目标。

若要创建属性集，请使用合成器类的 [**CreatePropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706802) 方法。 例如：

```cs
_sharedProperties = _compositor.CreatePropertySet();
```

在创建属性集后，你可以使用 [**CompositionPropertySet**](https://msdn.microsoft.com/library/windows/apps/Dn706772) 的 **Insert\*** 方法向其中添加属性和值。 例如：

```cs
_sharedProperties.InsertVector3(“NewOffset”, offset);
```

在创建表达式动画后，你可以使用某一引用参数从表达式字符串中的属性集引用属性。 例如：

```cs
var expression = _compositor.CreateExpressionAnimation(“sharedProperties.NewOffset”);
expression.SetReferenceParameter(“sharedProperties”, _sharedProperties);
```

### 表达式关键帧

在本文档的前面部分，我们介绍了如何创建关键帧动画及其各自的关键帧。 除了使用标准化的时间和值组件定义传统的关键帧外，你还可以定义表达式关键帧。

与其在关键帧中针对特定时间定义一个显式值，我们还不如使用表达式语法来描述如何在关键帧时间线的此特定 点计算值。 你可以将表达式关键帧与关键帧动画中的基本关键帧混合和匹配。

### 构建表达式关键帧

与传统关键帧类似，表达式关键帧可通过指定两个组件进行构建：

-   时间：介于 0.0 和 1.0 之间关键帧的标准化时间状态
-   值：用来描述应如何计算值的表达式字符串

例如，以下代码段将常规关键帧与表达式关键帧结合使用：

```cs
var animation = _compositor.CreateScalarKeyFrameAnimation();
animation.InsertExpressionKeyFrame(0.25, “VisualBOffset.X / VisualAOffset.Y”);
animation.InsertKeyFrame(1.00f, 0.8f);
```

### 使用当前值和起始值

在表达式语言中，可以同时引用动画的当前值和起始值。 在表达式语言中，这些值均采用前缀“this”：

-   this.CurrentValue
-   this.StartingValue

在表达式关键帧中使用上述值的示例如下：

```cs
animation.InsertExpressionKeyFrame(0.0f, “this.CurrentValue + delta”);
```

### 条件表达式

除了支持基本的数学表达式外，你也可以使用三元运算符定义条件表达式：

```cs
(condition ? first_expression : second_expression)
```

在表达式本身中，以下条件运算符在条件语句中受支持：

-   等于 (==)
-   不等于 (!=)
-   小于 (<)
-   小于或等于 (<=)
-   大于 (>)
-   大于或等于 (>=)

最后，支持将以下合取作为条件语句中的运算符或函数：

-   Not: ! / Not(bool1)
-   And: && / And(bool1, bool2)
-   Or: || / Or(bool1, bool2)

以下代码段显示了在表达式中使用条件语句的示例：

```cs
var expression = _compositor.CreateExpressionAnimation(“target.Offset.x > 50 ? 0.0f +   (target.Offset.x / parent.Offset.x) : 1.0f”);
```

 

 






<!--HONumber=Mar16_HO1-->


