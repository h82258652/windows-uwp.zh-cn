---
author: jwmsft
title: xDeferLoadStrategy 属性
description: xDeferLoadStrategy 会延迟元素及其子元素的创建，这将减少启动时间，不过内存使用量会略有增加。每个受影响的元素将使内存使用量增加大约 600 个字节。
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: cd958ba5f9025430be2736329c5a909233461039
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2018
ms.locfileid: "7429999"
---
# <a name="xdeferloadstrategy-attribute"></a>x:DeferLoadStrategy 属性

> [!IMPORTANT]
> 从 Windows 10 版本 1703（创意者更新）开始，**x: DeferLoadStrategy** 由 [**x:Load 属性**](x-load-attribute.md)取代。 使用 `x:Load="False"` 等同于 `x:DeferLoadStrategy="Lazy"`，但如有需要，将提供卸载 UI 的功能。 有关详细信息，请参阅 [x:Load 特性](x-load-attribute.md)。

可以使用 **x: DeferLoadStrategy ="Lazy"** 优化 XAML 应用的启动或树创建性能。 使用 **x: DeferLoadStrategy ="Lazy"** 时，将延迟创建元素及其子元素，这减少了启动时间和内存成本。 这对于减少偶尔出现或按一定条件出现的元素的成本非常有用。 从代码或 VisualStateManager 引用元素时，将实现该元素。

但是，XAML 框架对延迟元素的跟踪会使受影响的每个元素的内存使用量增加大约 600 字节。 延迟的元素树越大，节省的启动时间就越多，但内存占用也会相应增加。 因此，有可能出现过度使用此特性而导致性能下降的情况。

## <a name="xaml-attribute-usage"></a>XAML 属性用法

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## <a name="remarks"></a>备注

使用 **x:DeferLoadStrategy** 时的限制有：

- 你必须定义一个[X:name](x-name-attribute.md)元素，因为那里需要以后查找该元素的方法。
- 可以仅延迟派生自 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 或 [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249) 的类型。
- 不能在 [**Page**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.page)、[**UserControl**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol) 或 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) 中延迟根元素。
- 不能在 [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中延迟元素。
- 不能延迟载有 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 的松散 XAML。
- 移动父元素将清除所有尚未实现的元素。

有多种不同的方法可用于实现延迟的元素：

- 使用在元素上定义的名称来调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)。
- 使用在元素上定义的名称来调用 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)。
- 在 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) 中，使用以延迟元素为目标的 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 或 **Storyboard** 动画。
- 面向任何 **Storyboard** 中的延迟元素。
- 使用面向延迟元素的绑定。

> 注意：一旦启动元素的实例化，该元素便会在 UI 线程上进行创建，因此如果一次创建过多，则可能会导致 UI 不流畅。

以上面列出的任何方法创建了延迟元素后，会立即发生以下情况：

- 将引发该元素上的 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 事件。
- 将评估该元素上的所有绑定。
- 如果已注册为接收有关某属性（包含延迟元素的属性）的属性更改通知，将引发此通知。

可嵌套延迟元素，但必须从最外层的元素中实现它们。 如果尝试先实现子元素再实现父元素，将引发异常。

通常情况下，建议延迟第一帧中无法查看的元素。一个用于查找要延迟的候选项的准则是，查找正使用折叠的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 创建的元素。 此外，由用户交互触发的 UI 也是用于查找可延迟的元素的好位置。

延迟 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 中的元素时请谨慎操作，因为延迟此类元素虽然会减少启动时间，但同时也可能降低平移性能，具体取决于要创建的内容。 如果想要提升平移性能，请参阅 [{x:Bind} 标记扩展](x-bind-markup-extension.md)和 [x:Phase 特性](x-phase-attribute.md)文档。

如果将 [x:Phase 特性](x-phase-attribute.md)与 **x:DeferLoadStrategy** 结合使用，则在实现某个元素或元素树后，该绑定会应用至当前阶段并包括当前阶段。 为 **x:Phase** 指定的阶段不影响或控制元素延迟。 当作为平移操作的一部分来回收某个列表项时，实现的元素的行为方式与其他活动元素相同，并使用相同的规则处理已编译的绑定（**{x:Bind}** 绑定），包括分段。

一般原则是在操作前和操作后测量应用性能，以确保获得所需性能。

## <a name="example"></a>示例

```xml
<Grid x:Name="DeferredGrid" x:DeferLoadStrategy="Lazy">
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto" />
        <ColumnDefinition Width="Auto" />
    </Grid.ColumnDefinitions>

    <Rectangle Height="100" Width="100" Fill="#F65314" Margin="0,0,4,4" />
    <Rectangle Height="100" Width="100" Fill="#7CBB00" Grid.Column="1" Margin="4,0,0,4" />
    <Rectangle Height="100" Width="100" Fill="#00A1F1" Grid.Row="1" Margin="0,4,4,0" />
    <Rectangle Height="100" Width="100" Fill="#FFBB00" Grid.Row="1" Grid.Column="1" Margin="4,4,0,0" />
</Grid>
<Button x:Name="RealizeElements" Content="Realize Elements" Click="RealizeElements_Click"/>
```

```csharp
private void RealizeElements_Click(object sender, RoutedEventArgs e)
{
    // This will realize the deferred grid.
    this.FindName("DeferredGrid");
}
```
