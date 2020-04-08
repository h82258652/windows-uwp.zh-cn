---
title: 角半径
description: 了解圆角原则、设计方法和自定义选项。
ms.date: 10/08/2019
ms.topic: article
keywords: windows 10, uwp, 角半径, 圆
ms.openlocfilehash: a83473b5ad836633bc195aa2b5afe87fa092e0ee
ms.sourcegitcommit: 3c3730e968fba89b21459390735614cd4c9d9c67
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2020
ms.locfileid: "80320431"
---
# <a name="corner-radius"></a>角半径

从 2.2 版 [Windows UI 库](/uwp/toolkits/winui/) (WinUI) 开始，许多控件的默认样式已更新为使用圆角。 这些新样式旨在激起用户的温暖和信任感觉，使 UI 更易于被用户进行可视化处理。

下面是两个按钮控件，第一个没有圆角，第二个使用新的圆角样式。

![没有圆角的按钮和有圆角的按钮](images/rounded-corner/my-button.png)

安装 WinUI 2.2 或更高版的 NuGet 包时，会为 WinUI 控件和平台控件安装新的默认样式。 在应用中使用 WinUI 2.2 时，系统会自动使用这些样式；不需执行进一步的操作即可使用新样式。 不过，我们会在本文的后面部分介绍如何根据需要自定义圆角。

> [!IMPORTANT]
> 某些控件既可用于平台 ([Windows.UI.Xaml.Controls](/uwp/api/windows.ui.xaml.controls))，也可用于 WinUI ([Microsoft.UI.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls?view=winui-2.2))，例如 **TreeView** 或 **ColorPicker**。 在应用中使用 WinUI 时，应使用 WinUI 版控件。 角部圆化可能会在平台版中应用得不一致（与 WinUI 配合使用时）。

> **重要的 API**：[Control.CornerRadius 属性](/uwp/api/windows.ui.xaml.controls.control.cornerradius)

## <a name="default-control-designs"></a>默认控件设计

圆角样式用在三个控件区域：矩形元素、浮出控件元素和条形元素。

### <a name="corners-of-rectangle-ui-elements"></a>矩形 UI 元素的角

- 这些 UI 元素包含基本的控件，例如用户在屏幕上随时能够看到的按钮。
- 我们用于这些 UI 元素的默认半径值为 **2px**。

![突出显示圆角的按钮](images/rounded-corner/button.png)

**控件**

- AutoSuggestBox
- Button
  - ContentDialog 按钮
- CalendarDatePicker
- CheckBox
  - TreeView 多选复选框
- 组合框
- DatePicker
- DropDownButton
- FlipView
- PasswordBox
- RichEditBox
- SplitButton
- TextBox
- TimePicker
- ToggleButton
- ToggleSplitButton

### <a name="corners-of-flyout-and-overlay-ui-elements"></a>浮出控件和覆盖 UI 元素的角

- 这些元素可能是临时显示在屏幕上的瞬态 UI 元素（例如 MenuFlyout），也可能是覆盖其他 UI 的元素（例如 TabView 选项卡）。
- 我们用于这些 UI 元素的默认半径值为 **4px**。

![浮出控件示例](images/rounded-corner/flyout.png)

**控件**

- CommandBarFlyout
- ContentDialog
- 浮出控件
- MenuFlyout
- TabView 选项卡
- TeachingTip
- ToolTip
- 浮出控件部分（在打开的时候）
  - AutoSuggestBox
  - CalendarDatePicker
  - 组合框
  - DatePicker
  - DropDownButton
  - MenuBar
  - SplitButton
  - TimePicker
  - ToggleSplitButton

### <a name="bar-elements"></a>条形元素

- 这些 UI 元素（例如 ProgressBar）为条形或线形。
- 我们在这里使用的默认半径值为 **2px**。

![进度条示例](images/rounded-corner/bars.png)

**控件**

- NavigationView 选择指示器
- 透视选择指示器
- 进度栏
- ScrollBar（当 `IndicatorMode=TouchIndicator` 时）
- 滑块
  - ColorPicker 颜色滑块
  - MediaTransportControls 拖动条滑块

## <a name="customization-options"></a>自定义选项

我们提供的默认角半径值并非一成不变，可以通过几种方式轻松地修改角部的圆化程度。 可以通过两个全局资源来这样做，也可以通过 [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) 属性直接在控件上这样做，具体取决于所需的自定义粒度级别。

### <a name="when-not-to-round"></a>何时不使用圆化

有时候，控件的角部不应圆化，我们不默认将其圆化。

- 位于一个容器中的多个 UI 元素互相接触时，例如 SplitButton 的两个部分。 当它们接触时，不应留有空间。

![SplitButton](images/rounded-corner/split-button-2.png)

