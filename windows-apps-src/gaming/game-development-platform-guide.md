---
title: 通用 Windows 平台 (UWP) 应用的游戏技术
description: 在此指南中，你将了解可用于开发通用 Windows 平台 (UWP) 游戏的技术。
ms.assetid: bc4d4648-0d6e-efbb-7608-80bd09decd6e
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 技术, directx
ms.localizationpriority: medium
ms.openlocfilehash: c6d2ebad640849cd81d6a2704f89ca1f05cc1b27
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2018
ms.locfileid: "7707670"
---
# <a name="game-technologies-for-uwp-apps"></a>适用于 UWP 应用的游戏技术



在此指南中，你将了解可用于开发通用 Windows 平台 (UWP) 游戏的技术。

##  <a name="benefits-of-windows10-for-game-development"></a>对于游戏开发的 windows 10 的优势


UWP，windows 10 中引入，windows 10 标题将能够跨越所有 Microsoft 平台。 通过从以前版本的 Windows 的免费迁移，没有 windows 10 客户端数量将稳定增加。 这两个事实的结合意味着你的 windows 10 标题将能够访问大量通过 Microsoft 应用商店的客户。

此外，windows 10 还提供了许多对游戏极其有益的新功能：

-   减少了内存分页和整体内存系统大小
-   经改进的图形内存管理可为前台游戏主动分配和保护更多内存

## <a name="uwp-games-with-c-and-directx"></a>使用 C++ 和 DirectX 的 UWP 游戏


需要高性能的实时游戏应充分利用 DirectX API。 DirectX 是用于创建需要高性能的游戏和多媒体应用程序（如 3D 游戏）的本机 API 集合。

## <a name="development-environment"></a>开发环境


若要创建 UWP 游戏，你将需要通过安装 Visual Studio 2015 或更高版本来设置开发环境。 我们建议你安装最新版本的 Visual Studio 中，使你访问的最新的开发和安全更新。 Visual Studio 允许你创建 UWP 应用，并提供用于游戏开发工具：

-   用于 DX 游戏编程的 Visual Studio 工具 - Visual Studio 提供用于创建、编辑、预览和导出图像、模型和着色器资源的工具。 还有一些工具，可以用来在生成时转换资源以及调试 DirectX 图形代码。 有关详细信息，请参阅[使用 Visual Studio 工具进行游戏编程](set-up-visual-studio-for-game-development.md)。
-   Visual Studio 图形诊断功能 - 图形诊断工具现在在 Windows 中作为可选功能提供。 诊断工具允许你 执行图形调试、图形帧分析以及实时监视 GPU 使用情况。 有关详细信息，请参阅[使用 DirectX 运行时和 Visual Studio 图形诊断功能](use-the-directx-runtime-and-visual-studio-graphics-diagnostic-features.md)。

有关详细信息，请参阅“准备通用 Windows 平台和 [DirectX 编程](directx-programming.md)。

## <a name="getting-started-with-directx-game-project-templates"></a>DirectX 游戏项目模板入门


设置开发环境后，可以使用 DirectX 相关项目模板之一创建 UWP DirectX 游戏。 Visual Studio 2015 具有三个可用于创建新的 UWP DirectX 项目的模板：**DirectX 11 应用（通用 Windows）**、**DirectX 12 应用（通用 Windows）** 以及 **DirectX 11 和 XAML 应用（通用 Windows）**。 有关详细信息，请参阅[从模板创建通用 Windows 平台和 DirectX 游戏项目](user-interface.md)。

## <a name="windows-10-apis"></a>Windows 10 API


Windows 10 提供可用于游戏开发的大量 API 集合。 有用于游戏的几乎所有方面的 API，包括 3D 图形、2D 图形、音频、输入、文本资源、用户界面和网络。

有许多与游戏开发相关的 API，但并非所有游戏都需要使用所有 API。 例如，某些游戏仅使用 3D 图形，因此仅使用 Direct3D， 某些游戏可能仅使用 2D 图形，因此仅使用 Direct2D，而其他游戏可能同时使用这两者。 下图显示按功能类型分组的游戏开发相关 API。

![游戏平台技术](images/gameplatformtechnologies.png)

