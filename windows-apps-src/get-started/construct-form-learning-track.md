---
author: QuinnRadich
title: 学习轨迹 - 构建和配置表单
description: 了解在应用中创建可靠表单所需完成的工作。
ms.author: quradic
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: 入门, uwp, windows 10, 学习轨迹, 布局, 表单
ms.localizationpriority: medium
ms.openlocfilehash: c2a851a442cabca4529cd202c90db692c43adcb5
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/05/2018
ms.locfileid: "4391287"
---
# <a name="create-and-customize-a-form"></a>创建和自定义表单

在创建需要用户输入大量信息的应用时，可能需要创建表单来供用户填写。本文介绍创建有用和可靠的表单所需了解的知识。

这不是教程。 如果想查看教程，请参阅我们的[自适应布局教程](../design/basics/xaml-basics-adaptive-layout.md)，它提供了分步指南体验。

我们将讨论表单包含哪些 **XAML 控件**，如何最好地在页面上安排它们，以及如何为不同的屏幕大小优化表单。 但由于表单的核心内容在于视觉元素的相对位置，因此我们先讨论使用 XAML 时的页面布局。

## <a name="what-do-you-need-to-know"></a>需要了解哪些内容？

UWP 没有可添加到应用并进行配置的专门表单控件。 因此，需要通过在页面上安排一组 UI 元素来创建表单。

为此，你需要了解**布局面板**。 布局面板是容纳应用 UI 元素的容器，允许你安排 UI 元素和进行分组。 将布局面板放置在其他布局面板中，可以很好地控制一个项目相对于其他项目的位置和显示方式。 这也可以让应用更轻松地适应不同的屏幕大小。

请参阅[布局面板文档](../design/layout/layout-panels.md)。 表单通常显示在一个或多个垂直列中，因此需要将相似项目分组到 **StackPanel** 中，并根据需要将它们安排到 **RelativePanel** 中。 现在我们开始组织一些面板 - 如果需要参考，下面是两列表单的基本布局框架：

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Customer">
        <!--Save and Cancel buttons-->
    </StackPanel>
</RelativePanel>
```

## <a name="what-goes-in-a-form"></a>表单包含哪些元素？

需要使用各种 [XAML 控件](../design/controls-and-patterns/controls-and-events-intro.md)填充表单。 你可能对这些控件很熟悉，但如果需要复习，请重温以下参考资料。 特别是，你需要允许用户输入文本或从值列表中进行选择的控件。 以下是你可以添加的选项的基本列表-无需阅读有关它们的一切内容只足够需要了解它们的外观及其工作方式。

* [TextBox](../design/controls-and-patterns/text-box.md)到你的应用允许用户输入的文本。
* [ToggleSwitch](../design/controls-and-patterns/toggles.md) 让用户从两个选项中做出选择。
* [DatePicker](../design/controls-and-patterns/date-picker.md) 让用户选择一个日期值。
* [TimePicker](../design/controls-and-patterns/time-picker.md) 让用户选择一个时间值。
* [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) 展开以显示可选项目的列表。 可在[此处](../design/controls-and-patterns/lists.md#drop-down-lists)了解相关详细信息。

可能还需要添加[按钮](../design/controls-and-patterns/buttons.md)，以便用户可以保存或取消。

## <a name="format-controls-in-your-layout"></a>在布局中格式化控件

你知道如何安排布局面板和如何添加所需项目，但该如何格式化它们呢？ [表单](../design/controls-and-patterns/forms.md)页面有一些特定的设计指南。 请阅读**表单类型**和**布局**部分，以获取有用的建议。 很快我们将讨论辅助功能和相对布局。

请牢记以下建议：在向布局中添加选择的控件时，请确保为其分配标签和提供适当的间隔。 例如，下面是采用上述布局、控件和设计指南的单页表单的基本框架：

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" HorizontalAlignment="Left" />
            <RelativePanel>
                <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" HorizontalAlignment="Left" />
                <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0" RelativePanel.RightOf="City">
                    <!--List of valid states-->
                </ComboBox>
            </RelativePanel>
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.Below="Customer">
        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Associate">
        <Button Content="Save" Margin="24" />
        <Button Content="Cancel" Margin="24" />
    </StackPanel>
</RelativePanel>
```

根据需要对控件的更多属性进行自定义，以获得更好的视觉体验。

## <a name="make-your-layout-responsive"></a>采用响应式布局

用户可能会在具有不同屏幕宽度的各种设备上查看 UI。 为了确保他们在任意屏幕上都能获得良好的体验，应该采用[响应式设计](../design/layout/responsive-design.md)。 请通读该页面，以获得关于设计理念的良好建议并在继续时予以实践。

[采用 XAML 的响应式布局](../design/layout/layouts-with-xaml.md)页面详细介绍了如何实现这一点。 现在，我们将着重介绍**流动布局**和 **XAML 中的可视状态**。

上面给出的基本表单框架就是一个**流动布局**，因为它主要取决于控件的相对位置，最大限度减少了特定像素大小和位置的使用。 请在日后创建更多 UI 时牢记这一点。

