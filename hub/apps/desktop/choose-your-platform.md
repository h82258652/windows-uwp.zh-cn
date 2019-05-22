---
Description: 当你想要创建新的桌面应用时，所做的第一个决定是要使用的 Win32 和 COM API 或.NET。
ms.assetid: 82705644-F1F0-40F3-99B1-7A97BFB32831
title: 选择你的应用程序平台
ms.topic: article
ms.date: 03/18/2019
ms.openlocfilehash: 960dda5e4cb7e8edc1edf7ce2e81da8306555f1c
ms.sourcegitcommit: f0f933d5cf0be734373a7b03e338e65000cc3d80
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/21/2019
ms.locfileid: "65984487"
---
# <a name="choose-your-app-platform"></a>选择你的应用程序平台

当你想要为 Windows Pc 创建一个新的桌面应用程序时，所做的第一个决定是要使用的应用程序平台。 Windows 提供了四个主应用程序平台，每个都有各自的优点：

* [通用 Windows 平台 (UWP)](#uwp)
* [WPF (.NET)](#wpf)
* [Windows Forms (.NET)](#windows-forms)
* [Win32](#win32)

所有这些应用程序平台，可以创建如 Word、 Excel 和 Photoshop 的桌面应用程序运行在经典 Windows 桌面和 take 充分利用该环境的特定功能。 但是，这些平台的一些共享一些特征，并更好地适用于某些类型的应用程序：

* **UWP、 WPF 和 Windows 窗体**。 这些平台提供许多好处，尤其是在开发人员工作效率、 复杂且可自定义 UI 和应用程序安全方面的托管运行时环境 （Windows 运行时的 UWP 和.NET for Windows Forms 和 WPF）。 由于这些框架支持快速创建 UI 的可视化设计器和 UI 标记，所以它们非常适合业务线应用程序。

* **Win32 API**。 Win32 API （也称为 Windows API） 是原始的平台，用于本机 C /C++需要直接访问 Windows 和硬件的 Windows 应用程序。 它提供了一流的开发体验，而无具体取决于.NET 和 WinRT 类似的托管运行时环境。 这使得 Win32 API 的应用程序需要最高级别的性能和直接访问系统硬件的理想平台。

本文介绍了这些平台中更多详细信息，并可帮助您确定最适合你的应用程序。

## <a name="uwp"></a>UWP

UWP 是用于 Windows 10 应用程序和游戏的领先平台。 它是一个高度可自定义的平台，使用 XAML 标记与代码 （业务逻辑） 分离用户体验 (presentation)。 UWP 是适用于桌面应用程序的需要复杂的 UI、 样式自定义和图形密集型方案。 UWP 还具有内置的支持[Fluent 设计系统](/windows/uwp/design/fluent-design-system/)对于默认 UX 体验，并提供对访问[Windows 运行时 (WinRT) Api](/windows/uwp/get-started/universal-application-platform-guide#how-the-universal-windows-platform-relates-to-windows-runtime-apis)。 通过采用 Fluent，UWP 自动支持常见的输入的方法，如墨迹、 触控、 游戏板、 键盘和鼠标。

您可以使用 UWP 创建的 Windows 电脑桌面应用程序不仅 UWP 也是 Xbox、 HoloLens、 Surface Hub 应用程序，唯一支持的平台。 UWP 是我们最新的、 先进的应用程序的平台。

有关 UWP 的详细信息，请参阅[开始使用 Windows 10 应用](/windows/uwp/get-started/)。

## <a name="wpf"></a>WPF

WPF 是已建立的平台，用于托管 Windows 应用程序可以访问完整的.NET framework，并且它还使用 XAML 标记与代码分离用户体验。 此平台专为桌面应用程序需要的复杂的 UI、 样式自定义和图形密集型方案。 WPF 开发技能是类似于 UWP 开发技能，因此从 WPF 迁移到 UWP 应用是更容易从 Windows 窗体的迁移。

有关 WPF 的详细信息，请参阅[入门 (WPF)](https://docs.microsoft.com/dotnet/framework/wpf/getting-started/)。

## <a name="windows-forms"></a>Windows 窗体

Windows 窗体是用于托管 Windows 应用程序与一个轻型的 UI 模型并能访问到完整的.NET Framework 的原始平台。 在开发人员若要快速开始构建应用程序，即使对于开发人员熟悉该平台，它占有优势。 这是基于窗体的快速应用程序开发平台，具有大量可视和非可视拖放控件的内置集合。 Windows 窗体不使用 XAML，因此决定更高版本扩展到 UWP 应用程序需要完成重新写入你的 UI。

有关 Windows 窗体的详细信息，请参阅[Windows 窗体入门](https://docs.microsoft.com/dotnet/framework/winforms/getting-started-with-windows-forms)。

## <a name="platform-comparison-uwp-wpf-and-windows-forms"></a>平台比较：UWP、 WPF 和 Windows 窗体

下表比较了 Windows 窗体、 WPF 和 UWP 的详细信息中的各种特征。

| 功能或方案  |    UWP     |      WPF     |   Windows 窗体  |
|--------|--------|--------|--------|
| **支持的版本**      |  Windows 10   |  Windows 7 及更高版本 |  Windows 7 及更高版本  |
| **语言**      |   C\#， C++/WinRT， C++/CX，VB、 JavaScript   |  C\#， C++/CLI (托管扩展C++)，F\#，VB |  C\#， C++/CLI (托管扩展C++)，F\#，VB   |
| **用户界面运行时** |    本机 (C++/WinRT 和C++/CX) 和托管 (.NET Native)  |  托管 (.NET Framework)<br/><br/>.NET Core 3 的支持即将推出  |   托管 (.NET Framework)<br/><br/>.NET Core 3 的支持即将推出    |
| **开放源代码** | [是 （仅限 Windows UI 库）](https://github.com/Microsoft/microsoft-ui-xaml)  |  [是 (仅限.NET Core)](https://github.com/dotnet/wpf) | [是 (仅限.NET Core)](https://github.com/dotnet/winforms)  |
| **支持 XAML** |   是   |  是  |   否   |
| **优势**  |  <ul><li>UI 的 XAML 标记</li><li>丰富且可自定义用户体验</li><li>你的现有基本代码是.NET Standard 兼容</li><li>高 DPI 支持</li><li>对于 Windows 设备 （包括触摸、 笔、 游戏板、 鼠标和键盘） 跨多个输入类型的支持</li><li>对 Xbox、 HoloLens、 IoT 或 Surface Hub 的支持</li><li>对本机支持C++</li><li>优化的电池使用寿命</li><li>现代的辅助功能支持 （如屏幕阅读器）</li><li>富文本数据功能 （例如内置拼写检查）</li><li>墨迹支持</li><li>保护通过应用程序容器的执行 （例如，不受信任的内容进行沙箱处理）</li></ul>  |  <ul><li>UI 的 XAML 标记</li><li>丰富且可自定义用户体验</li><li>来自 Microsoft 和合作伙伴的控件的大型集合</li><li>密集的 UI</li><li>对 Windows 7 的支持</li><li>平台支持输入验证</li></ul> | <ul><li>快速开发应用程序</li><li>用于构建 UI 的 WYSIWYG 编辑器</li><li>来自 Microsoft 和合作伙伴的控件的大型集合</li><li>密集的 UI</li><li>对 Windows 7 的支持</li><li>键盘和鼠标输入</li></ul>          |
| **提供有限的支持的方案** |  <ul><li>密集的用户界面 （在其中创建密集 UI 需要自定义样式）<sup>1</sup></li><li>多个窗口支持<sup>1</sup></li><li>平台支持输入验证<sup>1</sup></li><li>不支持 Windows 7</li><li>有些 UWP Api 需要特定的最小版本的 Windows 10</li><li>完整的平台支持和 shell 集成 （例如，UWP 当前不支持系统任务栏集成或对所有设备完全访问权限）</li><li>直接访问磁盘上的所有文件</li><li>ADO.NET</li><li>使用非.NET Standard 的现有代码库类库或非-Windows 应用认证工具包符合 Api</li><li>本地网络环回支持 （即，如果您的应用程序需要与本地主机通信而无需在目标设备上创建环回例外）</li><li>大量的文件 I/O</li></ul>     |  <ul><li>高 DPI 支持<sup>2</sup></li><li>触摸输入<sup>2</sup></li></ul>  |  <ul><li>高 DPI 支持<sup>2</sup></li><li>触摸输入<sup>2</sup></li><li>可自定义 UI</li><li>丰富图形和用户体验 （如触摸和动画）</li><li>视图和数据模型的丰富抽象</li></ul>    |   |

<sup>1</sup>我们公开宣布了将解决这种情况下，Windows 10 的未来版本中的功能。

<sup>2</sup>虽然在平台缺少对此方案中的第一类 API 支持，开发人员可以支持此方案已有解决方法。

## <a name="win32"></a>Win32

使用 Win32 API 使用C++使可能采取更好地控制与非托管代码的目标平台不是在 WinRT 和.NET 等托管运行时环境可以实现最高级别的性能和效率。 但是，执行此类级别的控制应用程序的执行要求更高的护理和采取行动，以得到正确结果，并且贸易的运行时性能的开发工作效率。

以下是几个要点的 Win32 API 和C++产品/服务使你能够构建高性能应用程序。

-   硬件级别优化，包括严格控制资源分配、 对象生存期、 数据布局、 对齐方式、 字节封装，和的详细信息。
-   对注重性能的指令的访问设置 SSE 和 AVX 等通过内部函数。
-   使用模板的高效、 类型安全泛型编程。
-   有效而安全的容器和算法。
-   DirectX，在特定 Direct3D 和 DirectCompute （请注意，UWP 还提供了 DirectX 互操作）。
-   C++A M P。

有关详细信息，请参阅[开始使用桌面 Windows 应用程序使用 Win32 API](/windows/desktop/desktop-programming)并[桌面应用程序技术](/windows/desktop/desktop-app-technologies)。

### <a name="win32-and-c-for-traditional-desktop-applications"></a>Win32 和C++的传统桌面应用程序

编写桌面应用程序时C++，可以选择用于用户界面，或者一系列还支持非 Windows 平台的第三方应用程序框架 Win32 或 MFC。

-   **Win32:** 这是 Windows 平台，包括但不是限于 UI 功能，例如窗口化、 绘图和 UI 控件的句柄基于、 C 语言 API。 因为它是基于句柄的低级别、 C 语言 API，它是用于创建现代、 需要进行大量 UI 的应用程序很少选择。 但是，它提供与 Windows 平台进行交互所需的基本 Api，并且是对于具有简单的 UI 要求或者只是想在 Windows UI 保持开尽可能多地等游戏应用适当的选择。
-   **MFC （Microsoft 基础类库）：** 这是古老的应用程序框架，从 1992 年起已提供服务的 Windows 开发人员的 UI 库。 它是精简C++包装器，而基于句柄的、 C 语言的 Win32 API，并为许多预定义的 windows、 常用控件和其他 Windows 对象提供面向对象的接口。 尽管.NET 生态系统中的许多现代 UI 框架超过 MFC 中方便起见，但它仍是很多选择的本机 UI 框架C++创建 Windows 桌面应用程序的开发人员。
-   **第三方应用程序框架：** 因为C++可以在各种平台上运行，并且不绑定到 Windows 或.NET 运行时，第三方已经开发了新的应用程序和 UI 框架的C++，有助于简化跨平台应用程序与丰富的用户界面的开发。 这些框架的一些提供其自己的外观和感觉，而其他如 wxWidgets 或 Qt 使用或模拟平台的本机控件集。 使用这些库，就可以共享几乎所有的 Windows 或其他平台，例如 OSX 或 Linux 运行的应用程序的版本之间的应用程序的源代码。

## <a name="other-app-platforms"></a>其他应用平台

### <a name="progressive-web-apps-pwas"></a>渐进式 Web 应用 (Pwa)

Pwa 让他们的网站代码，使它可以安装和 Windows 10 电脑上运行等应用程序的开发人员包。 有关详细信息，请参阅[渐进式 Web 应用](https://docs.microsoft.com/microsoft-edge/progressive-web-apps/get-started)。

### <a name="xamarin"></a>Xamarin

使用 Xamarin 生成跨平台应用程序还可以在 iOS 运行的 Windows 10 和 Android。 有关详细信息，请参阅[Xamarin](https://docs.microsoft.com/xamarin/xamarin-forms/get-started/index)。
