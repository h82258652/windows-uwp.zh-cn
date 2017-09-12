---
author: jwmsft
ms.assetid: 16ad97eb-23f1-0264-23a9-a1791b4a5b95
title: "合成本机互操作"
description: "Windows.UI.Composition API 提供了本机互操作接口，以便内容可以直接移动到合成器中。"
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 96af2b92ec301a93cbae9eb14422af9e1ec8554c
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2017
---
# <a name="composition-native-interoperation-with-directx-and-direct2d"></a>与 DirectX 和 Direct2D 的合成本机互操作性

Windows.UI.Composition API 提供了 [**ICompositorInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620068)、[**ICompositionDrawingSurfaceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620058)、和 [**ICompositionGraphicsDeviceInterop**](https://msdn.microsoft.com/library/windows/apps/Mt620065) 本机互操作接口，以便内容可以直接移动到合成器中。

本机互操作基于受 DirectX 纹理支持的图面对象进行构建。 图面从名为 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) 的工厂对象进行创建。 此对象受基本的 Direct2D 或 Direct3D 设备对象支持，用来为图面分配视频内存。 合成 API 从不会创建基本的 DirectX 设备。 应用程序负责创建一个此类设备并将其传递给 **CompositionGraphicsDevice** 对象。 应用程序一次可以创建多个 **CompositionGraphicsDevice**，并且它可以使用与适用于多个 **CompositionGraphicsDevice** 对象的呈现设备相同的 DirectX 设备。

## <a name="creating-a-surface"></a>创建图面

每个 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) 均可用作图面工厂对象。 每个图面均使用初始大小（该大小可能是 0,0）进行创建，但不具有有效的像素。 例如，可以通过 [**CompositionSurfaceBrush**](https://msdn.microsoft.com/library/windows/apps/Mt589415) 和 [**SpriteVisual**](https://msdn.microsoft.com/library/windows/apps/Mt589433) 立即在可视化树中使用处于初始状态的图面，但处于初始状态的图面对屏幕输出没有影响。 它在所有用途下均是完全透明的，即使指定的 alpha 模式是“opaque”也是如此。

有时，DirectX 设备可能呈现为不可用。 除了其他原因，此情形的出现还可能出于以下原因：应用程序将无效参数传递给特定 DirectX API、系统重置了图形适配器，或者驱动程序进行了更新。 应用程序可使用 Direct3D 中的 API 异步发现设备是否因任一原因而丢失。 当 DirectX 设备丢失时，应用程序必须放弃它、创建一个新设备并将其传递给之前与错误的 DirectX 设备关联的任何 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) 对象。

## <a name="loading-pixels-into-a-surface"></a>将像素加载到图面

若要将像素加载到图面，应用程序必须调用 [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) 方法， 从而返回一个表示纹理或 Direct2D 上下文的 DirectX 接口，具体取决于应用程序请求的内容。 然后，应用程序必须呈现像素或将其上载到该纹理。 应用程序完成操作后，它必须调用 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 方法。 仅在该时间点处新像素才可用于合成，不过它们仍然不会显示在屏幕上，直到下次将所有更改提交到可视化树为止。 如果在调用 **EndDraw** 之前提交可视化树，正在进行的更新将不会在屏幕上显示，而图面将在 **BeginDraw** 调用之前继续显示其内容。 当调用 **EndDraw** 时，由 BeginDraw 返回的纹理或 Direct2D 上下文指针将无效。 应用程序永远不得在 **EndDraw** 调用之后缓存该指针。

