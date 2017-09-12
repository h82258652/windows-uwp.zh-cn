---
author: Jwmsft
Description: "切换开关表示允许用户打开或关闭选项的物理开关。"
title: "切换开关控件指南"
ms.assetid: 753CFEA4-80D3-474C-B4A9-555F872A3DEF
label: Toggle switches
template: detail.hbs
ms.author: jimwalk
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.openlocfilehash: 7cc1d11035d072fdd52bdfa4a1d0c6e66926ba9f
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/22/2017
---
# <a name="toggle-switches"></a>切换开关
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

[切换开关](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.aspx)表示允许用户打开或关闭选项的物理开关，如灯开关。 使用切换开关控件向用户显示两个相互排斥的选项（如开/关），选择其中一个选项会立即提供结果。 

若要创建一个切换开关控件，可以使用 [ToggleSwitch 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.aspx)。

> **重要 API**: [ToggleSwitch 类](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.aspx)、[IsOn 属性](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.ison.aspx)、[Toggled 事件](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.toggled.aspx)


## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用用户翻转切换开关后立即生效的二进制操作的切换开关。

![Wi-Fi 切换开关，开和关](images/toggleswitches01.png)

将切换开关视作设备的物理电源开关：当你想要启用或禁用设备执行的操作时将其打开或关闭。

为了使切换开关易于理解，请使用一个或两个词标记它，最好是名词，以描述它控制的功能。 例如，“WLAN”或“厨房灯”。  


### <a name="choosing-between-toggle-switch-and-check-box"></a>在切换开关和复选框之间进行选择

对于某些操作，既可使用切换开关也可使用复选框。 若要决定哪个控件效果更好，请按照以下提示操作：

-   对于用户更改后更改立即生效的二元设置，使用切换开关。

    ![切换开关与复选框](images/toggleswitches02.png)

    在此示例中，很显然需在使用切换开关时将无线设置为“启用”。 但使用复选框时，用户需要考虑是立即启用无线还是需要用户选中该框来启用无线。

-   对可选项使用复选框（“这个控件真好”）。 
-   当用户必须执行额外的步骤才能使更改生效时，使用复选框。 例如，如果用户必须单击“提交”或“下一步”按钮才能应用更改，则使用复选框。
-   当用户可选择与单个设置或功能相关的多个项时，请使用复选框。 

## <a name="toggle-switches-in-the-the-windows-ui"></a>Windows UI 中的切换开关

这些图像显示了 Windows UI 如何使用切换开关。 下面介绍智能存储设置屏幕如何使用切换开关：

![智能存储中的切换开关](images/SmartStorageToggle.png)

此示例来自“夜灯设置”页面：

![Windows“开始”菜单设置中的切换开关](images/NightLightToggle.png)

## <a name="create-a-toggle-switch"></a>创建切换开关

下面介绍了如何创建简单的切换开关。 此 XAML 创建前面所示的 WLAN 切换开关。

```xaml
<ToggleSwitch x:Name="wiFiToggle" Header="Wifi"/>
```
下面介绍了如何使用代码创建相同的切换开关。

```csharp
ToggleSwitch wiFiToggle = new ToggleSwitch();
wiFiToggle.Header = "WiFi";

// Add the toggle switch to a parent container in the visual tree.
stackPanel1.Children.Add(wiFiToggle);
```

### <a name="ison"></a>IsOn

该开关可以为开或关。 使用 [IsOn](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.ison.aspx) 属性确定该开关的状态。 当开关用于控制另一个二元属性的状态时，可以使用如下所示的绑定。

```
<StackPanel Orientation="Horizontal">
    <ToggleSwitch x:Name="ToggleSwitch1" IsOn="True"/>
    <ProgressRing IsActive="{x:Bind ToggleSwitch1.IsOn, Mode=OneWay}" Width="130"/>
</StackPanel>
```

### <a name="toggled"></a>Toggled

在其他情况下，你可以处理 [Toggled](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.toggled.aspx) 事件来响应状态的更改。

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

默认情况下，切换开关包括文本“开”和“关”标签，这些标签将自动本地化。 可以通过设置 [OnContent](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.oncontent.aspx) 和 [OffContent](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.offcontent.aspx) 属性来替换这些标签。

此示例将“开/关”标签替换为“显示/隐藏”标签。  

```xaml
<ToggleSwitch x:Name="imageToggle" Header="Show images"
              OffContent="Show" OnContent="Hide"
              Toggled="ToggleSwitch_Toggled"/>
```

还可以通过设置 [OnContentTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.oncontenttemplate.aspx) 和 [OffContentTemplate](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.toggleswitch.offcontenttemplate.aspx) 属性来使用更复杂的内容。

## <a name="recommendations"></a>建议

-    尽可能使用默认的开关标签；只有在必要时才替换切换开关的默认设置。 如果替换它们，请使用一个能更加准确描述切换的词。 一般情况下，如果“开”和“关”两词未能描述与切换开关相关的操作，你可能需要其他控件。
-    如果没有必要，则避免替换“开”和“关”标签；除非具体情况需要使用自定义的标签，否则继续使用默认标签。


## <a name="related-articles"></a>相关文章

- [ToggleSwitch 类](https://msdn.microsoft.com/library/windows/apps/hh701411)
- [单选按钮](radio-button.md)
- [切换开关](toggles.md)
- [复选框](checkbox.md)
