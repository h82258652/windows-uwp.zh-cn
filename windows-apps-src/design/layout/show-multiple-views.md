---
Description: 在单独的窗口中查看应用的不同组成部分。
title: 显示应用的多个视图
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e7d6ea614a9d85eadfcb807c6e6100dbe15ed0c4
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970732"
---
# <a name="show-multiple-views-for-an-app"></a>显示应用的多个视图

![使用多个窗口显示应用的框图](images/multi-view.gif)

通过让用户在单独的窗口中查看应用的独立组成部分，来帮助他们提高工作效率。 为应用创建多个窗口时，任务栏会单独显示每个窗口。 用户可以独立地移动、调整大小、显示和隐藏应用窗口，并且可以在应用窗口间切换，就像它们是单独的应用一样。

> **重要的 API**：[Windows.UI.ViewManagement 命名空间](/uwp/api/windows.ui.viewmanagement)、[Windows.UI.WindowManagement 命名空间](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>应用何时应使用多个视图？

有多种情况可从多个视图中获益。 以下是几个示例：

- 电子邮件应用，可让用户在撰写新电子邮件的同时查看收到的邮件列表
- 地址簿应用，可让用户并排比较多个人员的联系信息
- 音乐播放器应用，可让用户在观看播放内容的同时浏览其他可收听的音乐列表
- 笔记应用，可让用户将信息从笔记的一页复制到另一页
- 阅读应用，可让用户在审阅所有标题概要后，打开多篇文章供稍后阅读

尽管每个应用布局都是独特的，但我们建议在可预测的位置（例如，可在新窗口中打开的内容的右上角）包含一个“新建窗口”按钮。 另外，请考虑在“在新窗口中打开”中包含一个[上下文菜单](../controls-and-patterns/menus.md)选项。

若要创建应用的单独实例（而不是为同一实例创建不同的窗口），请参阅[创建多实例 Windows 应用](../../launch-resume/multi-instance-uwp.md)。

## <a name="windowing-hosts"></a>窗口宿主

可以通过不同的方式在应用中承载 Windows 内容。

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     应用视图是指将线程与应用用来显示内容的窗口进行 1:1 配对。 应用启动时创建的第一个视图称为*主视图*。 每个 CoreWindow/ApplicationView 在其自身的线程中运行。 如果非要使用不同的 UI 线程，多窗口应用可能会变得复杂。

    应用的主视图始终承载在 ApplicationView 中。 辅助窗口中的内容可以承载在 ApplicationView 或 AppWindow 中。

    若要了解如何使用 ApplicationView 在应用中显示辅助窗口，请参阅[使用 ApplicationView](application-view.md)。
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow 简化了多窗口 Windows 应用的创建，因为其创建和运行均在同一 UI 线程。

    从 Windows 10 版本 1903 (SDK 18362) 开始提供 AppWindow 类，以及 [WindowManagement](/uwp/api/windows.ui.windowmanagement) 命名空间中的其他 API。 如果应用面向早期版本的 Windows 10，则必须使用 ApplicationView 创建辅助窗口。

    若要了解如何使用 AppWindow 在应用中显示辅助窗口，请参阅[使用 AppWindow](app-window.md)。

    > [!NOTE]
    > AppWindow 目前以预览版提供。 这意味着，可将使用 AppWindow 的应用提交到 Store，但某些平台和框架组件已知不能与 AppWindow 配合工作（请参阅[限制](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)）。
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) (XAML Islands)

     使用 HWND 的 Win32 应用中的 UWP XAML 内容（也称为 XAML Islands）承载在 DesktopWindowXamlSource 中。

    有关 XAML Islands 的详细信息，请参阅[在桌面应用程序中使用 UWP XAML 宿主 API](/windows/apps/desktop/modernize/using-the-xaml-hosting-api)

### <a name="make-code-portable-across-windowing-hosts"></a>使代码可跨窗口宿主移植

