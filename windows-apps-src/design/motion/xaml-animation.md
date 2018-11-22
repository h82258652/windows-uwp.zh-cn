---
author: Jwmsft
ms.assetid: 0C8DEE75-FB7B-4E59-81E3-55F8D65CD982
title: 动画概述
description: 使用 Windows 运行时动画库中的动画将 Windows 外观集成到应用中。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d7c3c4a9e46ce38298d7dcdd50477c4de0e9960c
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/22/2018
ms.locfileid: "7578091"
---
# <a name="animations-in-xaml"></a>XAML 中的动画

UWP 动画可通过添加动作和交互性来增强应用。 通过使用 Windows 运行时动画库中的动画，你可以将其 Windows 外观集成到你的应用中。 本主题提供了使用每个动画的典型方案的动画和示例汇总。

> [!TIP]
> 使用 XAML 的 Windows 运行时控件包含某些类型的动画作为动画库中的内置行为。 通过在你的应用中使用这些控件，无需自己编程，即可获得动画外观。

来自 Windows 运行时动画库的动画提供以下优势：

-   符合[动画指南](https://msdn.microsoft.com/library/windows/apps/Dn611854)的运动
-   在 UI 状态之间进行快速流畅的过渡，使用户知晓但不会分心
-   向用户指示应用内过渡的视觉行为

例如，当用户将某个项添加到列表时，新项不会立即出现在列表中，而是采用动画形式过渡到相应的位置。 在很短的时间内，列表中的其他项也会采用动画形式移动到它们的新位置，以便为添加的项腾出空间。 此处的过渡行为可使控件更明显地与用户交互。

Windows 10 版本 1607 引入了一个用于实现动画的新 [**ConnectedAnimationService**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx) API，其中某一元素似乎在导航期间在视图之间进行动画处理。 此 API 具有不同于其他动画库 API 的使用模式。 将在[参考页面](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx)中介绍 **ConnectedAnimationService** 的用法。

该动画库不会为每个可能方案提供动画。 在某些情况下，你可能希望采用 XAML 创建自定义动画。 有关详细信息，请参阅[情节提要动画](storyboarded-animations.md)。

此外，对于根据 ScrollViewer 的滚动位置设置项目动画等某些高级方案，开发人员可能希望使用可视化层互操作来实现自定义动画。 有关详细信息，请参阅[可视化层](https://msdn.microsoft.com/windows/uwp/composition/visual-layer)。

## <a name="types-of-animations"></a>动画类型

Windows 运行时动画系统和动画库实现了更大的目标，即允许控件和 UI 的其他部分具备动画行为。 以下是几个不同类型的动画。

-   *主题过渡*会在 UI 中的某些条件（其中涉及预定义 Windows 运行时 XAML UI 类型的控件或元素）更改时自动应用。 这些称为 *“主题过渡”*，原因是动画支持 Windows 外观，并且定义所有应用从一个交互模式更改为另一个交互模式时为特定 UI 方案执行的操作。 主题过渡是动画库的一部分。
-   *“主题动画”* 是具备预定义 Windows 运行时 XAML UI 类型一个或多个属性的动画。 主题动画与主题过渡不同，因为主题动画面向一个特定元素并存在于控件中的特定视觉状态，而主题过渡将分配到存在于视觉状态外部的控件属性并影响这些状态之间的过渡。 许多 Windows 运行时 XAML 控件在情节提要中包含主题动画，这些主题动画是其控件模板的一部分且包含由视觉状态引发的动画。 只要你未修改模板，你都可以将这些内置主题动画用于 UI 中的控件。 但是，如果你替换了模板，则也将删除内置控件主题动画。 若要重新获取它们，你必须在视觉状态的控件集中定义包含主题动画的情节提要。 你也可以从不在视觉状态中的情节提要运行主题动画，并通过 [**Begin**](https://msdn.microsoft.com/library/windows/apps/BR210491) 方法开始运行，但这并不常见。 主题动画是动画库的一部分。
-   *“视觉转换”* 会在控件从其定义的视觉状态之一转换到其他状态时应用。 这些是你编写的自定义动画，通常与为控件编写的自定义模板和该模板中的视觉状态定义相关。 该动画仅在两个状态之间的时间运行，这个时间通常很短，最多几秒钟。 有关详细信息，请参阅[视觉状态的情节提要动画的“视觉转换”部分](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808#VisualTransition)。
-   *情节提要动画*可以随时间推移设置 Windows 运行时依赖属性的值的动画。 情节提要可以定义为可视化过渡的一部分，或者在运行时由应用程序触发。 有关详细信息，请参阅[情节提要动画](storyboarded-animations.md)。 有关依赖属性及其所处位置的详细信息，请参阅[依赖属性概述](https://msdn.microsoft.com/library/windows/apps/Mt185583)。
-   通过新 [**ConnectedAnimationService**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx) API 提供的*已连接动画*允许开发人员轻松创建以下效果：某一元素似乎在导航期间在视图之间进行动画处理。 从 Windows 10 版本 1607 开始，将提供此 API。 有关详细信息，请参阅 [**ConnectedAnimationService**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.connectedanimationservice.aspx)。

## <a name="animations-available-in-the-library"></a>库中提供的动画

动画库提供以下动画。 单击动画的名称即可了解有关其主要使用方案、这些方案的定义方式的详细信息，还可以查看动画示例。

-   [页面过渡](#page-transition)：在 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 中设置页面过渡动画。
-   [内容和进入过渡](#content-transition-and-entrance-transition)：让一条或一组内容以动画方式进入或退出视图。
-   [淡入/淡出和交叉进出](#fade-in-out-and-crossfade)：显示过渡元素或控件，或者刷新内容区域。
-   [指针向上/向下](#pointer-up-down)：提供点击或单击磁贴的视觉反馈。
-   [重新定位](#reposition)：将元素移动到新位置。
-   [显示/隐藏弹出元素](#show-hide-popup)：在视图顶部显示上下文 UI。
-   [显示/隐藏边缘 UI](#show-hide-edge-ui)：将基于边缘的 UI（包括诸如面板的较大 UI）滑入或滑出视图。
-   [列表项更改](#list-item-changes)：从列表中添加或删除某个项目，或者重新排序项目。
-   [拖放](#drag-drop)：在拖放操作期间提供视觉反馈。

### <a name="page-transition"></a>页面过渡

使用页面过渡，在应用中设置导航动画。 由于几乎所有应用均会使用某种类型的导航，所以页面过渡动画是应用使用的最常见主题动画类型。 有关页面过渡 API 的详细信息，请参阅 [**NavigationThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.navigationthemetransition)。



### <a name="content-transition-and-entrance-transition"></a>内容过渡和进入过渡

使用内容过渡动画 ([**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243103)) 可以将一条或一组内容移入或移出当前视图。 例如，内容过渡动画显示在首次加载页面时或者在更改页面某部分的内容时，不准备显示的内容。

[**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) 表示可以在首次加载页面或大部分 UI 时应用到内容的动作。 这样，首次出现的内容可以提供不同于对内容的更改的反馈。 [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) 等同于具有默认参数的 [**NavigationThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.navigationthemetransition)，但可在 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 外部使用。
 
 
<span id="fade-in-out-and-crossfade"/>

### <a name="fade-inout-and-crossfade"></a>淡入/淡出和交叉进出

使用淡入和淡出动画可以显示或隐藏过渡 UI 或控件。 在 XAML 中，这些动画将表示为 [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) 和 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302)。 一个示例就是在可能会由于用户交互而显示新控件的应用栏中。 另一个示例涉及到临时滚动条或平移指示器，它们将在一段时间内未检测到任何用户输入时淡出。 在随着动态加载内容而从占位符项过渡到最终项时，应用还应使用淡入动画。

使用交叉进出动画可以在某个项的状态发生改变时顺利进行过渡；例如，应用刷新视图的当前内容时。 XAML 动画库不提供专用交叉进出动画（不等效于 [**crossFade**](https://msdn.microsoft.com/library/windows/apps/BR212661)），但是你可以使用 [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) 和 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) 与重叠计时来获取相同的效果。

<span id="pointer-up-down"/>

### <a name="pointer-updown"></a>指针向上/向下

使用 [**PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969168) 和 [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969164) 动画可以为用户成功点击或单击磁贴提供反馈。 例如，当用户单击或点击磁贴时，播放指针向下动画。 释放单击或点击后，播放指针向上动画。

### <a name="reposition"></a>重新定位

使用重新定位动画（[**RepositionThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210421) 或 [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429)）可以将某个元素移动到新位置。 例如，在项目控件中移动标题将使用重新定位动画。

<span id="show-hide-popup"/>

### <a name="showhide-popup"></a>显示/隐藏弹出元素

当你在当前视图顶部显示和隐藏 [**Popup**](https://msdn.microsoft.com/library/windows/apps/BR227842) 或类似的上下文 UI 时，请使用 [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210383) 和 [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210391)。 [**PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969172) 是主题过渡，如果要轻型消除弹出元素，它是有用的反馈。

<span id="show-hide-edge-ui"/>

### <a name="showhide-edge-ui"></a>显示/隐藏边缘 UI

使用 [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh702324) 动画可以将较小的基于边缘的 UI 滑入和滑出视图。 例如，在屏幕顶部或底部显示自定义应用栏或在屏幕顶部显示用于错误和警告的 UI 表面时使用这些动画。

使用 [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969160) 动画可显示和隐藏窗格或面板。 此操作适用于基于边缘的较大 UI（例如自定义键盘或任务窗格）。

### <a name="list-item-changes"></a>列表项更改

当你在现有列表中添加或删除某个项目时，将使用 [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047) 动画添加动画的表现方式。 若要添加，过渡将首先重新定位列表中的现有项，以便为新项腾出空间，然后添加新项。 若要删除，过渡将从列表中删除项目，然后在移除删除的项目后，重新定位其余的列表项（如有必要）。

还存在一个当某个项目在列表中更改位置时应用的单独 [**ReorderThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210409)。 创建该过渡动画与使用关联的删除/添加动画删除某个项和将其添加到新位置中不同。

请注意，这些动画包括在默认 [**ListView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx) 模板中，因此如果你已使用这些控件，则无需手动添加这些动画。

<span id="drag-drop"/>

### <a name="dragdrop"></a>拖放

使用拖动动画（[**DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243173)、[**DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243177)）和放置动画 ([**DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243185)) 可以在用户拖放一个项时提供视觉反馈。

在激活时，动画将向用户显示该列表可以在放置的项目周围重新排列。 如果用户需要知道在当前位置放下该项目时会将该项目置于列表中的何处，则这样会很有帮助。 动画会提供视觉反馈，表明正在拖动的项目可以放到列表中的其他两个项目之间，以及这些项目将腾出空间。

## <a name="using-animations-with-custom-controls"></a>将动画与自定义控件结合使用

下表概述了在创建这些 Windows 运行时控件的自定义版本时应当使用的动画建议：

| UI 类型 | 推荐动画 |
|---------|-----------------------|
| 对话框 | [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) 和 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |
| 浮出控件 | [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation.aspx) 和 [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| 工具提示 | [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) 和 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |
| 上下文菜单 | [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popinthemeanimation.popinthemeanimation.aspx) 和 [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.popoutthemeanimation.popoutthemeanimation) |
| 命令栏 | [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.edgeuithemetransition.edgeuithemetransition) |
| 任务窗格或基于边缘的面板 | [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.panethemetransition.panethemetransition) |
| 任意 UI 容器的内容 | [**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.contentthemetransition.contentthemetransition) |
| 适用于控件或不应用其他动画时 | [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.fadeinthemeanimation.fadeinthemeanimation.aspx) 和 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) |

 

## <a name="transition-animation-examples"></a>过渡动画示例

理想情况下，你的应用使用动画来增强用户界面效果，或使它更具吸引力，而不会使用户觉得厌烦。 能够实现此目的的一种方式是为 UI 应用动画过渡，这样如果某些内容进入或离开了屏幕，或者发生了更改，动画可以提示用户注意所发生的更改。 例如，你的按钮可以快速淡入和淡出视图，而不仅仅是显示和消失。 我们创建了很多 API，可用于创建一致的建议或典型动画过渡。 此处的示例显示了如何为按钮应用动画，以使按钮轻快地滑入视图。

```xml
<Button Content="Transitioning Button">
     <Button.Transitions>
         <TransitionCollection> 
             <EntranceThemeTransition/>
         </TransitionCollection>
     </Button.Transitions>
 </Button>
 ```

在此代码中，我们将 [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) 对象添加到按钮的过渡集中。 现在，首次呈现按钮时，它轻快地滑入视图，而不是突兀地直接显示。 你可以在动画对象上设置一些属性，以调整它滑动的距离以及从什么方向滑动，但是它确实是一个针对特定方案的非常简单的 API，只用于做出引人注目的进入效果。

你也可以在应用的风格资源中定义过渡动画主题，这样可以统一地应用效果。 此示例与前一个示例效果相当，只不过它是使用 [**Style**](https://msdn.microsoft.com/library/windows/apps/BR208849) 来进行应用的：

```xml
<UserControl.Resources>
     <Style x:Key="DefaultButtonStyle" TargetType="Button">
         <Setter Property="Transitions">
             <Setter.Value>
                 <TransitionCollection>
                     <EntranceThemeTransition/>
                 </TransitionCollection>
             </Setter.Value>
        </Setter>
    </Style>
</UserControl.Resources>
      
<StackPanel x:Name="LayoutRoot">
    <Button Style="{StaticResource DefaultButtonStyle}" Content="Transitioning Button"/>
</StackPanel>
```

前面的示例为一个单独的控件应用了主题过渡，而在对象容器中应用主题过渡时会更有趣。 这样做时，容器的所有子对象都会参与过渡。 在下面的示例中，[**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) 应用到矩形的 [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 中。

```xml
<!-- If you set an EntranceThemeTransition animation on a panel, the
     children of the panel will automatically offset when they animate
     into view to create a visually appealing entrance. -->        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
            <EntranceThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- The sequence children appear depends on their order in 
         the panel's children, not necessarily on where they render
         on the screen. Be sure to arrange your child elements in
         the order you want them to transition into view. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

[**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) 的多个子矩形以令人视觉愉悦的方式一个接一个过渡到视图中，而如果你将此动画分别应用到每个矩形，这些矩形将一次全部显示过渡到视图。

下面是此动画的演示：

![显示过渡到视图中的子矩形的动画](./images/animation-child-rectangles.gif)

容器的子对象还可以在其中一个或多个子对象更改位置时重新流动。 在下面的示例中，我们将 [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) 应用到多个矩形的网格中。 如果你删除其中一个矩形，其他所有矩形将重新流动到新位置。

```xml
<Button Content="Remove Rectangle" Click="RemoveButton_Click"/>
        
<ItemsControl Grid.Row="1" x:Name="rectangleItems">
    <ItemsControl.ItemContainerTransitions>
        <TransitionCollection>
                    
            <!-- Without this, there would be no animation when items 
                 are removed. -->
            <RepositionThemeTransition/>
        </TransitionCollection>
    </ItemsControl.ItemContainerTransitions>
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapGrid Height="400"/>
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
            
    <!-- All these rectangles are just to demonstrate how the items
         in the grid re-flow into position when one of the child items
         are removed. -->
    <ItemsControl.Items>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
        <Rectangle Fill="Red" Width="100" Height="100" Margin="10"/>
    </ItemsControl.Items>
</ItemsControl>
```

```cs
private void RemoveButton_Click(object sender, RoutedEventArgs e)
{
    if (rectangleItems.Items.Count > 0)
    {    
        rectangleItems.Items.RemoveAt(0);
    }                         
}
```

```cpp
// .h
private:
void RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e);

//.cpp
void BlankPage::RemoveButton_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    if (rectangleItems->Items->Size > 0)
    {    
        rectangleItems->Items->RemoveAt(0);
    }
}
```

你可以将多个过渡动画应用到单个对象或对象容器。 例如，如果你想要矩形列表以动画方式进入视图，而且在更改位置时也采用动画方式，你可以按如下所示应用 [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) 和 [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288)：

```xml
...
<ItemsControl.ItemContainerTransitions>
    <TransitionCollection>
        <EntranceThemeTransition/>                    
        <RepositionThemeTransition/>
    </TransitionCollection>
</ItemsControl.ItemContainerTransitions>
...      
```

在对 UI 元素进行添加、删除、重新排序等操作时，可用多个过渡效果在 UI 元素上创建动画。 这些 API 的名称都包含“ThemeTransition”：

| API | 描述 |
|-----|-------------|
| [**NavigationThemeTransition**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.navigationthemetransition) | 在 [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) 中提供用于页面导航的 Windows 个性化动画。 |
| [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047) | 为控件添加或删除子对象或内容的情况提供动画过渡表现方式。 通常，控件是项目容器。 |
| [**ContentThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243103) | 为控件的内容发生更改的情况提供动画过渡表现方式。 可以在应用 [**AddDeleteThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR243047) 后再应用它。 |
| [**EdgeUIThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh702324) | 为（较小）边缘 UI 过渡提供动画过渡表现方式。 |
| [**EntranceThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210288) | 为控件第一次显示的情况提供动画过渡表现方式。 |
| [**PaneThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969160) | 为（较大边缘 UI）UI 过渡提供动画过渡表现方式。 |
| [**PopupThemeTransition**](https://msdn.microsoft.com/library/windows/apps/Hh969172) | 提供在控件的弹入组件（例如，对象上类似于工具提示的 UI）显示时应用到它们的动画过渡表现方式。 |
| [**ReorderThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210409) | 为列表视图控件项目更改顺序的情况提供动画过渡表现方式。 通常它作为拖放操作的结果出现。 不同的控件和主题可能具有不同的动画特征。 |
| [**RepositionThemeTransition**](https://msdn.microsoft.com/library/windows/apps/BR210429) | 为控件更改位置的情况提供动画过渡表现方式。 |

 

## <a name="theme-animation-examples"></a>主题动画示例

过渡动画的应用很简单。 但是你可能想要对动画效果的计时和顺序进行稍多的控制。 你可以使用主题动画来获得更多的控制，同时对动画的表现方式仍使用一致的主题。 而且，与自定义动画相比，主题动画需要的标记较少。 此处，我们使用 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) 将一个矩形淡出视图。

```xml
<StackPanel>    
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <FadeOutThemeAnimation TargetName="myRectangle" />
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle PointerPressed="Rectangle_Tapped" x:Name="myRectangle"  
              Fill="Blue" Width="200" Height="300"/>
</StackPanel>
```

```cs
// When the user taps the rectangle, the animation begins.
private void Rectangle_Tapped(object sender, PointerRoutedEventArgs e)
{
    myStoryboard.Begin();
}
```

```vb
' When the user taps the rectangle, the animation begins.
Private Sub Rectangle_Tapped(sender As Object, e As PointerRoutedEventArgs)
    myStoryboard.Begin()
End Sub
```

```cpp
//.h
void Rectangle_Tapped(Platform::Object^ sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs^ e);

//.cpp
void BlankPage::Rectangle_Tapped(Object^ sender, PointerRoutedEventArgs^ e)
{
    myStoryboard->Begin();
}
```

与过渡动画不同，主题动画没有自动运行的内置触发器（过渡）。 你必须使用 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 来包含主题动画，才能在 XAML 中定义它。 还可以更改动画的默认表现方式。 例如，可以通过增加 [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) 上的 [**Duration**](https://msdn.microsoft.com/library/windows/apps/BR243207) 时间值来放缓淡出。

**注意**对于显示基本动画技术的用途，我们将使用应用代码启动动画，通过调用[**情节提要**](https://msdn.microsoft.com/library/windows/apps/BR210490)的方法。 你可以使用 [**Begin**](https://msdn.microsoft.com/library/windows/apps/BR210491)、[**Stop**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.stop)、[**Pause**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.pause.aspx) 和 [**Resume**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.storyboard.resume.aspx) **Storyboard** 方法来控制 **Storyboard** 动画的运行方式。 但是，这并不是你将库动画包含在应用中的典型操作。 相反，你通常将库动画集成到应用于控件或元素的 XAML 样式和模板中。 了解模板和视觉状态会稍微复杂一些。 但是我们介绍了你可以如何使用视觉状态中的库动画，并将其作为[视觉状态的情节提要动画](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)主题的一部分。

 

你可以为 UI 元素应用多个其他主题动画以创建动画效果。 这些 API 的名称都包含“ThemeAnimation”：

| API | 说明 |
|-----|-------------|
| [**DragItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243173) | 表示应用到正在拖动的项元素的预配置动画。 |
| [**DragOverThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243177) | 表示应用到位于正在拖动的元素下方的元素的预配置动画。 |
| [**DropTargetItemThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243185) | 应用到潜在的拖放目标元素的预配置动画。 |
| [**FadeInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210298) | 控件第一次出现时应用到控件的预配置不透明度动画。 |
| [**FadeOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210302) | 控件从 UI 中删除或隐藏时应用到控件的预配置不透明度动画。 |
| [**PointerDownThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969164) | 用于用户点击或单击项目或元素操作的预配置动画。 |
| [**PointerUpThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/Hh969168) | 用于在用户点击项目或元素并释放该操作后运行的用户操作的预配置动画。 |
| [**PopInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210383) | 控件的弹入组件显示时应用到它们的预配置动画。 此动画结合了不透明度和转换。 |
| [**PopOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210391) | 控件的弹入组件关闭或删除时应用到它们的预配置动画。 此动画结合了不透明度和转换。 |
| [**RepositionThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210421) | 对象重新放置时该对象的预配置动画。 |
| [**SplitCloseThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210454) | 使用 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx) 打开和关闭样式的动画隐藏目标 UI 的预配置动画。 |
| [**SplitOpenThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/BR210472) | 使用 [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.combobox.aspx) 打开和关闭样式的动画显示目标 UI 的预配置动画。 |
| [**DrillInThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.drillinthemeanimation) | 表示在用户在逻辑层次结构中前进（如从主页到详细信息页）时运行的预配置动画。 |
| [**DrillOutThemeAnimation**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.animation.drilloutthemeanimation.aspx) | 表示在用户在逻辑层次结构中后退（如从详细信息页到主页）时运行的预配置动画。 |

 

## <a name="create-your-own-animations"></a>创建你自己的动画

如果主题动画不能满足你的需求，你可以创建自己的动画。 你可以通过创建一个或多个对象属性值的动画来创建对象的动画。 例如，你可以创建矩形的宽度、[**RotateTransform**](https://msdn.microsoft.com/library/windows/apps/BR242932) 的角度或按钮的颜色值的动画。 我们将此类型的自定义动画称为情节提要动画，用于将其从 Windows 运行时提供为预配置动画类型的库动画中区分出来。 对于情节提要动画，你可以使用可更改特定类型的值的动画（例如 [**DoubleAnimation**](https://msdn.microsoft.com/library/windows/apps/BR243136) 可创建 **Double** 动画），并将该动画放置在 [**Storyboard**](https://msdn.microsoft.com/library/windows/apps/BR210490) 中以对其进行控制。

为了创建动画，要动画显示的属性必须是*依赖属性*。 有关依赖属性的详细信息，请参阅[依赖属性概述](https://msdn.microsoft.com/library/windows/apps/Mt185583)。 有关创建自定义情节提要动画的详细信息，包括如何确定动画目标以及控制动画，请参阅[情节提要动画](storyboarded-animations.md)。

XAML（可以在其中定义自定义情节提要动画）中应用 UI 定义的最大领域是在 XAML 中定义控件的视觉状态。 你执行此操作的原因有两个：你要创建一个新控件类；或者你要重新创建现有控件的模板，且该控件的控件模板中具有视觉状态。 有关详细信息，请参阅[视觉状态的情节提要动画](https://msdn.microsoft.com/library/windows/apps/xaml/JJ819808)。

 

 




