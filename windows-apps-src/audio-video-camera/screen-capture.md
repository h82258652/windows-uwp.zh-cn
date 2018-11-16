---
author: eliotcowley
title: 屏幕捕获
description: Windows.Graphics.Capture 命名空间提供从屏幕或应用程序窗口获取帧的 API，以创建用于生成协作和交互式体验的视频流或快照。
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.author: elcowle
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, 屏幕捕获
ms.localizationpriority: medium
ms.openlocfilehash: d28ed1fce79a815155180ab8a3c708e2c8bf8916
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995025"
---
# <a name="screen-capture"></a>屏幕捕获

从 Windows 10 版本 1803 开始，[Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) 命名空间提供用于从屏幕或应用程序窗口获取帧的 API，以创建用于生成协作和交互式体验的视频流或快照。

通过屏幕捕获，开发人员调用安全系统 UI 以便最终用户选取要捕获的屏幕或应用程序窗口，然后系统会在当前正在捕获的项目四周绘制黄色通知边框。 如果同时存在多个捕获会话，系统会在每个正在捕获的项目四周绘制黄色边框。

> [!NOTE]
> 屏幕捕获 Api 仅支持桌面版和 Windows Mixed Reality 沉浸式头戴显示设备。

## <a name="add-the-screen-capture-capability"></a>添加屏幕捕获功能

**Windows.Graphics.Capture**命名空间中的 Api 需要在你的应用程序清单中声明一个常规功能：
    
1. 在**解决方案资源管理器**中打开**Package.appxmanifest** 。
2. 选择**功能**选项卡。
3. 检查**图形捕获**。

![图形捕获](images/screen-capture-1.png)

## <a name="launch-the-system-ui-to-start-screen-capture"></a>启动系统 UI 以开始捕获屏幕

在启动系统 UI 前，可以检查应用程序当前是否能够捕获屏幕。 有多种原因可能导致应用程序无法使用屏幕捕获，如设备不符合硬件要求，或要对其实施屏幕捕获的应用程序会阻止屏幕捕获。 使用 [GraphicsCaptureSession](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturesession) 类中的 **IsSupported** 方法来确定是否支持 UWP 屏幕捕获：

```cs
// This runs when the application starts.
public void OnInitialization()
{
    if (!GraphicsCaptureSession.IsSupported()) 
    {
        // Hide the capture UI if screen capture is not supported.
        CaptureButton.Visibility = Visibility.Collapsed; 
    }    
}
```

验证并确定支持屏幕捕获后，使用 [GraphicsCapturePicker](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscapturepicker) 类调用系统选取器 UI。 最终用户使用此 UI 选择要对其实施屏幕捕获的屏幕或应用程序窗口。 选取器会返回将用于创建 **GraphicsCaptureSession** 的 [GraphicsCaptureItem](https://docs.microsoft.com/uwp/api/windows.graphics.capture.graphicscaptureitem)：

```cs
public async Task StartCaptureAsync() 
{ 
    // The GraphicsCapturePicker follows the same pattern the 
    // file pickers do. 
    var picker = new GraphicsCapturePicker(); 
    GraphicsCaptureItem item = await picker.PickSingleItemAsync(); 

    // The item may be null if the user dismissed the 
    // control without making a selection or hit Cancel. 
    if (item != null) 
    {
        // We'll define this method later in the document.
        StartCaptureInternal(item); 
    } 
}
```

由于这是 UI 代码，则需要在 UI 线程上调用。 如果你要调用此方法从代码隐藏页面你的应用程序 （例如**MainPage.xaml.cs**) 这是为你自动完成，但如果不是，你可以强制它与下面的代码在 UI 线程上运行：

```cs
CoreWindow window = CoreApplication.MainView.CoreWindow;
           
await window.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, async () =>
{
    await StartCaptureAsync();
});
```

## <a name="create-a-capture-frame-pool-and-capture-session"></a>创建捕获帧池和捕获会话

