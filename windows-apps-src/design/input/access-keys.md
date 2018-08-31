---
author: Karl-Bridge-Microsoft
Description: Learn how to improve both the usability and the accessibility of your UWP app by providing an intuitive way for users to quickly navigate and interact with an app's visible UI through a keyboard instead of a pointer device (such as touch or mouse).
title: 访问键设计指南
label: Access keys design guidelines
keywords: 键盘, 访问键, 键提示, 辅助功能, 导航, 焦点, 文本, 输入, 用户交互
template: detail.hbs
ms.author: kbridge
ms.date: 06/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8e842d6c5b8e62a9c043c97849fdf17f524ccfc7
ms.sourcegitcommit: 1e5590dd10d606a910da6deb67b6a98f33235959
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2018
ms.locfileid: "3235789"
---
# <a name="access-keys"></a>访问键

访问键为键盘快捷方式，可以为用户提供一种直观的方式，使其能够通过键盘而非指针设备（如触控或鼠标）进行快速导航并与应用的可视 UI 进行交互，以此提高 Windows 应用程序的实用功能和辅助功能。

有关使用键盘快捷方式在 Windows 应用程序中调用常见操作的详细信息，请参阅[加速键](keyboard-accelerators.md)主题。 

> [!NOTE]
> 对于残疾人士用户而言，键盘是必不可少的工具（请参阅[键盘辅助功能](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)），并且对于将键盘认为是与应用交互的较有效方法的用户而言，键盘也非常重要。

通用 Windows 平台 (UWP) 通过视觉提示（称为键提示）为基于键盘的访问键和关联的 UI 反馈提供内置的跨平台控件支持。

## <a name="overview"></a>概述

访问键是 Alt 键与一个或多个字母数字键（有时称为*助记键*）的组合，通常是按顺序按下，而不是同时按下。

键提示是在控件旁边显示的锁屏提醒，在用户按下 Alt 键时，这些控件支持访问键。 每个键提示包含用于激活相关控件的字母数字键。

> [!NOTE]
> 带有单个字母数字字符的访问键自动支持键盘快捷方式。 例如，在 Word 中同时按下 Alt+F 将会打开“文件”菜单，而不显示键提示。

按 Alt 键将初始化访问键功能并在键提示中显示当前可用的所有键组合。 后续击键由访问键框架处理，在按下有效访问键或者按下 Enter、Esc、Tab 或箭头键禁用访问键并将击键处理返回至应用之前，它将拒绝无效键。

Microsoft Office 应用可为访问键提供广泛的支持。 下图所示为 Word 里面的“主页”选项卡及已激活的访问键（记录对数字和多个击键的支持）。

![Microsoft Word 访问键的键提示锁屏提醒](images/accesskeys/keytip-badges-word.png)

_Microsoft Word 访问键的键提示锁屏提醒_

若要为控件添加访问键，请使用 **AccessKey 属性**。 此属性值用于指定访问键序列、快捷键（如果是单个字母数字）和键提示。

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>使用访问键时

我们建议你在 UI 的合适位置指定访问键并在所有自定义控制中支持访问键。

1.  对于行动有障碍的用户，包括一次只能按一个键或者使用鼠标有困难的用户，**访问键可以让他们更容易地访问应用**。

    具有良好设计的键盘 UI 是软件辅助功能的一个重要方面。 它使具有视力缺陷或行动有障碍的用户能够在应用中导航并与应用的功能交互。 这些用户可能无法操作鼠标，而是依靠各种辅助技术，如键盘增强工具、屏幕键盘、屏幕放大器、屏幕阅读器、语音输入实用工具。 对于这些用户，广泛的命令覆盖面非常重要。

2.  对于喜欢通过键盘进行交互的高级用户，**访问键可以让他们更轻松地使用应用**。

    有经验的用户通常强烈倾向于使用键盘，因为可以更为快速地输入可让基于键盘的命令，而无需将双手从键盘上挪开。 对于这些用户，效率性和一致性体验至关重要；综合性体验仅对常用命令十分重要。

## <a name="set-access-key-scope"></a>设置访问键作用域

当支持访问键的屏幕上具有许多元素时，我们建议设置访问键的作用域，以降低**认知负荷**。 这可以减少屏幕上的访问键数量，从而可以更容易地进行定位并提高效率和生产力。

例如，Microsoft Word 提供两个访问键作用域：一个用于“功能区”选项卡的主作用域和一个用于选定选项卡上的命令的辅助作用域。

下图所示为 Word 中的两个作用域。 第一个显示可让用户选择选项卡和其他顶级命令的主访问键，第二个显示“主页”选项卡的辅助访问键。

![Microsoft Word 中的主访问键](images/accesskeys/primary-access-keys-word.png)
_Microsoft Word 中的主访问键_

