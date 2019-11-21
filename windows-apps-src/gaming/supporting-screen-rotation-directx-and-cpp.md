---
title: 支持屏幕方向（DirectX 和 C++）
description: 在这里，我们将讨论在 UWP DirectX 应用中处理屏幕旋转的最佳实践，以便有效有效地使用 Windows 10 设备的图形硬件。
ms.assetid: f23818a6-e372-735d-912b-89cabeddb6d4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 屏幕方向, directx
ms.localizationpriority: medium
ms.openlocfilehash: 5f6f50abeae643cccca2a23a4b3c20dc698d200e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258514"
---
# <a name="supporting-screen-orientation-directx-and-c"></a>支持屏幕方向（DirectX 和 C++）



通用 Windows 平台 (UWP) 应用可以在你处理 [**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 事件时支持多个屏幕方向。 在这里，我们将讨论在 UWP DirectX 应用中处理屏幕旋转的最佳实践，以便有效有效地使用 Windows 10 设备的图形硬件。

在开始之前，请记住图形硬件始终以同样的方式输出像素数据，而不管设备方向如何。 Windows 10 设备可以确定其当前显示方向（使用某种传感器或通过软件切换），并允许用户更改显示设置。 因此，Windows 10 自行处理图像的旋转，以确保它们根据设备的方向 "直立"。 默认情况下，你的应用会收到关于某些项目（例如，窗口大小）在方向上已更改的通知。 发生这种情况时，Windows 10 会立即为最终显示旋转图像。 对于四个特定屏幕方向的三个（稍后讨论），Windows 10 使用其他图形资源和计算来显示最终图像。

对于使用 DirectX 应用，[**DisplayInformation**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayInformation) 对象会提供你的应用可以查询的基本屏幕方向数据。 默认方向为“横向”，其中屏幕的像素宽度大于高度；替代方向为“纵向”， 其中屏幕会在任一方向上旋转 90 度，且宽度会变得小于高度。

Windows 10 定义了四种特定显示方向模式：

-   横向： Windows 10 的默认显示方向，被视为旋转的基本或恒等角度（0度）。
-   纵向 - 已将屏幕顺时针旋转 90 度（或逆时间旋转 270 度）。
-   横向(翻转) - 已将屏幕旋转 180 度（上下颠倒）。
-   纵向(翻转) - 已将显示顺时针旋转 90 度（或逆时间旋转 270 度）。

当显示从一个方向旋转到另一个方向时，Windows 10 会内部执行旋转操作以将绘制的图像与新方向对齐，并且用户会在屏幕上看到一个垂直图像。

此外，Windows 10 还会显示自动转换动画，以在从一个方向切换到另一个方向时，创建流畅的用户体验。 在屏幕方向转移时，用户会将这些转移看作显示的屏幕图像的一个固定缩放和旋转动画。 时间由 Windows 10 分配给应用以在新方向上进行布局。

总之，这是处理屏幕方向中的更改的一般过程：

1.  使用窗口边界值和屏幕方向数据的组合来保持交换链与设备的本机屏幕方向对齐。
2.  使用[**IDXGISwapChain1：： SetRotation**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation)向 Windows 10 通知交换链的方向。
3.  更改呈现代码以生成与设备的用户方向对齐的图像。

## <a name="resizing-the-swap-chain-and-pre-rotating-its-contents"></a>调整交换链的大小和预旋转其内容


若要在你的 UWP DirectX 应用中执行基本显示调整大小和预旋转其内容，请实现以下步骤：

1.  处理 [**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 事件。
2.  将交换链的大小调整为窗口的新尺寸。
3.  调用 [**IDXGISwapChain1::SetRotation**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) 来设置交换链的方向。
4.  重新创建任何与窗口大小相关的资源，如你的呈现目标和其他像素数据缓冲区。

现在，让我们更详细地查看这些步骤。

你的第一步是为 [**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 事件注册一个处理程序。 每次屏幕方向改变时（例如当旋转屏幕时），在你的应用上会引发此事件。

为了处理 [**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 事件，你使用必需的SetWindow[**方法为**DisplayInformation::OrientationChanged](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow) 连接你的处理程序，该方法是 你的视图提供程序必须实现的 [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) 接口的方法之一。

在该代码示例中，[**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 的事件处理器是称为 **OnOrientationChanged** 的方法。 引发 **DisplayInformation::OrientationChanged** 时，它会依次调用称为 **SetCurrentOrientation** 的 方法，然后调用 **CreateWindowSizeDependentResources**。

```cpp
void App::SetWindow(CoreWindow^ window)
{
  // ... Other UI event handlers assigned here ...
  
    currentDisplayInformation->OrientationChanged +=
        ref new TypedEventHandler<DisplayInformation^, Object^>(this, &App::OnOrientationChanged);

  // ...
}
}
```

```cpp
void App::OnOrientationChanged(DisplayInformation^ sender, Object^ args)
{
    m_deviceResources->SetCurrentOrientation(sender->CurrentOrientation);
    m_main->CreateWindowSizeDependentResources();
}

// This method is called in the event handler for the OrientationChanged event.
void DX::DeviceResources::SetCurrentOrientation(DisplayOrientations currentOrientation)
{
    if (m_currentOrientation != currentOrientation)
    {
        m_currentOrientation = currentOrientation;
        CreateWindowSizeDependentResources();
    }
}
```

接下来，为新屏幕方向调整交换链的大小并将其准备好以便在执行呈现时旋转图形管道的内容。 在此示例中，**DirectXBase::CreateWindowSizeDependentResources** 是用于处理以下操作的方法：调用 IDXGISwapChain::ResizeBuffers、设置 3D 和 2D 旋转矩阵、调用 SetRotation 以及重新创建你的资源。

```cpp
void DX::DeviceResources::CreateWindowSizeDependentResources() 
{
    // Clear the previous window size specific context.
    ID3D11RenderTargetView* nullViews[] = {nullptr};
    m_d3dContext->OMSetRenderTargets(ARRAYSIZE(nullViews), nullViews, nullptr);
    m_d3dRenderTargetView = nullptr;
    m_d2dContext->SetTarget(nullptr);
    m_d2dTargetBitmap = nullptr;
    m_d3dDepthStencilView = nullptr;
    m_d3dContext->Flush();

    // Calculate the necessary render target size in pixels.
    m_outputSize.Width = DX::ConvertDipsToPixels(m_logicalSize.Width, m_dpi);
    m_outputSize.Height = DX::ConvertDipsToPixels(m_logicalSize.Height, m_dpi);
    
    // Prevent zero size DirectX content from being created.
    m_outputSize.Width = max(m_outputSize.Width, 1);
    m_outputSize.Height = max(m_outputSize.Height, 1);

    // The width and height of the swap chain must be based on the window's
    // natively-oriented width and height. If the window is not in the native
    // orientation, the dimensions must be reversed.
    DXGI_MODE_ROTATION displayRotation = ComputeDisplayRotation();

    bool swapDimensions = displayRotation == DXGI_MODE_ROTATION_ROTATE90 || displayRotation == DXGI_MODE_ROTATION_ROTATE270;
    m_d3dRenderTargetSize.Width = swapDimensions ? m_outputSize.Height : m_outputSize.Width;
    m_d3dRenderTargetSize.Height = swapDimensions ? m_outputSize.Width : m_outputSize.Height;

    if (m_swapChain != nullptr)
    {
        // If the swap chain already exists, resize it.
        HRESULT hr = m_swapChain->ResizeBuffers(
            2, // Double-buffered swap chain.
            lround(m_d3dRenderTargetSize.Width),
            lround(m_d3dRenderTargetSize.Height),
            DXGI_FORMAT_B8G8R8A8_UNORM,
            0
            );

        if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
        {
            // If the device was removed for any reason, a new device and swap chain will need to be created.
            HandleDeviceLost();

            // Everything is set up now. Do not continue execution of this method. HandleDeviceLost will reenter this method 
            // and correctly set up the new device.
            return;
        }
        else
        {
            DX::ThrowIfFailed(hr);
        }
    }
    else
    {
        // Otherwise, create a new one using the same adapter as the existing Direct3D device.
        DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

        swapChainDesc.Width = lround(m_d3dRenderTargetSize.Width); // Match the size of the window.
        swapChainDesc.Height = lround(m_d3dRenderTargetSize.Height);
        swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM; // This is the most common swap chain format.
        swapChainDesc.Stereo = false;
        swapChainDesc.SampleDesc.Count = 1; // Don't use multi-sampling.
        swapChainDesc.SampleDesc.Quality = 0;
        swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
        swapChainDesc.BufferCount = 2; // Use double-buffering to minimize latency.
        swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL; // All UWP apps must use this SwapEffect.
        swapChainDesc.Flags = 0;    
        swapChainDesc.Scaling = DXGI_SCALING_NONE;
        swapChainDesc.AlphaMode = DXGI_ALPHA_MODE_IGNORE;

        // This sequence obtains the DXGI factory that was used to create the Direct3D device above.
        ComPtr<IDXGIDevice3> dxgiDevice;
        DX::ThrowIfFailed(
            m_d3dDevice.As(&dxgiDevice)
            );

        ComPtr<IDXGIAdapter> dxgiAdapter;
        DX::ThrowIfFailed(
            dxgiDevice->GetAdapter(&dxgiAdapter)
            );

        ComPtr<IDXGIFactory2> dxgiFactory;
        DX::ThrowIfFailed(
            dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
            );

        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForCoreWindow(
                m_d3dDevice.Get(),
                reinterpret_cast<IUnknown*>(m_window.Get()),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }

    // Set the proper orientation for the swap chain, and generate 2D and
    // 3D matrix transformations for rendering to the rotated swap chain.
    // Note the rotation angle for the 2D and 3D transforms are different.
    // This is due to the difference in coordinate spaces.  Additionally,
    // the 3D matrix is specified explicitly to avoid rounding errors.

    switch (displayRotation)
    {
    case DXGI_MODE_ROTATION_IDENTITY:
        m_orientationTransform2D = Matrix3x2F::Identity();
        m_orientationTransform3D = ScreenRotation::Rotation0;
        break;

    case DXGI_MODE_ROTATION_ROTATE90:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(90.0f) *
            Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
        m_orientationTransform3D = ScreenRotation::Rotation270;
        break;

    case DXGI_MODE_ROTATION_ROTATE180:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(180.0f) *
            Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
        m_orientationTransform3D = ScreenRotation::Rotation180;
        break;

    case DXGI_MODE_ROTATION_ROTATE270:
        m_orientationTransform2D = 
            Matrix3x2F::Rotation(270.0f) *
            Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
        m_orientationTransform3D = ScreenRotation::Rotation90;
        break;

    default:
        throw ref new FailureException();
    }


    //SDM: only instance of SetRotation
    DX::ThrowIfFailed(
        m_swapChain->SetRotation(displayRotation)
        );

    // Create a render target view of the swap chain back buffer.
    ComPtr<ID3D11Texture2D> backBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
        );

    DX::ThrowIfFailed(
        m_d3dDevice->CreateRenderTargetView(
            backBuffer.Get(),
            nullptr,
            &m_d3dRenderTargetView
            )
        );

    // Create a depth stencil view for use with 3D rendering if needed.
    CD3D11_TEXTURE2D_DESC depthStencilDesc(
        DXGI_FORMAT_D24_UNORM_S8_UINT, 
        lround(m_d3dRenderTargetSize.Width),
        lround(m_d3dRenderTargetSize.Height),
        1, // This depth stencil view has only one texture.
        1, // Use a single mipmap level.
        D3D11_BIND_DEPTH_STENCIL
        );

    ComPtr<ID3D11Texture2D> depthStencil;
    DX::ThrowIfFailed(
        m_d3dDevice->CreateTexture2D(
            &depthStencilDesc,
            nullptr,
            &depthStencil
            )
        );

    CD3D11_DEPTH_STENCIL_VIEW_DESC depthStencilViewDesc(D3D11_DSV_DIMENSION_TEXTURE2D);
    DX::ThrowIfFailed(
        m_d3dDevice->CreateDepthStencilView(
            depthStencil.Get(),
            &depthStencilViewDesc,
            &m_d3dDepthStencilView
            )
        );
    
    // Set the 3D rendering viewport to target the entire window.
    m_screenViewport = CD3D11_VIEWPORT(
        0.0f,
        0.0f,
        m_d3dRenderTargetSize.Width,
        m_d3dRenderTargetSize.Height
        );

    m_d3dContext->RSSetViewports(1, &m_screenViewport);

    // Create a Direct2D target bitmap associated with the
    // swap chain back buffer and set it as the current target.
    D2D1_BITMAP_PROPERTIES1 bitmapProperties = 
        D2D1::BitmapProperties1(
            D2D1_BITMAP_OPTIONS_TARGET | D2D1_BITMAP_OPTIONS_CANNOT_DRAW,
            D2D1::PixelFormat(DXGI_FORMAT_B8G8R8A8_UNORM, D2D1_ALPHA_MODE_PREMULTIPLIED),
            m_dpi,
            m_dpi
            );

    ComPtr<IDXGISurface2> dxgiBackBuffer;
    DX::ThrowIfFailed(
        m_swapChain->GetBuffer(0, IID_PPV_ARGS(&dxgiBackBuffer))
        );

    DX::ThrowIfFailed(
        m_d2dContext->CreateBitmapFromDxgiSurface(
            dxgiBackBuffer.Get(),
            &bitmapProperties,
            &m_d2dTargetBitmap
            )
        );

    m_d2dContext->SetTarget(m_d2dTargetBitmap.Get());

    // Grayscale text anti-aliasing is recommended for all UWP apps.
    m_d2dContext->SetTextAntialiasMode(D2D1_TEXT_ANTIALIAS_MODE_GRAYSCALE);

}

```

在为下次调用此方法保存窗口的当前高度和宽度值之后，请将显示边界的与设备无关的像素 (DIP) 值转换为像素。 在该示例中，你将调用 **ConvertDipsToPixels**，它是运行此代码的一个简单函数：

` floor((dips * dpi / 96.0f) + 0.5f);`

你将添加 0.5f 以确保舍入到最近的整数值。

顺便提一句，[**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 坐标始终是使用 DIP 定义的。 对于 Windows 10 及更早版本的 Windows，DIP 定义为 1/96th 每英寸，并与操作系统的定义*对齐。* 当显示方向旋转到纵向模式之后，应用会翻转 **CoreWindow** 的宽度和高度，且呈现目标大小（边界）必须相应地改变。 因为 Direct3D 的坐标始终使用物理像素，所以你必须首先从 **CoreWindow** 的 DIP 值转换为整数像素值，然后才能将这些值传递给 Direct3D 来设置交换链。

为了过程明智起见，如果你只是调整交换链的大小，那么你所做的工作会比预想的多一点：你将实际旋转你的图像的 Direct2D 和 Direct3D 组件，然后才合成它们以进行演示，并且你将告诉交换链你已在某个新方向中呈现结果。 以下是关于此过程的更详细一点的信息，如 **DX::DeviceResources::CreateWindowSizeDependentResources** 的代码示例中所示：

-   确定屏幕的新方向。 如果显示已从横向翻转到纵向，或者反之亦然，为显示边界交换高度和宽度值，当然，是从 DIP 值更改为像素。

-   然后，请查明是否忆创建交换链。 如果尚未创建交换链，请通过调用 [**IDXGIFactory2::CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow) 创建它。 否则，请通过调用 [**IDXGISwapchain:ResizeBuffers**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers) 将现有交换链的缓冲区的大小调整为新的显示尺寸。 尽管你不需要为旋转事件调整交换链的大小（毕竟你将输出已由你的呈现管道旋转的内容），但仍存在需要调整大小的其他大小更改事件（如贴靠和填充事件）。

-   此后，将相应的 2-D 或 3-D 矩阵转换呈现给交换链时，请在图形管道中将它们分别设置为应用于像素或顶点。 我们有四种可能的旋转矩阵：

    -   横向（DXGI\_模式\_轮换\_标识）
    -   纵向（DXGI\_模式\_旋转\_ROTATE270）
    -   横向、翻转（DXGI\_模式\_旋转\_ROTATE180）
    -   纵向，翻转（DXGI\_模式\_旋转\_ROTATE90）

    根据 Windows 10 提供的数据（如[**DisplayInformation：： OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged)的结果）选择正确的矩阵，以确定显示方向，并将其乘以场景中每个像素（Direct2D）或顶点（Direct3D）的坐标，从而有效地将其旋转，使其与屏幕方向保持一致。 （请注意，在 Direct2D 中，屏幕原点被定义为左上角，而在 Direct3D 中，该原点被定义为窗口的逻辑中心）。

> **请注意**   有关用于旋转的二维转换以及如何定义它们的详细信息，请参阅为[屏幕旋转定义矩阵（2-d）](#appendix-a-applying-matrices-for-screen-rotation-2-d)。 有关用于旋转的 3-D 转换的详细信息，请参阅[为屏幕旋转定义矩阵 (3-D)](#appendix-b-applying-matrices-for-screen-rotation-3-d)。

 

很重要的一点是：调用 [**IDXGISwapChain1::SetRotation**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) 并为其提供你的更新的旋转矩阵，如下所示：

`m_swapChain->SetRotation(rotation);`

在你的呈现方法可以在选定的旋转矩阵计算新投影时获取该矩阵的情况下，你还将存储该矩阵。 当你呈现你的最终 3-D 投影或合成你的最终 2-D 布局时，你将使用此矩阵 （它不会自动为你应用它）。

此后，请为已旋转的 3-D 视图创建一个新呈现目标，并为该视图创建一个新的深度模具缓冲区。 通过调用 [**ID3D11DeviceContext:RSSetViewports**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) 为已旋转场景设置 3-D 呈现视区。

最后，如果你有要旋转或布局的 2-D 图像，请使用 [**ID2D1DeviceContext::CreateBitmapFromDxgiSurface**](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-createbitmapfromdxgisurface(idxgisurface_constd2d1_bitmap_properties1__id2d1bitmap1)) 创建一个 2-D 呈现目标作为已调整大小的交换链的可写入位图，并为更新的方向合成你的新布局。 将你需要的所有属性设置到呈现目标上，如抗锯齿模式（就像在代码示例中所看到的那样）。

现在，请演示交换链。

## <a name="reduce-the-rotation-delay-by-using-corewindowresizemanager"></a>通过使用 CoreWindowResizeManager 减少旋转延迟


默认情况下，Windows 10 为任何应用提供了一个短暂但明显的时间窗口，无论使用何种应用程序模型或语言，都可以完成图像的旋转。 但是，很可能当你的应用使用此处所述的技术之一执行旋转计算时，在此时间窗口已关闭之前，此计算将很好地完成。 你更愿意回到那个时候并完成旋转动画，是吗？ 这就是 [**CoreWindowResizeManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindowResizeManager) 出现的位置。

以下是使用 [**CoreWindowResizeManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindowResizeManager) 的方法：当引发 [**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 事件时，在该事件的处理程序内调用 [**CoreWindowResizeManager::GetForCurrentView**](https://docs.microsoft.com/previous-versions/hh404170(v=vs.85)) 以获取 **CoreWindowResizeManager** 的实例，并且当完成并演示新方向的布局时，调用 [**NotifyLayoutCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindowresizemanager.notifylayoutcompleted) 以让 Windows 知道它可以完成旋转动画并显示应用屏幕。

以下是 [**DisplayInformation::OrientationChanged**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.orientationchanged) 的事件处理程序中的代码可能的外观：

```cpp
CoreWindowResizeManager^ resizeManager = Windows::UI::Core::CoreWindowResizeManager::GetForCurrentView();

// ... build the layout for the new display orientation ...

resizeManager->NotifyLayoutCompleted();
```

当用户旋转显示方向时，Windows 10 会将独立于应用的动画显示为用户的反馈。 按以下顺序发生的动画有三个部分：

-   Windows 10 会收缩原始映像。
-   Windows 10 在重建新布局所用的时间上保留映像。 这是你要减少的时间窗口， 因为你的应用很可能并不需要全部时间窗口。
-   当布局窗口过期时，或者当收到布局完成通知时，Windows 会旋转图像，然后对新方向进行交叉淡入淡出缩放。

如第三个项目符号中所述，当应用程序调用[**NotifyLayoutCompleted**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindowresizemanager.notifylayoutcompleted)时，Windows 10 将停止超时窗口，完成旋转动画并将控制权返回给应用程序，该应用程序现在在新的显示方向上进行绘制。 总体效果是你的应用现在感觉到更流畅一点且响应更快一点，且工作效率更高一点！

## <a name="appendix-a-applying-matrices-for-screen-rotation-2-d"></a>附录 A: 应用矩阵以进行屏幕旋转 (2-D)


在[调整交换链的大小和预旋转其内容](#resizing-the-swap-chain-and-pre-rotating-its-contents)中的示例代码中（以及在 [DXGI 交换链旋转示例](https://code.msdn.microsoft.com/windowsapps/DXGI-swap-chain-rotation-21d13d71)中），你可能已注意到，对于 Direct2D 输出和 Direct3D 输出，我们有单独的旋转矩阵。 我们首先看一下 2-D 矩阵。

我们无法将相同的旋转矩阵应用到 Direct2D 和 Direct3D 内容有两个原因：

-   一个原因是，他们使用不同的笛卡尔坐标模型。 Direct2D 使用右手规则，在该规则下 Y 坐标以正值增加，从原点向上移动。 但是，Direct3D 使用左手规则，在该规则下 Y 坐标以正值增加，从原点向右移动。 结果是屏幕坐标的原点位于 Direct2D 的左上角，而屏幕（投影平面）的原点位于 Direct3D 的左下角。 （有关详细信息，请参阅 [3-D 坐标系统](https://docs.microsoft.com/previous-versions/windows/desktop/bb324490(v=vs.85))）。

    ![direct3d 坐标系统。](images/direct3d-origin.png)![direct2d 坐标系统。](images/direct2d-origin.png)

-   第二个原因是，必须明确指定 3-D 旋转矩阵以避免舍入错误。

交换链假设原点位于左上角，因此你必须执行旋转以将右手 Direct2D 坐标系与交换链所使用的左手坐标系对齐。 特别是，你通过将旋转矩阵与已旋转坐标系统原点的转换矩阵相乘将图像重新定位到新的左手方向之下，并将图像从 [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) 的坐标空间转换到交换链的坐标空间。 当 Direct2D 呈现目标与交换链连接到一起时，你的应用还必须一致地应用此转换。 但是，如果你的应用正在绘制到不直接与交换链相关联的中间表面，请不要应用此坐标空间转换。

用于从四种可能的旋转选择正确的矩阵的代码可能如下所示（请注意到新坐标系统原点的转换）：

```cpp
   
// Set the proper orientation for the swap chain, and generate 2D and
// 3D matrix transformations for rendering to the rotated swap chain.
// Note the rotation angle for the 2D and 3D transforms are different.
// This is due to the difference in coordinate spaces.  Additionally,
// the 3D matrix is specified explicitly to avoid rounding errors.

switch (displayRotation)
{
case DXGI_MODE_ROTATION_IDENTITY:
    m_orientationTransform2D = Matrix3x2F::Identity();
    m_orientationTransform3D = ScreenRotation::Rotation0;
    break;

case DXGI_MODE_ROTATION_ROTATE90:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(90.0f) *
        Matrix3x2F::Translation(m_logicalSize.Height, 0.0f);
    m_orientationTransform3D = ScreenRotation::Rotation270;
    break;

case DXGI_MODE_ROTATION_ROTATE180:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(180.0f) *
        Matrix3x2F::Translation(m_logicalSize.Width, m_logicalSize.Height);
    m_orientationTransform3D = ScreenRotation::Rotation180;
    break;

case DXGI_MODE_ROTATION_ROTATE270:
    m_orientationTransform2D = 
        Matrix3x2F::Rotation(270.0f) *
        Matrix3x2F::Translation(0.0f, m_logicalSize.Width);
    m_orientationTransform3D = ScreenRotation::Rotation90;
    break;

default:
    throw ref new FailureException();
}
    
```

在你具有正确的 2-D 图像旋转矩阵和原点之后，在你对 [**ID2D1DeviceContext::BeginDraw**](https://docs.microsoft.com/windows/desktop/Direct2D/id2d1rendertarget-settransform) 和 [**ID2D1DeviceContext::EndDraw**](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw) 的调用之间使用对 [**ID2D1DeviceContext::SetTransform**](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-enddraw) 的调用设置该图像。

**警告**   Direct2D 没有转换堆栈。 如果你的应用也将 [**ID2D1DeviceContext::SetTransform**](https://docs.microsoft.com/windows/desktop/Direct2D/id2d1rendertarget-settransform) 用作其绘制代码的一部分，那么此矩阵需要与你已应用的任何其他转换进行后相乘。

 

```cpp
    ID2D1DeviceContext* context = m_deviceResources->GetD2DDeviceContext();
    Windows::Foundation::Size logicalSize = m_deviceResources->GetLogicalSize();

    context->SaveDrawingState(m_stateBlock.Get());
    context->BeginDraw();

    // Position on the bottom right corner.
    D2D1::Matrix3x2F screenTranslation = D2D1::Matrix3x2F::Translation(
        logicalSize.Width - m_textMetrics.layoutWidth,
        logicalSize.Height - m_textMetrics.height
        );

    context->SetTransform(screenTranslation * m_deviceResources->GetOrientationTransform2D());

    DX::ThrowIfFailed(
        m_textFormat->SetTextAlignment(DWRITE_TEXT_ALIGNMENT_TRAILING)
        );

    context->DrawTextLayout(
        D2D1::Point2F(0.f, 0.f),
        m_textLayout.Get(),
        m_whiteBrush.Get()
        );

    // Ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = context->EndDraw();
```

下次你演示交换链时，将旋转你的 2-D 图像以与新的显示方向匹配。

## <a name="appendix-b-applying-matrices-for-screen-rotation-3-d"></a>附录 B：应用矩阵以进行屏幕旋转 (3-D)


在[调整交换链的大小和预旋转其内容](#resizing-the-swap-chain-and-pre-rotating-its-contents)中的示例代码中（以及在 [DXGI 交换链旋转示例](https://code.msdn.microsoft.com/windowsapps/DXGI-swap-chain-rotation-21d13d71)中），我们为每个可能的屏幕方向定义了一个特定转换矩阵。 现在，我们看一下矩阵来旋转 3-D 场景。 像以前一样，你将为 4 种可能的方向中的每一种创建一组矩阵。 若要避免舍入错误并进而避免轻微的视觉假象，请在你的代码中显式声明矩阵。

你按如下方式设置这些 3-D 旋转矩阵。 以下代码示例中显示的矩阵是在相机的 3-D 场景空间中定义点的顶点的 0、90、180 和 270 度旋转的标准旋转矩阵。 计算场景的二维投影时，场景中的每个顶点的 \[x、y、z\] 坐标值与此旋转矩阵相乘。

```cpp
   
// 0-degree Z-rotation
static const XMFLOAT4X4 Rotation0( 
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 90-degree Z-rotation
static const XMFLOAT4X4 Rotation90(
    0.0f, 1.0f, 0.0f, 0.0f,
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 180-degree Z-rotation
static const XMFLOAT4X4 Rotation180(
    -1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, -1.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );

// 270-degree Z-rotation
static const XMFLOAT4X4 Rotation270( 
    0.0f, -1.0f, 0.0f, 0.0f,
    1.0f, 0.0f, 0.0f, 0.0f,
    0.0f, 0.0f, 1.0f, 0.0f,
    0.0f, 0.0f, 0.0f, 1.0f
    );            
    }
```

你通过对 [**IDXGISwapChain1::SetRotation**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-setrotation) 的调用在交换链上设置旋转类型，如下所示：

`   m_swapChain->SetRotation(rotation);`

现在，在你的呈现方法中，实施某些类似以下的代码：

``` syntax
struct ConstantBuffer // This struct is provided for illustration.
{
    // Other constant buffer matrices and data are defined here.

    float4x4 projection; // Current matrix for projection
} ;
ConstantBuffer  m_constantBufferData;          // Constant buffer resource data

// ...

// Rotate the projection matrix as it will be used to render to the rotated swap chain.
m_constantBufferData.projection = mul(m_constantBufferData.projection, m_rotationTransform3D);
```

现在，当您调用 render 方法时，它会将当前的旋转矩阵（由类变量 m 指定 **\_orientationTransform3D**）与当前投影矩阵相乘，并将该操作的结果分配为呈现器的新投影矩阵。 演示交换链以在更新的屏幕方向中查看场景。

 

 




