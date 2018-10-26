---
author: mtoepke
title: DirectX 和 XAML 互操作
description: 你可以在通用 Windows 平台 (UWP) 游戏中同时使用 Extensible Application Markup Language (XAML) 和 Microsoft DirectX。
ms.assetid: 0fb2819a-61ed-129d-6564-0b67debf5c6b
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx, xaml 互操作
ms.localizationpriority: medium
ms.openlocfilehash: 7f3a70be3dd31b0a5e4214621ab9fb4efa72cc54
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/26/2018
ms.locfileid: "5555177"
---
# <a name="directx-and-xaml-interop"></a>DirectX 和 XAML 互操作



你可以在通用 Windows 平台 (UWP) 游戏或应用中同时使用 Extensible Application Markup Language (XAML) 和 Microsoft DirectX。 XAML 和 DirectX 的结合可让你生成与 DirectX 呈现的内容互操作的灵活用户界面框架，并且对于图形密集型应用非常有用。 本主题将说明使用 DirectX 的 UWP 应用的结构，并指出在生成要与 DirectX 兼容的 UWP 应用时要使用的重要类型。

如果你的应用主要侧重于 2D 呈现，你可能需要使用 [Win2D](https://github.com/microsoft/win2d) Windows 运行时库。 此库由 Microsoft 维护，并且基于核心 Direct2D 技术生成。 它大大简化了实现 2D 图形的使用模式，并包括对本文档中所述的某些技术的有用抽象。 有关更多详细信息，请参阅项目页面。 本文档介绍有关选择*不*使用 Win2D 的应用开发人员的指南。

> **注意**DirectX Api 没有定义为 Windows 运行时类型，所以通常使用 VisualC + + 组件扩展 (C + + CX) 来开发可与 DirectX 互操作的 XAML UWP 组件。 此外，如果将 DirectX 调用包装在一个独立的 Windows 运行时元数据文件中，可以创建一个使用 DirectX 的 C# 和 XAML UWP 应用。

 

## <a name="xaml-and-directx"></a>XAML 和 DirectX

DirectX 提供了两个分别针对 2D 和 3D 图形的强大库：Direct2D 和 Microsoft Direct3D。 尽管 XAML 提供了对基本 2D 基元和效果的支持，但许多应用（例如建模和游戏应用）需要更复杂的图形支持。 对于这些应用，可以使用 Direct2D 和 Direct3D 呈现部分或全部图形，而使用 XAML 呈现所有其他内容。

如果要实现自定义 XAML 和 DirectX 互操作，需要知道以下两个概念：

-   共享图面是由 XAML（可以使用 DirectX 间接绘入）使用 [Windows::UI::Xaml::Media::ImageSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.media.imagesource.aspx) 类型定义的调整大小的显示区域。 对于共享图面，不对新内容显示在屏幕上的情形控制精确计时。 而将对共享图面的更新同步到 XAML 框架的更新。
-   [交换链](https://msdn.microsoft.com/library/windows/desktop/bb206356(v=vs.85).aspx)表示用于以最小延迟显示图形的缓冲区集合。 通常，交换链独立于 UI 线程以每秒 60 帧的速度进行更新。 但是，交换链会使用更多的内存和 CPU 资源来支持快速更新，并且更难以使用，因为你必须管理多个线程。

考虑 DirectX 的用途。 它是否用于合成适合显示窗口维度的单一控件或对其进行动画处理？ 它是否包含像游戏中那样需要呈现和实时控制的输出？ 如果是，你可能需要实现一个交换链。 否则，可以使用共享图面。

在确定希望使用 DirectX 的方式之后，即可使用这些 Windows 运行时类型之一将 DirectX 呈现合并到 UWP 应用中：

-   如果希望创作静态图像，或以事件驱动的间隔绘制复杂图像，可使用 [Windows::UI::Xaml::Media::Imaging::SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 绘制到共享图面。 此类型处理特定大小的 DirectX 绘图图面。 通常，在以位图形式创作图像或纹理来在一个文档或 UI 元素中显示时，可以使用此类型。 它不太适合实时交互，例如高性能游戏。 这是因为 **SurfaceImageSource** 对象的更新会同步到 XAML 用户界面更新，而且这可能导致你提供给用户的视觉反馈出现延迟，例如起伏不定的帧速率或实时输入时能感觉到的迟缓响应。 不过，更新对动态控制或数据模拟而言已经足够快了！

-   如果图像比所提供的屏幕空间要大，并且可由用户平移或缩放，则使用 [Windows::UI::Xaml::Media::Imaging::VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050)。 此类型处理一个比屏幕更大的特定大小 DirectX 绘图图面。 像 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 一样，你可以在动态创作复杂图像或控件时使用它。 而且，也像 **SurfaceImageSource** 一样，它不太适合高性能游戏。 可使用 **VirtualSurfaceImageSource** 的一些 XAML 元素示例包括地图控件，或者大型的、包含大量图像的文档查看器。

-   如果你使用 DirectX 提供实时更新的图形，或者在必须以低延迟的定期间隔更新的情况下，可以使用 [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 类，这样你无需同步到 XAML 框架刷新计时器即可刷新图形。 此类型使你能够直接访问图形设备的交换链 ([IDXGISwapChain1](https://msdn.microsoft.com/library/windows/desktop/hh404631))，并将 XAML 放在呈现目标之上。 此类型很适合需要基于 XAML 的用户界面的游戏和全屏 DirectX 应用。 若要使用此方法，你必须熟悉 DirectX，包括 Microsoft DirectX 图形基础结构 (DXGI)、Direct2D 和 Direct3D 技术。 有关详细信息，请参阅 [Direct3D 11 编程指南](https://msdn.microsoft.com/library/windows/desktop/ff476345)。

## <a name="surfaceimagesource"></a>SurfaceImageSource


[SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 提供要绘入的 DirectX 共享图面，然后将位编写到应用内容中。

下面是在代码隐藏文件中创建和更新 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 对象的基本过程：

1.  通过将高度和宽度传递到 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 构造函数定义共享图面的大小。 还可以指示图面是否需要 alpha（透明度）支持。

    例如：

    `SurfaceImageSource^ surfaceImageSource = ref new SurfaceImageSource(400, 300);`

2.  获取 [ISurfaceImageSourceNativeWithD2D](https://msdn.microsoft.com/library/windows/desktop/dn302137) 的指针。 将 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 对象强制转换为 [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821)（或 **IUnknown**），并对它调用 **QueryInterface** 以获取基础 **ISurfaceImageSourceNativeWithD2D** 实现。 你使用在此实现上定义的方法设置设备并运行绘图操作。

    ```cpp
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* sisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(surfaceImageSource);

    sisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **)&m_sisNativeWithD2D);
    ```

3.  首先调用 [D3D11CreateDevice](https://msdn.microsoft.com/library/windows/desktop/ff476082) 和 [D2D1CreateDevice](https://msdn.microsoft.com/library/windows/desktop/hh404272(v=vs.85).aspx)，然后将设备和文本传递到 [ISurfaceImageSourceNativeWithD2D::SetDevice](https://msdn.microsoft.com/library/dn302141.aspx)，以创建 DXGI 和 D2D 设备。 

    > [!NOTE]
    > 如果将从后台线程绘制到 **SurfaceImageSource**，则你也需要确保 DXGI 设备已启用多线程访问。 仅可在出于性能原因需要从后台线程进行绘制时才可进行此操作。

    例如：

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_sisNativeWithD2D->SetDevice(m_d2dDevice.Get());
    ```

4.  将 [ID2D1DeviceContext](https://msdn.microsoft.com/library/windows/desktop/bb174565) 对象的指针提供给 [ISurfaceImageSourceNativeWithD2D::BeginDraw](https://msdn.microsoft.com/library/dn302138.aspx)，然后使用返回的绘制上下文在 **SurfaceImageSource** 内绘制所需的矩形内容。 可从后台线程调用 **ISurfaceImageSourceNativeWithD2D::BeginDraw** 和绘制命令。 仅绘制在 *updateRect* 参数中指定的更新区域。

    此方法在 *offset* 参数中返回更新的目标矩形的点 (x,y) 偏移。 你可以使用此偏移来确定使用 **ID2D1DeviceContext** 绘制更新内容的位置。

    ```cpp
    Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

    HRESULT beginDrawHR = m_sisNative->BeginDraw(
        updateRect, 
        &drawingContext, 
        &offset);

    if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
        || beginDrawHR == DXGI_ERROR_DEVICE_RESET
        || beginDrawHR == D2DERR_RECREATE_TARGET)
    {
        // The D3D and D2D device was lost and need to be re-created.
        // Recovery steps are:
        // 1) Re-create the D3D and D2D devices
        // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the new D2D
        //    device
        // 3) Redraw the contents of the SurfaceImageSource
    }
    else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
    {
        // The devices were not lost but the entire contents of the surface
        // were. Recovery steps are:
        // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the D2D 
        //    device again
        // 2) Redraw the entire contents of the SurfaceImageSource
    }
    else 
    {
        // Draw your updated rectangle with the drawingContext
    }
    ```

5. 调用 [ISurfaceImageSourceNativeWithD2D::EndDraw](https://msdn.microsoft.com/library/dn302139.aspx) 完成此位图。 位图可用作 XAML [Image](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.image.aspx) 或 [ImageBrush](https://msdn.microsoft.com/library/windows/apps/br210101) 的源。 仅可从 UI 线程调用 **ISurfaceImageSourceNativeWithD2D::EndDraw**。

    ```cpp
    m_sisNative->EndDraw();

    // ...
    // The SurfaceImageSource object's underlying 
    // ISurfaceImageSourceNativeWithD2D object contains the completed bitmap.

    ImageBrush^ brush = ref new ImageBrush();
    brush->ImageSource = surfaceImageSource;
    ```

    > [!NOTE]
    > 当前调用 [SurfaceImageSource::SetSource](https://msdn.microsoft.com/library/windows/apps/br243255)（从 **IBitmapSource::SetSource** 继承）将引发异常。 不要从你的 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041) 对象调用它。

    > [!NOTE]
    > 当关联的 [Window](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 隐藏时，应用必须避免绘制到 **SurfaceImageSource**，否则，**ISurfaceImageSourceNativeWithD2D** API 将会失败。 为实现此目的，请为 [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) 事件注册为事件侦听器，以跟踪可见性变化。

## <a name="virtualsurfaceimagesource"></a>VirtualSurfaceImageSource

[VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 在内容可能比可用屏幕大时扩展 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041)，使内容可以获得最佳的呈现效果。

[VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 不同于 [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041)，因为它使用了一个回调 [IVirtualSurfaceImageSourceCallbacksNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848337)，你可以实现该回调，当图面区域在屏幕上可见时更新该区域。 你不需要清除隐藏的区域，因为 XAML 框架会负责此事。

这是在代码隐藏文件中创建和更新 [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 对象的基本过程：

1.  创建一个具有所需大小的 [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 实例。 例如：

    ```cpp
    VirtualSurfaceImageSource^ virtualSIS = 
        ref new VirtualSurfaceImageSource(2000, 2000);
    ```

2.  获取 [IVirtualSurfaceImageSourceNative](https://msdn.microsoft.com/library/windows/desktop/hh848328) 和 [ISurfaceImageSourceNativeWithD2D](https://msdn.microsoft.com/library/windows/desktop/dn302137) 的指针。 将 [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 对象强制转换为 [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821) 或 [IUnknown](https://msdn.microsoft.com/library/windows/desktop/ms680509)，并对它调用 [QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521) 以获取基础的 **IVirtualSurfaceImageSourceNative** 和 **ISurfaceImageSourceNativeWithD2D** 实现。 你使用在这些实现上定义的方法设置设备并运行绘图操作。

    ```cpp
    Microsoft::WRL::ComPtr<IVirtualSurfaceImageSourceNative>  m_vsisNative;
    Microsoft::WRL::ComPtr<ISurfaceImageSourceNativeWithD2D> m_sisNativeWithD2D;

    // ...

    IInspectable* vsisInspectable = 
        (IInspectable*) reinterpret_cast<IInspectable*>(virtualSIS);

    vsisInspectable->QueryInterface(
        __uuidof(IVirtualSurfaceImageSourceNative), 
        (void **) &m_vsisNative);

    vsisInspectable->QueryInterface(
        __uuidof(ISurfaceImageSourceNativeWithD2D), 
        (void **) &m_sisNativeWithD2D);
    ```

3.  首先调用 **D3D11CreateDevice** 和 **D2D1CreateDevice**，然后将 D2D 设备传递给 **ISurfaceImageSourceNativeWithD2D::SetDevice**，以创建 DXGI 和 D2D 设备。

    > [!NOTE]
    > 如果将从后台线程绘制到 **VirtualSurfaceImageSource**，则你也需要确保 DXGI 设备已启用多线程访问。 仅可在出于性能原因需要从后台线程进行绘制时才可进行此操作。

    例如：

    ```cpp
    Microsoft::WRL::ComPtr<ID3D11Device> m_d3dDevice;
    Microsoft::WRL::ComPtr<ID3D11DeviceContext> m_d3dContext;
    Microsoft::WRL::ComPtr<ID2D1Device> m_d2dDevice;

    // Create the DXGI device
    D3D11CreateDevice(
            NULL,
            D3D_DRIVER_TYPE_HARDWARE,
            NULL,
            flags,
            featureLevels,
            ARRAYSIZE(featureLevels),
            D3D11_SDK_VERSION,
            &m_d3dDevice,
            NULL,
            &m_d3dContext);  

    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);
    
    // To enable multi-threaded access (optional)
    Microsoft::WRL::ComPtr<ID3D10Multithread> d3dMultiThread;

    m_d3dDevice->QueryInterface(
        __uuidof(ID3D10Multithread), 
        (void **) &d3dMultiThread);

    d3dMultiThread->SetMultithreadProtected(TRUE);

    // Create the D2D device
    D2D1CreateDevice(m_dxgiDevice.Get(), NULL, &m_d2dDevice);

    // Set the D2D device
    m_vsisNativeWithD2D->SetDevice(m_d2dDevice.Get());

    m_vsisNative->SetDevice(dxgiDevice.Get());
    ```

4.  调用 [IVirtualSurfaceImageSourceNative::RegisterForUpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848334)，传入对 [IVirtualSurfaceUpdatesCallbackNative](https://msdn.microsoft.com/library/windows/desktop/hh848336) 实现的引用。

    ```cpp
    class MyContentImageSource : public IVirtualSurfaceUpdatesCallbackNative
    {
    // ...
      private:
         virtual HRESULT STDMETHODCALLTYPE UpdatesNeeded() override;
    }

    // ...

    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
      // .. Perform drawing here ...
    }

    void MyContentImageSource::Initialize()
    {
      // ...
      m_vsisNative->RegisterForUpdatesNeeded(this);
      // ...
    }
    ```

    框架在一个 [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050) 区域需要更新时调用你的 [IVirtualSurfaceUpdatesCallbackNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848334) 实现。

    在两个时刻可发生此情况：当框架确定该区域需要绘制时（例如当用户平移或缩放图面的视图时），或者在应用在该区域上调用 [IVirtualSurfaceImageSourceNative::Invalidate](https://msdn.microsoft.com/library/windows/desktop/hh848332) 时。

5.  在 [IVirtualSurfaceImageSourceNative::UpdatesNeeded](https://msdn.microsoft.com/library/windows/desktop/hh848337) 中，使用 [IVirtualSurfaceImageSourceNative::GetUpdateRectCount](https://msdn.microsoft.com/library/windows/desktop/hh848329) 和 [IVirtualSurfaceImageSourceNative::GetUpdateRects](https://msdn.microsoft.com/library/windows/desktop/hh848330) 方法确定必须绘制图面的哪些区域。

    ```cpp
    HRESULT STDMETHODCALLTYPE MyContentImageSource::UpdatesNeeded()
    {
        HRESULT hr = S_OK;

        try
        {
            ULONG drawingBoundsCount = 0;  
            m_vsisNative->GetUpdateRectCount(&drawingBoundsCount);

            std::unique_ptr<RECT[]> drawingBounds(
                new RECT[drawingBoundsCount]);

            m_vsisNative->GetUpdateRects(
                drawingBounds.get(), 
                drawingBoundsCount);
            
            for (ULONG i = 0; i < drawingBoundsCount; ++i)
            {
                // Drawing code here ...
            }
        }
        catch (Platform::Exception^ exception)
        {
            hr = exception->HResult;
        }

        return hr;
    }
    ```

6.  最后，对于必须进行更新的每个区域：

    1.  将 **ID2D1DeviceContext** 对象的指针提供给 **ISurfaceImageSourceNativeWithD2D::BeginDraw**，然后使用返回的绘制上下文在 **SurfaceImageSource** 内绘制所需的矩形内容。 可从后台线程调用 **ISurfaceImageSourceNativeWithD2D::BeginDraw** 和绘制命令。 仅绘制在 *updateRect* 参数中指定的更新区域。

        此方法在 *offset* 参数中返回更新的目标矩形的点 (x,y) 偏移。 你可以使用此偏移来确定使用 **ID2D1DeviceContext** 绘制更新内容的位置。

        ```cpp
        Microsoft::WRL::ComPtr<ID2D1DeviceContext> drawingContext;

        HRESULT beginDrawHR = m_sisNative->BeginDraw(
            updateRect, 
            &drawingContext, 
            &offset);

        if (beginDrawHR == DXGI_ERROR_DEVICE_REMOVED 
            || beginDrawHR == DXGI_ERROR_DEVICE_RESET 
            || beginDrawHR == D2DERR_RECREATE_TARGET)
        {
            // The D3D and D2D devices were lost and need to be re-created.
            // Recovery steps are:
            // 1) Re-create the D3D and D2D devices
            // 2) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    new D2D device
            // 3) Redraw the contents of the VirtualSurfaceImageSource
        }
        else if (beginDrawHR == E_SURFACE_CONTENTS_LOST)
        {
            // The devices were not lost but the entire contents of the 
            // surface were lost. Recovery steps are:
            // 1) Call ISurfaceImageSourceNativeWithD2D::SetDevice with the 
            //    D2D device again
            // 2) Redraw the entire contents of the VirtualSurfaceImageSource
        }
        else
        {
            // Draw your updated rectangle with the drawingContext
        }
        ```

    2.  将特定内容绘制到该区域，但是将你的图形限制在有限的区域以提高性能。

    3.  调用 **ISurfaceImageSourceNativeWithD2D::EndDraw**。 结果是位图。

> [!NOTE]
> 当关联的 [Window](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) 隐藏时，应用必须避免绘制到 **SurfaceImageSource**，否则，**ISurfaceImageSourceNativeWithD2D** API 将会失败。 为实现此目的，请将 [Window.VisibilityChanged](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.VisibilityChanged) 事件注册为事件侦听器，以跟踪可见性变化。

## <a name="swapchainpanel-and-gaming"></a>SwapChainPanel 和游戏


[SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 是用于支持高性能图形和游戏的 Windows 运行时类型，你可以在其中直接管理交换链。 在此情况下，你可以创建自己的 DirectX 交换链并管理所呈现内容的显示。

若要确保良好的性能，[SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 类型有一些限制：

-   每个应用的 [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 实例不超过 4 个。
-   你应该将 DirectX 交换链的高度和宽度（在 [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528) 中）设置为交换链元素的当前尺寸。 如果不这么做，将缩放显示内容（使用 **DXGI\_SCALING\_STRETCH**）以适合它。
-   必须将 DirectX 交换链的缩放模式（在 [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528) 中）设置为 **DXGI\_SCALING\_STRETCH**。
-   不能将 DirectX 交换链的 alpha 模式（在 [DXGI\_SWAP\_CHAIN\_DESC1](https://msdn.microsoft.com/library/windows/desktop/hh404528) 中）设置为 **DXGI\_ALPHA\_MODE\_PREMULTIPLIED**。
-   必须调用 [IDXGIFactory2::CreateSwapChainForComposition](https://msdn.microsoft.com/library/windows/desktop/hh404558) 来创建 DirectX 交换链。

你基于应用的需求来更新 [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834)，而不是 XAML 框架的更新。 如果需要将 **SwapChainPanel** 的更新与 XAML 框架的更新同步，可以注册 [Windows::UI::Xaml::Media::CompositionTarget::Rendering](https://msdn.microsoft.com/library/windows/apps/br228127) 事件。 否则，如果尝试通过与更新 **SwapChainPanel** 的线程不同的线程更新 XAML 元素，则必须考虑任何跨线程问题。

如果你需要接收 **SwapChainPanel** 的低延迟指针输入，请使用 [SwapChainPanel::CreateCoreIndependentInputSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.swapchainpanel.createcoreindependentinputsource)。 此方法将返回 [CoreIndependentInputSource](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coreindependentinputsource) 对象，该对象可用于在后台线程上以最小延迟接收输入事件。 请注意，在调用此方法后，将不会对 **SwapChainPanel** 引发正常 XAML 指针输入事件，因为所有输入都将重定向到后台线程。


> **注意**   通常，你的 DirectX 应用应当在横向创建交换链，并等于显示窗口大小（这在大多数 Microsoft Store 游戏中通常为本机屏幕分辨率）。 这可确保你的应用在没有任何可见 XAML 覆盖时使用最佳交换链实现。 如果应用旋转到纵向模式，你的应用应在现有交换链上调用 [IDXGISwapChain1::SetRotation](https://msdn.microsoft.com/library/windows/desktop/hh446801)，根据需要应用内容转换，然后在同一交换链上再次调用 [SetSwapChain](https://msdn.microsoft.com/library/windows/desktop/dn302144)。 类似地，每当通过调用 [IDXGISwapChain::ResizeBuffers](https://msdn.microsoft.com/library/windows/desktop/bb174577) 调整了交换链的大小时，你的应用都应当再次在同一交换链上调用**SetSwapChain**。


 

这是在代码隐藏文件中创建和更新 [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 对象的基本过程。

1.  获取应用的交换链面板的实例。 这些实例在 XAML 中使用 `<SwapChainPanel>` 标记表示。

    `Windows::UI::Xaml::Controls::SwapChainPanel^ swapChainPanel;`

    下面是一个示例 `<SwapChainPanel>` 标记。

    ```xml
    <SwapChainPanel x:Name="swapChainPanel">
        <SwapChainPanel.ColumnDefinitions>
            <ColumnDefinition Width="300*"/>
            <ColumnDefinition Width="1069*"/>
        </SwapChainPanel.ColumnDefinitions>
    …
    ```

2.  获取 [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143) 的指针。 将 [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834) 对象强制转换为 [IInspectable](https://msdn.microsoft.com/library/windows/desktop/br205821)（或 **IUnknown**），并对它调用 **QueryInterface** 以获取基础 **ISwapChainPanelNative** 实现。

    ```cpp
    Microsoft::WRL::ComPtr<ISwapChainPanelNative> m_swapChainNative;
    // ...
    IInspectable* panelInspectable = (IInspectable*) reinterpret_cast<IInspectable*>(swapChainPanel);
    panelInspectable->QueryInterface(__uuidof(ISwapChainPanelNative), (void **)&m_swapChainNative);
    ```

3.  创建 DXGI 设备和交换链，并将交换链设置为 [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143)，方法是将它传递给 [SetSwapChain](https://msdn.microsoft.com/library/windows/desktop/dn302144)。

    ```cpp
    Microsoft::WRL::ComPtr<IDXGISwapChain1>               m_swapChain;    
    // ...
    DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};
            swapChainDesc.Width = m_bounds.Width;
            swapChainDesc.Height = m_bounds.Height;
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;           // This is the most common swapchain format.
            swapChainDesc.Stereo = false; 
            swapChainDesc.SampleDesc.Count = 1;                          // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Quality = 0;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.BufferCount = 2;
            swapChainDesc.Scaling = DXGI_SCALING_STRETCH;
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // We recommend using this swap effect for all. applications
            swapChainDesc.Flags = 0;
                    
    // QI for DXGI device
    Microsoft::WRL::ComPtr<IDXGIDevice> dxgiDevice;
    m_d3dDevice.As(&dxgiDevice);

    // Get the DXGI adapter.
    Microsoft::WRL::ComPtr<IDXGIAdapter> dxgiAdapter;
    dxgiDevice->GetAdapter(&dxgiAdapter);

    // Get the DXGI factory.
    Microsoft::WRL::ComPtr<IDXGIFactory2> dxgiFactory;
    dxgiAdapter->GetParent(__uuidof(IDXGIFactory2), &dxgiFactory);
    // Create a swap chain by calling CreateSwapChainForComposition.
    dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,        // Allow on any display. 
                &m_swapChain
                );
            
    m_swapChainNative->SetSwapChain(m_swapChain.Get());
    ```

4.  绘制到 DirectX 交换链，并提供它来显示内容。

    ```cpp
    HRESULT hr = m_swapChain->Present(1, 0);
    ```

    在 Windows 运行时布局/呈现逻辑指示更新时，刷新 XAML 元素。

## <a name="related-topics"></a>相关主题

* [Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm)
* [SurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702041)
* [VirtualSurfaceImageSource](https://msdn.microsoft.com/library/windows/apps/hh702050)
* [SwapChainPanel](https://msdn.microsoft.com/library/windows/apps/dn252834)
* [ISwapChainPanelNative](https://msdn.microsoft.com/library/windows/desktop/dn302143)
* [Direct3D 11 编程指南](https://msdn.microsoft.com/library/windows/desktop/ff476345)

 

 




