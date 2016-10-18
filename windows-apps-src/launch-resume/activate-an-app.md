---
author: TylerMSFT
title: "处理应用激活"
description: "了解如何通过替代 OnLaunched 方法来处理应用激活。"
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
translationtype: Human Translation
ms.sourcegitcommit: a1bb0d5d24291fad1acab41c149dd9d763610907
ms.openlocfilehash: e41a683026a4543545556e98f6b4e9194099b362

---

# 处理应用激活


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


了解如何通过替代 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法处理应用激活。

## 替代启动处理程序

当激活应用时，无论任何原因，系统都会发送 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件。 有关激活类型的列表，请参阅 [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693) 枚举。

[**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 类定义了为处理各种激活类型而可以替代的一些方法。 对于其中一些激活类型，有特定的方法可以替代。 对于其他激活类型，则替代 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 方法。

为你的应用程序定义类。

```xml
<Application xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AppName.App" >
```

替代 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法。 只要用户启动该应用，就会调用此方法。 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) 参数包含你的应用之前的状态和激活参数。

**注意** 对于 Windows Phone 应用商店应用，每次用户从“开始”磁贴或应用列表启动应用时都会调用此方法，即使该应用当前在内存中已暂停也是如此。 在 Windows 上，从“开始”磁贴或应用列表启动暂停的应用不会调用此方法。

> [!div class="tabbedCodeSnippets"]
> ```cs
> using System;
> using Windows.ApplicationModel.Activation;
> using Windows.UI.Xaml;
>
> namespace AppName
> {
>    public partial class App
>    {
>       async protected override void OnLaunched(LaunchActivatedEventArgs args)
>       {
>          EnsurePageCreatedAndActivate();
>       }
>
>       // Creates the MainPage if it isn't already created.  Also activates
>       // the window so it takes foreground and input focus.
>       private MainPage EnsurePageCreatedAndActivate()
>       {
>          if (Window.Current.Content == null)
>          {
>              Window.Current.Content = new MainPage();
>          }
>
>          Window.Current.Activate();
>          return Window.Current.Content as MainPage;
>       }
>    }
> }
> ```
> ```vb
> Class App
>    Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
>       Window.Current.Content = New MainPage()
>       Window.Current.Activate()
>    End Sub
> End Class
> ```
> ```cpp
> using namespace Windows::ApplicationModel::Activation;
> using namespace Windows::Foundation;
> using namespace Windows::UI::Xaml;
> using namespace AppName;
> void App::OnLaunched(LaunchActivatedEventArgs^ args)
> {
>    EnsurePageCreatedAndActivate();
> }
>
> // Creates the MainPage if it isn't already created.  Also activates
> // the window so it takes foreground and input focus.
> void App::EnsurePageCreatedAndActivate()
> {
>     if (_mainPage == nullptr)
>     {
>         // Save the MainPage for use if we get activated later
>         _mainPage = ref new MainPage();
>     }
>     Window::Current->Content = _mainPage;
>     Window::Current->Activate();
> }
> ```

## 在应用暂停而后终止时还原应用程序数据


当用户切换到终止的应用时，系统将发送 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件，其中 [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 设置为 **Launch**，[**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 设置为 **Terminated** 或 **ClosedByUser**。 应用应该加载其保存的应用程序数据并刷新其显示的内容。

> [!div class="tabbedCodeSnippets"]
> ```cs
> async protected override void OnLaunched(LaunchActivatedEventArgs args)
> {
>    if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
>        args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
>    {
>       // TODO: Populate the UI with the previously saved application data
>    }
>    else
>    {
>       // TODO: Populate the UI with defaults
>    }
>
>    EnsurePageCreatedAndActivate();
> }
> ```
> ```vb
> Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
>    Dim restoreState As Boolean = False
>
>    Select Case args.PreviousExecutionState
>       Case ApplicationExecutionState.Terminated
>          ' TODO: Populate the UI with the previously saved application data
>          restoreState = True
>       Case ApplicationExecutionState.ClosedByUser
>          ' TODO: Populate the UI with the previously saved application data
>          restoreState = True
>       Case Else
>          ' TODO: Populate the UI with defaults
>    End Select
>
>    Window.Current.Content = New MainPage(restoreState)
>    Window.Current.Activate()
> End Sub
> ```
> ```cpp
> void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
> {
>    if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
>        args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
>    {
>       // TODO: Populate the UI with the previously saved application data
>    }
>    else
>    {
>       // TODO: Populate the UI with defaults
>    }
>
>    EnsurePageCreatedAndActivate();
> }
> ```

如果 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 的值为 **NotRunning**，则应用无法成功保存其应用程序数据，应用应该如同初始启动一样重新启动。

## 备注

> **注意** 对于 Windows Phone 应用商店应用，[**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件始终后跟 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 事件，即使你的应用当前已暂停且用户从主要磁贴或应用列表中重新启动它也是如此。 如果当前窗口上已有内容集，则应用可跳过初始化。 你可以检查 [**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736) 属性以确定该应用是从主要磁贴启动还是从辅助磁贴启动，并可根据该信息，确定是应显示新的应用体验还是应恢复应用体验。

## 相关主题

* [处理应用暂停](suspend-an-app.md)
* [处理应用恢复](resume-an-app.md)
* [应用暂停和恢复指南](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [应用生命周期](app-lifecycle.md)

**参考**

* [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
* [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324)

 

 



<!--HONumber=Aug16_HO3-->