通过使用 **GraphicsCaptureItem**，可使用 D3D 设备、按支持的像素格式 (**DXGI\_FORMAT\_B8G8R8A8\_UNORM**)、所需帧的数量（可以是任何整数）和帧大小，创建 [Direct3D11CaptureFramePool](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframepool)。 **GraphicsCaptureItem** 类的 **ContentSize** 属性可以用作帧的大小：

```cs
private GraphicsCaptureItem _item;
private Direct3D11CaptureFramePool _framePool;
private CanvasDevice _canvasDevice;
private GraphicsCaptureSession _session;

public void StartCaptureInternal(GraphicsCaptureItem item) 
{
    _item = item;

    _framePool = Direct3D11CaptureFramePool.Create( 
        _canvasDevice, // D3D device 
        DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
        2, // Number of frames 
        _item.Size); // Size of the buffers   
} 
```

接下来，通过将 **GraphicsCaptureItem** 传递到 **CreateCaptureSession** 方法为 **Direct3D11CaptureFramePool** 获取 **GraphicsCaptureSession** 类的实例：

```cs
_session = _framePool.CreateCaptureSession(_item);
```

在用户明确同意在系统 UI 中捕获应用程序窗口或屏幕后，可将 **GraphicsCaptureItem** 关联到多个 **CaptureSession** 对象。 通过这种方式，应用程序可选择针对各种应用体验捕获相同的项目。

若要同时捕获多个项目，应用程序必须为要捕获的每个项目创建一个捕获会话，这需要为要捕获的每个项目调用选取器 UI。

## <a name="acquire-capture-frames"></a>获取捕获帧

创建帧池和捕获会话后，调用 **GraphicsCaptureSession** 实例上的 **StartCapture** 方法，以通知系统开始向应用发送捕获帧：

```cs
_session.StartCapture();
```