-   3D 图形 - Windows 10 支持两个 3D 图形 API 集：Direct3D 11 和 [Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899121)。 这些 API 都提供创建 3D 和 2D 图形的功能。 Direct3D 11 和 Direct3D 12 不在一起使用，但是其中任何一个都可与 2D 图形和 UI 组中的任何 API 一起使用。 有关在游戏中使用图形 API 的详细信息，请参阅 [DirectX 游戏的基本 3D 图形](an-introduction-to-3d-graphics-with-directx.md)。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">描述</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct3D 12</td>
    <td align="left"><p>Direct3D 12 引入了下一版本的 Direct3D，它是 DirectX 的核心 3D 图形 API。 此版本的 Direct3D 设计为比以前版本的 Direct3D 更快速且更高效。 Direct3D 12 提升了速度的代价是，它较低级，需要你自行管理你的图形资源 并且拥有更广泛的图形编程经验才能实现速度的提升。</p>
    <p><strong>使用时间</strong></p>
    <p>当你需要最大限度提升游戏性能并且你的游戏占用大量 CPU 时，请使用 Direct3D 12。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 <a href="https://msdn.microsoft.com/library/windows/desktop/dn899121">Direct3d 12</a> 文档。</p></td>
    </tr>
    <tr class="even">
    <td align="left">Direct3D 11</td>
    <td align="left"><p>Direct3D 11 是以前版本的 Direct3D，可允许你使用比 D3D 12 更高级的硬件抽象创建 3D 图形。</p>
    <p><strong>使用时间</strong></p>
    <p>如果你有现有的 Direct3D 11 代码、你的游戏不占用大量 CPU，或者你希望拥有为你管理资源的优势，请使用 Direct3D 11。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 <a href="https://msdn.microsoft.com/library/windows/desktop/ff476080">Direct3D 11</a> 文档。</p></td>
    </tr>
    </tbody>
    </table>

     

-   2D 图形和 UI - 与 2D 图形（例如文本和用户界面）相关的 API。 所有 2D 图形和 UI API 都是可选项。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">描述</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Direct2D</td>
    <td align="left"><p>Direct2D 是硬件加速、直接模式的 2D 图形 API，可为 2D 几何图形、位图和文本提供高性能且高质量的渲染。 Direct2D API 基于 Direct3D 生成，设计用于与 GDI、GDI+ 和 Direct3D 进行良好的交互操作。</p>
    <p><strong>使用时间</strong></p>
    <p>可使用 Direct2D 而不是 Direct3D 来为纯 2D 游戏（如横版游戏和棋盘游戏）提供图形，或者可以与 Direct3D 结合使用来简化在 3D 游戏中创建 2D 图形（如用户界面或提醒显示）的工作。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 <a href="https://msdn.microsoft.com/library/windows/desktop/dd370990">Direct2D</a> 文档。</p></td>
    </tr>
    <tr class="even">
    <td align="left">DirectWrite</td>
    <td align="left"><p>DirectWrite 提供处理文本的额外功能，可与 Direct3D 或 Direct2D 结合使用，为需要文本的用户界面或其他区域提供文本输入。 DirectWrite 支持多格式文本的测试、绘制和命中测试。 DirectWrite 处理使用全球和本地化应用程序的所有受支持的语言的文本。 对于希望执行其自己的布局和 Unicode 到字形处理的开发人员，DirectWrite 还提供低级的字形呈现 API。</p>
    <p><strong>使用时间</strong></p>
    <p></p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 <a href="https://msdn.microsoft.com/library/windows/desktop/dd368038">DirectWrite</a> 文档。</p></td>
    </tr>
    <tr class="odd">
    <td align="left">DirectComposition</td>
    <td align="left"><p>DirectComposition 是一个 Windows 组件，可通过 转换、效果和动画支持高性能的位图合成。 应用程序开发人员可以使用 DirectComposition API 创建视觉上吸引人的用户界面，该用户界面以从一个视觉对象到另一个视觉对象的丰富且流畅的动画切换为特征。</p>
    <p><strong>使用时间</strong></p>
    <p>DirectComposition 设计用于简化合成视觉对象和创建动画过渡的过程。 如果游戏需要复杂的用户界面，可以使用 DirectComposition 简化 UI 的创建和管理。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 <a href="https://msdn.microsoft.com/library/windows/desktop/hh437371">DirectComposition</a> 文档。</p></td>
    </tr>
    </tbody>
    </table>

     

-   音频 - 与播放音频和应用音频效果相关的 API。 有关在游戏中使用音频 API 的信息，请参阅[游戏音频](working-with-audio-in-your-directx-game.md)。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">描述</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XAudio2</td>
    <td align="left"><p>XAudio2 是低级的音频 API，可提供信号处理和混音的基础。 XAudio 设计用于对游戏音频引擎快速响应，同时保持能够创建自定义音频效果与音频效果和过滤的复杂链。</p>
    <p><strong>使用时间</strong></p>
    <p>当游戏需要以最少的开销和延迟播放声音时，请使用 XAudio2。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 <a href="https://msdn.microsoft.com/library/windows/desktop/hh405049">XAudio2</a> 文档。</p></td>
    </tr>
    <tr class="even">
    <td align="left">媒体基础</td>
    <td align="left"><p>Microsoft 媒体基础设计用于媒体文件和流媒体（音频和视频）的播放，但是当需要比 XAudio2 更高级的功能并且可接受一些更多的开销时，也可用于游戏中。</p>
    <p><strong>使用时间</strong></p>
    <p>媒体基础对于游戏中的电影场景或非交互组件尤其有用。 媒体基础对于使用 XAudio2 解码音频文件用于播放也很有用。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 <a href="https://msdn.microsoft.com/library/windows/desktop/ms694197">Microsoft 媒体基础</a>概述。</p></td>
    </tr>
    </tbody>
    </table>

     

