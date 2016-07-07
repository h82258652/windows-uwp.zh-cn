---
author: jwmsft
title: "xDeferLoadStrategy 属性"
description: "xDeferLoadStrategy 会延迟元素及其子元素的创建，这将减少启动时间，不过内存使用量会略有增加。 每个受影响的元素将向内存使用量添加大约 600 个字节。"
ms.assetid: E763898E-13FF-4412-B502-B54DBFE2D4E4
translationtype: Human Translation
ms.sourcegitcommit: 98b9bca2528c041d2fdfc6a0adead321737932b4
ms.openlocfilehash: b989a31439444f06dacb86adb186f853d1637f6c

---

# x&#58;DeferLoadStrategy 属性

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**x:DeferLoadStrategy="Lazy"** 会延迟元素及其子元素的创建，这将减少启动时间，不过内存使用量会略有增加。 每个受影响的元素将向内存使用量添加大约 600 个字节。 你延迟的元素树越大，将节省的时间也就越多，不过内存占用也会有所增加。 因此，在性能降低至这种程度的情况下，可以过分使用此属性。

## XAML 属性用法

``` syntax
<object x:DeferLoadStrategy="Lazy" .../>
```

## 备注

使用 **x:DeferLoadStrategy** 时的限制是：

-   需要一个已定义的 [x:Name](x-name-attribute.md)，因为之后需要有一种方法来找到该元素。
-   仅 [**UIElement**](https://msdn.microsoft.com/library/windows/apps/br208911) 才能标记为延迟，派生自 [**FlyoutBase**](https://msdn.microsoft.com/library/windows/apps/dn279249) 的类型除外。
-   根元素既不能在 [**Page**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.page)、[**UserControls**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.ui.xaml.controls.usercontrol) 中延迟，也不能在 [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/br242348) 中延迟。
-   [**ResourceDictionary**](https://msdn.microsoft.com/library/windows/apps/br208794) 中的元素不能延迟。
-   不能用于通过 [**XamlReader.Load**](https://msdn.microsoft.com/library/windows/apps/br228048) 加载的松动 XAML。
-   移动父元素将清除所有尚未实现的元素。

有多种不同的方法可用于实现已延迟的元素：

-   使用元素上定义的名称来调用 [**FindName**](https://msdn.microsoft.com/library/windows/apps/br208715)。
-   使用元素上定义的名称来调用 [**GetTemplateChild**](https://msdn.microsoft.com/library/windows/apps/br209416)。
-   在 [**VisualState**](https://msdn.microsoft.com/library/windows/apps/br209007) 中，使用面向延迟元素的 [**Setter**](https://msdn.microsoft.com/library/windows/apps/br208817) 或 **Storyboard** 动画。
-   面向任何 **Storyboard** 中的已延迟元素。
-   使用面向延迟元素的绑定。
-   注意：一旦启动元素的实例化，该元素便会在 UI 线程上进行创建，因此如果一次创建过多，则可能会导致 UI 不流畅。

一旦延迟的元素由上面列出的方法进行创建，将出现以下几种情况：

-   该元素上的 [**Loaded**](https://msdn.microsoft.com/library/windows/apps/br208723) 事件将被引发。
-   该元素上所有绑定将进行求值。
-   如果应用程序已注册为接收关于包含延迟元素的属性的属性更改通知，将引发此类通知。

你可以嵌套延迟的元素，不过它们必须实现从最外层的元素中进行实现。  如果你尝试先实现子元素再实现父元素，将引发异常。

一般来说，建议延迟第一帧中不可见的项。  可用于找到要延迟的候选项的良好准则是，查找要创建带有折叠的 [**Visibility**](https://msdn.microsoft.com/library/windows/apps/br208992) 的元素。  此外，附带 UI（即，由用户交互触发的 UI）同样也是查找延迟元素的好位置。  

请注意 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 方案中的延迟元素，因为此类元素将减少启动时间，不过也可能会降低平移性能，具体取决于你要创建的内容。  如果你想要提升平移性能，请参阅 [{x:Bind} 标记扩展](x-bind-markup-extension.md)和 [x:Phase 属性](x-phase-attribute.md)文档。

如果 [x:Phase 属性](x-phase-attribute.md)与 **x:DeferLoadStrategy** 结合使用，则在调整某个元素或元素树的大小时，该绑定将会应用到当前阶段并包括当前阶段。 为 **x:Phase** 指定的阶段将不会影响或控制元素的延迟。 当列表项作为平移的一部分进行循环时，已实现的元素的行为方式将与其他活动元素相同，并且将会使用相同的规则处理已编译的绑定（**{x:Bind}** 绑定），包括分段。

一般原则是在确保获得所需的性能之前和之后测量你的应用程序。

## 示例

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
    this.FindName("DeferredGrid"); // This will realize the deferred grid
}
```




<!--HONumber=Jun16_HO4-->


