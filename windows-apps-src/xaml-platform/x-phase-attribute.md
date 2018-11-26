---
title: xPhase 属性
description: 将 xPhase 与 xBind 标记扩展结合使用，以便能以增量方式呈现 ListView 和 GridView 项并改进平移体验。
ms.assetid: BD17780E-6A34-4A38-8D11-9703107E247E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6def088b3e7f6410f12d1b2e411bcb547c90a09a
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7718981"
---
# <a name="xphase-attribute"></a>x:Phase 属性


将 **x:Phase** 与 [{x:Bind} 标记扩展](x-bind-markup-extension.md)结合使用，以便能以增量方式呈现 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 和 [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) 项并改进平移体验。 为了能实现与使用 [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914) 事件手动控制列表项的呈现相同的效果，**x:Phase** 提供了一种声明性方法。 另请参阅[以增量方式更新 ListView 和 GridView 项](../debug-test-perf/optimize-gridview-and-listview.md#update-items-incrementally)。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法


``` syntax
<object x:Phase="PhaseValue".../>
```

## <a name="xaml-values"></a>XAML 值


| 术语 | 说明 |
|------|-------------|
| PhaseValue | 一个用于指示将处理的元素所处的阶段的数字。 默认值为 0。 | 

## <a name="remarks"></a>备注

如果列表既能借助触摸快速平移又能使用鼠标滚轮实现，则该列表可能无法以足够快地速度呈现项目以跟上滚动速度，具体取决于数据模板的复杂程度。 这对于带有节能 CPU 的便携式设备（如手机或平板电脑）尤其如此。

阶段支持递增式呈现数据模板，以便可为内容设定优先级，即最重要的元素最先呈现。 这使列表能够在平移速度较快时为每个项显示相应内容，并且将在时间允许的情况下为每个模板呈现更多的元素。

## <a name="example"></a>示例

```xml
<DataTemplate x:Key="PhasedFileTemplate" x:DataType="model:FileItem">
    <Grid Width="200" Height="80">
        <Grid.ColumnDefinitions>
           <ColumnDefinition Width="75" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>
        <Image Grid.RowSpan="4" Source="{x:Bind ImageData}" MaxWidth="70" MaxHeight="70" x:Phase="3"/>
        <TextBlock Text="{x:Bind DisplayName}" Grid.Column="1" FontSize="12"/>
        <TextBlock Text="{x:Bind prettyDate}"  Grid.Column="1"  Grid.Row="1" FontSize="12" x:Phase="1"/>
        <TextBlock Text="{x:Bind prettyFileSize}"  Grid.Column="1"  Grid.Row="2" FontSize="12" x:Phase="2"/>
        <TextBlock Text="{x:Bind prettyImageSize}"  Grid.Column="1"  Grid.Row="3" FontSize="12" x:Phase="2"/>
    </Grid>
</DataTemplate>
```

数据模板描述了 4 个阶段：

1.  显示 DisplayName 文本块。 未指定阶段的所有控件将隐式视为阶段 0 的一部分。
2.  显示 prettyDate 文本块。
3.  显示 prettyFileSize 和 prettyImageSize 文本块。
4.  显示图像。

阶段是 [{x:Bind}](x-bind-markup-extension.md) 的一项功能，它适用于派生自 [**ListViewBase**](https://msdn.microsoft.com/library/windows/apps/br242879) 的控件，且以增量方式处理数据绑定的项目模板。 在呈现列表项时，**ListViewBase** 将先以视图形式为所有项呈现单个阶段，然后再移至下一阶段。 呈现工作将以时间分片批处理的形式进行执行，以便在滚动列表时可对所需的工作进行重新评估，从而可不执行不再可见的项。

**x:Phase** 可以在使用 [{x:Bind}](x-bind-markup-extension.md) 的数据模板中的任一元素上进行指定。 当元素具有一个非 0 的阶段时，将（通过 **Opacity**，而非 **Visibility**）从视图中隐藏该元素，直到处理完该阶段并更新绑定为止。 当滚动 [**ListViewBase**](https://msdn.microsoft.com/library/windows/apps/br242879) 派生的控件时，它将从不再显示在屏幕上的项中回收项目模板，以呈现新的可见项。 模板内的 UI 元素将保留其旧值，直到它们再次进行数据绑定。 阶段将导致数据绑定步骤延迟，因此它需要隐藏 UI 元素，以防这些元素过时。

每个 UI 元素可能只具有一个指定阶段。 如果是，这将应用于元素上的所有绑定。 如果未指定阶段，将假定阶段 0。

阶段数字不需要是连续的，不过需与 [**ContainerContentChangingEventArgs.Phase**](https://msdn.microsoft.com/library/windows/apps/dn298493) 值相同。 在处理 **x:Phase** 绑定前，将针对每个阶段引发 [**ContainerContentChanging**](https://msdn.microsoft.com/library/windows/apps/dn298914) 事件。

阶段仅影响 [{x:Bind}](x-bind-markup-extension.md) 绑定，而不会影响 [{Binding}](binding-markup-extension.md) 绑定。

仅当项目模板使用阶段感知的控件进行呈现时，才应用阶段。 对于 windows 10，这意味着[**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878)和[**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705)。 阶段既不会应用于其他项目控件中所使用的数据模板，也不会应用于诸如 [**ContentTemplate**](https://msdn.microsoft.com/library/windows/apps/br209369) 或 [**Hub**](https://msdn.microsoft.com/library/windows/apps/dn251843) 等部分中的方案，因为在这些用例中，将一次数据绑定所有 UI 元素。