![Microsoft Word 中的辅助访问键](images/accesskeys/secondary-access-keys-word.png)
_Microsoft Word 中的辅助访问键_

可以为不同域中的元素复制访问键。 在前面的示例中，“2”既是主域中“撤销”的访问键，也是辅助域中“斜体”的访问键。

下面我们介绍如何定义访问键范围。

``` xaml
<CommandBar x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

![CommandBar 的主访问键](images/accesskeys/primary-access-keys-commandbar.png)

_CommandBar 主作用域和受支持的访问键_

![CommandBar 的辅助访问键](images/accesskeys/secondary-access-keys-commandbar.png)

_CommandBar 辅助范围和受支持的访问键_

### <a name="windows-10-creators-update-and-older"></a>Windows 10 创意者更新和早期版本

在 Windows 10 Fall Creators Update 之前，某些控件（例如 CommandBar）不支持内置访问键范围。

以下示例展示了如何通过可用的访问键支持 CommandBar 的 SecondaryCommands，访问键在父命令被调用后可用（与 Word 中的“功能区”相似）。

```xaml
<local:CommandBarHack x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</local:CommandBarHack>
```

```csharp
public class CommandBarHack : CommandBar
{
    CommandBarOverflowPresenter secondaryItemsControl;
    Popup overflowPopup;

    public CommandBarHack()
    {
        this.ExitDisplayModeOnAccessKeyInvoked = false;
        AccessKeyInvoked += OnAccessKeyInvoked;
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();

        Button moreButton = GetTemplateChild("MoreButton") as Button;
        moreButton.SetValue(Control.IsTemplateKeyTipTargetProperty, true);
        moreButton.IsAccessKeyScope = true;

        // SecondaryItemsControl changes
        secondaryItemsControl = GetTemplateChild("SecondaryItemsControl") as CommandBarOverflowPresenter;
        secondaryItemsControl.AccessKeyScopeOwner = moreButton;

        overflowPopup = GetTemplateChild("OverflowPopup") as Popup;
    }

    private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
    {
        if (overflowPopup != null)
        {
            overflowPopup.Opened += SecondaryMenuOpened;
        }
    }

