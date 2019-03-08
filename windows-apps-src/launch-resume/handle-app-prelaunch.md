---
title: 处理应用预启动
description: 了解如何通过替代 OnLaunched 方法并调用 CoreApplication.EnablePrelaunch(true) 处理应用预启动。
ms.assetid: A4838AC2-22D7-46BA-9EB2-F3C248E22F52
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 11f68d9dd912c92ff7de8b861f576e8f0c4b4dde
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658702"
---
# <a name="handle-app-prelaunch"></a>处理应用预启动

了解如何通过替代 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法处理应用预启动。

## <a name="introduction"></a>简介

当可用系统资源允许时，通过主动启动用户的使用频率最高的应用在后台提高桌面设备系列设备上的 UWP 应用的启动性能。 预启动的应用在启动后不久就会处于暂停状态。 然后，在用户调用该应用时，它通过从暂停状态切换到运行状态而得到恢复，这比冷启动应用的速度要快得多。 用户的体验只是启动应用的速度非常快。

在 Windows 10 之前，应用不会自动利用预启动。 在 Windows 10 版本 1511，所有通用 Windows 平台 (UWP) 应用为正在 prelaunched 是候选项。 在 Windows 10 版本 1607 中，你必须通过调用 [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx) 选择加入预启动行为。 放置此调用的绝佳位置是进行 `if (e.PrelaunchActivated == false)` 检查的位置附近的 `OnLaunched()` 内。

是否预启动应用取决于系统资源。 如果系统遇到资源压力，则不会预启动应用。

某些类型的应用可能需要更改启动行为，以便可以与预启动很好地结合使用。 例如，在启动时可播放音乐的应用、假定当应用启动时用户存在并显示详细视觉效果的游戏和在启动期间更改用户的联机可见性的消息应用可以标识应用预启动的时间，并且可以更改其启动行为，如下面部分所述。

XAML 项目（C#、VB、C++）和 WinJS 的默认模板适合在 Visual Studio 2015 Update 3 中预启动。

## <a name="prelaunch-and-the-app-lifecycle"></a>预启动和应用生命周期

预启动应用后，它将进入暂停状态。 （请参阅[处理应用暂停](suspend-an-app.md)）。

## <a name="detect-and-handle-prelaunch"></a>检测和处理预启动

