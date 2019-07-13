---
Description: 为 TextBox、RichEditBox 等 UWP 文本控件（以及 AutoSuggestBox 之类的提供类似文本输入体验的控件）支持的墨迹转文本输入自定义内置的手写视图。
title: 带手写视图的文本输入
label: Text input with the handwriting view
template: detail.hbs
ms.date: 10/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 262e033c8393544e3b5b8394d0e9a5be410cd080
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319479"
---
# <a name="text-input-with-the-handwriting-view"></a>带手写视图的文本输入

![用笔点击时，文本框会展开](images/handwritingview/handwritingview2.gif)

为 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)、[RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) 等 UWP 文本控件以及从这些控件派生的控件（例如 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)）支持的墨迹转文本输入自定义内置的手写视图。

## <a name="overview"></a>概述

XAML 文本输入框默认支持使用 [Windows Ink](../input/pen-and-stylus-interactions.md) 进行笔输入。 当用户使用 Windows 笔点击进入文本输入框时，文本框会转换为手写图面，而不是打开一个单独的输入面板。

当用户在文本框中任意位置书写时，系统会进行文本识别，并有候选窗口显示识别结果。 用户可以点击结果进行选择，也可以继续书写以接受建议的候选字词。 候选窗口中包含文本（以及字母）识别结果，因此识别不限于字典中的字词。 当用户书写时，接受的文本输入会被转换为保持自然书写感觉的脚本字体。

> [!NOTE]
> 手写视图是默认启用的，但你可以在每个控件上禁用它，恢复使用文本输入面板。

![带墨迹和提示的文本框](images/handwritingview/handwritingview-inksuggestion1.gif)

用户可以使用标准手势和操作编辑其文本，如下面这些：

- 删除  或擦除  - 在字词中间划一横以删除某个字词或字词的一部分
- 联接  - 在字词之间绘制弧线以删除它们之间的空格
- 插入  - 绘制一个插入符号以插入空格
- 覆盖  - 在现有文本上书写以替换它

![带墨迹更正功能的文本框](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>禁用手写视图

默认启用内置的手写视图。

在以下情况下，可能需要禁用手写视图：已经在应用程序中提供等效的墨迹转文本功能，或者文本输入体验依赖于无法通过手写获取的某类格式设置或特殊字符（例如制表符）。

在此示例中，我们禁用手写视图的方式是将 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) 控件的 [IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled) 属性设置为 false。 所有支持手写视图的文本控件都支持一个类似的属性。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>指定手写视图的对齐方式

手写视图位于基础文本控件上方，其大小适合用户的手写首选项（请查看“设置”->“设备”->“笔和 Windows Ink”->“手写”->“直接写入文本字段中时的字体的大小”）。  此视图还会自动对齐应用中的文本控件及其位置。

应用程序 UI 不会针对较大的控件来重新排列，因此系统可能会导致此视图遮蔽重要 UI。

在这里，我们演示如何使用 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 的 [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment) 属性来指定使用基础文本控件上的哪个定位点来对齐手写视图。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView PlacementAlignment="TopLeft"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="disable-auto-completion-candidates"></a>禁用自动完成候选项

文本建议弹出窗口默认启用，用于提供一系列排名靠前的墨迹识别候选项，供用户在顶级候选项不正确的情况下进行选择。

如果应用程序已经提供强大的自定义识别功能，则可使用 [AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled) 属性禁用内置的建议，如以下示例所示。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen">
        <TextBox.HandwritingView>
            <HandwritingView AreCandidatesEnabled="False"/>
        </TextBox.HandwritingView>
