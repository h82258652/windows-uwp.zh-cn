---
title: 处理应用激活
description: 了解如何通过替代 OnLaunched 方法处理应用激活。
ms.assetid: DA9A6A43-F09D-4512-A2AB-9B6132431007
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: a75136f26aa6cfa330e4118e6709b0b4d4be4054
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7705290"
---
# <a name="handle-app-activation"></a>处理应用激活

了解如何通过重写[**Application.OnLaunched**](/uwp/api/windows.ui.xaml.application.onlaunched)方法处理应用激活。

## <a name="override-the-launch-handler"></a>替代启动处理程序

当激活应用时，出于任何原因，系统将发送[**CoreApplicationView.Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated)事件。 有关激活类型的列表，请参阅 [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693) 枚举。

[**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 类定义了为处理各种激活类型而可以替代的一些方法。 对于其中一些激活类型，有特定的方法可以替代。 对于其他激活类型，则替代 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 方法。

为你的应用程序定义类。

```xml
<Application
    x:Class="AppName.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
```

替代 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法。 只要用户启动该应用，就会调用此方法。 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) 参数包含你的应用之前的状态和激活参数。

> [!NOTE]
> 在 Windows 上，启动开始磁贴或应用列表从暂停的应用不会调用此方法。

```csharp
using System;
using Windows.ApplicationModel.Activation;
using Windows.UI.Xaml;

namespace AppName
{
   public partial class App
   {
      async protected override void OnLaunched(LaunchActivatedEventArgs args)
      {
         EnsurePageCreatedAndActivate();
      }

      // Creates the MainPage if it isn't already created.  Also activates
      // the window so it takes foreground and input focus.
      private MainPage EnsurePageCreatedAndActivate()
      {
         if (Window.Current.Content == null)
         {
             Window.Current.Content = new MainPage();
         }

         Window.Current.Activate();
         return Window.Current.Content as MainPage;
      }
   }
}
```

```vb
Class App
   Protected Overrides Sub OnLaunched(args As LaunchActivatedEventArgs)
      Window.Current.Content = New MainPage()
      Window.Current.Activate()
   End Sub
End Class
```

```cppwinrt
...
#include "MainPage.h"
#include "winrt/Windows.ApplicationModel.Activation.h"
#include "winrt/Windows.UI.Xaml.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
...
using namespace winrt;
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    /// <summary>
    /// Invoked when the application is launched normally by the end user.  Other entry points
    /// will be used such as when the application is launched to open a specific file.
    /// </summary>
    /// <param name="e">Details about the launch request and process.</param>
    void OnLaunched(LaunchActivatedEventArgs const& e)
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
        }

        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active
        if (rootFrame == nullptr)
        {
            // Create a Frame to act as the navigation context and associate it with
            // a SuspensionManager key
            rootFrame = Frame();

            rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

            if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated)
            {
                // Restore the saved session state only when appropriate, scheduling the
                // final launch steps after the restore is complete
            }

            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Place the frame in the current Window
                Window::Current().Content(rootFrame);
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
        else
        {
            if (e.PrelaunchActivated() == false)
            {
                if (rootFrame.Content() == nullptr)
                {
                    // When the navigation stack isn't restored navigate to the first page,
                    // configuring the new page by passing required information as a navigation
                    // parameter
                    rootFrame.Navigate(xaml_typename<BlankApp1::MainPage>(), box_value(e.Arguments()));
                }
                // Ensure the current window is active
                Window::Current().Activate();
            }
        }
    }
};
```

```cpp
using namespace Windows::ApplicationModel::Activation;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;
using namespace AppName;
void App::OnLaunched(LaunchActivatedEventArgs^ args)
{
   EnsurePageCreatedAndActivate();
}

// Creates the MainPage if it isn't already created.  Also activates
// the window so it takes foreground and input focus.
void App::EnsurePageCreatedAndActivate()
{
    if (_mainPage == nullptr)
    {
        // Save the MainPage for use if we get activated later
        _mainPage = ref new MainPage();
    }
    Window::Current->Content = _mainPage;
    Window::Current->Activate();
}
```

## <a name="restore-application-data-if-app-was-suspended-then-terminated"></a>在应用暂停而后终止时还原应用程序数据

当用户切换到终止的应用时，系统将发送 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件，其中 [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 设置为 **Launch**，[**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 设置为 **Terminated** 或 **ClosedByUser**。 应用应该加载其保存的应用程序数据并刷新其显示的内容。

```csharp
async protected override void OnLaunched(LaunchActivatedEventArgs args)
{
   if (args.PreviousExecutionState == ApplicationExecutionState.Terminated ||
       args.PreviousExecutionState == ApplicationExecutionState.ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

```vb
Protected Overrides Sub OnLaunched(args As Windows.ApplicationModel.Activation.LaunchActivatedEventArgs)
   Dim restoreState As Boolean = False

   Select Case args.PreviousExecutionState
      Case ApplicationExecutionState.Terminated
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case ApplicationExecutionState.ClosedByUser
         ' TODO: Populate the UI with the previously saved application data
         restoreState = True
      Case Else
         ' TODO: Populate the UI with defaults
   End Select

   Window.Current.Content = New MainPage(restoreState)
   Window.Current.Activate()
End Sub
```

```cppwinrt
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs const& e)
{
    if (e.PreviousExecutionState() == ApplicationExecutionState::Terminated ||
        e.PreviousExecutionState() == ApplicationExecutionState::ClosedByUser)
    {
        // Populate the UI with the previously saved application data.
    }
    else
    {
        // Populate the UI with defaults.
    }
    ...
}
```

```cpp
void App::OnLaunched(Windows::ApplicationModel::Activation::LaunchActivatedEventArgs^ args)
{
   if (args->PreviousExecutionState == ApplicationExecutionState::Terminated ||
       args->PreviousExecutionState == ApplicationExecutionState::ClosedByUser)
   {
      // TODO: Populate the UI with the previously saved application data
   }
   else
   {
      // TODO: Populate the UI with defaults
   }

   EnsurePageCreatedAndActivate();
}
```

如果 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 的值为 **NotRunning**，则应用无法成功保存其应用程序数据，应用应该如同初始启动一样重新启动。

## <a name="remarks"></a>备注

> [!NOTE]
> 如果当前窗口上已有内容集，则应用可跳过初始化。 你可以检查[**LaunchActivatedEventArgs.TileId**](https://msdn.microsoft.com/library/windows/apps/br224736)属性，以确定是否从主或辅助磁贴启动应用，并根据该信息，确定是否应显示新或恢复应用体验。

## <a name="important-apis"></a>重要的 API
* [Windows.ApplicationModel.Activation](https://msdn.microsoft.com/library/windows/apps/br224766)
* [Windows.UI.Xaml.Application](https://msdn.microsoft.com/library/windows/apps/br242324)

## <a name="related-topics"></a>相关主题
* [处理应用暂停](suspend-an-app.md)
* [处理应用恢复](resume-an-app.md)
* [应用暂停和恢复指南](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [应用生命周期](app-lifecycle.md)
