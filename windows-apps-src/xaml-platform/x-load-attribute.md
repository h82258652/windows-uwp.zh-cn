---
title: xLoad 特性
description: xLoad 可实现动态创建和析构元素及其子元素，从而减少了启动时间和内存使用量。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1fa0f12779ad56d57c92f667443644851dc3d5e5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629362"
---
# <a name="xload-attribute"></a>x:Load 属性

可使用 **x:Load** 优化启动、可视化树创建和 XAML 应用的内存使用。 使用 **x:Load** 具有与 **Visibility** 类似的视觉效果，有一点除外：如果未加载该元素，会释放其内存，并在内部使用一个小占位符来标记其在可视化树中的位置。

可通过代码或使用 [x:Bind](x-bind-markup-extension.md) 表达式加载和卸载具有 x:Load 特性的 UI 元素。 这对于减少偶尔出现或按一定条件出现的元素的成本非常有用。 在容器（如网格或 StackPanel）上使用 x:Load 时，会将该容器及其所有子容器作为一个组进行加载或卸载。

XAML 框架对延迟元素的跟踪会使每个具有 x:Load 的元素特性的内存使用量增加大约 600 字节，以供占位符使用。 因此，有可能出现过度使用此特性而导致性能下降的情况。 建议仅在需要隐藏的元素上使用该属性。 如果在容器上使用 x:Load，则仅为具有 x:Load 特性的元素支付开销。

> [!IMPORTANT]
> 可从 Windows 10，版本 1703 （创意者更新） 开始 x： 负载属性。 要使用 x:Load，Visual Studio 项目所面向的最低版本必须为 *Windows 10 创意者更新（10.0，版本 15063）*。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>加载元素

有多种不同的方法可用于加载元素：

- 使用 [x:Bind](x-bind-markup-extension.md) 表达式指定加载状态。 表达式应返回 **true** 以加载元素，返回 **false** 以卸载元素。
- 使用在元素上定义的名称来调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)。
- 使用在元素上定义的名称来调用 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)。
- 在 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) 中，使用面向 x:Load 元素的 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 或 **Storyboard** 动画。
- 面向任何 **Storyboard** 中的未加载的元素。

> 注意：启动一个元素的实例化后，会创建在 UI 线程，因此可能会导致 UI 明显变慢如果太多创建在一次。

以上面列出的任何方法创建了延迟元素后，会立即发生以下情况：

- 将引发该元素上的 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 事件。
- 设置 x:Name 的域。
- 将评估该元素上的所有 x:Bind 绑定。
- 如果已注册为接收有关某属性（包含延迟元素的属性）的属性更改通知，将引发此通知。

## <a name="unloading-elements"></a>卸载元素

卸载元素：

- 使用 x:Bind 表达式指定加载状态。 表达式应返回 **true** 以加载元素，返回 **false** 以卸载元素。
- 在页面或用户控件中，调用 **UnloadObject** 并在对象引用中传递
- 调用 **Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject** 并在对象引用中传递

卸载对象时，将在树中将其替换为占位符。 对象实例会保留在内存中，直至发布完所有引用为止。 页面/用户控件上的 UnloadObject API 用于发布 codegen 保留的 x:Name 和 x:Bind 的引用。 如果在应用代码中保留其他引用，还需要发布这些引用。

卸载元素时，将丢弃所有与元素相关联的状态，因此如果将 x:Load 用作 Visibility 的优化版本，那么在触发加载的事件时请确保通过绑定应用所有状态或通过代码重新应用所有状态。

## <a name="restrictions"></a>限制

使用 **x:Load** 时存在以下限制：

- 必须定义[X:name](x-name-attribute.md) 元素，因为存在需要一种方法来查找元素更高版本。
- 可仅在派生自 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 或 [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249) 的类型上使用 x:Load。
- 无法在 [**Page**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page)、[**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol) 或 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) 中的根元素上使用 x:Load。
- 无法在 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中的元素上使用 x:Load。
- 无法对载有 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 的松散 XAML 使用 x:Load。
- 移动父元素将清除所有尚未加载的元素。

## <a name="remarks"></a>备注

可对嵌套元素使用 x:Load，不过必须从最外层的元素中实现它们。  如果尝试先实现子元素再实现父元素，将引发异常。

通常情况下，建议延迟第一帧中无法查看的元素。 可用于找到要延迟的候选项的良好准则是，查找要创建带有折叠的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 的元素。 此外，由用户交互触发的 UI 也是用于查找可延迟的元素的好位置。

延迟 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 中的元素时请谨慎操作，因为延迟此类元素虽然会减少启动时间，但同时也可能降低平移性能，具体取决于要创建的内容。 如果想要提升平移性能，请参阅 [{x:Bind} 标记扩展](x-bind-markup-extension.md)和 [x:Phase 特性](x-phase-attribute.md)文档。

如果将 [x:Phase 特性](x-phase-attribute.md)与 **x:Load** 结合使用，则在实现某个元素或元素树后，该绑定会应用至当前阶段并包括当前阶段。 为 **x:Phase** 指定的阶段会影响或控制元素的加载状态。 当作为平移操作的一部分来回收某个列表项时，实现的元素的行为方式与其他活动元素相同，并使用相同的规则处理已编译的绑定（**{x:Bind}** 绑定），包括分段。

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

