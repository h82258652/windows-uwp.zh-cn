---
title: 语音启用 Xbox 上的 Shell (VES)
description: 了解如何将语音控件支持添加到你在 Xbox 上的 UWP 应用。
ms.date: 10/19/2017
ms.topic: article
keywords: windows 10、 uwp、 xbox、 语音、 语音启用 shell
ms.openlocfilehash: ea51216c804754e98c3bac459b79fb75dd9369cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596052"
---
# <a name="using-speech-to-invoke-ui-elements"></a>使用语音来调用 UI 元素

启用语音的 Shell (VES) 是 Windows 语音平台，能够在内部应用程序，提供一流的语音体验，允许用户调用屏幕上使用语音的扩展控件并插入通过听写的文本。 VES 致力于提供一种通用的端到端，请参阅 it say it 体验在所有 Windows 外壳程序和设备，与从应用程序开发人员所需的最小精力。  若要实现此目的，它利用 Microsoft 语音平台和 UI 自动化 (UIA) 框架。

## <a name="user-experience-walkthrough"></a>用户体验浏览 ##
以下是在 Xbox 上使用 VES 时，用户将体验的概述，它可以帮助在深入探讨 VES 的工作原理的详细信息之前设置上下文。

- 用户开启 Xbox 控制台，想要浏览其应用程序，若要查找的相关事情：

        User: "Hey Cortana, open My Games and Apps"

- 用户将保留在活动侦听模式 (ALM)，这意味着在控制台正在侦听为了让用户能够调用控件是显示在屏幕上，而无需说，"你好，小娜"每次。  现在，用户可以切换以查看应用程序和应用程序列表中的滚动：

        User: "applications"

- 若要滚动视图，用户可以只是说：

        User: "scroll down"

- 用户会看到的应用他们感兴趣，但忘记了名称的封面。  用户请求语音提示标签显示：

        User: "show labels"

- 现在，很明显要说，可以启动应用程序：

        User: "movies and TV"

- 若要退出活动侦听模式，用户会告诉 Xbox 可停止侦听：

        User: "stop listening"

- 更高版本上的新的活动侦听会话可以开始使用：

        User: "Hey Cortana, make a selection" or "Hey Cortana, select"

## <a name="ui-automation-dependency"></a>UI 自动化依赖关系 ##
VES 是 UI 自动化客户端，并且依赖于由通过其 UI 自动化提供程序应用程序公开的信息。 这是 Windows 平台上的讲述人功能已在使用相同的基础结构。  UI 自动化可以编程方式访问用户界面元素，包括控件、 其类型和哪些控件模式实现的名称。  随着 UI 的应用中的更改，VES 将对 UIA 更新事件做出响应，并重新分析已更新的 UI 自动化树，以查找所有可操作的项目，请使用此信息来生成语音识别语法。 

所有 UWP 应用有权访问的 UI 自动化框架和可以公开独立的图形框架的 UI 相关的信息 （XAML、 DirectX/Direct3D、 Xamarin 等） 为基础构建它们。  在某些情况下，如 XAML，大部分繁重的任务是通过框架，极大地减少支持讲述人和 VES 所需的工作。

UI 自动化的详细信息请参阅[UI 自动化基础知识](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "UI 自动化基础知识")。

## <a name="control-invocation-name"></a>控件调用名称 ##
VES 采用以下启发式方法用于确定什么短语，将注册语音识别器作为控件的名称 (即。 用户需要说来调用该控件)。  这也是短语，将显示在语音提示标签。

源的名称，按优先顺序：

1. 如果元素具有`LabeledBy`将使用附加的属性，VES`AutomationProperties.Name`的此文本标签。
2. `AutomationProperties.Name` 元素。  在 XAML 中，该控件的文本内容将使用的默认值为`AutomationProperties.Name`。
3. 如果控件是一个列表项或按钮，使用有效的第一个子元素将查找 VES `AutomationProperties.Name`。

## <a name="actionable-controls"></a>可操作控件 ##
VES 中将控件视为可操作，如果它实现了以下自动化控件模式之一：

- **InvokePattern** （例如。 按钮）-表示发起或执行单个明确操作并且不维护状态激活时的控件。

- **TogglePattern** （例如。 复选框）-表示可以循环通过一组状态并在设置后保持状态的控件。

- **SelectionItemPattern** （例如。 组合框）-表示充当可选子项集合的容器的控件。

- **ExpandCollapsePattern** （例如。 组合框）-表示控件的可视化方式展开此项可显示的内容并折叠隐藏内容。

- **ScrollPattern** （例如。 列表）-表示充当子元素的集合的可滚动容器的控件。

## <a name="scrollable-containers"></a>可滚动容器 ##
用于可滚动容器 ScrollPattern、 VES 将侦听的语音支持的"向左滚动"、"向右滚动"等类似的命令和用户触发的下列命令之一时将调用使用适当的参数的滚动。  值的基础注入滚动命令`HorizontalScrollPercent`和`VerticalScrollPercent`属性。  例如，如果`HorizontalScrollPercent`是大于 0，"滚动向左"将添加，如果它小于 100，"向右滚动"将添加，依此类推。

