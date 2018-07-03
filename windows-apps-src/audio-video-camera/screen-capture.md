---
author: eliotcowley
title: 屏幕捕获
description: Windows.Graphics.Capture 命名空间提供从屏幕或应用程序窗口获取帧的 API，以创建用于生成协作和交互式体验的视频流或快照。
ms.assetid: 349C959D-9C74-44E7-B5F6-EBDB5CA87B9F
ms.author: elcowle
ms.date: 5/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 屏幕捕获
ms.localizationpriority: medium
ms.openlocfilehash: e407842711d1bfcac0a54fdf484a38d39bc2b237
ms.sourcegitcommit: f9690c33bb85f84466560efac6f23cca2daf5a02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/23/2018
ms.locfileid: "1912905"
---
# <a name="screen-capture"></a>屏幕捕获

从 Windows 10 版本 1803 开始，[Windows.Graphics.Capture](https://docs.microsoft.com/uwp/api/windows.graphics.capture) 命名空间提供用于从屏幕或应用程序窗口获取帧的 API，以创建用于生成协作和交互式体验的视频流或快照。

通过屏幕捕获，开发人员调用安全系统 UI 以便最终用户选取要捕获的屏幕或应用程序窗口，然后系统会在当前正在捕获的项目四周绘制黄色通知边框。 如果同时存在多个捕获会话，系统会在每个正在捕获的项目四周绘制黄色边框。

> [!NOTE]
> 屏幕捕获 API 要求运行 Windows 10 专业版或企业版。

## <a name="add-the-screen-capture-capability"></a>添加屏幕捕获功能

在 **Windows.Graphics.Capture** 命名空间中找到的 API 需要一个在应用程序清单中声明的常规功能。 必须将其直接添加到文件：
    
1. 在**解决方案资源管理器**中，右键单击 **Package.appxmanifest**。 
2. 选择**打开方式...**。 
3. 选择 **XML (文本)编辑器**。 
4. 选择**确定**。
5. 在**包**节点中，添加以下属性：`xmlns:uap6="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"`
6. 同时在**包**节点中，将以下内容添加到 **IgnorableNamespaces** 属性：`uap6`
7. 在**功能**节点中，添加以下元素：`<uap6:Capability Name="graphicsCapture"/>`

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

以下代码片段是一个端到端示例，展示如何在 UWP 应用程序中实现屏幕捕获：

```cs
using Microsoft.Graphics.Canvas;
using System;
using System.Threading.Tasks;
using Windows.Graphics;
using Windows.Graphics.Capture;
using Windows.Graphics.DirectX;
using Windows.UI.Composition;

namespace CaptureSamples 
{
    class Sample
    {
        // Capture API objects.
        private SizeInt32 _lastSize; 
        private GraphicsCaptureItem _item; 
        private Direct3D11CaptureFramePool _framePool; 
        private GraphicsCaptureSession _session; 

        // Non-API related members.
        private CanvasDevice _canvasDevice; 
        private CompositionDrawingSurface _surface; 

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
            _session.Start(); 
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
 
                // Helper that handles the drawing for us, not shown. 
                FillSurfaceWithBitmap(_surface, canvasBitmap); 
            } 
            // This is the device-lost convention for Win2D.
            catch(Exception e) when (_canvasDevice.IsDeviceLost(e.HResult)) 
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
 
        private void ResetFramePool(Vector2 size, bool recreateDevice) 
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
                catch(Exception e) when (_canvasDevice.IsDeviceLost(e.HResult)) 
                { 
                    _canvasDevice = null; 
                    recreateDevice = true; 
                } 
            } while (_canvasDevice == null); 
        } 
    } 
} 
```

## <a name="see-also"></a>另请参阅

* [Windows.Graphics.Capture 命名空间](https://docs.microsoft.com/uwp/api/windows.graphics.capture)