对于响应式布局来说，更重要的是**可视状态**。 可视状态定义了当给定条件为真时应用于给定元素的属性值。 [了解如何在 XAML 中进行该操作](../design/layout/layouts-with-xaml.md#set-visual-states-in-xaml-markup)，然后在表单中实现它们。 下面对我们之前的示例做了更改，实现了一个*非常* 基础的可视状态：

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState>
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="640" />
            </VisualState.StateTriggers>

            <VisualState.Setters>
                <Setter Target="Associate.(RelativePanel.RightOf)" Value="Customer"/>
                <Setter Target="Associate.(RelativePanel.Below)" Value=""/>
                <Setter Target="Save.(RelativePanel.Below)" Value="Customer"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>

<RelativePanel>
    <!--Previous 3 stack panels-->
</RelativePanel>
```

为各种屏幕大小创建可视状态不切实际，而且也不大可能对应用的用户体验产生重大影响。 建议改为对关键断点进行设计 - 有关详细信息，请[在此了解更多信息](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)。

## <a name="add-accessibility-support"></a>添加辅助功能支持

设计好能够响应屏幕大小变化的良好布局后，改进用户体验的最后一步是[为应用添加辅助功能](../design/accessibility/accessibility-overview.md)。 关于这一点，可探讨的内容有很多，但就本例中的表单而言，实现起来非常容易。 重点关注以下内容：

* 键盘支持 - 确保 UI 面板中元素的顺序与其在屏幕上的显示方式相匹配，以便用户能够轻松地用 TAB 键循环选择它们。
* 屏幕阅读器支持 - 确保所有控件都具有描述性名称。

在创建包含更多视觉元素的复杂布局时，请查阅[辅助功能清单](../design/accessibility/accessibility-checklist.md)了解详细信息。 虽然辅助功能对应用来说不是必不可少的，但它能帮助应用覆盖和吸引更多受众。

## <a name="going-further"></a>深入探索

虽然本文创建的只是一个表单，但布局和控件的概念适用于你可能构建的所有 XAML UI。 可随意返回参阅我们链接给你和试验的表单，添加新 UI 功能并进一步改进用户体验的文档。 如果你希望通过更详细的布局功能的分步指南，请参阅我们的[自适应布局教程](../design/basics/xaml-basics-adaptive-layout.md)

表单不一定非得单独使用，你也可以更进一步 - 将它们嵌入到[大纲/细节模式](../design/controls-and-patterns/master-details.md)或[透视表控件](../design/controls-and-patterns/tabs-pivot.md)中。 或者，如果要对表单背后的代码做些改动，请参阅[事件概述](../xaml-platform/events-and-routed-events-overview.md)。

## <a name="useful-apis-and-docs"></a>有用的 API 和文档

以下是 API 和其他有用文档的核心简要，可帮助你了解如何处理数据绑定。

### <a name="useful-apis"></a>有用的 API

| API | 描述 |
|------|---------------|
| [对表单有用的控件](../design/controls-and-patterns/forms.md#input-controls) | 用于创建表单的有用输入控件的列表，以及在何处使用它们的基本指南。 |
| [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) | 用于以多行或多列布局安排元素的面板。 |
| [RelativePanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) | 用于相对于其他元素和面板边界安排项目的面板。 |
| [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) | 用于以单行或单列布局安排元素的面板。 |
| [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) | 允许你设置 UI 元素处于特定状态时的外观。 |

### <a name="useful-docs"></a>有用的文档

| 主题 | 描述 |
|-------|----------------|
| [辅助功能概述](../design/accessibility/accessibility-overview.md) | 对应用中辅助功能选项的宽泛介绍。 |
| [辅助功能清单](../design/accessibility/accessibility-checklist.md) | 确保应用符合辅助功能标准的实用清单。 |
| [事件概述](../xaml-platform/events-and-routed-events-overview.md) | 有关添加和构造事件以处理 UI 操作的详细信息。 |
| [表单](../design/controls-and-patterns/forms.md) | 关于创建表单的总括指南。 |
| [布局面板](../design/layout/layout-panels.md) | 概述布局面板类型以及在何处使用它们。 |
| [大纲/细节模式](../design/controls-and-patterns/master-details.md) | 可以围绕一个或多个表单实现的设计模式。 |
| [Pivot 控件](../design/controls-and-patterns/tabs-pivot.md) | 一种可以包含一个或多个表单的控件。 |
| [响应式设计](../design/layout/responsive-design.md) | 对响应式设计原则的宽泛介绍。 | 
| [采用 XAML 的响应式布局](../design/layout/layouts-with-xaml.md) | 关于响应式设计可视状态及其他实现的特定信息。 |
| [面向响应式设计的屏幕大小](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) | 关于应将响应式布局限制在哪些屏幕大小范围内的指南。 |

## <a name="useful-code-samples"></a>有用的代码示例

| 代码示例 | 描述 |
|-----------------|---------------|
| [自适应布局教程](../design/basics/xaml-basics-adaptive-layout.md) | 对于自适应布局和响应式设计的分步指导体验。 | 
| [客户订单数据库](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | 了解布局和表单在多页企业示例中的应用。 |
| [XAML 控件库](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) | 了解一些精选的 XAML 控件及其实现方式。 |
| [其他代码示例](https://developer.microsoft.com//windows/samples) | 在类别下拉列表中选择**控件、布局和文本**，以查看相关代码示例。 |
