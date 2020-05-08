---
Description: 了解流畅运动如何使用计时和缓动函数。
title: 计时和缓动
label: Timing and easing
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 098a75da573a977aa393197a61a62b0337f0dc06
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970502"
---
# <a name="timing-and-easing"></a>计时和缓动

虽然运动以现实世界为基础，但我们同时也是一个数字媒体，提供你期待的速度和效果。

## <a name="examples"></a>示例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果安装了<strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/EasingFunction">打开应用，并查看缓动函数的操作</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="how-fluent-motion-uses-time"></a>Fluent 运动如何使用时间

计时是让对象进入、退出或在 UI 内移动的运动感觉很自然的重要元素。

1. 进入视图的对象或场景很快，但是一种欢迎形式。 这些动画的持续时间通常比退出更长，以允许分层构建场景。
1. 退出视图的对象或场景非常快。 用户应该能够了解 UI 的去向。 但是，UI 消除后，它应该退出。
1. 跨场景转换的对象的持续时间应与它们移动的距离相对应。

## <a name="timing-in-fluent-motion"></a>Fluent 运动的计时

Fluent 的运动计时以 500 毫秒（或二分之一秒）作为基准，因为这是用户即时感知的最长时间。

![主图](images/time.gif)

### <a name="150ms-exit"></a>**150 毫秒**（退出）

:::row:::
    :::column:::
用于正在退出场景或关闭的对象或页面。
允许计时不会妨碍帧速率实现流畅动画的退出 UI 呈现非常快速的方向反馈。
    :::column-end:::
    :::column:::
        ![150ms 动作](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300 毫秒**（进入）

:::row:::
    :::column:::
用于进入场景或打开的对象或页面。
允许内容进入场景时留有欢迎内容的合理时间量。
    :::column-end:::
    :::column:::
        ![300 ms 运动](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤500 毫秒**（移动）

:::row:::
    :::column:::
用于在单个场景或多个场景间平移的对象。 
    :::column-end:::
    :::column:::
        ![500毫秒动作](images/500ms.gif)
    :::column-end:::
:::row-end:::

## <a name="easing-in-fluent-motion"></a>Fluent 运动的缓动

缓动是对对象的移动速度进行操作的方法。 它是将所有 Fluent 运动体验绑定在一起的纽带。 虽然比较极端，但系统中使用的缓动有助于统一对象在整个系统中移动的物理感觉。 这是模拟现实世界的一种方式，让运动中的对象感觉好像与其所在的环境融为一体。

![主图](images/easing.gif)

## <a name="apply-easing-to-motion"></a>对运动应用缓动

这些缓动将帮助你实现更自然的感觉，而且是我们用于 Fluent 运动的基准。

这些代码示例显示了如何向 Storyboard 动画 (XAML) 或合成动画 (C#) 应用推荐的缓动值。

### <a name="accelerate-exit"></a>**加快**（退出）

:::row:::
    :::column:::
用于 UI 或正在退出场景的对象。

对象处于开启状态并获得动力，直到达到 escape 速度。
所产生的结果是，该对象正在尝试最难进入用户的方式，并为新内容留出空间。
    :::column-end:::
    :::column:::
        ![加速缓动](images/accelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.7 , 0 , 1 , 0.5)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.15">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="4.5" EasingMode="EaseIn" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction accelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.7f, 0.0f), new Vector2(1.0f, 0.5f));
_exitAnimation = _compositor.CreateScalarKeyFrameAnimation();
_exitAnimation.InsertKeyFrame(0.0f, _startValue);
_exitAnimation.InsertKeyFrame(1.0f, _endValue, accelerate);
_exitAnimation.Duration = TimeSpan.FromMilliseconds(150);
```

### <a name="decelerate-enter"></a>**减速**（进入）

:::row:::
    :::column:::
用于对象或 UI 进入场景，即导航或生成。

一旦出现在场景中，就会通过极端的摩擦来满足对象，这会减缓对象的静止。
产生的结果是，对象从远处开始，并以极快的速度进入，或快速返回到 rest 状态。

即使前面有无响应，传入对象的速度也会影响感觉速度快且响应迅速。
    :::column-end:::
    :::column:::
        ![减速缓动](images/decelEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.1 , 0.9 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.3">
        <DoubleAnimation.EasingFunction>
            <ExponentialEase Exponent="7" EasingMode="EaseOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction decelerate =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.1f, 0.9f), new Vector2(0.2f, 1.0f));
_enterAnimation = _compositor.CreateScalarKeyFrameAnimation();
_enterAnimation.InsertKeyFrame(0.0f, _startValue);
_enterAnimation.InsertKeyFrame(1.0f, _endValue, decelerate);
_enterAnimation.Duration = TimeSpan.FromMilliseconds(300);
```

### <a name="standard-easing-move"></a>**标准缓动**（移动）

:::row:::
    :::column:::
这是系统内任何动画参数更改的基线缓动。
为屏幕上发生状态改变（如简单的位置改变）的对象使用标准缓动。 此外，为在场景内变形（如生长的对象）的对象使用此缓动。

结果就是，将状态从 A 更改为 B 的对象会得到克服，并被自然力接管。
    :::column-end:::
    :::column:::
        ![标准缓动](images/standardEase.gif)
    :::column-end:::
:::row-end:::

```
cubic-bezier(0.8 , 0 , 0.2 , 1)
```

```xaml
<!-- Use for XAML Storyboard animations. -->
<Storyboard x:Name="Storyboard">
    <DoubleAnimation Storyboard.TargetName="Translation" Storyboard.TargetProperty="X" From="0" To="200" Duration="0:0:0.5">
        <DoubleAnimation.EasingFunction>
            <CircleEase EasingMode="EaseInOut" />
        </DoubleAnimation.EasingFunction>
    </DoubleAnimation>
</Storyboard>
```

```csharp
// Use for Composition animations.
CubicBezierEasingFunction standard =
    _compositor.CreateCubicBezierEasingFunction(new Vector2(0.8f, 0.0f), new Vector2(0.2f, 1.0f));
 _moveAnimation = _compositor.CreateScalarKeyFrameAnimation();
 _moveAnimation.InsertKeyFrame(0.0f, _startValue);
 _moveAnimation.InsertKeyFrame(1.0f, _endValue, standard);
 _moveAnimation.Duration = TimeSpan.FromMilliseconds(500);
```

## <a name="related-articles"></a>相关文章

- [运动概述](index.md)
- [方向性和重力](directionality-and-gravity.md)