在激活期间，应用会收到 [**LaunchActivatedEventArgs.PrelaunchActivated**](https://msdn.microsoft.com/library/windows/apps/dn263740) 标志。 使用此标志来运行应仅运行时在用户显式启动应用程序中，如以下修改中所示的代码[ **Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335)。

```csharp
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // CoreApplication.EnablePrelaunch was introduced in Windows 10 version 1607
    bool canEnablePrelaunch = Windows.Foundation.Metadata.ApiInformation.IsMethodPresent("Windows.ApplicationModel.Core.CoreApplication", "EnablePrelaunch");

    // NOTE: Only enable this code if you are targeting a version of Windows 10 prior to version 1607
    // and you want to opt-out of prelaunch.
    // In Windows 10 version 1511, all UWP apps were candidates for prelaunch.
    // Starting in Windows 10 version 1607, the app must opt-in to be prelaunched.
    //if ( !canEnablePrelaunch && e.PrelaunchActivated == true)
    //{
    //    return;
    //}

    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        // On Windows 10 version 1607 or later, this code signals that this app wants to participate in prelaunch
        if (canEnablePrelaunch)
        {
            TryEnablePrelaunch();
        }

        // TODO: This is not a prelaunch activation. Perform operations which
        // assume that the user explicitly launched the app such as updating
        // the online presence of the user on a social network, updating a
        // what's new feed, etc.

        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();
    }
}

/// <summary>
/// Encapsulates the call to CoreApplication.EnablePrelaunch() so that the JIT
/// won't encounter that call (and prevent the app from running when it doesn't
/// find it), unless this method gets called. This method should only
/// be called when the caller determines that we are running on a system that
/// supports CoreApplication.EnablePrelaunch().
/// </summary>
private void TryEnablePrelaunch()
{
    Windows.ApplicationModel.Core.CoreApplication.EnablePrelaunch(true);
}
```

请注意`TryEnablePrelaunch()`正常工作，更高版本。 原因调用到`CoreApplication.EnablePrelaunch()`分解扩展到此函数是因为 JIT （实时编译） 时调用的方法，将尝试编译整个方法。 如果不支持的 Windows 10 版本上运行你的应用`CoreApplication.EnablePrelaunch()`，则 JIT 将失败。 通过分离到应用程序确定该平台支持时才调用的方法调用`CoreApplication.EnablePrelaunch()`，我们避免该问题。

没有还在上面的示例代码，您可以取消注释掉你的应用需要退出的预启动 Windows 10，版本 1511年上运行时。 在版本 1511年中，所有 UWP 应用已自动都选择为预启动，这可能不会适合您的应用程序。

## <a name="use-the-visibilitychanged-event"></a>使用 VisibilityChanged 事件

用户看不到通过预启动激活的应用。 它们在用户切换到它们时可见。 你可能想要延迟某些操作，直到可以看到应用主窗口为止。 例如，如果你的应用显示来自某个源的新增项列表，你可以在 [**VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458) 事件期间更新该列表，而非使用在预启动该应用时生成的列表，因为该列表可能在用户激活该应用时就已过时。 以下代码处理 **MainPage** 的 **VisibilityChanged** 事件：

```csharp
public sealed partial class MainPage : Page
{
    public MainPage()
    {
        this.InitializeComponent();

        Window.Current.VisibilityChanged += WindowVisibilityChangedEventHandler;
    }

    void WindowVisibilityChangedEventHandler(System.Object sender, Windows.UI.Core.VisibilityChangedEventArgs e)
    {
        // Perform operations that should take place when the application becomes visible rather than
        // when it is prelaunched, such as building a what's new feed
    }
}
```

## <a name="directx-games-guidance"></a>DirectX 游戏指南

DirectX 游戏通常不应启用预启动，因为许多 DirectX 游戏在可以检测到预启动之前执行其初始化。 从 Windows 1607 周年版本开始，默认情况下不会预启动你的游戏。  如果你希望你的游戏充分利用预启动，请调用 [CoreApplication.EnablePrelaunch(true)](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx)。

如果你的游戏面向早期版本的 Windows 10，你可以处理预启动条件退出应用程序：

```cppwinrt
void ViewProvider::OnActivated(CoreApplicationView const& /* appView */, Windows::ApplicationModel::Activation::IActivatedEventArgs const& args)
{
    if (args.Kind() == Windows::ApplicationModel::Activation::ActivationKind::Launch)
    {
        auto launchArgs{ args.as<Windows::ApplicationModel::Activation::LaunchActivatedEventArgs>()};
        if (launchArgs.PrelaunchActivated())
        {
            // Opt-out of Prelaunch.
            CoreApplication::Exit();
        }
    }
}

void ViewProvider::Initialize(CoreApplicationView const & appView)
{
    appView.Activated({ this, &App::OnActivated });
}
```

```cpp
void ViewProvider::OnActivated(CoreApplicationView^ appView,IActivatedEventArgs^ args)
{
    if (args->Kind == ActivationKind::Launch)
    {
        auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args);
        if (launchArgs->PrelaunchActivated)
        {
            // Opt-out of Prelaunch
            CoreApplication::Exit();
            return;
        }
    }
}
```

## <a name="winjs-app-guidance"></a>WinJS 应用指南

如果你的 WinJS 应用面向早期版本的 Windows 10，你可以在 [onactivated](https://msdn.microsoft.com/library/windows/apps/br212679.aspx) 处理程序中处理预启动条件：

```javascript
    app.onactivated = function (args) {
        if (!args.detail.prelaunchActivated) {
            // TODO: This is not a prelaunch activation. Perform operations which
            // assume that the user explicitly launched the app such as updating
            // the online presence of the user on a social network, updating a
            // what's new feed, etc.
        }
    }
```

## <a name="general-guidance"></a>一般指南

-   应用不应在预启动期间执行运行时间较长的操作，因为该应用会在无法快速暂停的情况下终止。
-   预启动应用时，应用不应从 [**Application.OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 启动音频播放，因为该应用不可见并且音频播放的原因并不明显。
-   假设应用对用户可见，或假设应用由用户显式启动，则应用不应在启动期间执行任何操作。 因为应用现在可以在后台启动并且无需用户显式操作，所以开发人员应该考虑隐私、用户体验和性能含义。
    -   一个隐私注意事项的示例是，社交应用应该何时将用户状态更改为在线。 它应该等到用户切换到该应用时才更改状态，而非在预启动应用时更改状态。
    -   一个用户体验注意事项的示例是，如果你拥有某个在启动时显示初级序列的应用（例如游戏），你可能会想要将该初级序列延迟到用户切换到该应用时。
    -   一个性能含义的示例是，你可能会等到用户切换到应用时才检索当前天气信息（而非在应用预启动时加载该信息），并且需要在应用可见时重新加载该信息，以确保该信息是最新的。
-   如果你的应用在启动时清除动态磁贴，请将此操作延迟到执行可见性更改事件时。
-   应用的遥测应可以区分正常磁贴激活和预启动激活，以便更轻松地缩小发生问题的方案。
-   如果你有 Microsoft Visual Studio 2015 Update 1 和 Windows 10，版本 1511，可以模拟预启动的应用在应用 Visual Studio 2015 中通过选择**调试** &gt; **其他调试目标**&gt; **调试 Windows 通用应用程序预启动**。

## <a name="related-topics"></a>相关主题

* [应用生命周期](app-lifecycle.md)
* [CoreApplication.EnablePrelaunch](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.core.coreapplication.enableprelaunch.aspx)
