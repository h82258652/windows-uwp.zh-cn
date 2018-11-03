---
author: stevewhims
title: 动画入门
ms.assetid: C1C3F5EA-B775-4700-9C45-695E78C16205
description: 在此项目中，我们将移动一个矩形，应用淡出效果，然后再使其显示在视图中。
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4822a436225bea92fdf1e981ad33378996adefe4
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5979791"
---
# <a name="getting-started-animation"></a>入门：动画


## <a name="adding-animations"></a>添加动画

在 iOS 中，通常可以以编程方式创建动画效果。 例如，你可以使用由基于块的 **UIView** 类的 **animateWithDuration** 方法提供的动画，也可以使用由基于非块的旧方法提供的动画。 或者，你可以明确使用 **CALayer** 类，对层进行动画处理。 Windows 应用中的动画可以使用编程方式创建，但也可以使用 Extensible Application Markup Language (XAML) 以声明方式定义。 可直接使用 Microsoft Visual Studio 编辑 XAML 代码，但是 Visual Studio 还附带了一个名为“Blend”**** 的工具，当你在设计器中处理动画时，你可以使用该工具创建 XAML 代码。 事实上，Blend 允许你以图形方式打开、设计、构建和运行完整的 Visual Studio 项目。 你可以在以下操作实例中尝试此操作。

创建新的通用 Windows 平台 (UWP) 应用，并将其命名为类似“SimpleAnimation”的名称。 在此项目中，我们将移动一个矩形、应用淡出效果，然后再使其显示在视图中。 XAML 中的动画基于*情节提要*概念（请勿和 iOS 情节提要混淆）。 情节提要使用*关键帧*对属性更改制作动画效果。

在项目处于打开状态时，在 **“解决方案资源管理器”** 中右键单击项目名称，然后选择 **“在 Blend 中打开”** 或 **“在 Blend 中设计”**，如下图所示。 Visual Studio 将继续在后台运行。

![“在 Blend 中打开”菜单命令](images/ios-to-uwp/vs-open-in-blend.png)

在 Blend 启动之后，你会看到与下图类似的内容。

![Blend 开发环境](images/ios-to-uwp/blend-1.png)

在左侧的 **“解决方案资源管理器”** 中，双击**MainPage.xaml**。 接下来，从中心 **“设计视图”** 边缘上的工具垂直条上，选择 **“矩形”** 工具，然后在 **“设计视图”** 中绘制矩形，如下图所示。

![向设计视图添加矩形](images/ios-to-uwp/blend-2.png)

若要将矩形着为绿色，请查看 **“属性”** 窗口，在 **“画笔”** 区域单击 **“纯色画笔”** 按钮，然后单击 **“颜色取色器”** 图标。 单击绿色色调带中的某个位置。

若要开始制作矩形动画，请在 **“对象和时间线”** 窗口中点击加号（**“新建”**）按钮（如下图所示），然后点击 **“确定”**。

![添加情节提要](images/ios-to-uwp/blend-3.png)

情节提要在 **“对象和时间线”** 窗口中显示（可能需要调整视图大小以正确查看）。 **“设计视图”** 会显示更改，以指示 **“情节提要 1 时间线记录开启”**。 要捕获矩形的当前状态，请在 **“对象和时间线”** 窗口中，点击黄色箭头正上方的 **“记录关键帧”** 按钮，如下图所示。

![记录关键帧](images/ios-to-uwp/blend-4.png)

现在让我们移动矩形并将其淡出。 为此，拖动橙色/黄色箭头到 2 秒位置，然后稍微向右移动绿色矩形。 接着，在 **“属性”** 窗口的 **“外观”** 区域，将 **“暗度”** 属性更改为 **“0”**，如下图中所示。 若要预览动画，请点击“情节提要”面板中的 **“播放”** 按钮。

![“属性”窗口和“播放”按钮](images/ios-to-uwp/blend-5.png)

接下来，我们让矩形再回到视图中。 在 **“对象和时间线”** 窗口中，双击 **“情节提要 1”**。 然后，在 **“属性”** 窗口的 **“通用”** 区域，选择 **“自动翻转”**，如下图中所示。

![选择一个情节提要](images/ios-to-uwp/blend-6.png)

最后，单击 **“播放”** 按钮看会发生什么。

可单击窗口顶部的绿色运行按钮（或直接按 F5），生成和运行项目。 执行此操作后，将看到项目确实已生成并在运行，但绿色矩形将依旧故我、纹丝不动，就像在超市通道中没得到糖果的幼儿一样。 若要启动动画，需向项目添加一行代码。 操作方法如下。

打开“文件”**** 菜单，选择“保存 MainPage.xaml”****，保存该项目。 返回到 Visual Studio。 如果 Visual Studio 显示对话框，询问是否要重新加载修改的文件，请选择 **“是”**。 双击隐藏在 **MainPage.xaml** 下的 **MainPage.xaml.cs** 文件以打开该文件，并在“public MainPage()”方法上添加以下代码：

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Add the following line of code.
    Storyboard1.Begin();
}
```

再次运行该项目，并观看矩形动画效果。 Hurrah！

如果打开 MainPage.xaml 文件，则会在 **“XAML”** 视图中看到当你在设计器中工作时 Blend 为你添加的 XAML 代码。 特别是要查看 `<Storyboard>` 和 `<Rectangle>` 元素中的代码。 下面的代码显示了一个示例。 为简便起见，椭圆形表示省略的不相关代码，并且为提高代码可读性，添加了换行符。

```xml
...
<Storyboard 
        x:Name="Storyboard1" 
        AutoReverse="True">
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.RenderTransform).(CompositeTransform.TranslateX)"
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="0"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2" 
                Value="185.075"/>
    </DoubleAnimationUsingKeyFrames>
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.RenderTransform).(CompositeTransform.TranslateY)" 
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="0"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2" 
                Value="2.985"/>
    </DoubleAnimationUsingKeyFrames>
    <DoubleAnimationUsingKeyFrames 
            Storyboard.TargetProperty="(UIElement.Opacity)" 
            Storyboard.TargetName="rectangle">
        <EasingDoubleKeyFrame 
                KeyTime="0" 
                Value="1"/>
        <EasingDoubleKeyFrame 
                KeyTime="0:0:2"
                Value="0"/>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
...
<Rectangle 
        x:Name="rectangle" 
        Fill="#FF00FF63" 
        HorizontalAlignment="Left" 
        Height="122" 
        Margin="151,312,0,0" 
        Stroke="Black" 
        VerticalAlignment="Top" 
        Width="239" 
        RenderTransformOrigin="0.5,0.5">
    <Rectangle.RenderTransform>
        <CompositeTransform/>
    </Rectangle.RenderTransform>
</Rectangle>
...
```

可手动编辑此 XAML，或返回到 Blend 以继续在其上执行操作。 Blend 使创建有趣的用户界面变得很有意思，并且使用图形工具设置界面动画的能力可极大加快开发速度。 有关动画的详细信息，请参阅[动画概述](https://msdn.microsoft.com/library/windows/apps/mt187350)。

**注意**适用于<span class="legacy-term">使用 JavaScript 和 HTML 的 UWP 应用</span>的动画信息，请参阅[设置动画的 UI (HTML)](https://msdn.microsoft.com/library/windows/apps/hh465165)。

### <a name="next-step"></a>下一步

[入门：下一步是什么？](getting-started-what-next.md)
