---
author: serenaz
Description: The Universal Windows Platform (UWP) provides a consistent back navigation system for traversing the user's navigation history within an app and, depending on the device, from app to app.
title: 导航历史记录和向后导航（Windows 应用）
ms.assetid: e9876b4c-242d-402d-a8ef-3487398ed9b3
isNew: true
label: History and backwards navigation
template: detail.hbs
op-migration-status: ready
ms.author: sezhen
ms.date: 11/22/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 824f0e83408893bf95d856067282b1fea1313876
ms.sourcegitcommit: 588171ea8cb629d2dd6aa2080e742dc8ce8584e5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/17/2018
ms.locfileid: "1895404"
---
#  <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>UWP 应用的导航历史记录和向后导航

> **重要的 API**：[BackRequested 事件](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested)、[SystemNavigationManager 类](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager)、[OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

通用 Windows 平台 (UWP) 提供了一个一致的后退导航系统，用于遍历用户在应用内和应用之间（具体取决于设备）的导航历史记录。

要在应用中实现向后导航，请在应用的 UI 的左上角放置一个[后退按钮](#Back-button)。 如果你的应用使用了 [NavigationView](../controls-and-patterns/navigationview.md) 控件，你可以使用 [NavigationView 的内置后退按钮](../controls-and-patterns/navigationview.md#backwards-navigation)。 

用户预期后退按钮导航到应用导航历史记录中的上一个位置。 请注意，由你来决定要将哪些导航操作添加到导航历史记录以及如何响应后退按钮按下操作。

## <a name="back-button"></a>后退按钮

要创建后退按钮，请将[按钮](../controls-and-patterns/buttons.md)控件与 `NavigationBackButtonNormalStyle` 样式结合使用，并将按钮放在应用的 UI 的左上角。

![应用的 UI 的左上角的后退按钮](images/back-nav/BackEnabled.png)

```xaml
<Button Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

如果你的应用有顶部 [CommandBar](../controls-and-patterns/app-bars.md)，高度为 44px 的 Button 控件将无法很好地与 48px AppBarButtons 对齐。 但是，为了避免不一致，请在 48px 边界内对齐 Button 控件的顶部。

![顶部命令栏上的后退按钮](images/back-nav/CommandBar.png)

```xaml
<Button VerticalAlignment="Top" HorizontalAlignment="Left" 
Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

为了将应用内四处移动的 UI 元素最小化，请在 Backstack 中没有任何内容时显示一个禁用的后退按钮（参阅下面的代码示例）。

![后退按钮状态](images/back-nav/BackDisabled.png)

## <a name="code-example"></a>代码示例

以下代码示例演示如何使用后退按钮实现向后导航行为。 该代码将响应 Button [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) 事件并在 [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)（在导航到新页面时调用）中禁用/启用按钮可见性。 该代码示例还通过为 [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) 事件注册侦听器来处理来自从硬件和软件系统后退键的输入。

```xaml
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

代码隐藏：

```csharp
public MainPage()
{
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    BackButton.IsEnabled = this.Frame.CanGoBack;
}

private void Back_Click(object sender, RoutedEventArgs e)
{
    On_BackRequested();
}

// Handles system-level BackRequested events and page-level back button Click events
private bool On_BackRequested()
{
    if (this.Frame.CanGoBack)
    {
        this.Frame.GoBack();
        return true;
    }
    return false;
}

private void BackInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}
```

在此处，我们将为 `App.xaml` 代码隐藏文件中的 [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) 事件注册全局侦听器。 如果你想要从后退导航排除特定页面，或想要在显示页面前执行页面级别代码，可以在每个页面中注册此事件。

App.xaml 代码隐藏：

```csharp
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
Frame rootFrame = Window.Current.Content;
rootFrame.PointerPressed += On_PointerPressed;

private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    e.Handled = On_BackRequested();
}

