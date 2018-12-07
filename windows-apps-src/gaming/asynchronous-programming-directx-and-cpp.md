---
title: 异步编程（DirectX 和 C++）
description: 本主题介绍在你使用 DirectX 进行异步编程和线程处理时需要考虑的各种注意事项。
ms.assetid: 17613cd3-1d9d-8d2f-1b8d-9f8d31faaa6b
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, 异步编程, directx
ms.localizationpriority: medium
ms.openlocfilehash: 8551a49512d4b17ab1bab704596d9e5389de3eb6
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8780435"
---
# <a name="asynchronous-programming-directx-and-c"></a>异步编程（DirectX 和 C++）



本主题介绍在你使用 DirectX 进行异步编程和线程处理时需要考虑的各种注意事项。

## <a name="async-programming-and-directx"></a>异步编程和 DirectX


如果你刚开始了解 DirectX，或者你已经有使用它的经验，请考虑将你的所有图形处理管道置于一个线程中。 在游戏的给定场景中，存在诸如位图、着色器和其他需要独占访问权的资产这样的公共资源。 这些相同资源需要你在并行线程之间同步对这些资源的任何访问。 在多个线程间并行处理时，呈现是一个非常困难的过程。

但是，如果你的游戏非常复杂，或者如果你希望提高性能，你可以使用异步编程来并行处理非特定于你的呈现管道的某些组件。 现代硬件都具有多核的超线程 CPU，你的应用应该利用此特点！ 你可以通过为你的游戏中不需要直接访问 Direct3D 设备上下文的某些组件使用异步编程来确保这一点，例如：

-   文件 I/O
-   物理特性
-   AI
-   联网
-   音频
-   控件
-   基于 XAML 的 UI 组件

你的应用可以在多个并发线程上处理这些组件。 文件 I/O（尤其是资源加载）可以从异步加载中得到很大好处，因为在加载或流式传输数兆（或数百兆）字节的资源时，你的游戏或应用可以处于交互状态。 创建和管理这些线程的最简单方式是使用[并行模式库](https://msdn.microsoft.com/library/dd492418.aspx)和 **task** 模式，它们包含在 PPLTasks.h 中定义的 **concurrency** 命名空间中。 如果使用[并行模式库](https://msdn.microsoft.com/library/dd492418.aspx)，将直接利用多核和超线程 CPU，并且可以改进从预知加载时间到伴随密集 CPU 计算或网络处理出现的停滞和滞后等所有方面。

> **注意**在通用 Windows 平台 (UWP) 应用，用户界面完全在单线程单元 (STA) 中运行。 如果你要为使用 [XAML 互操作](directx-and-xaml-interop.md)的 DirectX 游戏创建 UI，则只能使用 STA 访问控件。

 

## <a name="multithreading-with-direct3d-devices"></a>多线程处理 Direct3D 设备


多线程处理设备上下文仅适用于支持 Direct3D 功能级别 11_0 或更高版本的图形设备。 但是，你可能需要在许多平台上充分使用强大的 GPU，例如专用游戏平台。 在最简单的情况下，你可能需要从 3D 场景呈现和投影中分离抬头显示 (HUD) 覆盖层的呈现，让两个组件使用单独的并行管道。 两个线程都必须使用相同的 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385) 来创建和管理资源对象（纹理、网格、着色器和其他资源），尽管它属于单线程并且需要你实现某种同步机制（例如关键部分）以对其进行安全访问。 虽然你可以为不同线程上的设备上下文创建单独的命令列表（为了延迟呈现），但是你无法同时在同一个 **ID3D11DeviceContext** 实例中播放这些命令列表。

现在，你的应用还可以使用 [**ID3D11Device**](https://msdn.microsoft.com/library/windows/desktop/ff476379)（它可以安全地进行多线程处理）创建资源对象。 那么，为什么不始终使用 **ID3D11Device** 来代替 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)？ 这是因为，在当前，某些图形接口可能无法提供对多线程处理的驱动程序支持。 你可以查询设备并查明它是否确实支持多线程处理，但如果你想要获得最广泛的受众，你可能仍然需要使用单线程的 **ID3D11DeviceContext** 来进行资源对象管理。 也就是说，当图形设备驱动程序不支持多线程处理或命令列表时，Direct3D 11 将尝试内部处理对设备上下文的同步访问；如果命令列表不受支持，它将提供某种软件实现。 因此，你可以编写多线程代码，让它运行于图形接口缺乏对多线程设备上下文访问的驱动程序支持的平台上。

如果你的应用支持单独的线程用来处理命令列表和显示帧，你可能希望保持 GPU 处于活动状态，从而在显示帧时及时地处理命令列表，不会出现可觉察的间断或滞后。 在这种情况下，你可以为每个线程使用单独的 [**ID3D11DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/ff476385)，并通过创建带有 D3D11\_RESOURCE\_MISC\_SHARED 标志的资源来共享这些资源（如纹理）。 在此方案中，必须先对处理线程调用 [**ID3D11DeviceContext::Flush**](https://msdn.microsoft.com/library/windows/desktop/ff476425) 以完成命令列表的执行，然后在显示线程中显示处理资源对象的结果。

## <a name="deferred-rendering"></a>延迟呈现


延迟呈现将图形命令记录在命令列表中，以便这些命令可在另外的某个时间播放，并且延迟呈现设计为在一个线程上支持呈现，而在其他线程上记录呈现命令。 在完成这些命令后，可以对生成最后显示对象（帧缓冲区、纹理或其他图形输出）的线程执行这些命令。

使用 [**ID3D11Device::CreateDeferredContext**](https://msdn.microsoft.com/library/windows/desktop/ff476505) 创建延迟上下文（而不是使用 [**D3D11CreateDevice**](https://msdn.microsoft.com/library/windows/desktop/ff476082) 或 [**D3D11CreateDeviceAndSwapChain**](https://msdn.microsoft.com/library/windows/desktop/ff476083)，它们创建即时上下文）。 有关详细信息，请参阅[即时呈现和延迟呈现](https://msdn.microsoft.com/library/windows/desktop/ff476892)。

## <a name="related-topics"></a>相关主题


* [Direct3D 11 中的多线程处理简介](https://msdn.microsoft.com/library/windows/desktop/ff476891)

 

 




