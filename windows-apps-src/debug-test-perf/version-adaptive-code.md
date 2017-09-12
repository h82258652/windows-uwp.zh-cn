---
author: jwmsft
title: "版本自适应代码"
description: "使用 ApiInformation 类以在保持与以前版本的兼容性的同时利用新 API"
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 3293e91e-6888-4cc3-bad3-61e5a7a7ab4e
ms.openlocfilehash: d5b9a3b02c5acbb2ad7bcd00b9af4f7d6edd91de
ms.sourcegitcommit: 73ea31d42a9b352af38b5eb5d3c06504b50f6754
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/27/2017
---
# <a name="version-adaptive-code"></a>版本自适应代码

你可以考虑编写自适应代码，这与考虑如何[创建自适应 UI](https://msdn.microsoft.com/windows/uwp/layout/layouts-with-xaml) 类似。 当你检测到自己的应用在较大的屏幕上运行时，你可以设计基本的 UI 以在最小的屏幕上运行，然后移动或添加元素。 借助自适应代码，可编写基础代码以在最低的操作系统版本上运行，并且可以在检测到应用运行所在的较高版本上提供新功能时，添加精心挑选的功能。

有关 ApiInformation、API 协定和配置 Visual Studio 的重要背景信息，请参阅[版本自适应应用](version-adaptive-apps.md)。

### <a name="runtime-api-checks"></a>运行时 API 检查

有条件地在代码中使用 [Windows.Foundation.Metadata.ApiInformation](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.aspx) 类，测试是否存在要调用的 API。 此条件将进行评估（无论你的应用在何处运行），但仅针对存在相应 API 的设备评估为 **True**，从而可调用该 API。 这将允许你编写版本自适应代码，以便创建相关应用，它们使用仅在特定操作系统版本上提供的 API。

下面我们看一下面向 Windows Insider Preview 中的新功能的特定示例。 有关使用 **ApiInformation** 的总体概述，请参阅 [UWP 应用指南](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)和博客文章[使用 API 合约动态检测功能](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)。

> [!TIP]
> 许多运行时 API 检查可能会影响你的应用的性能。 在这些示例中，我们将以内联形式演示检查。 在生产代码中，应执行检查一次并缓存结果，然后在整个应用中使用缓存的结果。 

### <a name="unsupported-scenarios"></a>不受支持的方案

在大多数情况下，你可以将应用的最低版本设置为 SDK 版本 10240 并使用运行时检查，以便在应用运行在更高的版本上时启用任何新的 API。 但在某些情况下，必须提高应用的最低版本才能使用新功能。

如果要使用以下内容，则必须提高应用的最低版本：
- 需要一项在较早版本中并未提供的功能的新 API。 必须将受支持的最低版本提高到包含该功能的版本。 有关详细信息，请参阅[应用功能声明](../packaging/app-capability-declarations.md)。
- 任何已添加到 generic.xaml 并且在早期版本中不可用的新资源键。 运行时所使用的 generic.xaml 版本由设备运行所在的操作系统版本确定。 无法使用运行时 API 检查来确定是否存在 XAML 资源。 因此，你只能使用应用支持的最低版本中提供的资源键，否则 [XAMLParseException](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.markup.xamlparseexception.aspx) 将导致应用在运行时崩溃。

### <a name="adaptive-code-options"></a>自适应代码选项

有两种方法创建自适应代码。 在大多数情况下，编写要在最低版本上运行的应用标记，然后通过应用代码来利用较新的操作系统功能（如果有）。 但是，如果你需要在视觉状态中更新某一属性，并且操作系统版本之间仅有一个属性或枚举值更改，则可以创建可扩展的状态触发器，该触发器根据是否存在 API 进行激活。

我们在此处比较这些选项。

**应用代码**

使用场合：
- 推荐所有自适应代码方案，但下面针对可扩展触发器定义的特定情况除外。

优势：
- 避免开发人员开销/将 API 差异绑定到标记的复杂性。

缺点：
- 无设计器支持。

**状态触发器**

使用场合：
- 在操作系统版本之间仅有一个属性或枚举更改（无需逻辑更改）并且已连接到视觉状态时使用。

优势：
- 允许你创建特定的视觉状态，它们根据是否存在 API 进行触发。
- 某些设计器支持可用。

缺点：
- 自定义触发器的使用仅限于视觉状态，它不适合复杂的自适应布局。
- 必须使用 Setter 指定值更改，因此仅允许简单的更改。
- 自定义状态触发器在设置和使用时相当繁琐。

## <a name="adaptive-code-examples"></a>自适应代码示例

在本部分中，我们将显示多个使用 Windows 10 版本 1607 (Windows Insider Preview) 中新增 API 的自适应代码的示例。

### <a name="example-1-new-enum-value"></a>示例 1：新的枚举值

Windows 10 版本 1607 向 [InputScopeNameValue](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.inputscopenamevalue.aspx) 枚举添加了一个新值：**ChatWithoutEmoji**。 这一新输入范围与 **Chat** 输入范围具有相同的输入行为（拼写检查、自动完成、首字母自动大写），但其无需表情符号按钮即可映射到触摸键盘。 如果你要创建自己的表情符号选取器，并希望禁用触摸键盘中内置的表情符号按钮，这将很有用。 

此示例显示了如何检查是否存在 **ChatWithoutEmoji** 枚举值，并设置 **TextBox** 的 [InputScope](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.inputscope.aspx) 属性（如果存在该枚举值）。 如果在应用运行所在的系统上不存在该枚举值，则改为将 **InputScope** 设置为 **Chat**。 所示代码可放置在 Page 构造函数或 Page.Loaded 事件处理程序中。

> [!TIP]
> 在检查 API 时，使用静态字符串而不是依赖于 .NET 语言功能，否则你的应用可能会尝试访问未定义的类型并在运行时崩溃。

**C#**
```csharp
// Create a TextBox control for sending messages 
// and initialize an InputScope object.
TextBox messageBox = new TextBox();
messageBox.AcceptsReturn = true;
messageBox.TextWrapping = TextWrapping.Wrap;
InputScope scope = new InputScope();
InputScopeName scopeName = new InputScopeName();

// Check that the ChatWithEmoji value is present.
// (It's present starting with Windows 10, version 1607,
//  the Target version for the app. This check returns false on earlier versions.)         
if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
{
    // Set new ChatWithoutEmoji InputScope if present.
    scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
}
else
{
    // Fall back to Chat InputScope.
    scopeName.NameValue = InputScopeNameValue.Chat;
}

// Set InputScope on messaging TextBox.
scope.Names.Add(scopeName);
messageBox.InputScope = scope;

// For this example, set the TextBox text to show the selected InputScope.
messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();

// Add the TextBox to the XAML visual tree (rootGrid is defined in XAML).
rootGrid.Children.Add(messageBox);
```

在上一示例中，在代码中创建了 TextBox 并设置了所有属性。 但是，如果你具有现有的 XAML 并且只需更改支持新值的系统上的 InputScope 属性，则可以在不更改 XAML 的情况下执行此操作，如下所示。 在 XAML 中将默认值设置为 **Chat**，当要在代码中覆盖它（如果存在 **ChatWithoutEmoji** 值）。

**XAML**
```xaml
<TextBox x:Name="messageBox"
         AcceptsReturn="True" TextWrapping="Wrap"
         InputScope="Chat"
         Loaded="messageBox_Loaded"/>
```

**C#**
```csharp
private void messageBox_Loaded(object sender, RoutedEventArgs e)
{
    if (ApiInformation.IsEnumNamedValuePresent("Windows.UI.Xaml.Input.InputScopeNameValue", "ChatWithoutEmoji"))
    {
        // Check that the ChatWithEmoji value is present.
        // (It's present starting with Windows 10, version 1607,
        //  the Target version for the app. This code is skipped on earlier versions.)
        InputScope scope = new InputScope();
        InputScopeName scopeName = new InputScopeName();
        scopeName.NameValue = InputScopeNameValue.ChatWithoutEmoji;
        // Set InputScope on messaging TextBox.
        scope.Names.Add(scopeName);
        messageBox.InputScope = scope;
    }

    // For this example, set the TextBox text to show the selected InputScope.
    // This is outside of the API check, so it will happen on all OS versions.
    messageBox.Text = messageBox.InputScope.Names[0].NameValue.ToString();
}
```

既然我们已经有具体示例，让我们来看一下如何对其应用目标版本和最低版本设置。

在这些示例中，可以在不检查的情况下在 XAML 或代码中使用 Chat 枚举值，因为它存在于受支持的最低操作系统版本中。 

如果要在不检查的情况下在 XAML 或代码中使用 ChatWithoutEmoji 值，它将编译且不会出现错误，因为它存在于目标操作系统版本中。 它还将在具有目标操作系统版本的系统上运行且不会出现错误。 但是，当应用在使用最低操作系统版本的系统上运行时，它将在运行时崩溃，因为 ChatWithoutEmoji 枚举值不存在。 因此，你只能在代码中使用此值，并将其封装在运行时 API 检查中，以便它仅在当前系统上受支持时才调用。

### <a name="example-2-new-control"></a>示例 2：新控件

新的 Windows 版本通常会将新控件引入到 UWP API 图面，进而为平台引入新功能。 若要利用存在的新控件，请使用 [ApiInformation.IsTypePresent](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx) 方法。

Windows 10 版本 1607 引入了名为 [**MediaPlayerElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaplayerelement.aspx) 的新媒体控件。 此控件基于 [MediaPlayer](https://msdn.microsoft.com/library/windows/apps/windows.media.playback.mediaplayer.aspx) 类生成，因此它将提供相关功能（例如能够轻松绑定到后台音频），并且它将使用媒体堆栈中的体系结构改进。

但是，如果应用运行所在的设备运行的是版本 1607 之前的 Windows 10 版本，则必须使用 [**MediaElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.mediaelement.aspx) 控件而不是新的 **MediaPlayerElement** 控件。 可以使用 [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.metadata.apiinformation.istypepresent.aspx) 方法来检查在运行时是否存在 MediaPlayerElement 控件，并加载适用于应用运行所在的系统的控件。

此示例显示了如何创建使用新 MediaPlayerElement 或旧 MediaElement 的应用，这具体取决于是否存在 MediaPlayerElement 类型。 在此代码中，使用 [UserControl](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.usercontrol.aspx) 类来组件化控件及其相关 UI 和代码，以便你可以基于操作系统版本切换它们。 作为替代方法，你可以使用自定义控件，该控件提供的功能和自定义行为比这一简单示例所需的内容要多。
 
**MediaPlayerUserControl** 

`MediaPlayerUserControl` 封装 **MediaPlayerElement** 和用于按帧跳过媒体框架的多个按钮。 UserControl 使你可以将这些控件视为单个实体，并使在较旧的系统上使用 MediaElement 切换变得更容易。 此用户控件应当仅在存在 MediaPlayerElement 的系统上使用，因此你无需在此用户控件内的代码中使用 ApiInformation 检查。

> [!NOTE]
> 若要使此示例既简单又有焦点，请将帧步骤按钮放置在媒体播放器的外部。 若要实现更好的用户体验，应自定义 MediaTransportControls 以包括自定义按钮。 有关详细信息，请参阅[自定义传输控件](https://msdn.microsoft.com/windows/uwp/controls-and-patterns/custom-transport-controls)。 

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaPlayerUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid x:Name="MPE_grid>
        <Grid.RowDefinitions>
            <RowDefinition/>
            <RowDefinition Height="Auto"/>
        </Grid.RowDefinitions>
        <StackPanel Orientation="Horizontal" 
                    HorizontalAlignment="Center" Grid.Row="1">
            <RepeatButton Click="StepBack_Click" Content="Step Back"/>
            <RepeatButton Click="StepForward_Click" Content="Step Forward"/>
        </StackPanel>
    </Grid>
</UserControl>
```

**C#**
```csharp
using System;
using Windows.Media.Core;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;

namespace MediaApp
{
    public sealed partial class MediaPlayerUserControl : UserControl
    {
        public MediaPlayerUserControl()
        {
            this.InitializeComponent();
            
            // The markup code compiler runs against the Minimum OS version so MediaPlayerElement must be created in app code
            MPE = new MediaPlayerElement();
            Uri videoSource = new Uri("ms-appx:///Assets/UWPDesign.mp4");
            MPE.Source = MediaSource.CreateFromUri(videoSource);
            MPE.AreTransportControlsEnabled = true;
            MPE.MediaPlayer.AutoPlay = true;

            // Add MediaPlayerElement to the Grid
            MPE_grid.Children.Add(MPE);

        }

        private void StepForward_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }

        private void StepBack_Click(object sender, RoutedEventArgs e)
        {
            // Step forward one frame, only available using MediaPlayerElement.
            MPE.MediaPlayer.StepForwardOneFrame();
        }
    }
}
```

**MediaElementUserControl**
 
`MediaElementUserControl` 封装 **MediaElement** 控件。

**XAML**
```xaml
<UserControl
    x:Class="MediaApp.MediaElementUserControl"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MediaApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d"
    d:DesignHeight="300"
    d:DesignWidth="400">

    <Grid>
        <MediaElement AreTransportControlsEnabled="True" 
                      Source="Assets/UWPDesign.mp4"/>
    </Grid>
</UserControl>
```

> [!NOTE]
> `MediaElementUserControl` 的代码页仅包含生成的代码，因此它不会显示。

**基于 IsTypePresent 初始化控件**

在运行时，调用 **ApiInformation.IsTypePresent** 以检查 MediaPlayerElement。 如果存在，则加载 `MediaPlayerUserControl`；如果不存在，则加载 `MediaElementUserControl`。

**C#**
```csharp
public MainPage()
{
    this.InitializeComponent();

    UserControl mediaControl;

    // Check for presence of type MediaPlayerElement.
    if (ApiInformation.IsTypePresent("Windows.UI.Xaml.Controls.MediaPlayerElement"))
    {
        mediaControl = new MediaPlayerUserControl();
    }
    else
    {
        mediaControl = new MediaElementUserControl();
    }

    // Add mediaControl to XAML visual tree (rootGrid is defined in XAML).
    rootGrid.Children.Add(mediaControl);
}
```

> [!IMPORTANT]
> 请记住，此检查仅将 `mediaControl` 对象设置为 `MediaPlayerUserControl` 或 `MediaElementUserControl`。 要求在代码中需确定是使用 MediaPlayerElement API 还是使用 MediaElement API 的任何其他位置执行这些条件检查。 应执行检查一次并缓存结果，然后在整个应用中使用缓存的结果。

## <a name="state-trigger-examples"></a>状态触发器示例

借助可扩展状态触发器，你可以结合使用标记和代码来基于检查代码的条件触发视觉状态更改；在此情况下，基于是否存在特定 API。 对于常见自适应代码方案，因涉及到开销以及仅限视觉状态而不推荐状态触发器。 

应仅当不同操作系统版本之间的较小 UI 更改不影响其余 UI （例如控件上的属性或枚举值更改）时，才将状态触发器用于自适应代码。

### <a name="example-1-new-property"></a>示例 1：新属性

设置可扩展状态触发器的第一步是子类化 [StateTriggerBase](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.statetriggerbase.aspx) 类来创建自定义触发器，该触发器将基于是否存在 API 来激活。 此示例演示了在存在属性与 XAML 中设置的 `_isPresent` 变量匹配时激活的触发器。

**C#**
```csharp
class IsPropertyPresentTrigger : StateTriggerBase
{
    public string TypeName { get; set; }
    public string PropertyName { get; set; }

    private Boolean _isPresent;
    private bool? _isPropertyPresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;
            if (_isPropertyPresent == null)
            {
                // Call into ApiInformation method to determine if property is present.
                _isPropertyPresent =
                ApiInformation.IsPropertyPresent(TypeName, PropertyName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isPropertyPresent);
        }
    }
}
```

下一步是在 XAML 中设置视觉状态触发器，以便基于是否存在该 API 生成两个不同的视觉状态。 

Windows 10 版本 1607 在 [FrameworkElement](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.aspx) 类上引入了名为 [AllowFocusOnInteraction](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.frameworkelement.allowfocusoninteraction.aspx) 的新属性，该属性用于确定控件是否在用户与其交互时获得焦点。 如果要在用户单击某个按钮时使焦点位于文本框上以便进行数据输入（并使触摸键盘保持显示），这将很有用。

如果该属性存在，该示例中的触发器将处于选中状态。 如果该属性存在，Button 上的 **AllowFocusOnInteraction** 属性将设为 **false**；如果该属性不存在，Button 将保留其原始状态。 TextBox 包括在内，以便于在运行代码时更轻松地查看此属性的效果。

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
    <StackPanel>
        <TextBox Width="300" Height="36"/>
        <!-- Button to set the new property on. -->
        <Button x:Name="testButton" Content="Test" Margin="12"/>
    </StackPanel>

    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="propertyPresentStateGroup">
            <VisualState>
                <VisualState.StateTriggers>
                    <!--Trigger will activate if the AllowFocusOnInteraction property is present-->
                    <local:IsPropertyPresentTrigger 
                        TypeName="Windows.UI.Xaml.FrameworkElement" 
                        PropertyName="AllowFocusOnInteraction" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="testButton.AllowFocusOnInteraction" 
                            Value="False"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

### <a name="example-2-new-enum-value"></a>示例 2：新的枚举值

此示例演示了如何基于是否存在某个值设置不同的枚举值。 它使用自定义状态触发器来实现与之前的 chat 示例相同的效果。 在此示例中，使用新的 ChatWithoutEmoji 输入范围（如果设备运行的是 Windows 10 版本 1607），否则使用 **Chat** 输入范围。 使用此触发器的视觉状态可采用 *if-else* 样式进行设置，其中输入范围基于是否存在新的枚举值进行选择。

**C#**
```csharp
class IsEnumPresentTrigger : StateTriggerBase
{
    public string EnumTypeName { get; set; }
    public string EnumValueName { get; set; }

    private Boolean _isPresent;
    private bool? _isEnumValuePresent = null;

    public Boolean IsPresent
    {
        get { return _isPresent; }
        set
        {
            _isPresent = value;

            if (_isEnumValuePresent == null)
            {
                // Call into ApiInformation method to determine if value is present.
                _isEnumValuePresent =
                ApiInformation.IsEnumNamedValuePresent(EnumTypeName, EnumValueName);
            }

            // If the property presence matches _isPresent then the trigger will be activated;
            SetActive(_isPresent == _isEnumValuePresent);
        }
    }
}
```

**XAML**
```xaml
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

    <TextBox x:Name="messageBox"
     AcceptsReturn="True" TextWrapping="Wrap"/>


    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="EnumPresentStates">
            <!--if-->
            <VisualState x:Name="isPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="True"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="ChatWithoutEmoji"/>
                </VisualState.Setters>
            </VisualState>
            <!--else-->
            <VisualState x:Name="isNotPresent">
                <VisualState.StateTriggers>
                    <local:IsEnumPresentTrigger 
                        EnumTypeName="Windows.UI.Xaml.Input.InputScopeNameValue" 
                        EnumValueName="ChatWithoutEmoji" IsPresent="False"/>
                </VisualState.StateTriggers>
                <VisualState.Setters>
                    <Setter Target="messageBox.InputScope" Value="Chat"/>
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Grid>
```

## <a name="related-articles"></a>相关文章

- [UWP 应用指南](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [使用 API 合约动态检测功能](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