在 [CoreWindow](/uwp/api/windows.ui.core.corewindow) 中显示 XAML 内容时，始终有一个关联的 [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) 和 XAML [Window](/uwp/api/windows.ui.xaml.window)。 可以使用这些类中的 API 来获取窗口边界等信息。 若要检索这些类的实例，请使用静态 [CoreWindow.GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 方法、[ApplicationView.GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) 方法或 [Window.Current](/uwp/api/windows.ui.xaml.window.current) 属性。 此外，有许多类使用 `GetForCurrentView` 模式来检索类的实例，例如 [DisplayInformation.GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview)。

这些 API 之所以能够正常工作，是因为 CoreWindow/ApplicationView 只有单个 XAML 内容树，因此 XAML 知道承载它的上下文是该 CoreWindow/ApplicationView。

当 XAML 内容在 AppWindow 或 DesktopWindowXamlSource 中运行时，你可以获得同时在同一线程中运行的多个 XAML 内容树。 在这种情况下，这些 API 不会提供适当的信息，因为内容不再在当前 CoreWindow/ApplicationView（和 XAML 窗口）中运行。

若要确保代码可在所有窗口宿主中正常工作，应将依赖于 [CoreWindow](/uwp/api/windows.ui.core.corewindow)、[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)和 [Window](/uwp/api/windows.ui.xaml.window) 的 API 替换为从 [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot) 类获取其上下文的新 API。
XamlRoot 类表示 XAML 内容树以及有关承载该内容的上下文的信息，而不管该上下文是 CoreWindow、AppWindow 还是 DesktopWindowXamlSource。 无论 XAML 在哪个窗口宿主中运行，此抽象层都可让你编写相同的代码。

下表显示了无法在窗口宿主中正常工作的代码、可用于替换这些代码的新可移植代码，以及一些不需要更改的 API。

| 如果使用了... | 请替换为... |
| - | - |
| CoreWindow.GetForCurrentThread().[Bounds](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow.GetForCurrentThread().[SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.[Visible](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_.XamlRoot.[IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow.[VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.GetForCurrentThread().[GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | 未更改。 AppWindow 和 DesktopWindowXamlSource 支持此代码。 |
| CoreWindow.GetForCurrentThread().[GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | 未更改。 AppWindow 和 DesktopWindowXamlSource 支持此代码。 |
| Window.[Current](/uwp/api/windows.ui.xaml.window.current) | 返回与当前 CoreWindow 密切绑定的主 XAML Window 对象。 请参阅此表格后面的“说明”。 |
| Window.Current.[Bounds](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Window.Current.[Content](/uwp/api/windows.ui.xaml.window.content) | UIElement root =  _uiElement_.XamlRoot.[Content](/uwp/api/windows.ui.xaml.xamlroot.content) |
| Window.Current.[Compositor](/uwp/api/windows.ui.xaml.window.compositor) | 未更改。 AppWindow 和 DesktopWindowXamlSource 支持此代码。 |
| VisualTreeHelper.[GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>在 XAML Islands 应用中，此代码会引发错误。 在 AppWindow 应用中，此代码会返回主窗口中打开的弹出窗口。 | VisualTreeHelper.[GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot)(_uiElement_.XamlRoot) |
| FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_)(_uiElement_.XamlRoot) |
| contentDialog.ShowAsync() | contentDialog.[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) = _uiElement_.XamlRoot;<br/>contentDialog.ShowAsync(); |
| menuFlyout.ShowAt(null, new Point(10, 10)); | menuFlyout.[XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) = _uiElement_.XamlRoot;<br/>menuFlyout.ShowAt(null, new Point(10, 10)); |

> [!NOTE]
> 对于 DesktopWindowXamlSource 中的 XAML 内容，线程上存在一个 CoreWindow/Window，但此对象始终不可见且大小为 1x1。 此对象仍可供应用访问，但不会返回有意义的边界或可见性。
>
>对于 AppWindow 中的 XAML 内容，同一线程上始终只有一个 CoreWindow。 如果调用 `GetForCurrentView` 或 `GetForCurrentThread` API，该 API 会返回一个对象，该对象反映线程上的 CoreWindow 状态，但不反映该线程上可能正在运行的任何 AppWindow 的状态。


## <a name="dos-and-donts"></a>应做事项和禁止事项

- 务必利用“打开新窗口”字形为辅助视图提供清晰的入口点。
- 务必向用户传达辅助视图的用途。
- 务必确保应用在单个视图中完全正常运行，用户打开辅助视图只是出于方便。
- 不依赖辅助视图提供通知或其他暂时性的视觉内容。

## <a name="related-topics"></a>相关主题

- [使用 AppWindow](app-window.md)
- [使用 ApplicationView](application-view.md)
- [ApplicationViewSwitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
