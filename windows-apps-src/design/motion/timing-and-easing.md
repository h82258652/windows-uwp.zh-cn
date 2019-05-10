---
Description: 了解如何 Fluent motion 使用计时和缓动函数。
title: 计时和缓动 - UWP 应用中的动画
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
ms.openlocfilehash: b736a10a7284e3cc9aa193e082dc654e908afe40
ms.sourcegitcommit: cc0ef75f314658b14376eb60ef8e5bb4d7726e04
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2019
ms.locfileid: "65444180"
---
# <a name="timing-and-easing"></a>计时和缓动

虽然运动以现实世界为基础，但我们同时也是一个数字媒体，提供你期待的速度和效果。

## <a name="examples"></a>示例

<table>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果有<strong style="font-weight: semi-bold">XAML 控件库</strong>应用程序安装，请单击此处<a href="xamlcontrolsgallery:/item/EasingFunction">打开应用并查看在操作中的缓动函数</a>。</p>
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
用于对象或退出场景或关闭页面。
允许计时不会妨碍帧速率实现流畅动画的退出 UI 呈现非常快速的方向反馈。
    :::column-end:::
    :::column:::
        ![150ms motion](images/150msAlt.gif)
    :::column-end:::
:::row-end:::

### <a name="300ms-enter"></a>**300 毫秒**（进入）

:::row:::
    :::column:::
用于对象或输入在场景或打开的页面。
允许内容进入场景时留有欢迎内容的合理时间量。
    :::column-end:::
    :::column:::
        ![300ms motion](images/300ms.gif)
    :::column-end:::
:::row-end:::

### <a name="500ms-move"></a>**≤500 毫秒**（移动）

:::row:::
    :::column:::
用于在一个场景或多个场景之间转换的对象。 
    :::column-end:::
    :::column:::
        ![500ms motion](images/500ms.gif)
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
使用 UI 或在退出场景的对象。

对象成为提供支持，并获得动力，直到其达到转义速度。
生成的感觉是对象尝试难度最大利用用户的方式，并为新内容进入留出空间。
    :::column-end:::
    :::column:::
        ![accelerate easing](images/accelEase.gif)
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
用于对象或 UI 输入场景中，导航或生成。

一旦场景，该对象是满足极端阻力，这会降低 rest 的对象。
该对象从长时间距离之外的行程和极端的速度，在输入或是快速回到 rest 状态，生成的感觉。

即使它前面有片刻无响应，传入对象的速度起作用的感觉快速且高度可响应。
    :::column-end:::
    :::column:::
        ![decelerate easing](images/decelEase.gif)
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
这是在系统内任何经过动画处理的参数更改缓动的基线。
为屏幕上发生状态改变（如简单的位置改变）的对象使用标准缓动。 此外，为在场景内变形（如生长的对象）的对象使用此缓动。

生成的感觉是更改状态从 A 到 B 的对象用来解决，并会强制执行由，自然。
    :::column-end:::
    :::column:::
        ![standard easing](images/standardEase.gif)
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

- [Motion 概述](index.md)
- [方向性和重力](directionality-and-gravity.md)
