---
author: jwmsft
title: xLoad 属性
description: xLoad 启用动态创建和销毁元素及其子级，从而会降低启动时间和内存使用情况。
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8f34ea43b981de65c81e9cce2b8a896c3577e595
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "610782"
---
# <a name="xload-attribute"></a>x:Load 属性

您可以使用**x： 加载**优化启动、 可视化树创建和 XAML 应用程序的内存使用率。 使用**x： 负载**具有**可见性**，类似的视觉效果，只是不加载元素，发布其内存，内部小型占位符用于标记可视化树中的其位置。

X： 负载属性化的 UI 元素可以加载和卸载通过代码，或使用[x： 绑定](x-bind-markup-extension.md)表达式。 这对于减少偶尔出现或按一定条件出现的元素的成本非常有用。 如网格或 StackPanel 的容器上使用 x： 加载时，将加载或卸载作为一个组容器和所有子域。

延迟元素的 XAML 框架的跟踪向每个元素以体现占位符使用 x： 负载时，其分配的内存使用情况的约为 600 个字节。 因此，很可能过度使用此属性的范围内实际上会降低性能。 我们建议您仅使用它需要为隐藏的元素。 如果您的容器上使用 x： 加载，然后开销支付仅对具有 x： 负载特性的元素。

> [!IMPORTANT]
> X： 负载属性是可启动 Windows 10，版本 1703 （创建者更新） 中。 要使用 x:Load，Visual Studio 项目所面向的最低版本必须为 *Windows 10 创意者更新（10.0，版本 15063）*。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object x:Load="True" .../>
<object x:Load="False" .../>
<object x:Load="{x:Bind Path.to.a.boolean, Mode=OneWay}" .../>
```

## <a name="loading-elements"></a>加载元素

有多种不同的方式加载元素：

- 使用[x： 绑定](x-bind-markup-extension.md)表达式指定的加载状态。 该表达式应返回**true**加载和**false**卸载元素。
- 使用在元素上定义的名称来调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)。
- 使用在元素上定义的名称来调用 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)。
- 在[**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007)，使用[**资源库**](https://msdn.microsoft.com/library/windows/apps/br208817)或**情节提要**动画的 x： 加载元素。
- 目标中任何**情节提要**的卸载元素。

> 注意：一旦启动元素的实例化，该元素便会在 UI 线程上进行创建，因此如果一次创建过多，则可能会导致 UI 不流畅。

以上面列出的任何方法创建了延迟元素后，会立即发生以下情况：

- 将引发该元素上的 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 事件。
- 设置 x： 名称的域。
- 计算元素上的任何 x： 绑定绑定。
- 如果已注册为接收有关某属性（包含延迟元素的属性）的属性更改通知，将引发此通知。

## <a name="unloading-elements"></a>卸载元素

若要卸载元素：

- 使用 x： 绑定表达式指定的加载状态。 该表达式应返回**true**加载和**false**卸载元素。
- 在页面或 UserControl，调用**UnloadObject**并传递中的对象引用
- 调用**Windows.UI.Xaml.Markup.XamlMarkupHelper.UnloadObject**并传递中的对象引用

卸载对象时，它将替换树中的占位符。 对象实例将保留在内存中，直到已发布的所有引用。 上页/UserControl UnloadObject API 旨在释放保留 x: Name 和 x： 绑定代码生成器的引用。 如果他们还需要释放的应用程序代码中保留其他参考。

卸载元素时，与元素关联的所有状态将放弃，因此如果使用 x： 加载作为优化版本的可见性，然后确保所有状态绑定，通过应用或重新应用代码激发加载的事件时。

## <a name="restrictions"></a>限制

使用**x： 负载**的限制是：

- 必须定义元素的 [x:Name](x-name-attribute.md)，因为需要有在以后查找该元素的方法。
- 对从[**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911)或[**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249)派生类型只能使用 x： 负载。
- 不能使用 x： 加载[**页面**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page)、 [**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol)或[**数据模板**](https://msdn.microsoft.com/library/windows/apps/br242348)中的根元素。
- 不能使用 x： 加载[**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794)中的元素。
- 不能用[**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048)加载的松散 XAML 上使用 x： 负载。
- 未加载任何元素将清除移动父元素。

## <a name="remarks"></a>备注

但他们需要实现从外部大多数元素中，可以嵌套元素中使用 x： 负载。  如果尝试先实现子元素再实现父元素，将引发异常。

通常情况下，建议延迟第一帧中无法查看的元素。 一个用于查找要延迟的候选项的准则是，查找正使用折叠的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 创建的元素。 此外，由用户交互触发的 UI 也是用于查找可延迟的元素的好位置。

延迟 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 中的元素时请谨慎操作，因为延迟此类元素虽然会减少启动时间，但同时也可能降低平移性能，具体取决于要创建的内容。 如果想要提升平移性能，请参阅 [{x:Bind} 标记扩展](x-bind-markup-extension.md)和 [x:Phase 特性](x-phase-attribute.md)文档。

如果[x： 阶段属性](x-phase-attribute.md)结合使用带有**x： 负载**然后时实现元素或元素树，绑定将应用到和包括的当前阶段。 指定**x：** 阶段的阶段并不会影响或控制元素的加载状态。 Panning 的一部分回收列表项时，实现元素的行为方式相同，其他活动的元素，并使用相同的规则，包括逐步处理编译的绑定 （**{x： 绑定}** 绑定）。

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