</TextBox>
```

## <a name="use-handwriting-font-preferences"></a>使用手写字体首选项

基于墨迹识别呈现文本时，用户可以从预定义的一系列基于手写的字体中进行选择（请查看“设置”->“设备”->“笔和 Windows Ink”->“手写”->“使用手写时的字体”）。 

> [!NOTE]
> 用户甚至可以根据自己的手写内容创建字体。
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

应用可以访问此设置，并在文本控件中使用所识别文本的选定字体。

在以下示例中，我们侦听 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) 的 [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged) 事件，并在文本更改源自 HandwritingView 的情况下应用用户的选定字体（否则应用默认字体）。

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>在复合控件中访问 HandwritingView

使用 [TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) 或 [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox) 控件（例如 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)）的复合控件也支持 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)。

若要在复合控件中访问 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)，请使用 [VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API。

以下 XAML 代码片段显示 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) 控件。

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

在相应的代码隐藏中，我们演示了如何禁用 [AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox) 上的 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)。

1. 首先，我们处理应用程序的 Loaded 事件，调用 FindInnerTextBox 函数来启动可视化树遍历。

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. 然后，我们开始调用 FindVisualChildByName，在 FindInnerTextBox 函数中循环访问可视化树（从 AutoSuggestBox 开始）。

    ```csharp
    private bool FindInnerTextBox(AutoSuggestBox autoSuggestBox)
    {
        if (autoSuggestInnerTextBox == null)
        {
            // Cache textbox to avoid multiple tree traversals. 
            autoSuggestInnerTextBox = 
                (TextBox)FindVisualChildByName<TextBox>(autoSuggestBox);
        }
        return (autoSuggestInnerTextBox != null);
    }
    ```

3. 最后，该函数循环访问可视化树，直至检索 TextBox。

    ```csharp
    private FrameworkElement FindVisualChildByName<T>(DependencyObject obj)
    {
        FrameworkElement element = null;
        int childrenCount = 
            VisualTreeHelper.GetChildrenCount(obj);
        for (int i = 0; (i < childrenCount) && (element == null); i++)
        {
            FrameworkElement child = 
                (FrameworkElement)VisualTreeHelper.GetChild(obj, i);
            if ((child.GetType()).Equals(typeof(T)) || (child.GetType().GetTypeInfo().IsSubclassOf(typeof(T))))
            {
                element = child;
            }
            else
            {
                element = FindVisualChildByName<T>(child);
            }
        }
        return (element);
    }
    ```

## <a name="reposition-the-handwritingview"></a>重新定位 HandwritingView

在某些情况下，可能需确保 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 涵盖在其他情况下可能不会涵盖的 UI 元素。

在这里，我们创建一个支持听写的 TextBox（其实现方式是：将 TextBox 和听写按钮置于 StackPanel 中）。

![使用听写的 TextBox](images/handwritingview/textbox-with-dictation.png)

由于 StackPanel 现在比 TextBox 大，[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 可能不会遮蔽所有复合控件。

![使用听写的 TextBox](images/handwritingview/textbox-with-dictation-handwritingview.png)

为了解决此问题，请将 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 的 PlacementTarget 属性设置为应该与之对齐的 UI 元素。

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" BorderBrush="DarkGray" 
    Height="55" Width="500" Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" BorderThickness="0" 
        FontSize="24" VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView PlacementTarget="{Binding ElementName=DictationBox}"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray"     Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="resize-the-handwritingview"></a>重设 HandwritingView 大小

也可设置 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 的大小。在需要确保视图不遮蔽重要 UI 时，可以使用该类。

与以前的示例一样，我们创建一个支持听写的 TextBox（其实现方式是：将 TextBox 和听写按钮置于 StackPanel 中）。

![使用听写的 TextBox](images/handwritingview/textbox-with-dictation.png)

在这种情况下，需确保听写按钮始终可见。

![使用听写的 TextBox](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

为此，我们将 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 的 MaxWidth 属性绑定到 UI 元素的遮蔽宽度。

```xaml
<StackPanel Name="DictationBox" 
    Orientation="Horizontal" 
    VerticalAlignment="Top" 
    HorizontalAlignment="Left" 
    BorderThickness="1" 
    BorderBrush="DarkGray" 
    Height="55" Width="500" 
    Margin="50">
    <TextBox Name="DictationTextBox" 
        Width="450" 
        BorderThickness="0" 
        FontSize="24" 
        VerticalAlignment="Center">
        <TextBox.HandwritingView>
            <HandwritingView 
                PlacementTarget="{Binding ElementName=DictationBox}“
                MaxWidth="{Binding ElementName=DictationTextBox, Path=Width"/>
        </TextBox.HandwritingView>
    </TextBox>
    <Button Name="DictationButton" 
        Height="48" Width="48" 
        FontSize="24" 
        FontFamily="Segoe MDL2 Assets" 
        Content="&#xE720;" 
        Background="White" Foreground="DarkGray" 
        Tapped="DictationButton_Tapped" />
</StackPanel>
```

## <a name="reposition-custom-ui"></a>重新定位自定义 UI

如果有一个在响应文本输入时会显示的自定义 UI（例如信息性弹出窗口），则可能需要重新定位该 UI，确保它不遮蔽手写视图。

![带自定义 UI 的 TextBox](images/handwritingview/textbox-with-customui.png)

以下示例演示了如何侦听 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 的 [Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened)、[Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed) 和 [SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged) 事件，以便设置[弹出窗口](https://docs.microsoft.com/uwp/api/windows.ui.popups)的位置。

```csharp
private void Search_HandwritingViewOpened(
    HandwritingView sender, HandwritingPanelOpenedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewClosed(
    HandwritingView sender, HandwritingPanelClosedEventArgs args)
{
    UpdatePopupPositionForHandwritingView();
}

private void Search_HandwritingViewSizeChanged(
    object sender, SizeChangedEventArgs e)
{
    UpdatePopupPositionForHandwritingView();
}

private void UpdatePopupPositionForHandwritingView()
{
if (CustomSuggestionUI.IsOpen)
    CustomSuggestionUI.VerticalOffset = GetPopupVerticalOffset();
}

private double GetPopupVerticalOffset()
{
    if (SearchTextBox.HandwritingView.IsOpen)
        return (SearchTextBox.Margin.Top + SearchTextBox.HandwritingView.ActualHeight);
    else
        return (SearchTextBox.Margin.Top + SearchTextBox.ActualHeight);    
}
```

## <a name="retemplate-the-handwritingview-control"></a>重新设置 HandwritingView 控件的模板

就像使用所有 XAML 框架控件一样，可以按特定要求自定义 [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 的可视结构和可视行为。

若要查看有关如何创建自定义模板的完整示例，请参阅[创建自定义传输控件](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls)操作方法或[自定义编辑控件示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)。