若要获取这些捕获帧（即 [Direct3D11CaptureFrame](https://docs.microsoft.com/uwp/api/windows.graphics.capture.direct3d11captureframe) 对象），可以使用 **Direct3D11CaptureFramePool.FrameArrived** 事件：

```cs
_framePool.FrameArrived += (s, a) => 
{ 
    // The FrameArrived event fires for every frame on the thread that         
    // created the Direct3D11CaptureFramePool. This means we don't have to 
    // do a null-check here, as we know we're the only one  
    // dequeueing frames in our application.  

    // NOTE: Disposing the frame retires it and returns  
    // the buffer to the pool.
    using (var frame = _framePool.TryGetNextFrame()) 
    {
        // We'll define this method later in the document.
        ProcessFrame(frame); 
    }  
}; 
```

建议尽量避免对 **FrameArrived** 使用 UI 线程，因为每次出现可用的新帧（出现次数频繁）时都会引发此事件。 如果选择在 UI 线程上侦听 **FrameArrived**，请注意每次事件触发时所执行的工作量。

或者，可以使用 **Direct3D11CaptureFramePool.TryGetNextFrame** 方法手动请求帧，直到获取所有需要的帧。

**Direct3D11CaptureFrame** 对象包含 **ContentSize**、**Surface** 和 **SystemRelativeTime** 属性。 **SystemRelativeTime** 是可用于同步其他媒体元素的 QPC ([QueryPerformanceCounter](https://msdn.microsoft.com/library/windows/desktop/ms644904)) 时间。

## <a name="processing-capture-frames"></a>处理捕获帧

调用 **TryGetNextFrame** 时会签出 **Direct3D11CaptureFramePool** 中的每一帧，然后根据 **Direct3D11CaptureFrame** 对象的生存期重新签入。 对于本机应用程序，发布 **Direct3D11CaptureFrame** 对象便可以将帧重新签入到帧池中。 对于托管的应用程序，建议使用 **Direct3D11CaptureFrame.Dispose**（C++ 中的 **Close**）方法。 **Direct3D11CaptureFrame** 实现 [IClosable](https://docs.microsoft.com/uwp/api/Windows.Foundation.IClosable) 界面，该界面投射为 C# 调用方的 [IDisposable](https://msdn.microsoft.com/library/system.idisposable.aspx)。

应用程序不应将引用保存到 **Direct3D11CaptureFrame** 对象，也不应在重新签入帧之后将引用保存到基础 Direct3D 图面。

处理帧时，建议应用程序在与 **Direct3D11CaptureFramePool** 对象关联的相同设备上采用 [ID3D11Multithread](https://msdn.microsoft.com/library/windows/desktop/mt644886) 锁定。

基础 Direct3D 图面始终为创建（或重新创建）**Direct3D11CaptureFramePool** 时所指定的大小。 如果内容超过帧的大小，内容将剪裁为帧的大小。 如果内容小于帧的大小，帧的剩余部分会包含未定义的数据。 建议应用程序使用 **ContentSize** 属性为该 **Direct3D11CaptureFrame** 拷贝出一个 sub-rect，以避免显示未定义的内容。

## <a name="react-to-capture-item-resizing-or-device-lost"></a>针对捕获项目大小重设或设备丢失作出响应

在捕获过程中，应用程序可能会想要更改其 **Direct3D11CaptureFramePool** 的某些方面。 这包括提供新的 Direct3D 设备、更改帧缓冲区的大小或甚至更改池中的缓冲区的数量。 在上述每种方案中，均建议使用 **Direct3D11CaptureFramePool** 对象上的 **Recreate** 方法。

调用 **Recreate** 时，将丢弃所有现有的帧。 这是为了防止将具有后列特征的帧分发出去：这些帧的基础 Direct3D 图面归属于应用程序无法再访问的设备。 为此，建议在调用 **Recreate** 之前处理完所有待处理的帧。

## <a name="putting-it-all-together"></a>整合到一起

以下代码段是如何在 UWP 应用程序中实现屏幕捕获的端到端示例。 在此示例中，我们已经有一个按钮的前端，单击时，调用**Button_ClickAsync**方法。

> [!NOTE]
> 此代码段使用[Win2D](http://microsoft.github.io/Win2D/html/Introduction.htm)，2D 图形呈现的库。 请参阅有关如何将其设置为你的项目信息其文档。

```cs
using Microsoft.Graphics.Canvas;
using Microsoft.Graphics.Canvas.UI.Composition;
using System;
using System.Numerics;
using System.Threading.Tasks;
using Windows.Foundation;
using Windows.Graphics;
using Windows.Graphics.Capture;
using Windows.Graphics.DirectX;
using Windows.UI;
using Windows.UI.Composition;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Hosting;

namespace WindowsGraphicsCapture
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        // Capture API objects.
        private SizeInt32 _lastSize;
        private GraphicsCaptureItem _item;
        private Direct3D11CaptureFramePool _framePool;
        private GraphicsCaptureSession _session;

        // Non-API related members.
        private CanvasDevice _canvasDevice;
        private CompositionGraphicsDevice _compositionGraphicsDevice;
        private Compositor _compositor;
        private CompositionDrawingSurface _surface;

        public MainPage()
        {
            this.InitializeComponent();
            Setup();
        }

        private void Setup()
        {
            _canvasDevice = new CanvasDevice();
            _compositionGraphicsDevice = CanvasComposition.CreateCompositionGraphicsDevice(Window.Current.Compositor, _canvasDevice);
            _compositor = Window.Current.Compositor;

            _surface = _compositionGraphicsDevice.CreateDrawingSurface(
                new Size(400, 400),
                DirectXPixelFormat.B8G8R8A8UIntNormalized,
                DirectXAlphaMode.Premultiplied);    // This is the only value that currently works with the composition APIs.

            var visual = _compositor.CreateSpriteVisual();
            visual.RelativeSizeAdjustment = Vector2.One;
            var brush = _compositor.CreateSurfaceBrush(_surface);
            brush.HorizontalAlignmentRatio = 0.5f;
            brush.VerticalAlignmentRatio = 0.5f;
            brush.Stretch = CompositionStretch.Uniform;
            visual.Brush = brush;
            ElementCompositionPreview.SetElementChildVisual(this, visual);
        }

        public async Task StartCaptureAsync()
        {
            // The GraphicsCapturePicker follows the same pattern the 
            // file pickers do. 
            var picker = new GraphicsCapturePicker();
            GraphicsCaptureItem item = await picker.PickSingleItemAsync();

            // The item may be null if the user dismissed the 
            // control without making a selection or hit Cancel. 
            if (item != null)
            {
                StartCaptureInternal(item);
            }
        }

        private void StartCaptureInternal(GraphicsCaptureItem item)
        {
            // Stop the previous capture if we had one.
            StopCapture();

            _item = item;
            _lastSize = _item.Size;

            _framePool = Direct3D11CaptureFramePool.Create(
               _canvasDevice, // D3D device 
               DirectXPixelFormat.B8G8R8A8UIntNormalized, // Pixel format 
               2, // Number of frames 
               _item.Size); // Size of the buffers 

            _framePool.FrameArrived += (s, a) =>
            {
                // The FrameArrived event is raised for every frame on the thread
                // that created the Direct3D11CaptureFramePool. This means we 
                // don't have to do a null-check here, as we know we're the only 
                // one dequeueing frames in our application.  

                // NOTE: Disposing the frame retires it and returns  
                // the buffer to the pool.

                using (var frame = _framePool.TryGetNextFrame())
                {
                    ProcessFrame(frame);
                }
            };

            _item.Closed += (s, a) =>
            {
                StopCapture();
            };

            _session = _framePool.CreateCaptureSession(_item);
            _session.StartCapture();
        }

        public void StopCapture()
        {
            _session?.Dispose();
            _framePool?.Dispose();
            _item = null;
            _session = null;
            _framePool = null;
        }

        private void ProcessFrame(Direct3D11CaptureFrame frame)
        {
            // Resize and device-lost leverage the same function on the
            // Direct3D11CaptureFramePool. Refactoring it this way avoids 
            // throwing in the catch block below (device creation could always 
            // fail) along with ensuring that resize completes successfully and 
            // isn’t vulnerable to device-lost.   
            bool needsReset = false;
            bool recreateDevice = false;

            if ((frame.ContentSize.Width != _lastSize.Width) ||
                (frame.ContentSize.Height != _lastSize.Height))
            {
                needsReset = true;
                _lastSize = frame.ContentSize;
            }

            try
            {
                // Take the D3D11 surface and draw it into a  
                // Composition surface.

                // Convert our D3D11 surface into a Win2D object.
                var canvasBitmap = CanvasBitmap.CreateFromDirect3D11Surface(
                    _canvasDevice,
                    frame.Surface);

                // Helper that handles the drawing for us.
                FillSurfaceWithBitmap(canvasBitmap);
            }

            // This is the device-lost convention for Win2D.
            catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
            {
                // We lost our graphics device. Recreate it and reset 
                // our Direct3D11CaptureFramePool.  
                needsReset = true;
                recreateDevice = true;
            }

            if (needsReset)
            {
                ResetFramePool(frame.ContentSize, recreateDevice);
            }
        }

        private void FillSurfaceWithBitmap(CanvasBitmap canvasBitmap)
        {
            CanvasComposition.Resize(_surface, canvasBitmap.Size);

            using (var session = CanvasComposition.CreateDrawingSession(_surface))
            {
                session.Clear(Colors.Transparent);
                session.DrawImage(canvasBitmap);
            }
        }

        private void ResetFramePool(SizeInt32 size, bool recreateDevice)
        {
            do
            {
                try
                {
                    if (recreateDevice)
                    {
                        _canvasDevice = new CanvasDevice();
                    }

                    _framePool.Recreate(
                        _canvasDevice,
                        DirectXPixelFormat.B8G8R8A8UIntNormalized,
                        2,
                        size);
                }
                // This is the device-lost convention for Win2D.
                catch (Exception e) when (_canvasDevice.IsDeviceLost(e.HResult))
                {
                    _canvasDevice = null;
                    recreateDevice = true;
                }
            } while (_canvasDevice == null);
        }

        private async void Button_ClickAsync(object sender, RoutedEventArgs e)
        {
            await StartCaptureAsync();
        }
    }
}
```

## <a name="see-also"></a>另请参阅

* [Windows.Graphics.Capture 命名空间](https://docs.microsoft.com/uwp/api/windows.graphics.capture)