- 当一个控件位于另一个容器中时，例如 ScrollBar 的条和按钮，它们属于 ScrollBar 容器的一部分，而该容器也是 ScrollViewer 的一部分。

![ScrollBar](images/rounded-corner/scrollbar.png)

- 当某个浮出控件 UI 元素连接到某个 UI，而该 UI 在一侧调用该浮出控件时。

![AutoSuggest](images/rounded-corner/autosuggest.png)

### <a name="keyboard-focus-rectangle-and-shadow"></a>键盘焦点矩形和阴影

我们的默认设计并不特意圆化键盘焦点矩形或控件阴影的角部。 使用较高的角半径值不会在功能方面造成影响，但仍必须了解这一点，避免因值较大而引入不需要的视觉故障。

下面是一个示例，说明角半径较大可能会导致阴影看起来不舒服：

![ContentDialogShadow](images/rounded-corner/larger-corner-radius.png)

### <a name="rounded-corners-and-performance"></a>圆角和性能

渲染圆角自然会比渲染方角消耗更多的绘制力。 选择默认的角半径值时，我们不仅考虑设计原则，而且还需谨慎行事，确保默认控件在应用中使用时性能良好。

在这种情况下考虑应用性能时，应主要考虑页面加载时间和应用启动时间。 要考虑到，圆角在较大的 UI 图面上对性能的影响较大。 避免在全屏应用 UI 上绘制圆角。 如果 UI 显示时间短且是在页面加载后显示的（例如 ContentDialog），则这样做问题不大。

### <a name="page-or-app-wide-cornerradius-changes"></a>页面或应用范围的 CornerRadius 更改

可以通过 2 项应用资源来控制所有控件的角半径：

- `ControlCornerRadius` - 默认值为 2px。
- `OverlayCornerRadius` - 默认值为 4px。

如果在任意范围覆盖这些资源的值，则会相应地影响该范围内的所有控件。

这意味着，若要在能够应用圆度的地方更改所有控件的圆度，可以使用新的 CornerRadius 值在应用级别定义这两项资源，如下所示：

```xaml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <ResourceDictionary>
                <CornerRadius x:Key="OverlayCornerRadius">4</CornerRadius>
                <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
            </ResourceDictionary>
            <XamlControlsResources xmlns="using:Microsoft.UI.Xaml.Controls" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

或者，若要在特定范围内（例如在页面或容器级别）更改所有控件的圆度，可以遵循相似的模式：

```xaml
<Grid>
    <Grid.Resources>
        <CornerRadius x:Key="ControlCornerRadius">8</CornerRadius>
    </Grid.Resources>
    <Button Content="Button"/>
</Grid>
```

> [!NOTE]
> `OverlayCornerRadius` 资源必须在应用级别定义才能生效。
>
>这是因为弹出窗口和浮出控件是动态的，在可视化树中的根元素处创建，因此它们使用的任何资源也必须在该处定义。 否则，它们会超出范围。

### <a name="per-control-cornerradius-changes"></a>按控件进行的 CornerRadius 更改

如果只希望更改选定数量的控件的圆度，则可直接修改控件上的 [CornerRadius](/uwp/api/windows.ui.xaml.controls.control.cornerradius) 属性。

|默认值 | 修改的属性 |
|:-- |:-- |
|![DefaultCheckBox](images/rounded-corner/default-checkbox.png)| ![CustomCheckBox](images/rounded-corner/custom-checkbox.png)|
|`<CheckBox Content="Checkbox"/>` | `<CheckBox Content="Checkbox" CornerRadius="5"/> ` |

并非所有控件的角都会响应其被修改的 `CornerRadius` 属性。 若要确保你要将其角部圆化的控件会真正地按你期望的方式响应其 `CornerRadius` 属性，请先检查 `ControlCornerRadius` 或 `OverlayCornerRadius` 全局资源是否会影响相关控件。 如果它们不影响，请检查要圆化的控件是否有角。 我们的许多控件不渲染实际的边缘，因此不能正确使用 `CornerRadius` 属性。

### <a name="basing-custom-styles-on-winui"></a>基于 WinUI 创建自定义样式

在样式中指定正确的 `BasedOn`，可以使自定义样式以 WinUI 圆角样式为基础。 例如，若要创建以 WinUI 按钮样式为基础的自定义按钮样式，请执行以下操作：

```xaml
<Style x:Key="MyCustomButtonStyle" BasedOn="{StaticResource DefaultButtonStyle}">
   ...
</Style>
```

通常，WinUI 控件样式遵循一致的命名约定：“DefaultXYZStyle”，其中“XYZ”是控件的名称。 如需完整的参考，可以浏览 WinUI 存储库中的 XAML 文件。
