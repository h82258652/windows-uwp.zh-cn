---
author: Jwmsft
Description: "将下拉刷新模式与列表视图结合使用。"
title: "下拉刷新"
label: Pull-to-refresh
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: aaeb1e74-b795-4015-bf41-02cb1d6f467e
pm-contact: predavid
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.openlocfilehash: 51a8c9a2e4618e054374308918a74cf2095119ef
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2017
---
# <a name="pull-to-refresh"></a>下拉刷新

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

下拉刷新模式允许用户使用触摸来下拉数据列表，以检索更多数据。 下拉刷新已在移动应用上广泛使用，但在任何具有触摸屏的设备上也很有用。 可以处理[操作事件](../input-and-devices/touch-interactions.md#manipulation-events)以在应用中实现下拉刷新。

> **重要 API**：[ListView 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)，[GridView 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)

[下拉刷新示例](http://go.microsoft.com/fwlink/p/?LinkId=620635)展示了如何扩展 [ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx) 控件以支持这种模式。 在本文中，我们使用此示例解释实现下拉刷新的关键点。

![下拉刷新示例](images/ptr-phone-1.png)

## <a name="is-this-the-right-pattern"></a>这是正确的模式吗？

如果你拥有用户希望定期刷新的数据列表或网格，并且应用可能在触摸优先的移动设备上运行，请使用下拉刷新模式。

## <a name="implement-pull-to-refresh"></a>实现下拉刷新

若要实现下拉刷新，需要处理操作事件来检测用户下拉列表的时间、提供视觉反馈，并刷新数据。 此处，我们看一下[下拉刷新示例](http://go.microsoft.com/fwlink/p/?LinkId=620635)中实现此操作的方法。 我们在此处不会展示所有代码，你可以下载示例或在 GitHub 上查看代码。

下拉刷新示例创建了名为 `RefreshableListView` 的自定义控件，用于扩展 **ListView** 控件。 此控件添加了刷新指示器来提供视觉反馈，并处理了列表视图内部滚动查看器上的操作事件。 它还添加了 2 个事件，以便通知你何时列表下拉以及何时应刷新数据。 RefreshableListView 仅提供关于应刷新的数据的通知。 需要处理应用中的事件以更新数据，并且每个应用的代码都不同。

RefreshableListView 提供“自动刷新”模式，用于确定请求刷新的时间和刷新指示器超出视图的方式。 可以打开或关闭自动刷新。
- 关闭：仅在超出 `PullThreshold` 的同时释放列表才请求刷新。 当用户释放滚动条时，指示器会以动画方式退出视图。 如果它（在手机上）可用，将显示状态栏指示器。
- 打开：在超出 `PullThreshold` 时立即请求刷新，无论是否已释放列表。 指示器仍保留在视图中，直到检索到新数据，之后以动画方式退出视图。 **Deferral** 用于在提取数据完成时通知应用。

> **注意**&nbsp;&nbsp;示例中的代码也适用于 [GridView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.gridview.aspx)。 若要修改 GridView，请从 GridView（而非 ListView）派生自定义类，并修改默认 GridView 模板。

## <a name="add-a-refresh-indicator"></a>添加刷新指示器

务必向用户提供视觉反馈，以便让他们知道应用支持下拉刷新。 RefreshableListView 具有 `RefreshIndicatorContent` 属性，可支持在 XAML 中设置指示器视觉效果。 如果未设置 `RefreshIndicatorContent`，则它还包括回退的默认文本指示器。

以下是推荐的刷新指示器指南。

![刷新指示器红线](images/ptr-redlines-1.png)

**修改列表视图模板**

在下拉刷新示例中，`RefreshableListView` 控件模板通过添加刷新指示器来修改标准 **ListView** 模板。 刷新指示器放置在 [ItemsPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemspresenter.aspx) 上的 [Grid](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.grid.aspx) 中，它是显示列表项目的部件。

> **注意**&nbsp;&nbsp;`DefaultRefreshIndicatorContent` 文本框提供仅在没有设置 `RefreshIndicatorContent` 属性时显示的文本回退指示器。

以下是在默认 ListView 模板中修改的控件模板的部件。

**XAML**
```xaml
<!-- Styles/Styles.xaml -->
<Grid x:Name="ScrollerContent" VerticalAlignment="Top">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
        <RowDefinition Height="Auto"/>
    </Grid.RowDefinitions>
    <Border x:Name="RefreshIndicator" VerticalAlignment="Top" Grid.Row="1">
        <Grid>
            <TextBlock x:Name="DefaultRefreshIndicatorContent" HorizontalAlignment="Center" 
                       Foreground="White" FontSize="20" Margin="20, 35, 20, 20"/>
            <ContentPresenter Content="{TemplateBinding RefreshIndicatorContent}"></ContentPresenter>
        </Grid>
    </Border>
    <ItemsPresenter FooterTransitions="{TemplateBinding FooterTransitions}" 
                    FooterTemplate="{TemplateBinding FooterTemplate}" 
                    Footer="{TemplateBinding Footer}" 
                    HeaderTemplate="{TemplateBinding HeaderTemplate}" 
                    Header="{TemplateBinding Header}" 
                    HeaderTransitions="{TemplateBinding HeaderTransitions}" 
                    Padding="{TemplateBinding Padding}"
                    Grid.Row="1"
                    x:Name="ItemsPresenter"/>
</Grid>
```

**在 XAML 中设置内容**

在 XAML 中为列表视图设置刷新指示器的内容。 所设置的 XAML 内容由刷新指示器的 [ContentPresenter](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.contentpresenter.aspx) (`<ContentPresenter Content="{TemplateBinding RefreshIndicatorContent}">`) 显示。 如果不设置此内容，将改为显示默认文本指示器。

**XAML**
```xaml
<!-- MainPage.xaml -->
<c:RefreshableListView
    <!-- ... See sample for removed code. -->
    AutoRefresh="{x:Bind Path=UseAutoRefresh, Mode=OneWay}"
    ItemsSource="{x:Bind Items}"
    PullProgressChanged="listView_PullProgressChanged"
    RefreshRequested="listView_RefreshRequested">

    <c:RefreshableListView.RefreshIndicatorContent>
        <Grid Height="100" Background="Transparent">
            <FontIcon
                Margin="0,0,0,30"
                HorizontalAlignment="Center"
                VerticalAlignment="Bottom"
                FontFamily="Segoe MDL2 Assets"
                FontSize="20"
                Glyph="&#xE72C;"
                RenderTransformOrigin="0.5,0.5">
                <FontIcon.RenderTransform>
                    <RotateTransform x:Name="SpinnerTransform" Angle="0" />
                </FontIcon.RenderTransform>
            </FontIcon>
        </Grid>
    </c:RefreshableListView.RefreshIndicatorContent>
    
    <!-- ... See sample for removed code. -->

</c:RefreshableListView>
```

**设置微调框的动画**

下拉列表时，将发生 RefreshableListView 的 `PullProgressChanged` 事件。 可以在应用中处理此事件以控制刷新指示器。 在该示例中，此情节提要开始设置指示器 [RotateTransform](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.rotatetransform.aspx) 的动画，并旋转刷新指示器。 

**XAML**
```xaml
<!-- MainPage.xaml -->
<Storyboard x:Name="SpinnerStoryboard">
    <DoubleAnimation
        Duration="00:00:00.5"
        FillBehavior="HoldEnd"
        From="0"
        RepeatBehavior="Forever"
        Storyboard.TargetName="SpinnerTransform"
        Storyboard.TargetProperty="Angle"
        To="360" />
</Storyboard>
```

## <a name="handle-scroll-viewer-manipulation-events"></a>处理滚动查看器操作事件

列表视图控件模板包括支持用户滚动列表项目的内置 [ScrollViewer](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.aspx)。 若要实现下拉刷新，必须处理内置滚动查看器上的操作事件和几个相关事件。 有关操作事件的详细信息，请参阅[触摸交互](../input-and-devices/touch-interactions.md)。

** OnApplyTemplate**

若要访问滚动查看器和其他模板部件，以便可以添加事件处理程序并在以后在代码中调用它们，必须替代 [OnApplyTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.onapplytemplate.aspx) 方法。 在 OnApplyTemplate 中，可以调用 [GetTemplateChild](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.gettemplatechild.aspx) 以获取控件模板中已命名部件的引用，可保存该引用以待以后在代码中使用。

在该示例中，用于存储模板部件的变量已在私有变量区域中声明。 在 OnApplyTemplate 方法中进行检索后，为 [DirectManipulationStarted](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.directmanipulationstarted.aspx)、[DirectManipulationCompleted](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.directmanipulationcompleted.aspx)、[ViewChanged](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.viewchanged.aspx) 和 [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) 事件添加事件处理程序。

**DirectManipulationStarted**

为了启动下拉刷新操作，在用户开始下拉时，内容必须滚动到滚动查看器的顶部。 否则，系统将假设用户拉动是为了向上平移列表。 此处理程序中的代码确定操作是否从滚动查看器的顶部内容开始，并且确定是否会刷新列表。 控件的“可刷新”状态也会相应地进行设置。 

如果控件可以刷新，则还可以添加动画的事件处理程序。

**DirectManipulationCompleted**

当用户停止下拉列表时，此处理程序中的代码会检查在操作期间是否激活了刷新。 如果刷新已激活，将引发 `RefreshRequested` 事件并执行 `RefreshCommand` 命令。

还将删除动画的事件处理程序。

根据 `AutoRefresh` 属性的值，列表可立即以动画形式备份，或者等到刷新完成后再以动画形式备份。 [Deferral](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx) 对象用于标记刷新已完成。 此时将隐藏刷新指示器 UI。

DirectManipulationCompleted 事件处理程序中的这个部件可以引发 `RefreshRequested` 事件，并在需要时获取 Deferral。

**C#**
```csharp
if (this.RefreshRequested != null)
{
    RefreshRequestedEventArgs refreshRequestedEventArgs = new RefreshRequestedEventArgs(
        this.AutoRefresh ? new DeferralCompletedHandler(RefreshCompleted) : null);
    this.RefreshRequested(this, refreshRequestedEventArgs);
    if (this.AutoRefresh)
    {
        m_scrollerContent.ManipulationMode = ManipulationModes.None;
        if (!refreshRequestedEventArgs.WasDeferralRetrieved)
        {
            // The Deferral object was not retrieved in the event handler.
            // Animate the content up right away.
            this.RefreshCompleted();
        }
    }
}
```

**ViewChanged**

有两种情形需要在 ViewChanged 事件处理程序中进行处理。

第一，如果视图由于滚动查看器缩放而更改，则取消控件的“可刷新”状态。

第二，如果内容在自动刷新结束时完成动画设置，则隐藏填充矩形、重新启用与滚动查看器的触摸交互，并且 [VerticalOffset](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.scrollviewer.verticaloffset.aspx) 设为 0。

**PointerPressed**

下拉刷新仅在通过触摸操作下拉列表时发生。 在 PointerPressed 事件处理程序中，代码会检查引发此事件的指针类型，并设置变量 (`m_pointerPressed`) 以指示它是否是触摸指针。 此变量在 DirectManipulationStarted 处理程序中使用。 如果指针不是触摸指针，无需执行任何操作便会返回 DirectManipulationStarted 处理程序。

## <a name="add-pull-and-refresh-events"></a>添加下拉刷新事件

“RefreshableListView”添加可以在应用中处理的 2 个事件，以刷新数据和管理刷新指示器。

有关事件的详细信息，请参阅[事件和路由事件概述](https://msdn.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview)。

**RefreshRequested**

“RefreshRequested”事件会通知应用用户已拉动列表以刷新它。 处理此事件可提取新数据，并更新列表。

以下是来自示例的事件处理程序。 请务必注意，它检查列表视图的 `AutoRefresh` 属性，并在 Deferral 为 **true** 时获取它。 凭借 Deferral，在刷新完成前，系统不会停用和隐藏刷新指示器。

**C#**
```csharp
private async void listView_RefreshRequested(object sender, RefreshableListView.RefreshRequestedEventArgs e)
{
    using (Deferral deferral = listView.AutoRefresh ? e.GetDeferral() : null)
    {
        await FetchAndInsertItemsAsync(_rand.Next(1, 5));

        if (SpinnerStoryboard.GetCurrentState() != Windows.UI.Xaml.Media.Animation.ClockState.Stopped)
        {
            SpinnerStoryboard.Stop();
        }
    }
}
```

**PullProgressChanged**

在该示例中，提供了刷新指示器的内容，并且该内容由应用控制。 “PullProgressChanged”事件将在用户拉动列表时通知应用，以便你可以启动、停用和重置刷新指示器。 

## <a name="composition-animations"></a>合成动画

默认情况下，当滚动条到达顶部时，滚动查看器中的内容将停止。 若要使用户继续下拉列表，需要访问可视化层，并设置列表内容的动画。 该示例将[合成动画](https://msdn.microsoft.com/windows/uwp/composition/composition-animation)用于此操作；具体而言，使用的是[表达式动画](https://msdn.microsoft.com/windows/uwp/composition/composition-animation#expression-animations)。

在该示例中，这项工作主要是使用 `CompositionTarget_Rendering` 事件处理程序和 `UpdateCompositionAnimations` 方法来完成。

## <a name="related-articles"></a>相关文章

- [设置控件样式](styling-controls.md)
- [触摸交互](../input-and-devices/touch-interactions.md)
- [列表视图和网格视图](listview-and-gridview.md)
- [列表视图项模板](listview-item-templates.md)
- [表达式动画](https://msdn.microsoft.com/windows/uwp/composition/composition-animation#expression-animations)