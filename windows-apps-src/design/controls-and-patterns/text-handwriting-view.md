---
author: Karl-Bridge-Microsoft
Description: Customize the built-in handwriting view for ink to text input that is supported by UWP text controls such as the TextBox, RichEditBox (and controls like the AutoSuggestBox that provide a similar text input experience).
title: 使用手写视图的文本输入
label: Text input with the handwriting view
template: detail.hbs
ms.author: kbridge
ms.date: 10/13/18
ms.topic: article
keywords: windows 10, uwp
pm-contact: sewen
design-contact: minah.kim
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: aa235086f2410fb97ea60e35fb03c586824928a2
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5883291"
---
# <a name="text-input-with-the-handwriting-view"></a>使用手写视图的文本输入

![用手写笔点击时，文本框会扩展](images/handwritingview/handwritingview2.gif)

自定义墨迹转换为受 UWP 文本控件，如[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)、 [RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)的文本输入的内置手写视图，并从这些如[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)派生的控件。

## <a name="overview"></a>概述

XAML 文本输入的框功能进行笔输入使用[Windows Ink](../input/pen-and-stylus-interactions.md)的嵌入式的支持。 当用户点击到使用 Windows 手写笔的文本输入框时，文本框将转换为手写图面，而不是打开一个单独的输入的面板。

当用户书写时任意位置即可在文本框中，另一个候选窗口显示识别结果，将识别文本。 用户可以点击结果进行选择，也可以继续书写以接受建议的候选字词。 候选窗口中包含文本（以及字母）识别结果，因此识别不限于字典中的字词。 当用户书写时，接受的文本输入会被转换为保持自然书写感觉的脚本字体。

> [!NOTE]
> 默认情况下，启用手写视图，但你可以禁用基于每个控件并改为恢复为文本输入面板。

![使用墨迹和建议的文本框](images/handwritingview/handwritingview-inksuggestion1.gif)

用户可以使用标准手势和操作编辑其文本，如下面这些：

- _删除_ 或_擦除_ - 绘制通过以删除某个字词或字词部分
- _联接_ - 在字词之间绘制弧线以删除它们之间的空格
- _插入_ - 绘制一个插入符号以插入空格
- _覆盖_ - 在现有文本上书写以替换它

![带有墨迹更正文本框](images/handwritingview/handwritingview-inkcorrection1.gif)

## <a name="disable-the-handwriting-view"></a>禁用手写视图

默认情况下启用内置手写视图。

你可能想要禁用手写视图，如果你已在你的应用程序中, 提供等效墨迹到文本转换功能或文本输入的体验依赖于某种类型的格式设置或特殊字符 （如选项卡） 通过手写不可用。

在此示例中，我们通过[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)控件[IsHandwritingViewEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.ishandwritingviewenabled)属性设置为 false 禁用手写视图。 支持手写视图的所有文本控件都支持的一个类似属性。

```xaml
<TextBox Name="SampleTextBox"
    Height="50" Width="500" 
    FontSize="36" FontFamily="Segoe UI" 
    PlaceholderText="Try taping with your pen" 
    IsHandwritingViewEnabled="False">
</TextBox>
```

## <a name="specify-the-alignment-of-the-handwriting-view"></a>指定手写视图的对齐方式

手写视图是上方基础的文本控件和调整大小以适应用户的手写首选项 (请参阅**设置-> 设备-> 笔和 Windows Ink-> 手写时直接写入文本字段-> 的字体大小**). 视图还会自动相对于文本控件和应用内的位置对齐。

应用程序 UI 不会重排以适应更大的控件，因此，系统可能会导致视图以会阻挡重要的 UI。

此处，我们显示了如何使用[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) [PlacementAlignment](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.placementalignment)属性来指定基础的文本控件上的定位点用于将手写视图对齐。

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

默认情况下启用文本建议弹出窗口提供识别候选项，用户可以从中选择的最顶端的候选项不正确的情况下的最顶端的墨迹的列表。

如果你的应用程序已提供可靠，自定义识别功能，你可以使用[AreCandidatesEnabled](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.arecandidatesenabled)属性若要禁用内置的建议，如下面的示例中所示。

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

用户可以从集合中选择预定义的基于手写字体时要使用基于墨迹识别呈现文本 (请参阅**设置-> 设备-> 笔和 Windows Ink-> 手写字体-> 时使用手写**)。