-   输入 - 与从键盘、鼠标、游戏板和其他用户输入源的输入相关的 API。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">描述</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">XInput</td>
    <td align="left"><p>XInput 游戏控制器 API 支持应用程序从游戏控制器接收输入。</p>
    <p><strong>使用时间</strong></p>
    <p>如果游戏需要支持游戏板输入，并且你有现有的 XInput 代码，则可以继续使用 XInput。 XInput 已替换为适用于 UWP 的 Windows.Gaming.Input，如果你要编写新的输入代码，应使用 Windows.Gaming.Input，而非 XInput。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 <a href="https://msdn.microsoft.com/library/windows/desktop/hh405053">XInput</a> 文档。</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Gaming.Input</td>
    <td align="left"><p>Windows.Gaming.Input API 替换了 XInput，并提供相同的功能，但具有以下 Xinput 不具备的优势：</p>
    <ul>
    <li>资源使用率较低</li>
    <li>检索输入时 API 调用延迟较低</li>
    <li>可同时处理多于 4 个游戏板</li>
    <li>可访问其他 Xbox One 游戏板功能，例如触发振动电动机</li>
    <li>可在控制器连接/断开连接时通过事件而非轮询收到通知</li>
    <li>可将输入分配给特定用户 (Windows.System.User)</li>
    </ul>
    <p><strong>使用时间</strong></p>
    <p>如果游戏需要支持游戏板输入，并且不使用现有 XInput 代码，或者需要以上列出的优势之一，则应使用 Windows.Gaming.Input。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 <a href="https://msdn.microsoft.com/library/windows/apps/dn707817">Windows.Gaming.Input</a> 文档。</p></td>
    </tr>
    <tr class="odd">
    <td align="left">Windows.UI.Core.CoreWindow</td>
    <td align="left"><p>Windows.UI.Core.CoreWindow 类提供用于跟踪指针按下和移动的事件，以及键按下和键放开事件。</p>
    <p><strong>使用时间</strong></p>
    <p>需要在游戏中跟踪鼠标或按键时，请使用 Windows.UI.Core.CoreWindows 事件。</p>
    <p><strong>有关详细信息</strong></p>
    <p>有关在游戏中使用鼠标或键盘的详细信息，请参阅<a href="https://docs.microsoft.com/windows/uwp/gaming/tutorial--adding-move-look-controls-to-your-directx-game">适用于游戏的移动观看控件</a>。</p></td>
    </tr>
    </tbody>
    </table>

     

-   数学 - 与简化常用数学运算相关的 API。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">描述</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectXMath</td>
    <td align="left"><p>DirectXMath API 提供 SIMD 友好的 C++ 类型和函数，用于游戏中常用的线性代数和图形数学运算。</p>
    <p><strong>使用时间</strong></p>
    <p>可以选择使用 DirectXMath，并且可以简化常用数学运算。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 <a href="https://msdn.microsoft.com/library/windows/desktop/hh437833">DirectXMath</a> 文档。</p></td>
    </tr>
    </tbody>
    </table>

     

-   网络 - 有关通过 Internet 或专用网络与其他计算机和设备通信的 API。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">API</th>
    <th align="left">描述</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">Windows.Networking.Sockets</td>
    <td align="left"><p>Windows.Networking.Sockets 命名空间提供 TCP 和 UDP 套接字，这些套接字允许可靠或不可靠的网络通信。</p>
    <p><strong>使用时间</strong></p>
    <p>如果游戏需要通过网络与其他计算机或设备通信，请使用 Windows.Networking.Sockets。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅<a href="https://docs.microsoft.com/windows/uwp/gaming/work-with-networking-in-your-directx-game">在你的游戏中使用网络</a>。</p></td>
    </tr>
    <tr class="even">
    <td align="left">Windows.Web.HTTP</td>
    <td align="left"><p>Windows.Web.HTTP 命名空间提供可用于访问网站的 HTTP 服务器的可靠连接。</p>
    <p><strong>使用时间</strong></p>
    <p>如果游戏需要访问网站以检索或存储信息，请使用 Windows.Web.HTTP。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅<a href="https://docs.microsoft.com/windows/uwp/gaming/work-with-networking-in-your-directx-game">在你的游戏中使用网络</a>。</p></td>
    </tr>
    </tbody>
    </table>

     

