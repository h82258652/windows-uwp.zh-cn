---
title: xLoad 属性
description: xLoad 支持动态创建和销毁元素及其子元素，减少启动时间和内存使用情况。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1fa0f12779ad56d57c92f667443644851dc3d5e5
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8703138"
---
# <a name="xload-attribute"></a>x:Load 属性

你可以使用**X:load**优化启动、 可视化树创建和 XAML 应用的内存使用量。 使用**X:load**具有**可见性**，类似的视觉效果，不同之处在于，元素不加载时，会释放其内存和内部小占位符用于标记它的可视化树中的位置。

使用 X:load 属性化的 UI 元素可以是加载和卸载通过代码，或使用[X:bind](x-bind-markup-extension.md)表达式。 这对于减少偶尔出现或按一定条件出现的元素的成本非常有用。 如网格或 StackPanel 的容器上使用 X:load 时，容器及其所有及其子元素加载或卸载成组。

由 XAML 框架延迟元素的跟踪向每个元素的占位符考虑使用 X:load，归因内存使用量添加约 600 个字节。 因此，它是可以过分使用此属性的范围内实际上会降低性能。 我们建议你仅使用它需要隐藏的元素上。 如果你的容器上使用 X:load，仅适用于使用 X:load 属性元素支付开销。

> [!IMPORTANT]
> 从 Windows 10 版本 1703 （创意者更新） 开始，可 X:load 属性。 要使用 x:Load，Visual Studio 项目所面向的最低版本必须为 *Windows 10 创意者更新（10.0，版本 15063）*。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>加载元素

有多种不同的方法来加载元素：

- 使用[X:bind](x-bind-markup-extension.md)表达式指定的加载状态。 表达式应返回**true**加载与**false**卸载该元素。
- 使用在元素上定义的名称来调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)。
- 使用在元素上定义的名称来调用 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)。
- 在[**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)中，使用 X:load 元素作为目标的[**资源库**](https://msdn.microsoft.com/library/windows/apps/br208817)或**情节提要**动画。
- 面向任何**情节提要**中的卸载的元素。

> 注意：一旦启动元素的实例化，该元素便会在 UI 线程上进行创建，因此如果一次创建过多，则可能会导致 UI 不流畅。

以上面列出的任何方法创建了延迟元素后，会立即发生以下情况：

- 将引发该元素上的 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 事件。
- 设置 X:name 的字段。
- 将评估该元素上的任何 X:bind 绑定。
- 如果已注册为接收有关某属性（包含延迟元素的属性）的属性更改通知，将引发此通知。

## <a name="unloading-elements"></a>卸载的元素

若要卸载元素：

- 使用 X:bind 表达式指定的加载状态。 表达式应返回**true**加载与**false**卸载该元素。
- 在页面或 UserControl，调用**UnloadObject**并传入对象引用
- 调用**Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject**并传入对象引用

当卸载对象时，它将被替换在树中的占位符。 对象实例将保留在内存中，直到已发布的所有引用。 页面/UserControl UnloadObject API 旨在释放持有 X:name 和 x: Bind 的代码生成器的引用。 如果他们也需要发布的应用代码中保留额外的引用。

卸载元素时，与元素相关联的所有状态将被丢弃，因此如果使用 X:load 作为优化版本的可见性，然后确保所有状态绑定，通过应用或者重新应用由代码加载的事件触发时。

## <a name="restrictions"></a>限制

使用**X:load**的限制是：

- 你必须定义一个[X:name](x-name-attribute.md)元素，因为存在需要以后查找该元素的方法。
- 你只能从[**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)或[**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249)派生的类型上使用 X:load。
- 你无法使用 X:load[**页面**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page)、 [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)或在[**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348)中的根元素上。
- 不能在[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)中的元素上使用 X:load。
- 你无法在使用[**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)加载的松动 XAML 使用 X:load。
- 移动父元素将清除尚未加载任何元素。

## <a name="remarks"></a>备注

你可以嵌套元素上使用 X:load，不过它们必须实现从最外层的元素中。 如果尝试先实现子元素再实现父元素，将引发异常。

通常情况下，建议延迟第一帧中无法查看的元素。一个用于查找要延迟的候选项的准则是，查找正使用折叠的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 创建的元素。 此外，由用户交互触发的 UI 也是用于查找可延迟的元素的好位置。

延迟 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 中的元素时请谨慎操作，因为延迟此类元素虽然会减少启动时间，但同时也可能降低平移性能，具体取决于要创建的内容。 如果想要提升平移性能，请参阅 [{x:Bind} 标记扩展](x-bind-markup-extension.md)和 [x:Phase 特性](x-phase-attribute.md)文档。

如果使用[X:phase 属性](x-phase-attribute.md)结合**X:load**然后，在实现某个元素或元素树后，该绑定会应用至当前阶段并包括当前阶段。 **X:phase**为指定的阶段不会影响或控制元素的加载状态。 平移，一部分来回收某个列表项时，实现的元素将行为相同的方式与其他活动元素，并使用相同的规则，包括分段处理已编译的绑定 （**{x: Bind}** 绑定）。

一般原则是在操作前和操作后测量应用性能，以确保获得所需性能。

## <a name="example"></a>示例

```xml
<StackPanel>
    <Grid x:Name="DeferredGrid" x:Load="False">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="Auto" />
            <ColumnDefinition Width="Auto" />
        </Grid.ColumnDefinitions>

        <Rectangle Height="100" Width="100" Fill="Orange" Margin="0,0,4,4"/>
        <Rectangle Height="100" Width="100" Fill="Green" Grid.Column="1" Margin="4,0,0,4"/>
        <Rectangle Height="100" Width="100" Fill="Blue" Grid.Row="1" Margin="0,4,4,0"/>
        <Rectangle Height="100" Width="100" Fill="Gold" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="one" x:Load="{x:Bind (x:Boolean)CheckBox1.IsChecked, Mode=OneWay}"/>
        <Rectangle Height="100" Width="100" Fill="Silver" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0"
                   x:Name="two" x:Load="{x:Bind Not(CheckBox1.IsChecked), Mode=OneWay}"/>
    </Grid>

    <Button Content="Load elements" Click="LoadElements_Click"/>
    <Button Content="Unload elements" Click="UnloadElements_Click"/>
    <CheckBox x:Name="CheckBox1" Content="Swap Elements" />
</StackPanel>
```

```csharp
// This is used by the bindings between the rectangles and check box.
private bool Not(bool? value) { return !(value==true); }

private void LoadElements_Click(object sender, RoutedEventArgs e)
{
    // This will load the deferred grid, but not the nested
    // rectangles that have x:Load attributes.
    this.FindName("DeferredGrid"); 
}

private void UnloadElements_Click(object sender, RoutedEventArgs e)
{
     // This will unload the grid and all its child elements.
     this.UnloadObject(DeferredGrid);
}
```