private void On_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    bool isXButton1Pressed = e.GetCurrentPoint(sender as UIElement).Properties.PointerUpdateKind == PointerUpdateKind.XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled = On_BackRequested();
    }
}
```

## <a name="optimizing-for-different-device-and-form-factors"></a>针对不同的设备和外形规格进行优化

本向后导航设计指南适用于所有设备，但不同的设备和外形规格可以受益于优化。 这还取决受不同的 shell 支持的硬件后退按钮。

- **手机平板/电脑**：机和平板电脑上总是有硬件或软件后退按钮，但为了清楚起见，我们建议绘制应用内后退按钮。
- **台式机/中心**：在应用的 UI 的左上角绘制应用内后退按钮。
- **Xbox/电视**：不要绘制后退按钮，因为这会为 UI 增加不必要的杂乱感。 相反，应依靠手柄 B 按钮进行向后导航。

如果你的应用将在多台设备上运行，请[创建用于 Xbox 的自定义视觉触发器](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox)以切换按钮的可见性。 如果你的应用正在 Xbox 上运行，NavigationView 控件将自动切换后退按钮的可见性。 

我们建议对后退导航支持以下输入。 （请注意，这些输入中有一些不受系统 BackRequested 的支持，必须由单独的事件处理。）

| 输入 | 事件 |
| --- | --- |
| Windows-Backspace 键 | BackRequested |
| 硬件后退按钮 | BackRequested |
| Shell 平板电脑模式后退按钮 | BackRequested |
| VirtualKey.XButton1 | PointerPressed |
| VirtualKey.GoBack | KeyboardAccelerator.BackInvoked |
| Alt+LeftArrow 键 | KeyboardAccelerator.BackInvoked |

上面提供的代码示例展示了如何处理所有这些输入。

## <a name="system-back-behavior-for-backward-compatibilities"></a>针对后向兼容性的系统后退行为

以前，UWP 应用使用 [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility) 来实现向后导航。 API 将继续获得向后兼容性支持，但我们不会再建议依靠标题栏后退按钮。 相反，你的应用应该绘制自己的应用内后退按钮。

如果你的应用继续使用 [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility)，后退按钮将像往常一样在标题栏中呈现。

![标题栏后退按钮](images/nav-back-pc.png)

## <a name="guidelines-for-custom-back-navigation-behavior"></a>自定义后退导航行为指南

如果你选择提供你自己的后退堆栈导航，则体验应与其他应用保持一致。 我们建议你遵循以下导航操作模式：

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">导航操作</th>
<th align="left">添加到导航历史记录？</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>页面到页面，不同的对等组</strong></td>
<td style="vertical-align:top;"><strong>是</strong>
<p>在此图中，用户从应用的级别 1 导航到级别 2，并且跨对等组，因此该导航将添加到导航历史记录。</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>在下图中，用户在两个同级别的对等组之间导航，并且再次跨对等组，因此该导航将添加到导航历史记录。</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>页面到页面，同一对等组，无屏幕导航元素</strong>
<p>用户从一个页面导航到同一对等组内的另一个页面。 没有始终存在的导航元素（例如顶部导航窗格或停靠的左侧导航窗格）可提供到两个页面的直接导航。</p></td>
<td style="vertical-align:top;"><strong>是</strong>
<p>在下图中，用户在同一对等组中的两个页面之间导航。 这些页面不使用顶部导航栏或停靠的左侧导航窗格，因此该导航将添加到导航历史记录。</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>页面到页面，同一对等组，带有屏幕导航元素</strong>
<p>用户从一个页面导航到同一对等组内的另一个页面。 两个页面显示在相同的导航元素中。 例如，两个页面使用相同的顶部导航元素，或者两个页面都显示在停靠的左侧导航窗格中。</p></td>
<td style="vertical-align:top;"><strong>视情况而定</strong>
<p>是的，添加到导航历史记录，但有 2 个明显例外。 如果预计应用的用户经常在对等组中的页面之间切换，或者希望在对等组的页面中保留导航状态/历史记录，则不要添加到导航历史记录。 在这种情况下，当用户按下后退时，将在用户导航到当前对等组之前返回到上一个页面。 </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>显示瞬态 UI</strong>
<p>应用显示弹出窗口或子窗口（例如对话框、初始屏幕或屏幕键盘），或应用进入特殊模式（例如多重选择模式）。</p></td>
<td style="vertical-align:top;"><strong>否</strong>
<p>当用户按下后退按钮时，取消瞬态 UI（隐藏屏幕键盘、取消对话框等）并返回到生成瞬态 UI 的页面。</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>枚举项目</strong>
<p>应用显示屏幕项目的内容，例如大纲/细节列表中的选定项目的详细信息。</p></td>
<td style="vertical-align:top;"><strong>否</strong>
<p>枚举项目与在对等组内导航类似。 当用户按下后退时，导航到位于当前页面前面的具有项目枚举的页面。</p>
<p><img src="images/back-nav/nav-enumerate.png" alt="Iterm enumeration" /></p></td>
</tr>
</tbody>
</table>
</div>

### <a name="resuming"></a>恢复中

当用户切换到其他应用并返回到你的应用时，我们建议返回到导航历史记录中的最后一页

## <a name="related-articles"></a>相关文章

- [导航基础知识](navigation-basics.md)