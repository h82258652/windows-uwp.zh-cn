---
Description: Use a tooltip to reveal more info about a control before asking the user to perform an action.
title: 工具提示
ms.assetid: A21BB12B-301E-40C9-B84B-C055FD43D307
label: Tooltips
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: yulikl
design-contact: kimsea
dev-contact: stpete
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 651914cfb2abd4326c6ac6295f10ad359925d465
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8737047"
---
# <a name="tooltips"></a>工具提示

工具提示是链接到另一个控件或对象的简短描述。 工具提示可帮助用户了解未在 UI 中直接描述的不熟悉的对象。 它们会在用户将焦点移动到控件、长按控件或将鼠标指针停在控件上时自动显示。 几秒钟后或者当用户移动手指、指针或键盘/游戏板焦点时，工具提示将消失。

![工具提示](images/controls/tool-tip.png)

> **重要 API**:[工具提示类](/uwp/api/Windows.UI.Xaml.Controls.ToolTip)，[ToolTipService 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.tooltipservice)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

要求用户执行操作之前，使用工具提示显示有关控件的详细信息。 工具提示应尽量少用，仅应在用户为尝试完成某个任务而需要添加不同的值时才使用。 一个经验规则是，如果信息在同一体验的其他位置提供，则不需要工具提示。 一个有价值的工具提示可以澄清不明确的操作。

应在何时使用工具提示？ 在决定之前，请考虑以下问题：

- **信息是否应当基于指针悬停显示？**
    如果不是，请使用其他控件。 仅在与用户交互时显示提示，工具提示从来不会自行显示。

- **控件是否有文本标签？**
    如果没有，请使用工具提示提供标签。 比较好的 UX 设计做法是以内联方式为大多数控件添加标签，对于这些控件，你不需要使用工具提示。 仅显示图标的工具栏控件和命令按钮需要工具提示。

- **对象是否受益于相关说明或更详细的信息？**
    如果是，请使用工具提示。 但是，文本必须是补充性的文本，也就是说不是主要任务必需的文本。 如果是必需的文本，请将它直接放在 UI 中，这样用户便不必查找或搜寻它。

- **补充信息是否为错误、警告或状态？**
    如果是，请使用其他 UI 元素（如弹出窗口）。

- **用户是否需要与提示进行交互？**
    如果是，请使用其他控件。 用户不能与提示进行交互，因为移动鼠标会导致提示消失。

- **用户是否需要打印补充信息？**
    如果是，请使用其他控件。

- **用户是否会觉得提示令人厌烦或者让人分心？**
    如果是，请考虑使用其他解决方案（包括不执行任何操作）。 如果你的确使用了可能会让人分心的提示，请允许用户关闭它们。

## <a name="example"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/ToolTip">打开此应用，了解 ToolTip 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

“必应地图”应用中的工具提示。

![“必应地图”应用中的工具提示](images/control-examples/tool-tip-maps.png)

## <a name="create-a-tooltip"></a>创建工具提示

[工具提示](/uwp/api/Windows.UI.Xaml.Controls.ToolTip)必须分配到其所有者的其他 UI 元素。 [ToolTipService](/uwp/api/windows.ui.xaml.controls.tooltipservice) 类提供用于显示工具提示的静态方法。

在 XAML 中，可使用 **ToolTipService.Tooltip** 附加属性将工具提示分配给所有者。

```xaml
<Button Content="Submit" ToolTipService.ToolTip="Click to submit"/>
```

在代码中，可使用 [ToolTipService.SetToolTip](/uwp/api/windows.ui.xaml.controls.tooltipservice.settooltip) 方法将工具提示分配给所有者。

```xaml
<Button x:Name="submitButton" Content="Submit"/>
```

```csharp
ToolTip toolTip = new ToolTip();
toolTip.Content = "Click to submit";
ToolTipService.SetToolTip(submitButton, toolTip);
```

### <a name="content"></a>内容

可以使用任何对象作为工具提示的[内容](/uwp/api/windows.ui.xaml.controls.contentcontrol.content)。 下面是一个在工具提示中使用[图像](/uwp/api/windows.ui.xaml.controls.image)的示例。

```xaml
<TextBlock Text="store logo">
    <ToolTipService.ToolTip>
        <Image Source="Assets/StoreLogo.png"/>
    </ToolTipService.ToolTip>
</TextBlock>
```

### <a name="placement"></a>放置

默认情况下，工具提示在指针上方居中显示。 工具提示的放置位置不受应用窗口的约束，因此工具提示可能部分或完全显示在应用窗口边界的外部。

对于广泛的调整，用于[放置](/uwp/api/windows.ui.xaml.controls.tooltip.placement)属性或**ToolTipService.Placement**附加属性指定是否在工具提示应绘制上方、 下方、 左方或右方指针。 你可以设置[VerticalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.verticaloffset)或[HorizontalOffset](/uwp/api/windows.ui.xaml.controls.tooltip.horizontaloffset)属性，以更改在指针和工具提示之间的距离。 只有一个两个偏移量值将会影响最终位置-VerticalOffset 放置时顶部或底部，HorizontalOffset 向左放置时或向右。

```xaml
<!-- An Image with an offset ToolTip. -->
<Image Source="Assets/StoreLogo.png">
    <ToolTipService.ToolTip>
        <ToolTip Content="Offset ToolTip."
                 Placement="Right"
                 HorizontalOffset="20"/>
    </ToolTipService.ToolTip>
</Image>
```

如果工具提示遮盖了它所指的内容，你可以调整它准确地使用新的**PlacementRect**属性的位置。 PlacementRect 定位在工具提示的位置，并且还可用作工具提示将不会阻挡，一个区域提供没有足够的屏幕空间，以绘制该区域外的工具提示。 你可以指定相对于工具提示的所有者和高度矩形的原点和排除区域的宽度。 如果工具提示应绘制上方、 下方、 左方或右方 PlacementRect，定义的[位置](/uwp/api/windows.ui.xaml.controls.tooltip.placement)属性。 

```xaml
<!-- An Image with a non-occluding ToolTip. -->
<Image Source="Assets/StoreLogo.png" Height="64" Width="96">
    <ToolTipService.ToolTip>
        <ToolTip Content="Non-occluding ToolTip."
                 PlacementRect="0,0,96,64"/>
    </ToolTipService.ToolTip>
</Image>
```

## <a name="recommendations"></a>建议

- 慎用工具提示（或者完全不使用）。 工具提示可使用户中断。 工具提示的干扰性类似于弹出窗口，因此不要使用它们，除非它们可以显著增加价值。
- 使工具提示文本保持简单。 工具提示最适用简短语句和语句片段。 较大的文本块可能会让人不知所措，并且工具提示可能会在用户阅读完之前就超时了。
- 创建有帮助的补充性工具提示文本。 工具提示文本的内容必须丰富。 不要使工具提示过于明显，也不要只是复制屏幕上已有的内容。 由于工具提示文本不总是可见，因此它应当是用户不必阅读的补充信息。 使用明白易懂的控件标签或就地的补充文本来表示重要信息。
- 适当时使用图像。 有时最好在工具提示中使用图像。 例如，当用户将鼠标指针悬停在超链接上时，你可以使用工具提示显示链接页面的预览。
- 不要使用工具提示来显示 UI 中已有的文本。 例如，不要在按钮上放置显示内容与按钮文本相同的工具提示。
- 不要在工具提示内部放置交互式控件。
- 不要将看似交互式控件的图像放在工具提示内部。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [ToolTip 类](https://msdn.microsoft.com/library/windows/apps/br227608)
