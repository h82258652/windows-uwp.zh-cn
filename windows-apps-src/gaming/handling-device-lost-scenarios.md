---
title: 在 Direct3D 11 中处理设备删除方案
description: 本主题介绍了图形适配器被删除或重新初始化时，如何重新创建 Direct3D 和 DXGI 的设备界面链。
ms.assetid: 8f905acd-08f3-ff6f-85a5-aaa99acb389a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx 11, 设备丢失
ms.localizationpriority: medium
ms.openlocfilehash: 949da8d7577a6ca376d7de745ebc2fc5b3538cb1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368679"
---
# <a name="span-iddevgaminghandlingdevice-lostscenariosspanhandle-device-removed-scenarios-in-direct3d-11"></a><span id="dev_gaming.handling_device-lost_scenarios"></span>处理在 Direct3D 11 中的设备已删除方案



本主题介绍了图形适配器被删除或重新初始化时，如何重新创建 Direct3D 和 DXGI 的设备界面链。

在 DirectX 9 中，应用程序可能会遇到“[设备丢失](https://docs.microsoft.com/windows/desktop/direct3d9/lost-devices)”状况，在该情况下，D3D 设备会进入非可操作状态。 例如，当全屏 Direct3D 9 应用程序失去焦点且 Direct3D 设备变为“已丢失”时，使用丢失设备进行绘制的任何尝试都将静默失败。 Direct3D 11 使用虚拟图形设备界面，这使多个程序可以共享相同的物理图形设备，并可以解除应用无法控制 Direct3D 设备的情况。 但是，图形适配器可用性仍可能发生更改。 例如：

-   图形驱动程序已升级。
-   系统从节能图形适配器更改为性能图形适配器。
-   图形设备停止响应并进行重置。
-   已采用物理方式附加或删除图形适配器。

出现此情况时，DXGI 将返回一个错误代码，该代码指示必须重新初始化 Direct3D 设备且必须重新创建设备资源。 此操作实例演示了 Direct3D 11 应用和游戏如何检测和响应以下情况：重置、删除或更改图形适配器。 DirectX 11 应用 (通用 Windows) 模板提供与 Microsoft Visual Studio 2015 中提供了代码示例。

## <a name="instructions"></a>说明

### <a name="spanspanstep-1"></a><span></span>步骤 1:

包含用于呈现循环中设备删除错误的检查。 通过调用 [**IDXGISwapChain::Present**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present)（或 [**Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1) 等）呈现帧。 然后，检查它是否返回[ **DXGI\_错误\_设备\_已删除**](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dxgi-error)或**DXGI\_错误\_设备\_重置**。

首先，模板存储由 DXGI 交换链返回的 HRESULT：

```cpp
HRESULT hr = m_swapChain->Present(1, 0);
```

完成呈现帧的所有其他工作后，模板将检查设备删除错误。 如果需要，它将调用某个方法来处理设备删除状况：

```cpp
// If the device was removed either by a disconnection or a driver upgrade, we
// must recreate all device resources.
if (hr == DXGI_ERROR_DEVICE_REMOVED || hr == DXGI_ERROR_DEVICE_RESET)
{
    HandleDeviceLost();
}
else
{
    DX::ThrowIfFailed(hr);
}
```

### <a name="step-2"></a>步骤 2：

还包含用于在响应窗口大小更改时出现的设备删除错误的检查。 这是要检查的一个好[ **DXGI\_错误\_设备\_已删除**](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dxgi-error)或**DXGI\_错误\_设备\_重置**有几个原因：

-   调整交换链大小需要调用基础 DXGI 适配器，该适配器可以返回设备删除错误。
-   该应用可能已移动到附加到不同图形设备的监视器中。
-   删除或重置图形设备后，桌面分辨率通常发生更改，从而导致窗口大小更改。

模板将检查由 [**ResizeBuffers**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers) 返回的 HRESULT：

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
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
```

### <a name="step-3"></a>步骤 3:

您的应用程序接收任何时候[ **DXGI\_错误\_设备\_已删除**](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dxgi-error)错误，它必须重新初始化的 Direct3D 设备并重新创建任何设备相关资源。 释放对使用早期 Direct3D 设备创建的图形设备资源的任何引用；这些资源现在无效，在创建新交换链之前，必须释放对交换链的所有引用。

HandleDeviceLost 方法释放交换链并通知应用组件释放设备资源：

```cpp
m_swapChain = nullptr;

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that device resources need to be released.
    // This ensures all references to the existing swap chain are released so that a new one can be created.
    m_deviceNotify->OnDeviceLost();
}
```

然后，它将创建一个新交换链，并重新初始化由设备管理类控制的依赖设备的资源：

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();
```

