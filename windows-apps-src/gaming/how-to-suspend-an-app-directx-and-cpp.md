---
title: 如何暂停应用（DirectX 和 C++）
description: 本主题介绍了系统在暂停通用 Windows 平台 (UWP) DirectX 应用时，如何保存重要系统状态和应用数据。
ms.assetid: 5dd435e5-ec7e-9445-fed4-9c0d872a239e
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 暂停, directx
ms.localizationpriority: medium
ms.openlocfilehash: 0b588d6bf6e7cbf43651d94a7fd46e9a767c6f09
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8740302"
---
# <a name="how-to-suspend-an-app-directx-and-c"></a>如何暂停应用（DirectX 和 C++）



本主题介绍了系统在暂停通用 Windows 平台 (UWP) DirectX 应用时，如何保存重要系统状态和应用数据。

## <a name="register-the-suspending-event-handler"></a>注册暂停事件处理程序


首先，注册以处理 [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 事件，当某个用户或系统操作将你的应用移动到某个已暂停状态时， 会发生此事件。

将此代码添加到你的视图提供程序 的 [**IFrameworkView::Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) 方法的实现中：

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
  //...
  
    CoreApplication::Suspending +=
        ref new EventHandler<SuspendingEventArgs^>(this, &App::OnSuspending);

  //...
}
```

## <a name="save-any-app-data-before-suspending"></a>在暂停之前保存所有应用数据


当你的应用处理 [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 事件时，它将有机会将其重要的应用程序数据保存到处理程序函数中。 应用应该使用 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 存储 API 来同步保存简单的应用程序数据。 如果你要开发一个游戏，请保存所有关键游戏状态信息。 请不要忘记去挂起音频处理！

现在，请实施回调。 将应用数据保存在此方法中。

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}
```

此回调必须在 5 秒内完成。 在此回调期间，你必须通过调用 [**SuspendingOperation::GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) 来请求一个延迟，这会启动递减计数。 当你的应用完成保存操作之后，请调用 [**SuspendingDeferral::Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 来告知系统你的应用现在已准备好进行挂起。 如果你不请求延迟，或者如果你的应用保存数据所花费的时间超过 5 秒，则会自动暂停你的应用。

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

## <a name="call-trim"></a>调用 Trim()


从 Windows8.1 开始，所有 DirectX UWP 应用时必须都调用[**idxgidevice3:: Trim**](https://msdn.microsoft.com/library/windows/desktop/dn280346)暂停。 此调用指示图形驱动程序释放为该应用分配的所有临时缓冲区，这样可以减少应用在暂停状态时被终止以回收内存资源的情况。 这是 Windows8.1 认证要求。

```cpp
void App::OnSuspending(Platform::Object^ sender, SuspendingEventArgs^ args)
{
    // Save app state asynchronously after requesting a deferral. Holding a deferral
    // indicates that the application is busy performing suspending operations. Be
    // aware that a deferral may not be held indefinitely. After about five seconds,
    // the app will be forced to exit.
    SuspendingDeferral^ deferral = args->SuspendingOperation->GetDeferral();

    create_task([this, deferral]()
    {
        m_deviceResources->Trim();

        // Insert your code here.

        deferral->Complete();
    });
}

// Call this method when the app suspends. It provides a hint to the driver that the app 
// is entering an idle state and that temporary buffers can be reclaimed for use by other apps.
void DX::DeviceResources::Trim()
{
    ComPtr<IDXGIDevice3> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    dxgiDevice->Trim();
}
```

## <a name="release-any-exclusive-resources-and-file-handles"></a>释放所有独占资源和文件句柄


当你的应用处理 [**CoreApplication::Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860) 事件时，它还将有机会释放独占资源和文件句柄。 显式释放独占资源和文件句柄有助于确保：你的应用未使用它们时其他应用可以访问它们。 当在终止后又激活应用时，它应该打开其独占资源和文件句柄。

## <a name="remarks"></a>备注


每当用户切换到桌面或其他应用时，系统都会挂起你的应用。 每当用户切回到你的应用时，系统就会恢复你的应用。 当系统恢复你的应用时，你的变量和数据结构的内容与系统将你的应用暂停之前的内容相同。 系统会将你的应用完全恢复到你离开时的状态，使用户感觉你的应用好像一直在后台运行一样。

当你的应用被暂停后，系统会尝试将你的应用及其数据保留在内存中。 但是，如果系统没有资源将你的应用保存在内存里，则将终止你的应用。 当用户切换回已终止的暂停应用时，系统会发送 [**Activated**](https://msdn.microsoft.com/library/windows/apps/br225018) 事件，且应该在其处理程序中为 **CoreApplicationView::Activated** 事件还原其应用程序数据。

当终止应用时系统不会通知应用，因此当暂停应用时，你的应用必须保存其应用程序数据并释放独占资源和文件句柄，并且在终止后又激活应用时还原这些内容。

## <a name="related-topics"></a>相关主题

* [如何恢复应用（DirectX 和 C++）](how-to-resume-an-app-directx-and-cpp.md)
* [如何激活应用（DirectX 和 C++）](how-to-activate-an-app-directx-and-cpp.md)

 

 




