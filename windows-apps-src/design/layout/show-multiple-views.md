---
Description: 在单独的窗口中查看你的应用程序的不同部分。
title: 显示应用的多个视图
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ee49b5fe5b5956e9069ea196c4d2e029b3a15763
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729511"
---
# <a name="show-multiple-views-for-an-app"></a>显示应用的多个视图

![使用多个窗口显示应用的线框](images/multi-view.gif)

通过让用户在单独窗口中查看应用的独立部分，可帮助他们提高效率。 当你为某个应用程序创建多个窗口时, 任务栏单独显示每个窗口。 用户可以独立地移动、调整大小、显示和隐藏应用窗口，并且可以在应用窗口间切换，就像它们是单独的应用一样。

> **重要的 API**：[Windows.ui.viewmanagement 命名空间](/uwp/api/windows.ui.viewmanagement), [WindowManagement 命名空间](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>应用应在何时使用多个视图？

可以从多个视图中获益的各种方案。 以下是几个示例：

- 电子邮件应用，可让用户在撰写新电子邮件的同时查看收到邮件的列表
- 地址簿应用，可让用户并排比较多名人员的联系信息
- 音乐播放器应用，可让用户在查看播放内容的同时浏览其他可收听音乐的列表
- 记笔记应用，可让用户将信息从笔记的一页复制到另一页
- 阅读应用，可让用户在错过细读所有高水平的头条新闻后，打开多篇文章以备以后阅读

虽然每个应用布局都是唯一的，但我们建议在可预测位置加入“新窗口”按钮，如你可以在新窗口中打开的内容右上角。 还应考虑将[上下文菜单](../controls-and-patterns/menus.md)选项包括为 "在新窗口中打开"。

若要创建应用的单独实例 (而不是针对同一实例的单独窗口), 请参阅[创建多实例 UWP 应用](../../launch-resume/multi-instance-uwp.md)。

## <a name="windowing-hosts"></a>窗口化主机

可以通过不同的方式在应用程序中承载 UWP 内容。

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     应用视图是指将线程与应用用来显示内容的窗口进行 1:1 配对。 应用启动时创建的第一个视图称为*主视图*。 每个 CoreWindow/w 都在其自己的线程中运行。 不得不处理不同的 UI 线程会使多窗口应用变得复杂。

    应用的主视图始终托管在 w 中。 辅助窗口中的内容可以托管在 w 中, 也可以托管在 AppWindow 中。

    若要了解如何使用 w 在应用程序中显示辅助窗口, 请参阅[使用 w](application-view.md)。
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow 简化了多窗口 UWP 应用的创建, 因为它在从其创建的同一 UI 线程上运行。

    从 Windows 10 版本 1903 (SDK 18362) 开始, [WindowManagement](/uwp/api/windows.ui.windowmanagement)命名空间中的 AppWindow 类和其他 api 可用。 如果你的应用面向 Windows 10 的早期版本, 则必须使用 w 创建辅助窗口。

    若要了解如何使用 AppWindow 在应用程序中显示辅助窗口, 请参阅[使用 AppWindow](app-window.md)。

    > [!NOTE]
    > AppWindow 目前处于预览阶段。 这意味着你可以将使用 AppWindow 的应用提交到应用商店, 但知道某些平台和框架组件不能使用 AppWindow (请参阅[限制](/uwp/api/windows.ui.windowmanagement.appwindow#limitations))。
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource)(XAML 岛)

     Win32 应用 (使用 HWND) (也称为 XAML 孤岛) 中的 UWP XAML 内容托管在 DesktopWindowXamlSource 中。

    有关 XAML 孤岛的详细信息, 请参阅[在桌面应用程序中使用 UWP XAML 宿主 API](/windows/apps/desktop/modernize/using-the-xaml-hosting-api)

### <a name="make-code-portable-across-windowing-hosts"></a>使代码可在窗口化主机间移植

当 XAML 内容显示在[CoreWindow](/uwp/api/windows.ui.core.corewindow)中时, 始终有一个关联的[w](/uwp/api/windows.ui.viewmanagement.applicationview)和 XAML[窗口](/uwp/api/windows.ui.xaml.window)。 您可以使用这些类上的 Api 来获取窗口边界等信息。 若要检索这些类的实例, 请使用静态[CoreWindow. GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)方法、 [w](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview)或[Current](/uwp/api/windows.ui.xaml.window.current)属性。 此外, 还有许多类使用`GetForCurrentView`模式来检索类的实例, 例如[DisplayInformation. GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview)。

由于 CoreWindow/w 只有一个 XAML 内容树, 因此这些 Api 工作正常, 因此 XAML 知道承载它的上下文是 CoreWindow/w。

当 XAML 内容在 AppWindow 或 DesktopWindowXamlSource 中运行时, 可以同时在同一线程上运行多个 XAML 内容树。 在这种情况下, 这些 Api 不会给出正确的信息, 因为内容不再在当前 CoreWindow/w (和 XAML 窗口) 中运行。

若要确保你的代码在所有窗口主机上正常工作, 你应该将依赖于[CoreWindow](/uwp/api/windows.ui.core.corewindow)、 [w](/uwp/api/windows.ui.viewmanagement.applicationview)和[windows](/uwp/api/windows.ui.xaml.window)的 api 替换为从[XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)类获取其上下文的新 api。
XamlRoot 类表示 XAML 内容的树和有关承载该内容的上下文的信息, 无论是 CoreWindow、AppWindow 还是 DesktopWindowXamlSource。 此抽象层允许您编写相同的代码, 而无需考虑 XAML 在哪个窗口中运行。

此表显示了在窗口化主机上无法正常工作的代码, 以及可以用来替换它的新的可移植代码, 以及一些不需要更改的 Api。

| 如果使用了 。 | 替换为 。 |
| - | - |
| CoreWindow. GetForCurrentThread ()。[边界](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_。XamlRoot.[大小](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow. GetForCurrentThread ()。[SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_。XamlRoot.[更改](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.[可见](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_。XamlRoot.[IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow.[VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_。XamlRoot.[更改](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow. GetForCurrentThread ()。[GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | 变化. AppWindow 和 DesktopWindowXamlSource 支持此项。 |
| CoreWindow. GetForCurrentThread ()。[GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | 变化. AppWindow 和 DesktopWindowXamlSource 支持此项。 |
| 窗口.[当前](/uwp/api/windows.ui.xaml.window.current) | 返回与当前 CoreWindow 密切绑定的主 XAML 窗口对象。 请参阅此表后面的说明。 |
| 当前窗口。[边界](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_。XamlRoot.[大小](/uwp/api/windows.ui.xaml.xamlroot.size) |
| 当前窗口。[内容](/uwp/api/windows.ui.xaml.window.content) | UIElement root = _uielement_。XamlRoot.[内容](/uwp/api/windows.ui.xaml.xamlroot.content) |
| 当前窗口。[组合](/uwp/api/windows.ui.xaml.window.compositor)器 | 变化. AppWindow 和 DesktopWindowXamlSource 支持此项。 |
| VisualTreeHelper.[GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>在 XAML 孤岛应用中, 这会引发错误。 在 AppWindow 应用中, 这将返回主窗口上打开的弹出窗口。 | VisualTreeHelper.[GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot)(_uiElement_。XamlRoot) |
| System.windows.input.focusmanager>.[System.windows.input.focusmanager.getfocusedelement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | System.windows.input.focusmanager>.[System.windows.input.focusmanager.getfocusedelement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_)(_uiElement_。XamlRoot) |
| contentDialog. ShowAsync () | contentDialog.[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) =  _uiElement_。XamlRoot;<br/>contentDialog. ShowAsync (); |
| menuFlyout. ShowAt (null, new Point (10, 10)); | menuFlyout.[XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) =  _uiElement_。XamlRoot;<br/>menuFlyout. ShowAt (null, new Point (10, 10)); |

> [!NOTE]
> 对于 DesktopWindowXamlSource 中的 XAML 内容, 线程上存在一个 CoreWindow/窗口, 但它始终不可见且大小为1x1。 应用程序仍可对其进行访问, 但不会返回有意义的边界或可见性。
>
>对于 AppWindow 中的 XAML 内容, 同一线程上始终只有一个 CoreWindow。 如果调用`GetForCurrentView`或`GetForCurrentThread` api, 该 api 将返回一个对象, 该对象反映线程上 CoreWindow 的状态, 而不是任何可能在该线程上运行的 AppWindows。


## <a name="dos-and-donts"></a>应做事项和禁止事项

- 务必利用“打开新窗口”字形为辅助视图提供清晰的入口点。
- 务必向用户传达辅助视图的用途。
- 确保你的应用程序在单个视图中完全正常运行, 并且用户只是为了方便起见, 将打开辅助视图。
- 不依靠辅助视图提供通知或其他暂时的显示内容。

## <a name="related-topics"></a>相关主题

- [使用 AppWindow](app-window.md)
- [使用 w](application-view.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