## <a name="narrator-overlap"></a>讲述人重叠 ##
讲述人应用程序也是一个 UI 自动化客户端并使用`AutomationProperties.Name`属性作为其中一个源，它会读取的当前所选的用户界面元素的文本。  若要提供更好的可访问性体验很多应用程序开发人员曾经试图重载`Name`具有长的描述性文本，目的是提供详细信息和讲述人读取时的上下文属性。  但是，这将导致两个功能之间存在冲突：VES 需要短匹配或接近该控件，讲述人受益于较长、 更具描述性的短语，以便更好的上下文时的可见文本的短语。

若要解决此问题，从 Windows 10 创意者更新开始，讲述人已更新为还看`AutomationProperties.HelpText`属性。  如果此属性不为空，讲述人会说出除其内容`AutomationProperties.Name`。  如果`HelpText`是空的讲述人将只读取内容的名称。  这将启用再描述性字符串，在需要时，要使用，但会维护一个较短，语音识别友好短语中的`Name`属性。

![](images/ves_narrator.jpg)

有关详细信息请参阅[UI 中的辅助功能支持的自动化属性](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "UI 中的辅助功能支持的自动化属性")。

## <a name="active-listening-mode-alm"></a>活动侦听模式 (ALM) ##
### <a name="entering-alm"></a>输入 ALM ###
在 Xbox 上 VES 未不断地侦听语音输入。  用户需要显式语输入主动侦听模式：

- "您好 Cortana，选择"，或
- "您好 Cortana，使所选内容"

有几个其他还将保留在完成后，例如"你好，小娜，登录"或"你好，小娜，转到主页"的活动侦听用户的 Cortana 命令。 

输入 ALM 将具有以下影响：

- 在 Cortana 覆盖区上将显示在右上角，告知用户他们可以说他们看到的内容。  虽然用户正在说话，语音识别器识别的短语片段也会显示在此位置。
- VES 分析 UIA 树、 查找所有可操作的控件、 将语音识别语法中，其文本和启动持续侦听会话。

    ![](images/ves_overlay.png)

### <a name="exiting-alm"></a>退出 ALM ###
在用户与 UI 使用语音交互时，系统将保留在 ALM。  有两种方法退出 ALM:

- 用户明确指出，"停止侦听"，或
- 如果自上次正识别或输入 ALM 的 17 秒内没有正识别，则会发生超时

## <a name="invoking-controls"></a>调用控件 ##
ALM 用户可以交互使用语音在 UI 中时。  如果 UI 配置正确 （与名称匹配的可见文本属性），使用语音来执行操作应自然的无缝体验。  用户应该能够说他们在屏幕上看到的内容。

## <a name="overlay-ui-on-xbox"></a>覆盖在 Xbox 上的 UI ##
名称 VES 派生的控件可能会不同于在 UI 中的实际可见文本。  这可能是由于`Name`属性的控件或附加`LabeledBy`元素进行显式设置为不同的字符串。  或者，该控件不具有 GUI 文本，但只有一个图标或图像元素。

在这些情况下，用户需要了解哪些内容需要才能调用此类控件说一种方法。  因此，一次在活动侦听语音提示可以显示通过说"显示标签"。  这将导致语音提示标签以显示每个可操作的控件的顶部。

没有限制为 100 的标签，因此如果应用程序的 UI 具有 100 个更具操作性控件将会有一些不会显示语音提示标签。  选择的标签在这种情况下是不确定性的因为它依赖的结构和组合的当前 UI 中的 UIA 树 first 枚举。

一旦语音提示标签显示为没有命令以将其隐藏，它们将保持可见之前可能出现以下事件之一：

- 用户调用控件
- 用户离开当前场景
- 用户所说，"停止侦听"
- 活动侦听模式超时

## <a name="location-of-voice-tip-labels"></a>语音提示标签的位置 ##
语音提示标签水平和垂直居中中控件的 BoundingRectangle。  标签控件小型和紧密组合时，可以由其他人变得重叠/模糊和 VES 将尝试将其推送这些标签间隔来分隔这些选项并确保它们可见。  但是，这不保证工作 100%的时间。  如果没有非常拥挤的 UI，可能会导致其他人正在被遮盖某些标签。 请查看你的 UI 与"显示标签"以确保没有足够的空间用于语音提示可见性。

![](images/ves_labels.png)

## <a name="combo-boxes"></a>组合框 ##
组合框展开组合框中的每个单个项目时获取其自己的语音提示标签，并通常它们会在后下拉列表中的现有控件之上。  若要避免提供具有混乱和混淆，弄乱的标签 （其中组合框项标签后的组合框的控件的标签与混合） 组合框展开仅标签将显示其子项; 时 将隐藏所有其他语音提示标签。  用户可以然后选择其中一个下拉列表项或者"关闭"组合框。

