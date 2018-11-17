---
author: QuinnRadich
Description: The toggle switch represents a physical switch that allows users to turn things on or off.
title: 切换开关控件指南
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: Toggle switches
template: detail.hbs
ms.author: quradic
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d166e872af0824c9eb5df15510f9597d0303f483
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2018
ms.locfileid: "7165695"
---
# <a name="toggle-switches"></a>切换开关

切换开关表示允许用户打开或关闭选项的物理开关，如灯开关。 使用切换开关控件向用户显示两个相互排斥的选项（如开/关），选择其中一个选项会立即提供结果。

若要创建一个切换开关控件，可以使用 [ToggleSwitch 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)。

> **重要 API**: [ToggleSwitch 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)、[IsOn 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison)、[Toggled 事件](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用用户翻转切换开关后立即生效的二进制操作的切换开关。

![Wi-Fi 切换开关，开和关](images/toggleswitches01.png)

将切换开关视作设备的物理电源开关：当你想要启用或禁用设备执行的操作时将其打开或关闭。

为了使切换开关易于理解，请使用一个或两个词标记它，最好是名词，以描述它控制的功能。 例如，“WiFi”或“厨房灯”。 

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处打开该应用，了解 <a href="xamlcontrolsgallery:/item/ToggleSwitch">ToggleSwitch</a> 或 <a href="xamlcontrolsgallery:/item/ToggleButton">ToggleButton</a> 的实际应用。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="choosing-between-toggle-switch-and-check-box"></a>在切换开关和复选框之间进行选择

对于某些操作，既可使用切换开关也可使用复选框。 若要决定哪个控件效果更好，请按照以下提示操作：

- 对于用户更改后更改立即生效的二元设置，使用切换开关。

    ![切换开关与复选框](images/toggleswitches02.png)

    在此示例中，很显然需在使用切换开关时将厨房灯设置为“启用”。 但使用复选框时，用户需要考虑是立即启用灯光还是需要用户选中该框来启用灯光。

- 对可选项使用复选框（“这个控件真好”）。
- 当用户必须执行额外的步骤才能使更改生效时，使用复选框。 例如，如果用户必须单击“提交”或“下一步”按钮才能应用更改，则使用复选框。
- 当用户可选择与单个设置或功能相关的多个项时，请使用复选框。

## <a name="toggle-switches-in-the-windows-ui"></a>Windows UI 中的切换开关

这些图像显示了 Windows UI 如何使用切换开关。 下面介绍智能存储设置屏幕如何使用切换开关：

![智能存储中的切换开关](images/SmartStorageToggle.png)

此示例来自“夜灯设置”页面：

![Windows“开始”菜单设置中的切换开关](images/NightLightToggle.png)

## <a name="create-a-toggle-switch"></a>创建切换开关

下面介绍了如何创建简单的切换开关。 此 XAML 创建前面所示的切换开关。

```xaml
<ToggleSwitch x:Name="lightToggle" Header="Kitchen Lights"/>
```

下面介绍了如何使用代码创建相同的切换开关。

```csharp
ToggleSwitch lightToggle = new ToggleSwitch();
lightToggle.Header = "Kitchen Lights";

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(lightToggle);
```

### <a name="ison"></a>IsOn

该开关可以为开或关。 使用 [IsOn](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.ison) 属性确定该开关的状态。 当开关用于控制另一个二元属性的状态时，可以使用如下所示的绑定。

```xaml
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### <a name="toggled"></a>Toggled

在其他情况下，你可以处理 [Toggled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.toggled) 事件来响应状态的更改。

此示例显示了如何使用 XAML 和代码添加一个 Toggled 事件处理程序。 处理 Toggled 事件以打开或关闭进度环，并更改其可见性。

```xaml
<ToggleSwitch x:Name="toggleSwitch1" IsOn="True"
              Toggled="ToggleSwitch_Toggled"/>
```

下面介绍了如何使用代码创建相同的切换开关。

```csharp
// Create a new toggle switch and add a Toggled event handler.
ToggleSwitch toggleSwitch1 = new ToggleSwitch();
toggleSwitch1.Toggled += ToggleSwitch_Toggled;

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(toggleSwitch1);
```

下面是 Toggled 事件的处理程序。

```csharp
private void ToggleSwitch_Toggled(object sender, RoutedEventArgs e)
{
    ToggleSwitch toggleSwitch = sender as ToggleSwitch;
    if (toggleSwitch != null)
    {
        if (toggleSwitch.IsOn == true)
        {
            progress1.IsActive = true;
            progress1.Visibility = Visibility.Visible;
        }
        else
        {
            progress1.IsActive = false;
            progress1.Visibility = Visibility.Collapsed;
        }
    }
}
```

### <a name="onoff-labels"></a>开/关标签

默认情况下，切换开关包括文本“开”和“关”标签，这些标签将自动本地化。 可以通过设置 [OnContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontent) 和 [OffContent](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontent) 属性来替换这些标签。

此示例将“开/关”标签替换为“显示/隐藏”标签。

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide"
              Toggled="ToggleSwitch_Toggled"/>
```

还可以通过设置 [OnContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.oncontenttemplate) 和 [OffContentTemplate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch.offcontenttemplate) 属性来使用更复杂的内容。

## <a name="recommendations"></a>建议

- 尽可能使用默认的开关标签；只有在必要时才替换切换开关的默认设置。 如果替换它们，请使用一个能更加准确描述切换的词。 一般情况下，如果“开”和“关”两词未能描述与切换开关相关的操作，你可能需要其他控件。
- 如果没有必要，则避免替换“开”和“关”标签；除非具体情况需要使用自定义的标签，否则继续使用默认标签。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [ToggleSwitch 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.toggleswitch)
- [单选按钮](radio-button.md)
- [切换开关](toggles.md)
- [复选框](checkbox.md)
