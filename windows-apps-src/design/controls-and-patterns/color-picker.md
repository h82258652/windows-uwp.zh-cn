---
author: Jwmsft
Description: A color picker lets a user browse through and select colors.
title: 颜色选取器
label: Color Picker
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: llongley
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f2b6270fcd7bc3dcf45d6e80dd547ee783d8e9ae
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6977308"
---
# <a name="color-picker"></a>颜色选取器

颜色选取器用于浏览和选择颜色。 默认情况下，它使用户可以在色谱上浏览颜色，或在红-绿-蓝 (RGB)、色调饱和度值 (HSV) 或十六进制文本框中指定颜色。

> **重要 API**：[ColorPicker 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker)、[Color 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color)、[ColorChanged 事件](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged)

![默认颜色选取器](images/color-picker-default.png)


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用颜色选取器可使用户可以应用中选择颜色。 例如，使用它可更改颜色设置，如字体颜色、背景或应用主题颜色。

如果应用是用于绘图或使用触笔的类似任务，请考虑使用[墨迹书写控件](inking-controls.md)以及颜色选取器。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/ColorPicker">打开此应用，了解 ColorPicker 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-color-picker"></a>创建颜色选取器

此示例演示如何在 XAML 中创建默认颜色选取器。

```xaml
<ColorPicker x:Name="myColorPicker"/>
```

默认情况下，颜色选取器在色谱旁的矩形栏中显示所选颜色的预览。 可以使用 [ColorChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorChanged) 事件或 [Color](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.Color) 属性访问所选颜色并在应用中使用它。 有关详细代码，请参阅下面的示例。

### <a name="bind-to-the-chosen-color"></a>绑定到所选颜色

当颜色选择应立即生效时，可以使用数据绑定绑定到 Color 属性，或处理 ColorChanged 事件以在代码中访问所选颜色。

在此示例中，会将用作 Rectangle 的 Fill 的 SolidColorBrush 的 Color 属性直接绑定到颜色选取器的所选颜色。 对颜色选取器进行的任何更改都会导致绑定属性发生实时更改。

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape=”Ring”
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>

<Rectangle Height="50" Width="50">
    <Rectangle.Fill>
        <SolidColorBrush Color="{x:Bind myColorPicker.Color, Mode=OneWay}"/>
    </Rectangle.Fill>
</Rectangle>
```

此示例使用只带有圆形和滑块的简化颜色选取器，这是常用的“非正式”颜色选取体验。 当颜色更改可以进行显示并在受影响对象上实时发生时，你无需显示颜色预览栏。 有关详细信息，请参阅*自定义颜色选取器*部分。

### <a name="save-the-chosen-color"></a>保存所选颜色

在某些情况下，你不想立即应用颜色更改。 例如，在浮出控件中承载颜色选取器时，建议仅在用户确认选择或关闭浮出控件之后，才应用所选颜色。 还可以保存所选颜色值以供将来使用。

在此示例中，在具有 Confirm 和 Cancel 按钮的浮出控件中承载颜色选取器。 当用户确认其颜色选择时，保存所选颜色以便将来在应用中使用。

```xaml
<Page.Resources>
    <Flyout x:Key="myColorPickerFlyout">
        <RelativePanel>
            <ColorPicker x:Name="myColorPicker"
                         IsColorChannelTextInputVisible="False"
                         IsHexInputVisible="False"/>

            <Grid RelativePanel.Below="myColorPicker"
                  RelativePanel.AlignLeftWithPanel="True"
                  RelativePanel.AlignRightWithPanel="True">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition/>
                    <ColumnDefinition/>
                </Grid.ColumnDefinitions>
                <Button Content="OK" Click="confirmColor_Click"
                        Margin="0,12,2,0" HorizontalAlignment="Stretch"/>
                <Button Content="Cancel" Click="cancelColor_Click"
                        Margin="2,12,0,0" HorizontalAlignment="Stretch"
                        Grid.Column="1"/>
            </Grid>
        </RelativePanel>
    </Flyout>
</Page.Resources>

<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <Button x:Name="colorPickerButton"
            Content="Pick a color"
            Flyout="{StaticResource myColorPickerFlyout}"/>
</Grid>
```

```csharp
private Color mycolor;

private void confirmColor_Click(object sender, RoutedEventArgs e)
{
    // Assign the selected color to a variable to use outside the popup.
    myColor = myColorPicker.Color;

    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}