-   支持实用工具 - 基于 Windows 10 API 生成的库。

    <table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">库</th>
    <th align="left">描述</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td align="left">DirectX 工具包</td>
    <td align="left"><p>DirectX 工具包 (DirectXTK) 是用于使用 C++ 编写 DirectX 11.x 代码的帮助程序类集合。</p>
    <p><strong>使用时间</strong></p>
    <p>如果你是正在寻找旧版 D3DX 实用工具代码的现代替代项的 C++ 开发人员，或者你是过渡到本机 C++ 的 XNA Game Studio 开发人员，请使用 DirectX 工具包。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 DirectX 工具包项目页面 <a href="https://github.com/Microsoft/DirectXTK">https://github.com/Microsoft/DirectXTK</a>。</p></td>
    </tr>
    <tr class="even">
    <td align="left">Win2D</td>
    <td align="left"><p>Win2D 是易于使用的 Windows 运行时 API，用于直接模式 2D 图形渲染。</p>
    <p><strong>使用时间</strong></p>
    <p>如果你是一名 C++ 开发人员并且需要更易于使用的 Direct2D 和 DirectWrite 的 WinRT 包装器，或者你是要使用 Direct2D 和 DirectWrite 的 C# 开发人员，请使用 Win2D。</p>
    <p><strong>有关详细信息</strong></p>
    <p>请参阅 Win2D 项目页面 <a href="https://github.com/Microsoft/Win2D">https://github.com/Microsoft/Win2D</a>。</p></td>
    </tr>
    </tbody>
    </table>

     

## <a name="xbox-live-services"></a>Xbox Live 服务

[Xbox Live 创意者计划](https://developer.microsoft.com/games/xbox/xboxlive/creator)，任何开发人员可以将 Xbox Live 集成到其 UWP 游戏中并发布到 Xbox One 和 windows 10。 你可以使用最少的开发时间，将 Xbox Live 社交体验（如登录、状态、排行榜等）集成到游戏中。 Xbox Live 社交功能旨在有机增加你的受众，在 5,500 多万名活跃玩家中扩大你的知名度。

如果想访问主 Xbox One 应用商店中特别推荐的更多 Xbox Live 功能、专门的营销和开发支持和机会，则你可以申请 [ID@Xbox](http://www.xbox.com/developers/id) 计划。 若要查看可用于 Xbox Live 创意者计划和 ID@Xbox 计划的功能，请参阅[功能表](../xbox-live/developer-program-overview.md#feature-table)。

有关详细信息，请转到[将 Xbox Live 添加到游戏](e2e.md#adding-xbox-live-to-your-game)。

##  <a name="alternatives-to-writing-games-with-directx-and-uwp"></a>使用 DirectX 和 UWP 编写游戏的替代项


### <a name="uwp-games-without-directx"></a>不使用 DirectX 的 UWP 游戏

具有最小性能要求的较简单的游戏（例如纸盘或棋盘游戏）可在不使用 DirectX 的情况下编写，并且不一定需要使用 C++ 编写。 此类游戏可使用 UWP 支持的任何语言，例如 C#、Visual Basic、C++ 和 HTML/JavaScript。 如果你的游戏不需要高性能和密集图形，请查阅 [JavaScript 和 HTML5 触摸游戏示例](http://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031)作为一个示例。

### <a name="game-engines"></a>游戏引擎

作为使用 Windows 游戏开发 API 编写你自己的游戏引擎的替代项，许多基于 Windows 游戏开发 API 生成的高质量游戏引擎都可用于 在 Windows 平台上开发游戏。 在考虑使用游戏引擎或库时，你有多个选项：

-   完整的游戏引擎 - 一个完整的游戏引擎会封装在从头开始编写游戏引擎时将使用的大多数或全部 Windows 10 API，例如图形、音频、输入和网络。 完整的游戏引擎还提供游戏逻辑功能，例如人工智能和寻路。
-   图形引擎 - 图形引擎封装 Windows 10 图形 API、管理图形资源并支持各种模型和现实格式。
-   音频引擎 - 音频引擎封装 Windows 10 音频 API、管理音频资源并提供高级音频处理和效果。
-   网络引擎 - 网络引擎封装用于将点对点或基于服务器的多人支持添加到游戏中的 Windows 10 网络 API，并且可能包括用于支持大量玩家的高级网络功能。
-   人工智能和寻路引擎 - AI 和寻路引擎提供用于控制游戏中的代理行为的框架。
-   特殊用途引擎 - 存在各种其他引擎用于处理你可能遇到的几乎任何与游戏开发相关的任务，例如创建存货系统和对话树。

## <a name="submitting-a-game-to-the-store"></a>将游戏提交到应用商店


一旦准备好发布游戏，将需要创建一个开发者帐户并将游戏提交到 Microsoft Store。

有关将游戏提交到 Microsoft Store 的信息，请参阅[提交和发布游戏](e2e.md#submitting-and-publishing-your-game)。

 

 