> [!NOTE]
> 用户甚至可以创建基于其自己的手写字体。
> [!VIDEO https://www.youtube.com/embed/YRR4qd4HCw8]

你的应用可以访问此设置，并使用所选的字体的文本控件中识别出的文本。

在此示例中，我们可以侦听[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox) [TextChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.textchanged)事件，并应用在用户选择的字体，如果文本更改来自 HandwritingView （或默认字体，如果不是）。

```csharp
private void SampleTextBox_TextChanged(object sender, TextChangedEventArgs e)
{
    ((TextBox)sender).FontFamily = 
        ((TextBox)sender).HandwritingView.IsOpen ?
            new FontFamily(PenAndInkSettings.GetDefault().FontFamilyName) : 
            new FontFamily("Segoe UI");
}
```

## <a name="access-the-handwritingview-in-composite-controls"></a>复合控件中 HandwritingView 的访问权限

此外使用[TextBox](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.textbox)或[RichEditBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.richeditbox)控件，如[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)的复合控件支持[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)。

若要访问[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)复合控件中，使用[VisualTreeHelper](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.visualtreehelper) API。

以下 XAML 代码段显示[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)控件。

```xaml
<AutoSuggestBox Name="SampleAutoSuggestBox" 
    Height="50" Width="500" 
    PlaceholderText="Auto Suggest Example" 
    FontSize="16" FontFamily="Segoe UI"  
    Loaded="SampleAutoSuggestBox_Loaded">
</AutoSuggestBox>
```

在相应的代码隐藏中，我们显示了如何禁用[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)上[AutoSuggestBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.autosuggestbox)。

1. 首先，我们处理应用程序的 Loaded 的事件，我们调用开始菜单可视化树遍历 FindInnerTextBox 函数。

    ```csharp
    private void SampleAutoSuggestBox_Loaded(object sender, RoutedEventArgs e)
    {
        if (FindInnerTextBox((AutoSuggestBox)sender))
            autoSuggestInnerTextBox.IsHandwritingViewEnabled = false;
    }
    ```

2. 然后，我们开始循环调用 FindVisualChildByName FindInnerTextBox 功能中的可视化树 （从 AutoSuggestBox 开始）。

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

3. 最后，此函数循环的可视化树直到检索 TextBox。

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

在某些情况下，你可能需要确保[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)介绍它否则不可能的 UI 元素。

此处，我们将创建一个文本框，用于支持 （由将文本框和听写按钮放入 StackPanel 实现） 的听写。

![听写的文本框](images/handwritingview/textbox-with-dictation.png)

在 StackPanel 大于现在 TextBox，如[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)可能不会会阻挡所有复合 cotnrol。

![听写的文本框](images/handwritingview/textbox-with-dictation-handwritingview.png)

若要解决此问题，请将[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)先属性设置为与之应对齐的 UI 元素。

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

## <a name="resize-the-handwritingview"></a>调整大小 HandwritingView

你还可以设置的大小[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)，你需要确保在视图不会阻挡重要的 UI 时这会很有用。

如前面的示例中，我们将创建支持 （由将文本框和听写按钮放入 StackPanel 实现） 的听写 TextBox。

![听写的文本框](images/handwritingview/textbox-with-dictation.png)

在此情况下，我们想要确保听写按钮始终可见。

![听写的文本框](images/handwritingview/textbox-with-dictation-handwritingview-resize.png)

若要执行此操作，我们将[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) MaxWidth 属性绑定到它应该会阻挡的 UI 元素的宽度。

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

如果你有自定义显示 UI，以响应文本输入，如信息的弹出窗口，你可能需要重新定位该 UI，因此它不会阻挡手写视图。

![自定义 UI 的文本框](images/handwritingview/textbox-with-customui.png)

下面的示例显示了如何侦听[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview)设置位置的[弹出窗口](https://docs.microsoft.com/uwp/api/windows.ui.popups)[打开](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.opened)，[已关闭](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview.closed
)，并且[SizeChanged](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.sizechanged)事件。

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

## <a name="retemplate-the-handwritingview-control"></a>重新 HandwritingView 控件

与所有 XAML 框架控件一样，你可以针对特定要求自定义的可视结构和可视行为[HandwritingView](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.handwritingview) 。

若要查看创建自定义模板签出[创建自定义传输控件](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/custom-transport-controls)操作方法或[自定义编辑控件示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)的完整示例。