重新建立设备和交换链后，它将通知应用组件重新初始化依赖设备的资源：

```cpp
// Create the new device and swap chain.
CreateDeviceResources();
m_d2dContext->SetDpi(m_dpi, m_dpi);
CreateWindowSizeDependentResources();

if (m_deviceNotify != nullptr)
{
    // Notify the renderers that resources can now be created again.
    m_deviceNotify->OnDeviceRestored();
}
```

当 HandleDeviceLost 方法退出时，控制将返回到呈现循环，该循环将继续绘制下一个帧。

## <a name="remarks"></a>备注


### <a name="investigating-the-cause-of-device-removed-errors"></a>调查设备删除错误的原因

如果重复出现带有 DXGI 设备删除错误的问题，这可能指示你的图像代码在绘制例程期间创建的条件无效。 它还可能指示硬件失败或图形驱动程序中的错误。 若要调查设备删除错误的原因，请调用 [**ID3D11Device::GetDeviceRemovedReason**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-getdeviceremovedreason)，然后再释放 Direct3D 设备。 此方法返回六个可能的 DXGI 错误代码之一，指示设备删除错误的原因：

-   **DXGI\_错误\_设备\_挂起**:由于无效的应用发送的图形命令组合而导致响应停止图形驱动程序。 如果你反复收到此错误，这可能指示你的应用导致设备挂起，并且需要进行调试。
-   **DXGI\_错误\_设备\_已删除**:图形设备已被以物理方式删除，已关闭，或升级驱动程序已发生。 这种情况偶尔发生，实属正常情况；你的应用和游戏应重新创建设备资源，如本主题中所述。
-   **DXGI\_错误\_设备\_重置**:图形设备失败，因为格式不正确的命令。 如果你反复收到此错误，这可能意味着你的代码发送的绘制命令无效。
-   **DXGI\_ERROR\_DRIVER\_INTERNAL\_ERROR**:图形驱动程序遇到错误，将设备重置。
-   **DXGI\_错误\_无效\_调用**:应用程序提供了无效的参数数据。 如果你收到过一次此错误，这意味着你的代码导致了设备删除状况，必须进行调试。
-   **S\_确定**:当图形设备已启用、 禁用或重置不会导致当前图形设备无效时返回。 例如，如果应用使用的是 [Windows 高级光栅化平台 (WARP)](https://docs.microsoft.com/windows/desktop/direct3darticles/directx-warp) 且硬件适配器变为可用，则可能返回此错误代码。

下面的代码将检索[ **DXGI\_错误\_设备\_已删除**](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dxgi-error)错误代码并将其输出到调试控制台。 将此代码插入 HandleDeviceLost 方法的开头：

```cpp
    HRESULT reason = m_d3dDevice->GetDeviceRemovedReason();

#if defined(_DEBUG)
    wchar_t outString[100];
    size_t size = 100;
    swprintf_s(outString, size, L"Device removed! DXGI_ERROR code: 0x%X\n", reason);
    OutputDebugStringW(outString);
#endif
```

有关更多详细信息，请参阅[ **GetDeviceRemovedReason** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-getdeviceremovedreason)并[ **DXGI\_错误**](https://docs.microsoft.com/windows/desktop/direct3ddxgi/dxgi-error)。

### <a name="testing-device-removed-handling"></a>测试设备删除处理

对于与 Visual Studio 图形诊断相关的 Direct3D 事件捕获和播放，Visual Studio 开发人员命令提示符支持命令行工具“dxcap”。 可以使用命令行选项"-forcetdr"您的应用程序运行时这将强制 GPU 超时检测和恢复事件，从而触发 DXGI\_错误\_设备\_已删除并使你可以测试您的错误处理代码。

> **注意** DXCap 及其支持 DLL 将安装到 system32/syswow64 中作为适用于 Windows 10 的图形工具的一部分，从而将不会再通过 Windows SDK 进行分配。 不过，它们将通过图形工具按需功能（它是可选的操作系统组件）进行提供，并且必须先进行安装，然后才能启用并使用 Windows 10 上的图形工具。 可以在此处找到有关如何安装图形工具适用于 Windows 10 的详细信息： <https://msdn.microsoft.com/library/mt125501.aspx#InstallGraphicsTools>
