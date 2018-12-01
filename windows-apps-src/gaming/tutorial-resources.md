---
title: 扩展游戏示例
description: 了解如何为 UWP DirectX 游戏实现 XAML 覆盖。
keywords: DirectX, XAML
ms.date: 10/24/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 7cb1c9f9cf6cbc6cce0c5d4547ed503bb9a06e56
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2018
ms.locfileid: "8335149"
---
# <a name="extend-the-game-sample"></a>扩展游戏示例

此时，我们已经介绍了基本的通用 Windows 平台 (UWP) DirectX 3D 游戏的关键组件。 你可以设置游戏的框架，包括视图提供程序和呈现管道，并实现基本的游戏循环。 还可以创建基本的用户界面覆盖层、合并声音、实现控件。 你已经踏上了创建自己游戏的旅程，如果你需要更多帮助和信息，请查看这些资源。

-   [DirectX 图形和游戏](https://msdn.microsoft.com/library/windows/desktop/ee663274)
-   [Direct3D 11 概述](https://msdn.microsoft.com/library/windows/desktop/ff476345)
-   [Direct3D 11 参考](https://msdn.microsoft.com/library/windows/desktop/ff476147)

## <a name="using-xaml-for-the-overlay"></a>将 XAML 用于覆盖层


还有一种方法我们未深入讨论，即使用 XAML 代替 [Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd370990) 作为覆盖层。 与 Direct2D 相比，XAML 在绘制用户界面元素方面有许多优势。 最重要的一点是，它可以将 windows 10 外观和感觉融入 DirectX 游戏更方便。 许多用于定义 UWP 应用的常用元素、 样式和行为都紧密集成到 XAML 模型中，大幅减少了游戏开发人员的实现工作。 如果你自己设计的游戏有一个复杂的用户界面，请考虑使用 XAML 代替 Direct2D。

使用 XAML，我们可以制作一个外观与之前制作的 Direct2D 界面类似的游戏界面。

### <a name="xaml"></a>XAML
![XAML 覆盖](./images/simple-dx-game-extend-xaml.PNG)

### <a name="direct2d"></a>Direct2D
![D2D 覆盖](./images/simple-dx-game-extend-d2d.PNG)

虽然它们的最终结果相似，但实现 Direct2D 和 XAML 界面之间很多区别。

功能 | XAML| Direct2D
:----------|:----------- | :-----------
定义覆盖 | 在 XAML 文件 `\*.xaml` 中定义。 在了解了 XAML 后，与 Direct2D 相比，创建和配置更复杂的覆盖变得更加简单。| 定义为手动放置并写入 Direct2D 目标缓冲区的 Direct2D 基元和 [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) 字符串的集合。 
用户界面元素 | XAML 用户界面元素来自属于 Windows 运行时 XAML API 一部分的标准化元素，其中包括 [**Windows::UI::Xaml**](https://msdn.microsoft.com/library/windows/apps/br209045) 和 [**Windows::UI::Xaml::Controls**](https://msdn.microsoft.com/library/windows/apps/br227716)。 处理 XAML 用户界面元素行为的代码在代码隐藏文件 Main.xaml.cpp 中定义。 | 简单的形状可以像矩形和省略号一样绘制。
调整窗口大小 | 自然地处理调整大小和视图状态更改事件，并相应地转换覆盖层 | 需要手动指定如何重绘覆盖层的组件。


另一个较大的区别是[交换链](https://docs.microsoft.com/windows/uwp/graphics-concepts/swap-chains)。 你不必将交换链附加到 [**Windows::UI::Core::CoreWindow**](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow) 对象。 而在构建新的 [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.swapchainpanel) 对象时，合并 XAML 的 DirectX 应用将关联一个交换链。 

以下代码段演示如何在 [**DirectXPage.xaml**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml) 文件中声明 **SwapChainPanel** 的 XAML。
```xml
<Page
    x:Class="Simple3DGameXaml.DirectXPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:Simple3DGameXaml"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">


    <SwapChainPanel x:Name="DXSwapChainPanel">

    <!-- ... XAML user controls and elements -->

    </SwapChainPanel>
</Page>
```

**SwapChainPanel** 对象由应用单一实例设置为[启动时](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp#L45-L51)创建的当前窗口对象的 [**Content**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.Content) 属性。

```cpp
void App::OnLaunched(_In_ LaunchActivatedEventArgs^ /* args */)
{
    m_mainPage = ref new DirectXPage();

    Window::Current->Content = m_mainPage;
    // Bring the application to the foreground so that it's visible
    Window::Current->Activate();
}
```


若要将配置的交换链连接到 XAML 定义的 [**SwapChainPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SwapChainPanel) 实例，必须获取底层本机 [**ISwapChainPanelNative**](https://msdn.microsoft.com/library/dn302143) 接口实现的指针并对其调用 [**ISwapChainPanelNative::SetSwapChain**](https://msdn.microsoft.com/library/windows/desktop/dn302144)，从而向其传递配置的交换链。 

以下 [**DX::DeviceResources::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/Common/DeviceResources.cpp#L218-L521) 的代码段详细介绍了 DirectX/XAML 互操作的这个方面：

```cpp
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

        // When using XAML interop, the swap chain must be created for composition.
        DX::ThrowIfFailed(
            dxgiFactory->CreateSwapChainForComposition(
                m_d3dDevice.Get(),
                &swapChainDesc,
                nullptr,
                &m_swapChain
                )
            );

        // Associate swap chain with SwapChainPanel
        // UI changes will need to be dispatched back to the UI thread
        m_swapChainPanel->Dispatcher->RunAsync(CoreDispatcherPriority::High, ref new DispatchedHandler([=]()
        {
            // Get backing native interface for SwapChainPanel
            ComPtr<ISwapChainPanelNative> panelNative;
            DX::ThrowIfFailed(
                reinterpret_cast<IUnknown*>(m_swapChainPanel)->QueryInterface(IID_PPV_ARGS(&panelNative))
                );
            DX::ThrowIfFailed(
                panelNative->SetSwapChain(m_swapChain.Get())
                );
        }, CallbackContext::Any));

        // Ensure that DXGI does not queue more than one frame at a time. This both reduces latency and
        // ensures that the application will only render after each VSync, minimizing power consumption.
        DX::ThrowIfFailed(
            dxgiDevice->SetMaximumFrameLatency(1)
            );
    }
```

有关此过程的详细信息，请参阅 [DirectX 和 XAML 互操作](directx-and-xaml-interop.md)。

## <a name="sample"></a>示例

要下载为覆盖层使用 XAML 的此游戏的版本，请转到 [Direct3D 射击游戏示例 (XAML)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameXaml)。


与其余主题中讨论的游戏示例版本不同，XAML 版本分别在 [App.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/App.xaml.cpp) 和 [DirectXPage.xaml.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/DirectXPage.xaml.cpp) 文件中定义其框架，而不是在 [App.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/App.cpp) 和 [GameInfoOverlay.cpp](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp) 中。