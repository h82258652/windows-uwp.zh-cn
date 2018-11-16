---
author: Karl-Bridge-Microsoft
Description: Use visual feedback to show users when their interactions with a UWP app are detected, interpreted, and handled.
title: 视觉反馈
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: 视觉反馈, 焦点反馈, 触摸反馈, 接触可视化, 输入, 交互
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3440a0c64c0546b12c7f2c1bf1ea30168886d1db
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2018
ms.locfileid: "6976668"
---
# <a name="guidelines-for-visual-feedback"></a>视觉反馈指南

当检测、解释和处理用户的交互时，可使用视觉反馈显示给用户。 视觉反馈可通过鼓励交互来帮助用户。 它将指示交互是否成功，以加强用户的控制感觉。 它还可以传送系统状态并减少错误。

> **重要 API**：[**Windows.Devices.Input**](https://msdn.microsoft.com/library/windows/apps/br225648)、[**Windows.UI.Input**](https://msdn.microsoft.com/library/windows/apps/br242084)、[**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)

## <a name="recommendations"></a>建议

- 尝试将控件模板的修改限制为与设计意图直接相关的部分，因为大量更改可能会影响控件和应用程序的性能及辅助功能。 
    - 有关自定义控件属性（包括视觉状态属性）的详细信息，请参阅 [XAML 样式](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles)。
    - 有关更改控件模板的详细信息，请参阅 [UserControl 类](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol)
    - 如果需要对控件模板进行重大更改，请考虑创建自定义模板化控件。 有关自定义模板化控件的示例，请参阅[自定义编辑控件示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)。
- 请勿使用可能会干扰应用使用的触摸视觉化。 有关详细信息，请参阅 [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969)。
- 除非绝对有必要，否则不要显示反馈。 除非你要添加的值在其他任何地方都不可用，否则，请不要显示视觉反馈来保持 UI 干净整洁。
- 尽量不要大幅自定义内置 Windows 手势的视觉反馈行为，因为这样会产生不一致的情况，并且会带来混淆的用户体验。

## <a name="additional-usage-guidance"></a>其他使用指南

对于要求准确性和精度的触摸交互来说，触摸可视化是非常重要的。 例如，你的应用应清楚地指示点击位置，以便让用户知道他们是否错过了自己的目标、他们错过目标多长时间，以及他们必须进行哪些调整。

使用可用的默认 XAML 平台控件，可确保你的应用在所有设备上以及在所有输入情形下都可正常工作。 如果你的应用具有需要自定义反馈的自定义交互功能，你应该确保反馈合适、支持各种输入设备并且不会干扰用户执行其任务。 在视觉反馈可能与关键 UI 冲突或者甚至会遮盖关键 UI 的游戏或绘图应用中，这可能会是特殊问题。

> [!Important]
> 我们不建议更改内置手势的交互行为。

**跨设备反馈**

视觉反馈通常依赖输入设备（触摸、触摸板、鼠标、笔/触笔、键盘等）。 例如，鼠标的内置反馈通常涉及到移动和更改光标，而触摸和笔需要接触可视化，键盘输入和导航使用焦点矩形和突出显示。

使用 [**ShowGestureFeedback**](https://msdn.microsoft.com/library/windows/apps/br241969) 设置平台手势的反馈行为。

如果要自定义反馈 UI，请确保你提供支持而且适合所有输入模式的反馈。

下面是 Windows 中内置的接触可视化的示例。

| ![触摸反馈](images/TouchFeedback.png) | ![鼠标反馈](images/MouseFeedback.png) | ![笔反馈](images/PenFeedback.png) | ![键盘反馈](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| 触摸可视化 | 鼠标/触摸板可视化 | 笔可视化 | 键盘可视化 |

## <a name="high-visibility-focus-visuals"></a>高可见性焦点视觉

所有 Windows 应用都在应用程序内的可互动控件的周围明显定义了许多焦点视觉。 这些新的焦点视觉完全可以根据需要进行自定义以及禁用。

对于 Xbox 和电视通常使用的 **10 英尺体验**，Windows 支持**显示焦点** - 一种灯光效果，当可聚焦元素（例如按钮）通过游戏板或键盘输入获得焦点时，其边框将显示动画效果。 有关详细信息，请参阅[针对 Xbox 和电视进行设计](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv#reveal-focus)。

## <a name="color-branding--customizing"></a>颜色外观方案和自定义

**边框属性**

高可见性焦点视觉分为两部分：主边框和辅助边框。 主边框为 **2px** 粗，在辅助边框的*外部*周围运行。 辅助边框为 **1px** 粗，在主边框的*内部*周围运行。
![高可见性焦点视觉去除](images/FocusRectRedlines.png)

若要更改任一边框类型（主或辅助）的粗细，请分别使用 **FocusVisualPrimaryThickness** 或 **FocusVisualSecondaryThickness**。
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![高可见性焦点视觉边距厚度](images/FocusMargin.png)

边距是类型 [**Thickness**](https://msdn.microsoft.com/library/system.windows.thickness) 的属性，因此可以自定义边距以仅显示在控件的特定侧。 如下所示：![仅底部显示高可见性焦点视觉边距厚度](images/FocusThicknessSide.png)

边距是控件的视觉边界与焦点视觉*辅助边框*的开始部分之间的间距。 默认边距与控件边界的距离为 **1px**。 可以通过更改 **FocusVisualMargin** 属性来基于每个控件编辑此边距：
```XAML
<Slider Width="200" FocusVisualMargin="-5"/>
```
![高可见性焦点视觉边距差异](images/FocusPlusMinusMargin.png)

*负边距会将边框推离控件的中心，而正边距会将边框移向控件的中心。*

若要完全关闭基于控件的焦点视觉，只需禁用 **UseSystemFocusVisuals**：
```XAML
<Slider Width="200" UseSystemFocusVisuals="False"/>
```

粗细、边距或者是否呈现应用开发人员所希望的焦点视觉，完全取决于每个控件。

**颜色属性**

焦点视觉仅有两种颜色属性：主边框颜色和辅助边框颜色。 这些焦点视觉边框颜色可以在页面级别对每一控件进行更改，而在应用级别则为全局更改：

若要呈现应用级别的焦点视觉，请替代系统画笔：
```XAML
<SolidColorBrush x:Key="SystemControlFocusVisualPrimaryBrush" Color="DarkRed"/>
<SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="Pink"/>
```
![高可见性焦点视觉颜色更改](images/FocusRectColorChanges.png)

若要基于每个控件更改颜色，只需编辑所需控件的焦点视觉属性：
```XAML
<Slider Width="200" FocusVisualPrimaryBrush="DarkRed" FocusVisualSecondaryBrush="Pink"/>
```

## <a name="related-articles"></a>相关文章

**对于设计人员**
* [平移指南](guidelines-for-panning.md)

**对于开发人员**
* [自定义用户交互](https://msdn.microsoft.com/library/windows/apps/mt185599)

**示例**
* [基本输入示例](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [低延迟输入示例](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [用户交互模式示例](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [焦点视觉示例](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**存档示例**
* [输入：XAML 用户输入事件示例](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [输入：设备功能示例](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [输入：触摸点击测试示例](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML 滚动、平移以及缩放示例](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [输入：简化的墨迹示例](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [输入：Windows 8 手势示例](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [输入：操作和手势 (C++) 示例](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX 触控输入示例](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 
