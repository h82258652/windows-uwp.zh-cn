---
Description: 自定义墨迹转换为 UWP 文本控件支持文本框中，如 RichEditBox （和类似 AutoSuggestBox 提供类似的文本输入的体验的控件） 的文本输入的内置手写视图。
title: 使用手写视图的文本输入
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
ms.openlocfilehash: f7b31898e6a90410e4edc73ee36f71a7e4d94155
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57634912"
---
# <a name="text-input-with-the-handwriting-view"></a>使用手写视图的文本输入

![用手写笔点击时，文本框会扩展](images/handwritingview/handwritingview2.gif)

自定义墨迹转换为文本输入由如 UWP 文本控件支持内置手写视图[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)， [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)，和控件如派生自这些[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)。

## <a name="overview"></a>概述

XAML 文本输入的框功能嵌入的支持笔输入使用[Windows 墨迹](../input/pen-and-stylus-interactions.md)。 当用户点击到使用 Windows 笔文本输入框中时，在文本框中将转换成手写内容图面，而不是打开一个单独的输入的面板。

如用户编写的任意位置中的文本框中和一个候选窗口会显示识别结果，识别文本。 用户可以点击结果进行选择，也可以继续书写以接受建议的候选字词。 候选窗口中包含文本（以及字母）识别结果，因此识别不限于字典中的字词。 当用户书写时，接受的文本输入会被转换为保持自然书写感觉的脚本字体。

> [!NOTE]
> 默认情况下，启用手写内容视图，但可以禁用基于每个控件，并改为还原到文本输入面板。

![具有墨迹和建议的文本框](images/handwritingview/handwritingview-inksuggestion1.gif)

用户可以使用标准手势和操作编辑其文本，如下面这些：

- _删除_ 或_擦除_ - 绘制通过以删除某个字词或字词部分
- _联接_ - 在字词之间绘制弧线以删除它们之间的空格
- _插入_ - 绘制一个插入符号以插入空格
- _覆盖_ - 在现有文本上书写以替换它

![使用墨迹更正的文本框中](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>禁用手写内容视图

默认情况下启用内置的手写内容视图。

你可能想要禁用手写内容视图，如果已在应用程序中提供等效的墨迹到文本功能或你的文本输入的体验依赖于某种类型的格式设置或特殊字符 （如选项卡上） 通过手写内容不可用。

在此示例中，我们手写视图通过设置来禁用[IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled)的属性[文本框中](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)控件为 false。 支持手写输入视图的所有文本控件都支持类似的属性。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>指定的对齐方式的手写内容视图

手写视图位于基础文本控件的上方，以适应用户的手写首选项 (请参阅**设置-> 设备-> 笔和墨迹 Windows-> 手写内容直接写入时的字体大小->文本字段**)。 该视图还会自动对齐相对于文本控件和应用内的其位置。

应用程序 UI 不会不重排，以容纳更大的控制，因此系统可能会导致要遮蔽重要 UI 的视图。

在这里，我们演示如何使用[PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment)的属性[文本框](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)以指定基础文本控件上的定位点用于对齐手写视图。

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

## <a name="disable-auto-completion-candidates"></a>禁用自动完成的候选项

默认情况下启用文本建议弹出窗口提供一系列顶部墨迹识别候选项，用户可以从中选择在前几个候选不正确的情况下。

如果应用程序已提供了可靠的自定义识别功能，则可以使用[AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled)属性可用于禁用内置的建议，如下面的示例中所示。

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

用户可以选择从预定义的基于手写的字体时要使用集合基于墨迹识别呈现文本 (请参阅**设置-> 设备-> 笔和墨迹 Windows-> 手写内容时使用的手写->字体**).

> [!NOTE]
> 用户甚至可以创建基于其自己的手写内容的字体。
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

您的应用程序可以访问此设置，并使用所选的字体进行文本控件中的已识别文本。

在此示例中，我们侦听[TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged)的事件[文本框中](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)和适用于用户的所选的字体的文本更改源自 HandwritingView （或默认字体，如果没有）。

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>访问 HandwritingView 复合控件中

复合控件，使用[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)或[RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)控件，如[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)还支持[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)。

访问[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)在复合控件中，使用[VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API。

以下 XAML 代码段显示[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)控件。

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

在相应的代码隐藏，我们演示如何禁用[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)上[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)。

1. 首先，我们处理应用程序的加载的事件，其中我们调用 FindInnerTextBox 函数以启动对可视化树遍历。

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. 然后，我们开始循环通过调用 FindVisualChildByName FindInnerTextBox 函数中的可视化树 （从 AutoSuggestBox 开始）。

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

3. 最后，此函数循环访问的可视化树直到检索到文本框中。

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

在某些情况下，您可能需要请确保[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)涵盖它否则不可能的 UI 元素。

在这里，我们创建一个文本框，用于支持听写 （通过在 StackPanel 中放置一个文本框和听写按钮来实现）。

![通过使用听写的文本框](images/handwritingview/textbox-with-dictation.png)

StackPanel 现已大于文本框中，如[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)不可能所有复合 cotnrol 遮蔽。

![通过使用听写的文本框](images/handwritingview/textbox-with-dictation-handwritingview.png)

若要解决此问题，设置的 PlacementTarget 属性[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)到应对齐到的用户界面元素。

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

## <a name="resize-the-handwritingview"></a>重设大小 HandwritingView

此外可以设置的大小[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)，这一点可以非常有用，当您需要确保视图时不会遮蔽重要的 UI。

如上述示例中，我们创建一个文本框，用于支持听写 （通过在 StackPanel 中放置一个文本框和听写按钮来实现）。

![通过使用听写的文本框](images/handwritingview/textbox-with-dictation.png)

在这种情况下，我们想要确保听写按钮始终可见。

![通过使用听写的文本框](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

若要执行此操作，我们将绑定的 MaxWidth 属性[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)应遮蔽的 UI 元素的宽度。

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

如果您有自定义 UI 都会出现在响应文本输入，如信息性弹出窗口中，您可能需要重新定位该 UI，使它不会遮蔽手写视图。

![具有自定义 UI 的 TextBox](images/handwritingview/textbox-with-customui.png)

下面的示例演示如何侦听[Opened](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened)， [Closed](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
)，和[SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged)事件[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)设置位置[弹出窗口](https://docs.microsoft.com/uwp/api/windows.ui.popups)。

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

## <a name="retemplate-the-handwritingview-control"></a>Retemplate HandwritingView 控件

与所有 XAML 框架控件，可以自定义可视结构和可视行为[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)针对特定要求。

若要创建自定义模板签出的完整示例，请参阅[创建自定义传输控件](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls)操作方法或[自定义编辑控件示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)。








