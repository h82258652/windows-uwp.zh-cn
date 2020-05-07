---
Description: 让用户在给定的范围内设置值。
title: 滑块
ms.assetid: 7EC7EA33-BE7E-4FD5-B205-B8FA7B729ACC
label: Sliders
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: cf4e9885fdb17780c176e2740101a0cd530328ec
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "80081545"
---
# <a name="sliders"></a>滑块

滑块是一种可让用户通过沿轨迹移动 thumb 控件从一个值范围中进行选择的控件。

![滑块控件](images/controls/slider.png)

**获取 Windows UI 库**

|  |  |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) | Windows UI 库 2.2 或更高版本包含此控件的使用圆角的新模板。 有关详细信息，请参阅[圆角半径](/windows/uwp/design/style/rounded-corner)。 WinUI 是一种 NuGet 包，其中包含用于 UWP 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](https://docs.microsoft.com/uwp/toolkits/winui/)。 |

> **平台 API**：[Slider 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.slider)、[Value 属性](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.value)、[ValueChanged 事件](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

当你希望用户能够设置所定义的连续值（如音量或亮度）或一系列离散值（如屏幕分辨率设置）时，应使用滑块。

当你知道用户用户将该值视为相对数量而不是数值时，滑块是最好的选择。 例如，用户考虑将其音量设置为低或中—而不是考虑将该值设置为 2 或 5。

不要对二元设置使用滑块。 改用[切换开关](toggles.md)。

下面是决定是否使用滑块时要考虑的一些其他因素：

- **该设置是否是相对数量？** 如果不是，使用[单选按钮](radio-button.md)或[列表框](lists.md)。
- **该设置是否为确切的已知数值？** 如果是，请使用数值[文本框](text-box.md)。
- **用户是否会从设置更改效果的即时反馈中获益？** 如果是，请使用滑块。 例如，用户可以通过立即查看色调、饱和度或亮度值更改的效果，从而更加轻松地选择颜色。
- **该设置是否包含四个或更多值？** 如果不是，请使用[单选按钮](radio-button.md)。
- **用户是否可以更改该值？** 滑块用于用户交互。 如果用户无法更改该值，请改为使用只读文本。

如果你在滑块和数值文本框之间进行选择，请在以下情况下使用数值文本框：

- 屏幕空间紧凑。
- 用户可能首选使用键盘。

在以下情况下使用滑块：

- 用户将获得即时反馈所带来的好处。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/Slider">打开此应用，了解 Slider 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Windows Phone 上用于控制音量的滑块。

![Windows Phone 上用于控制音量的滑块](images/control-examples/slider-phone.png)

Windows 显示设置中用于更改文本大小的滑块。

![Windows 显示设置中用于更改文本大小的滑块](images/control-examples/slider-display-settings.png)

## <a name="create-a-slider"></a>创建滑块

下面介绍了如何使用 XAML 创建滑块。

```xaml
<Slider x:Name="volumeSlider" Header="Volume" Width="200"
        ValueChanged="Slider_ValueChanged"/>
```

下面介绍了如何使用代码创建滑块。

```csharp
Slider volumeSlider = new Slider();
volumeSlider.Header = "Volume";
volumeSlider.Width = 200;
volumeSlider.ValueChanged += Slider_ValueChanged;

// Add the slider to a parent container in the visual tree.
stackPanel1.Children.Add(volumeSlider);
```

从 [Value](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.value) 属性中获取并设置滑块的值。 若要响应值更改，可使用数据绑定以绑定到 Value 属性或处理 [ValueChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.rangebase.valuechanged) 事件。

```csharp
private void Slider_ValueChanged(object sender, RangeBaseValueChangedEventArgs e)
{
    Slider slider = sender as Slider;
    if (slider != null)
    {
        media.Volume = slider.Value;
    }
}
```

## <a name="recommendations"></a>建议

-   调整控件大小，以便用户可以轻松设置所需的值。 对于具有离散值的设置，请确保用户可以使用鼠标轻松选择任何值。 确保滑块的终点始终位于视图边界内。
-   在用户选择时或选择后，提供即时反馈（如果实际）。 例如，Windows 音量控件会蜂鸣以指示选定的音频音量。
-   使用标签显示值范围。 例外：如果滑块垂直放置，并且顶部标签为“最大”、“高”、“更多”或同等标签，则可以忽略其他标签，因为它的意思已明确。
-   在禁用滑块时禁用所有关联标签或反馈视觉对象。
-   当设置你的滑块的流向和/或方向时，请考虑文本方向。 在某些语言中，书写文字从左向右排列，在另一些语言中却从右向左排列。
-   不使用滑块作为进度指示器。
-   不要将滑块的滑动条大小更改为默认大小以外的大小。
-   如果值范围较大，并且用户很可能选择该范围内几个代表值之一，请不要创建相邻滑块。 相反，请将这些值用作唯一允许的步骤。 例如，如果时间值最多为 1 个月，但用户仅需要从 1 分钟、1 小时、1 天或 1 个月中选取，则创建仅具有 4 个步骤点的滑块。

## <a name="additional-usage-guidance"></a>其他使用指南

### <a name="choosing-the-right-layout-horizontal-or-vertical"></a>选择适当的布局：水平或垂直

你可以水平或垂直布置滑块。 使用这些指南可确定使用哪个布局。

-   使用自然方向。 例如，如果滑块代表一个真实值（通常垂直显示），例如温度，请使用垂直方向。
-   如果使用控件在媒体中进行搜索，例如在视频应用中，请使用水平方向。
-   当在可以沿着一个方向（水平或垂直）移动的页面中使用滑块时，请针对滑块使用与移动方向不同的方向。 否则，用户在尝试平移页面时，可能会意外地轻扫滑块从而更改其值。
-   如果仍然不确定使用哪个方向，请使用最适合你的页面布局的方向。

### <a name="range-direction"></a>范围方向

范围方向是指当你从滑块的当前值滑动到它的最大值时，滑块的移动方向。

-   对于垂直滑块，无论从哪个方向阅读，都将最大值放在滑块的顶部。 例如，对于音量滑块，始终将最大音量设置放在滑块顶部。 对于其他类型的值（例如星期几），请按照页面的阅读说明操作。
-   对于水平样式，如果是从左到右的页面布局，请将较小的值放在滑块的左侧；如果是从右到左的页面布局，请将较小的值放在滑块的右侧。
-   上述指南的一个例外是，对于媒体定位栏：始终将较小的值放在滑块的左侧。

### <a name="steps-and-tick-marks"></a>步长和刻度线

-   如果你不希望滑块允许使用最小值和最大值之间的任意值，请使用步骤点。例如，如果使用滑块指定要购买的电影票数，请不要允许浮点值。 为它提供一个步骤值 1。
-   如果你指定步骤（也称为贴靠点），请确保最后一步与滑块的最大值对齐。
-   当你希望向用户显示主要或有意义值的位置时，请使用刻度线。 例如，控制缩放的滑块可能有 50%、100% 和 200% 的刻度线。
-   当用户需要了解设置的近似值时显示刻度线。
-   当用户需要知道他们所选择的设置的确切值而不想与控件交互时，显示刻度线和值标签。 否则，他们可以使用值工具提示来查看确切值。
-   当步骤点不明显时，始终显示刻度线。 例如，如果滑块的宽度为 200 像素，有 200 个贴靠点，则可以隐藏刻度线，因为用户不会注意到贴靠行为。 但是如果只有 10 个贴靠点，则显示刻度线。

### <a name="labels"></a>标签

-   **滑块标签**

    滑块标签指示滑块用于何处。

    -   使用没有结束标点的标签（对于所有控件标签而言，这是惯例）。
    -   当滑块采用的形式会将它的大部分标签放在其控件上方时，应将标签放在滑块上方。
    -   当滑块采用的形式会将它的大部分标签放在其控件一侧时，请将标签放在滑块的一侧。
    -   避免将标签放在滑块下方，因为当用户触摸滑块时，用户的手指可能会盖住标签。
-   **范围标签**

    范围（或填充）标签描述滑块的最小和最大值。

    -   标记滑块范围的两端，除非垂直方向使得不必要这样做。
    -   如果可能，为每个标签仅使用一个词。
    -   不要使用结束标点符号。
    -   确保这些标签具有描述性且相对应。 示例：最大/最小，更多/更少，高/低、温和/高声。
-   **值标签**

    值标签显示滑块的当前值。

    -   如果需要值标签，将它显示在滑块下。
    -   相对于控件居中放置文本，并包括单位（例如像素）。
    -   由于滑块的 Thumb 控件在推动时被遮盖，请考虑以其他方式（采用标签或其他视觉对象）显示当前值。 用于设置文本大小的滑块可以在滑块旁边呈现一些正确大小的示例文本。

### <a name="appearance-and-interaction"></a>外观和交互

滑块由一条轨迹和 Thumb 控件构成。 该轨迹是一个栏（它可以选择性地显示各种刻度线样式），它代表可以输入的值范围。 Thumb 控件是一个选择器，用户可通过点击轨迹或来回推动来决定它的位置。

滑块具有较大的触摸目标。 要维护触摸辅助功能，滑块应该与显示器边缘保持足够远的距离。

设计自定义滑块时，请考虑尽可能清楚地向用户提供所有需要的信息的方法。 如果用户需要通过了解单位来了解设置，请使用值标签；寻找使用图形创意地代表这些值的方法。 例如，控制音量的滑块可以在滑块最小值一端显示不带有音波的扬声器图形，并在最大值一端显示带有音波的扬声器图形。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-topics"></a>相关主题
- [切换开关](toggles.md)
- [Slider 类](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider)