- 折叠的组合框的标签：

    ![](images/ves_combo_closed.png)

- 扩展的组合框的标签：

    ![](images/ves_combo_open.png)


## <a name="scrollable-controls"></a>可滚动的控件 ##
对于可滚动控件，将在每个控件的边缘上居中的滚动命令的语音提示。  语音提示才会显示的滚动方向，是可操作，例如，如果不可用垂直滚动，"向上滚动"和"向下滚动"将不显示。  VES 存在多个可滚动区域时将使用序号 （例如地区分它们。 "向下滚动右侧 1"，"向下滚动右侧 2"，等等。)。

![](images/ves_scroll.png) 

## <a name="disambiguation"></a>消除二义性 ##
当多个 UI 元素具有相同的名称，或语音识别器匹配多个候选项时，VES 将进入消除二义性模式。  在此模式语音提示中标签将显示的元素所涉及，以便用户可以选择正确。 用户可以通过依次选择"取消"取消退出消除二义性模式。

例如：

- 在活动侦听模式下前消除二义性;显示用户，"是我不明确":

    ![](images/ves_disambig1.png) 

- 匹配; 这两个按钮消除二义性开始：

    ![](images/ves_disambig2.png) 

- 显示，请单击"选择 2"选择操作：

    ![](images/ves_disambig3.png) 
 
## <a name="sample-ui"></a>示例 UI ##
下面是基于 UI，以各种方式设置将 AutomationProperties.Name 的 XAML 示例：

    <Page
        x:Class="VESSampleCSharp.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:VESSampleCSharp"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">
        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
            <Button x:Name="button1" Content="Hello World" HorizontalAlignment="Left" Margin="44,56,0,0" VerticalAlignment="Top"/>
            <Button x:Name="button2" AutomationProperties.Name="Launch Game" Content="Launch" HorizontalAlignment="Left" Margin="44,106,0,0" VerticalAlignment="Top" Width="99"/>
            <TextBlock AutomationProperties.Name="Day of Week" x:Name="label1" HorizontalAlignment="Left" Height="22" Margin="168,62,0,0" TextWrapping="Wrap" Text="Select Day of Week:" VerticalAlignment="Top" Width="137"/>
            <ComboBox AutomationProperties.LabeledBy="{Binding ElementName=label1}" x:Name="comboBox" HorizontalAlignment="Left" Margin="310,57,0,0" VerticalAlignment="Top" Width="120">
                <ComboBoxItem Content="Monday" IsSelected="True"/>
                <ComboBoxItem Content="Tuesday"/>
                <ComboBoxItem Content="Wednesday"/>
                <ComboBoxItem Content="Thursday"/>
                <ComboBoxItem Content="Friday"/>
                <ComboBoxItem Content="Saturday"/>
                <ComboBoxItem Content="Sunday"/>
            </ComboBox>
            <Button x:Name="button3" HorizontalAlignment="Left" Margin="44,156,0,0" VerticalAlignment="Top" Width="213">
                <Grid>
                    <TextBlock AutomationProperties.Name="Accept">Accept Offer</TextBlock>
                    <TextBlock Margin="0,25,0,0" Foreground="#FF5A5A5A">Exclusive offer just for you</TextBlock>
                </Grid>
            </Button>
        </Grid>
    </Page>


此处使用上面的示例是使用和不使用语音提示标签 UI 外观。
 
- 在活动侦听模式下，不带标签所示：

    ![](images/ves_alm_nolabels.png) 

- 在活动侦听模式下，用户说"显示标签"后：

    ![](images/ves_alm_labels.png) 

情况下`button1`，XAML 自动填充`AutomationProperties.Name`属性使用来自控件的可见文本内容的文本。  这就是原因没有语音提示标签即使没有显式`AutomationProperties.Name`设置。

与`button2`，我们显式设置`AutomationProperties.Name`以外的控件的文本。

与`comboBox`，我们使用了`LabeledBy`属性来引用`label1`作为自动化的源`Name`，然后在`label1`我们设置`AutomationProperties.Name`为比要呈现在屏幕上的内容更自然短语 ("Day of Week"而不是"选择天内一周中的"）。

最后，在使用`button3`，获取 VES`Name`以来的第一个子元素`button3`本身不具有`AutomationProperties.Name`设置。

## <a name="see-also"></a>另请参阅
- [UI 自动化基础知识](https://msdn.microsoft.com/en-us/library/ms753107(v=vs.110).aspx "UI 自动化基础知识")
- [在用户界面中的辅助功能支持的自动化属性](https://msdn.microsoft.com/en-us/library/ff400332(vs.95).aspx "UI 中的辅助功能支持的自动化属性")
- [常见问题](frequently-asked-questions.md)
- [在 Xbox One 上 UWP](index.md)
