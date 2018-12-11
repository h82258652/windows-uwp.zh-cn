---
title: 如何恢复应用（DirectX 和 C++）
description: 本主题介绍了系统在恢复通用 Windows 平台 (UWP) DirectX 应用时，如何还原重要的应用程序数据。
ms.assetid: 5e6bb673-6874-ace5-05eb-f88c045f2178
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 恢复, directx
ms.localizationpriority: medium
ms.openlocfilehash: f0aa60061ae9fc14392bfe4beb0693ba50fda0df
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8947338"
---
# <a name="how-to-resume-an-app-directx-and-c"></a>如何恢复应用（DirectX 和 C++）



本主题介绍了系统在恢复通用 Windows 平台 (UWP) DirectX 应用时，如何还原重要的应用程序数据。

## <a name="register-the-resuming-event-handler"></a>注册恢复事件处理程序


注册以处理 [**CoreApplication::Resuming**](https://msdn.microsoft.com/library/windows/apps/br205859) 事件， 该事件指示用户从你的应用切换离开，而后又切换回你的应用。

将此代码添加到你的视图提供程序的 [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 方法的实现中：

```cpp
// The first method is called when the IFrameworkView is being created.
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Resuming +=
        ref new EventHandler<Platform::Object^>(this, &App::OnResuming);
    
  //...

}
```

## <a name="refresh-displayed-content-after-suspension"></a>暂停之后刷新显示的内容


当你的应用处理 Resuming 事件时，它将有机会刷新其显示的内容。 还原你已使用你的 [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 处理程序保存的所有应用，然后重新启动处理。 游戏开发人员：如果你已暂停你的音频引擎，那么现在该重新启动它了。

```cpp
void App::OnResuming(Platform::Object^ sender, Platform::Object^ args)
{
    // Restore any data or state that was unloaded on suspend. By default, data
    // and state are persisted when resuming from suspend. Note that this event
    // does not occur if the app was previously terminated.

    // Insert your code here.
}
```

对于应用的 [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)，此回调是由 [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) 处理的一条事件消息。 如果你没有从你的应用的主回路调用 [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215)（在你的查看提供程序的 [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) 方法中实现），那么将不会调用此回调。

``` syntax
// This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

## <a name="remarks"></a>备注


每当用户切换到桌面或其他应用时，系统都会挂起你的应用。 每当用户切回到你的应用时，系统就会恢复你的应用。 当系统恢复你的应用时，你的变量和数据结构的内容与系统将你的应用暂停之前的内容相同。 系统会将你的应用完全恢复到你离开时的状态，使用户感觉你的应用好像一直在后台运行一样。 但是，应用可能已暂停很长一段时间，因此，它应当刷新在应用暂停之后可能已发生更改的任何显示内容，并且重新启动任何呈现或音频处理线程。 如果你已在上一个暂停事件期间保存任何游戏状态数据，现在请还原它。

## <a name="related-topics"></a>相关主题

* [如何暂停应用（DirectX 和 C++）](how-to-suspend-an-app-directx-and-cpp.md)
* [如何激活应用（DirectX 和 C++）](how-to-activate-an-app-directx-and-cpp.md)

 

 