    private void SecondaryMenuOpened(object sender, object e)
    {
        //This is not neccesay given we are automatically pushing the scope.
        var item = secondaryItemsControl.Items.First();
        if (item != null && item is Control)
        {
            (item as Control).Focus(FocusState.Keyboard);
        }
        overflowPopup.Opened -= SecondaryMenuOpened;
    }
}
```

## <a name="avoid-access-key-collisions"></a>避免访问键冲突

当相同作用域中的两个或更多元素具有相同的访问键或者以相同的字母数字字符开头时，将会发生访问键冲突。

系统将会处理添加到虚拟树的第一个元素的访问键，忽略所有其他访问键，以此解决访问键重复问题。

当多个访问键以相同字符（如“A”、“A1”和“AB”）开头时，系统将会处理单个字符访问键并忽略所有其他访问键。

使用唯一的访问键或限定命令的作用域来避免冲突。

## <a name="choose-access-keys"></a>选择访问键

选择访问键时，请考虑以下事项：

-   使用单个字符来减少击键并支持默认的快捷键 (Alt+AccessKey)
-   避免使用两个以上字符
-   避免访问键冲突
-   避免使用难于与其他字符区分的字符，例如字母“I”与数字“1”或者字母“O”与数字“0”
-   使用其他常用应用（如 Word）中的大家熟知的先例（“F”代表“文件”，“H”代表“主页”等）
-   使用命令名称的第一个字符或者与命令最相关、有助于记起的字符
    -   如果第一个字母已分配，请使用与命令名称的第一个字母最接近的字母（“N”代表“插入”）
    -   使用与其他命令名称不同的辅音（“W”代表“视图”）
    -   使用命令名称中的元音。

## <a name="localize-access-keys"></a>本地化访问键

如果你的应用将被本地化成多种语言，则你还应**考虑本地化访问键**。 例如，“H”代表英语中的“主页”，“I”代表西班牙语中的“主页”。

在标记中使用 x:Uid 扩展，以应用已本地化的资源，如此处所示。

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
每种语言的资源将添加到项目中对应的字符串文件夹：

![英语和西班牙语资源字符串文件夹](images/accesskeys/resource-string-folders.png)

_英语和西班牙语资源字符串文件夹_

在项目的 resources.resw 文件中指定已本地化的访问键：

![指定已在 resources.resw 文件中指定的 AccessKey 属性](images/accesskeys/resource-resw-file.png)

_指定已在 resources.resw 文件中指定的 AccessKey 属性_

有关详细信息，请参阅[翻译 UI 资源](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965329(v=win.10).aspx)

## <a name="key-tip-positioning"></a>键提示定位

键提示显示为与其对应的 UI 元素相关的浮动锁屏提醒，以考虑存在的其他 UI 元素、其他键提示和屏幕边缘。

通常情况下，默认的键提示位置已足够，并为自适应 UI 提供内置支持。

![自动键提示放置示例](images/accesskeys/auto-keytip-position.png)

_自动键提示放置示例_

但是，如果你需要更好地控制键提示放置，我们的建议如下：

1.  **明显关联原则**：用户可以轻松地将控件与键提示关联在一起。

    a.  键提示应**靠近**具有访问键的元素（所有者）。  
    b.  键提示应**避免覆盖具有访问键的已启用元素**。   
    c.  如果无法将键提示置于其所有者附近，则应覆盖其所有者。 

2.  **可发现性**：用户可以通过键提示快速发现控件。

    a.  键提示不得**覆盖**其他键提示。  

3.  **轻松扫描**：用户可以轻松地浏览键提示。

    a.  键提示相互之间以及与 UI 元素之间应**对齐**。
    b.  应尽可能多地将键提示**分组**。 

### <a name="relative-position"></a>相对位置

使用 **KeyTipPlacementMode** 属性以每个元素或每个组为基础自定义键提示的放置。

放置模式包括：顶部、底部、右侧、左侧、隐藏、居中和自动。

![键提示放置模式](images/accesskeys/keytip-postion-modes.png)

_键提示放置模式_

控件的中心线用于计算键提示的垂直和水平对齐。

以下示例显示了如何使用 StackPanel 容器的 KeyTipPlacementMode 属性设置一组控件的键提示放置。

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>偏移量

使用元素的 KeyTipHorizontalOffset 和 KeyTipVerticalOffset 属性更精确地控制键提示的位置。

> [!NOTE]
> 当 KeyTipPlacementMode 设为“自动”时，无法设置偏移量。

KeyTipHorizontalOffset 属性表示键提示向左或向右移动的距离。 示例显示了如何设置按钮的键提示偏移量。

![键提示放置模式](images/accesskeys/keytip-offsets.png)

_设置键提示的垂直和水平偏移量_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>屏幕边缘对齐 {#screen-edge-alignment .ListParagraph}

键提示的位置将会根据屏幕边缘自动调整，以确保键提示完全可见。 发生此情况时，控件与键提示对齐点之间的距离可能与指定的水平和垂直偏移量值不同。

![键提示放置模式](images/accesskeys/keytips-screen-edge.png)

_屏幕边缘导致键提示自动重新定位自身_

## <a name="key-tip-style"></a>键提示样式

我们建议为平台主题使用内置的键提示支持，包括高对比度。

如果你需要指定自己的键提示样式，请使用应用程序资源，如 KeyTipFontSize（字体大小）、KeyTipFontFamily（字体系列）、KeyTipBackground（背景）、KeyTipForeground（前景）、KeyTipPadding（填充）、KeyTipBorderBrush（边框颜色）和 KeyTipBorderThemeThickness（边框粗细）。

![键提示放置模式](images/accesskeys/keytip-customization.png)

_键提示自定义选项_

此示例展示了如何更改应用程序资源：

 ```xaml  
<Application.Resources>
  <SolidColorBrush Color="DarkGray" x:Key="MyBackgroundColor" />
  <SolidColorBrush Color="White" x:Key="MyForegroundColor" />
  <SolidColorBrush Color="Black" x:Key="MyBorderColor" />
  <StaticResource x:Key="KeyTipBackground" ResourceKey="MyBackgroundColor" />
  <StaticResource x:Key="KeyTipForeground" ResourceKey="MyForegroundColor" />
  <StaticResource x:Key="KeyTipBorderBrush" ResourceKey="MyBorderColor"/>
  <FontFamily x:Key="KeyTipFontFamily">Consolas</FontFamily>
  <x:Double x:Key="KeyTipContentThemeFontSize">18</x:Double>
  <Thickness x:Key="KeyTipBorderThemeThickness">2</Thickness>
  <Thickness x:Key="KeyTipThemePadding">4,4,4,4</Thickness>
</Application.Resources>
```

## <a name="access-keys-and-narrator"></a>访问键和讲述人

XAML 框架公开了自动化属性，支持 UI 自动化客户端发现与用户界面中的元素相关的信息。

如果你在 UIElement 或 TextElement 控件上指定了 AccessKey 属性，则可以通过 [AutomationProperties.AccessKey](https://msdn.microsoft.com/library/windows/apps/hh759763) 属性来获得此值。 每当元素获得焦点时，辅助功能客户端（如讲述人）将会读取分此属性值。

## <a name="related-articles"></a>相关文章

* [键盘交互](keyboard-interactions.md)
* [键盘加速键](keyboard-accelerators.md)

**示例**
* [XAML 控件库 (也称为 XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