对于任何给定的 [**CompositionGraphicsDevice**](https://msdn.microsoft.com/library/windows/apps/Dn706749) 而言，应用程序一次只能在一个图面上调用 BeginDraw。 在调用 [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) 之后，应用程序必须先在该图面上调用 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060)，然后才能在另一图面上调用 **BeginDraw**。 因为该 API 为敏捷型，所以应用程序应在要从多个 worker 线程执行呈现时负责同步这些调用。 如果应用程序想要中断呈现某一图面而临时切换到另一图面，则应用程序可以使用 [**SuspendDraw**](https://msdn.microsoft.com/library/windows/apps/mt620064.aspx) 方法。 这将使另一个 **BeginDraw** 成功调用，但无法针对屏幕上合成提供第一个图面更新。 这将允许应用程序以事务性方式执行多个更新。 图面暂停后，应用程序可以通过调用 [**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) 方法继续更新，也可以通过调用 **EndDraw** 声明该更新已完成。 这意味着对于任何给定的 **CompositionGraphicsDevice** 而言，一次只能主动更新一个图面。 由于每个图形设备均独立地保持此状态，因此应用程序可以在两个图面从属于不同的图形设备时同时呈现它们。 但是，这将排除汇聚在一起的两个图面的视频内存，使得内存利用率较低。

如果应用程序执行一个不正确的操作（例如传递无效的参数，或者先在某一图面上调用 **BeginDraw** 然后在另一图面上调用 **EndDraw**），[**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx)、[**SuspendDraw**](https://msdn.microsoft.com/library/windows/apps/mt620064.aspx)、[**ResumeDraw**](https://msdn.microsoft.com/library/windows/apps/mt620062) 和 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 方法将返回失败。 这些类型的故障表示应用程序 Bug，因此预期结果是它们将快速进行处理，而结果为失败。 如果基础 DirectX 设备丢失，**BeginDraw** 还可能会返回一项失败。 此故障并非严重性故障，因为应用程序可以重新创建其 DirectX 设备，然后重试。 因此，应用程序应仅通过跳过呈现就可以处理设备丢失。 如果出于任何原因而使得 **BeginDraw** 无法调用，应用程序同样也不能调用 **EndDraw**，因为必须先调用前者然后才能调用后者。

## <a name="scrolling"></a>滚动

出于性能原因，当应用程序调用 [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) 时，返回的纹理并不能保证是上一图面的内容。 应用程序必须假定内容是随机的，同时应用形程序也必须确保所有像素均有涉及，无论是通过在呈现前清除，还是通过绘制充足的不透明内容以便覆盖整个更新的矩形。 此外，再加上纹理指针仅在 **BeginDraw** 和 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 调用期间才有效，从而使得应用程序无法从图面中复制之前的内容。 出于此原因，我们提供了 [**Scroll**](https://msdn.microsoft.com/library/windows/apps/mt620063) 方法，该方法允许应用程序执行一次相同图面像素复制。

## <a name="usage-example"></a>使用示例

以下示例介绍了一个非常简单的方案，其中一个应用程序创建绘图图面，并使用 [**BeginDraw**](https://msdn.microsoft.com/library/windows/apps/mt620059.aspx) 和 [**EndDraw**](https://msdn.microsoft.com/library/windows/apps/mt620060) 来填充带有文本的图面。 应用程序使用 DirectWrite 来布局文本（不显示详细信息），然后使用 Direct2D 呈现它。 合成图形设备直接在初始化时接受 Direct2D 设备。 这将允许 **BeginDraw** 返回 ID2D1DeviceContext 接口指针，相比于通过让应用程序创建 Direct2D 上下文以便在每次绘制操作中包装返回的 ID3D11Texture2D 接口，此操作更为有效。

```cpp
//------------------------------------------------------------------------------
//
// Copyright (C) Microsoft. All rights reserved.
//
//------------------------------------------------------------------------------

#include "stdafx.h"

using namespace Microsoft::WRL;
using namespace Windows::Foundation;
using namespace Windows::Graphics::DirectX;
using namespace Windows::UI::Composition;

// This is an app-provided helper to render lines of text
class SampleText
{
private:
    // The text to draw
    ComPtr<IDWriteTextLayout> _text;

    // The composition surface that we use in the visual tree
    ComPtr<ICompositionDrawingSurfaceInterop> _drawingSurfaceInterop;

    // The device that owns the surface
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;

    // For managing our event notifier
    EventRegistrationToken _deviceReplacedEventToken;

public:
    SampleText(IDWriteTextLayout* text, ICompositionGraphicsDevice* compositionGraphicsDevice) :
        _text(text),
        _compositionGraphicsDevice(compositionGraphicsDevice)
    {
        // Create the surface just big enough to hold the formatted text block.
        DWRITE_TEXT_METRICS metrics;
        FailFastOnFailure(text->GetMetrics(&metrics));
        Windows::Foundation::Size surfaceSize = { metrics.width, metrics.height };
        ComPtr<ICompositionDrawingSurface> drawingSurface;
        FailFastOnFailure(_compositionGraphicsDevice->CreateDrawingSurface(
            surfaceSize,
            DirectXPixelFormat::DirectXPixelFormat_B8G8R8A8UIntNormalized,
            DirectXAlphaMode::DirectXAlphaMode_Ignore,
            &drawingSurface));

        // Cache the interop pointer, since that's what we always use.
        FailFastOnFailure(drawingSurface.As(&_drawingSurfaceInterop));

        // Draw the text
        DrawText();

        // If the rendering device is lost, the application will recreate and replace it. We then
        // own redrawing our pixels.
        FailFastOnFailure(_compositionGraphicsDevice->add_RenderingDeviceReplaced(
            Callback<RenderingDeviceReplacedEventHandler>([this](
                ICompositionGraphicsDevice* source, IRenderingDeviceReplacedEventArgs* args)
                -> HRESULT
            {
                // Draw the text again
                DrawText();
                return S_OK;
            }).Get(),
            &_deviceReplacedEventToken));
    }

    ~SampleText()
    {
        FailFastOnFailure(_compositionGraphicsDevice->remove_RenderingDeviceReplaced(
            _deviceReplacedEventToken));
    }

    // Return the underlying surface to the caller
    ComPtr<ICompositionSurface> get_Surface()
    {
        // To the caller, the fact that we have a drawing surface is an implementation detail.
        // Return the base interface instead
        ComPtr<ICompositionSurface> surface;
        FailFastOnFailure(_drawingSurfaceInterop.As(&surface));
        return surface;
    }

private:
    // We may detect device loss on BeginDraw calls. This helper handles this condition or other
    // errors.
    bool CheckForDeviceRemoved(HRESULT hr)
    {
        if (SUCCEEDED(hr))
        {
            // Everything is fine -- go ahead and draw
            return true;
        }
        else if (hr == DXGI_ERROR_DEVICE_REMOVED)
        {
            // We can't draw at this time, but this failure is recoverable. Just skip drawing for
            // now. We will be asked to draw again once the Direct3D device is recreated
            return false;
        }
        else
        {
            // Any other error is unexpected and, therefore, fatal
            FailFast();
        }
    }

    // Renders the text into our composition surface
    void DrawText()
    {
        // Begin our update of the surface pixels. If this is our first update, we are required
        // to specify the entire surface, which nullptr is shorthand for (but, as it works out,
        // any time we make an update we touch the entire surface, so we always pass nullptr).
        ComPtr<ID2D1DeviceContext> d2dDeviceContext;
        POINT offset;
        if (CheckForDeviceRemoved(_drawingSurfaceInterop->BeginDraw(nullptr,
            __uuidof(ID2D1DeviceContext), &d2dDeviceContext, &offset)))
        {
            // Create a solid color brush for the text. A more sophisticated application might want
            // to cache and reuse a brush across all text elements instead, taking care to recreate
            // it in the event of device removed.
            ComPtr<ID2D1SolidColorBrush> brush;
            FailFastOnFailure(d2dDeviceContext->CreateSolidColorBrush(
                D2D1::ColorF(D2D1::ColorF::Black, 1.0f), &brush));

            // Draw the line of text at the specified offset, which corresponds to the top-left
            // corner of our drawing surface. Notice we don't call BeginDraw on the D2D device
            // context; this has already been done for us by the composition API.
            d2dDeviceContext->DrawTextLayout(D2D1::Point2F(offset.x, offset.y), _text.Get(),
                brush.Get());

            // Our update is done. EndDraw never indicates rendering device removed, so any
            // failure here is unexpected and, therefore, fatal.
            FailFastOnFailure(_drawingSurfaceInterop->EndDraw());
        }
    }
};

class SampleApp
{
    ComPtr<ICompositor> _compositor;
    ComPtr<ID2D1Device> _d2dDevice;
    ComPtr<ICompositionGraphicsDevice> _compositionGraphicsDevice;
    std::vector<ComPtr<SampleText>> _textSurfaces;

public:
    // Run once when the application starts up
    void Initialize(ICompositor* compositor)
    {
        // Cache the compositor (created outside of this method)
        _compositor = compositor;

        // Create a Direct2D device (helper implementation not shown here)
        FailFastOnFailure(CreateDirect2DDevice(&_d2dDevice));

        // To create a composition graphics device, we need to QI for another interface
        ComPtr<ICompositorInterop> compositorInterop;
        FailFastOnFailure(_compositor.As(&compositorInterop));

        // Create a graphics device backed by our D3D device
        FailFastOnFailure(compositorInterop->CreateGraphicsDevice(
            _d2dDevice.Get(),
            &_compositionGraphicsDevice));
    }

    // Called when Direct3D signals the device lost event
    void OnDirect3DDeviceLost()
    {
        // Create a new device
        FailFastOnFailure(CreateDirect2DDevice(_d2dDevice.ReleaseAndGetAddressOf()));

        // Restore our composition graphics device to good health
        ComPtr<ICompositionGraphicsDeviceInterop> compositionGraphicsDeviceInterop;
        FailFastOnFailure(_compositionGraphicsDevice.As(&compositionGraphicsDeviceInterop));
        FailFastOnFailure(compositionGraphicsDeviceInterop->SetRenderingDevice(_d2dDevice.Get()));
    }

    // Create a surface that is asynchronously filled with an image
    ComPtr<ICompositionSurface> CreateSurfaceFromTextLayout(IDWriteTextLayout* text)
    {
        // Create our wrapper object that will handle downloading and decoding the image (assume
        // throwing new here)
        SampleText* textSurface = new SampleText(text, _compositionGraphicsDevice.Get());

        // Keep our image alive
        _textSurfaces.push_back(textSurface);

        // The caller is only interested in the underlying surface
        return textSurface->get_Surface();
    }

    // Create a visual that holds an image
    ComPtr<IVisual> CreateVisualFromTextLayout(IDWriteTextLayout* text)
    {
        // Create a sprite visual
        ComPtr<ISpriteVisual> spriteVisual;
        FailFastOnFailure(_compositor->CreateSpriteVisual(&spriteVisual));

        // The sprite visual needs a brush to hold the image
        ComPtr<ICompositionSurfaceBrush> surfaceBrush;
        FailFastOnFailure(_compositor->CreateSurfaceBrushWithSurface(
            CreateSurfaceFromTextLayout(text).Get(),
            &surfaceBrush));

        // Associate the brush with the visual
        ComPtr<ICompositionBrush> brush;
        FailFastOnFailure(surfaceBrush.As(&brush));
        FailFastOnFailure(spriteVisual->put_Brush(brush.Get()));

        // Return the visual to the caller as the base class
        ComPtr<IVisual> visual;
        FailFastOnFailure(spriteVisual.As(&visual));

        return visual;
    }

private:
    // This helper (implementation not shown here) creates a Direct2D device and registers
    // for a device loss notification on the underlying Direct3D device. When that notification is
    // raised, assume the OnDirect3DDeviceLost method is called.
    HRESULT CreateDirect2DDevice(ID2D1Device** ppDevice);
};
```

 

 