private void cancelColor_Click(object sender, RoutedEventArgs e)
{
    // Close the Flyout.
    colorPickerButton.Flyout.Hide();
}
```

### <a name="configure-the-color-picker"></a>配置颜色选取器

使用户可以选取颜色并不一定需要所有字段，因此颜色选取器十分灵活。 它提供了各种选项，使你可以配置控件以符合需求。

例如，当用户无需精确控制时（如在笔记应用中选取荧光笔颜色），则可以使用简化 UI。 可以隐藏文本输入字段，并将色谱更改为圆形。

当用户需要精确控制时（如在图形设计应用中），可以为颜色的每个方面都显示滑块和文本输入字段。

#### <a name="show-the-circle-spectrum"></a>显示圆形色谱

此示例演示如何使用 [ColorSpectrumShape](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.colorpicker.ColorSpectrumShape) 属性将颜色选取器配置为使用圆形色谱而不是默认的正方形。

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"/>
```

![具有圆形色谱的颜色选取器](images/color-picker-ring.png)

必须在正方形与圆形色谱之间进行选择时，一个主要考虑因素是准确性。 用户在使用正方形选择特定颜色时会拥有更多控制权，因为会显示更多色域。 应将圆形色谱视为更多“非正式”颜色选择体验。

#### <a name="show-the-alpha-channel"></a>显示 alpha 通道

在此示例中，会在颜色选取器上启用不透明度滑块和文本框。

```xaml
<ColorPicker x:Name="myColorPicker"
             IsAlphaEnabled="True"/>
```

![IsAlphaEnabled 设置为 true 的颜色选取器](images/color-picker-alpha.png)

#### <a name="show-a-simple-picker"></a>显示简单选取器

此示例演示如何配置具有简单 UI 的颜色选取器以进行“非正式”使用。 会显示圆形色谱并隐藏默认文本输入框。 当颜色更改可以进行显示并在受影响对象上实时发生时，你无需显示颜色预览栏。 否则，应让颜色预览保持可见。

```xaml
<ColorPicker x:Name="myColorPicker"
             ColorSpectrumShape="Ring"
             IsColorPreviewVisible="False"
             IsColorChannelTextInputVisible="False"
             IsHexInputVisible="False"/>
```

![简单颜色选取器](images/color-picker-casual.png)

#### <a name="show-or-hide-additional-features"></a>显示或隐藏其他功能

下表显示可以用于配置 ColorPicker 控件的所有选项。

功能 | 属性
--------|-----------
色谱 | IsColorSpectrumVisible、ColorSpectrumShape、ColorSpectrumComponents
颜色预览 | IsColorPreviewVisible
颜色值| IsColorSliderVisible、IsColorChannelTextInputVisible
不透明度值 | IsAlphaEnabled、IsAlphaSliderVisible、IsAlphaTextInputVisible
十六进制值 | IsHexInputVisible

> [!NOTE]
> IsAlphaEnabled 必须为 **true**，才能显示不透明度文本框和滑块。 随后可以使用 IsAlphaTextInputVisible 和 IsAlphaSliderVisible 属性修改输入控件的可见性。 有关详细信息，请参阅 API 文档。

## <a name="dos-and-donts"></a>应做事项和禁止事项

- 考虑适合于应用的颜色选取体验的类型。 某些方案可能无需精细的颜色选取，可受益于简化选取器
- 若要获得最准确的颜色选取体验，请使用正方形色谱并确保它至少是 256x256 像素，或是包含文本输入字段以便让用户可以优化其所选颜色。
- 在浮出控件中使用时，在色谱中点击或单独调整滑块不应提交颜色选择。 若要提交选择，请执行以下操作：
  - 提供提交和取消按钮以应用或取消选择。 点击后退按钮或在浮出控件外部点击会清除它，而不保存用户选择。
  - 或者，通过在浮出控件外部点击或点击后退按钮，在清除浮出控件时提交选择。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [UWP 应用中的笔和触笔交互](../input/pen-and-stylus-interactions.md)
- [墨迹书写](inking-controls.md)

<!--
<div class=”microsoft-internal-note”>
<p>
<p>
Note: For more info, see the [color picker redlines](http://uni/DesignDepot.FrontEnd/#/ProductNav/3666/15/dv/?t=Windows%7CControls&f=RS2) on UNI.
</div>
-->