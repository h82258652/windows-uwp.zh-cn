---
author: stevewhims
description: 新闻和更改到 C + + WinRT。
title: 新增功能在 C + + WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: windows 10，uwp，标准、 c + +，cpp，winrt，投影，新闻内容的、 新
ms.localizationpriority: medium
ms.openlocfilehash: 1ada059dc2acfa96dd61b6f3460e25736d96ff68
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/02/2018
ms.locfileid: "5971970"
---
# <a name="whats-new-in-cwinrt"></a>新增功能在 C + + WinRT

下表包含新闻和变为[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)在最新公开发布版本的 Windows SDK，这是 10.0.17763.0 (Windows 10 版本 1809年)。 这些更改也可能是更高版本的 SDK Insider Preview 版本中存在。

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>新闻以及变化，Windows SDK 版本 10.0.17763.0 (Windows 10 版本 1809年)

| 新的或更改的功能 | 详细信息 |
| - | - |
| **重大更改**。 为其进行编译，C + + WinRT 不依赖于从 Windows SDK 标头。 | 请参阅下面的[Windows SDK 标头文件中的隔离](#isolation-from-windows-sdk-header-files)。 |
| Visual Studio 项目系统格式已发生更改。 | 请参阅[如何重定目标 C + + 到更高版本的 Windows SDK 的 WinRT 项目](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)下面。 |
| 有新的函数和基本类以帮助你可以将集合对象传递给 Windows 运行时函数，或实现你自己的集合属性和集合类型。 | 请参阅[集合通过 C + + WinRT](collections.md)。 |
| 你可以使用[{Binding}](/windows/uwp/xaml-platform/binding-markup-extension)标记扩展 C + + WinRT 运行时类。 | 有关详细信息和代码示例，请参阅[数据绑定概述](/windows/uwp/data-binding/data-binding-quickstart)。 |
| 支持取消协同程序允许你注册取消回调。 | 有关详细信息和代码示例，请参阅[取消异步操作，并取消回调](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks)。 |
| 创建时指向成员函数的委托，你可以建立的强引用或弱引用 （而不是原始*此*指针） 在其中注册处理程序的点的当前对象。 | 有关详细信息和代码示例，请参阅**如果你使用的成员函数用作代理**子部分中的[安全地访问*此*指针事件处理委托](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate)部分。 |
| Bug 修复已发现的 Visual Studio 的改进符合标准的 c + +。 LLVM 和 Clang 工具链也更好地利用验证 C + + /winrt 的标准合规性。 | 你将不会再遇到此问题[中所述的原因无法我的新项目编译？在我使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，和 SDK 版本 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

其他更改。

- **重大更改**。 [**winrt::get_abi(winrt::hstring const&)**](/uwp/cpp-ref-for-winrt/get-abi)现在返回`void*`而不是`HSTRING`。 你可以使用`static_cast<HSTRING>(get_abi(my_hstring));`若要获取 HSTRING。
- **重大更改**。 [**winrt::put_abi(winrt::hstring&)**](/uwp/cpp-ref-for-winrt/put-abi)现在返回`void**`而不是`HSTRING*`。 你可以使用`reinterpret_cast<HSTRING*>(put_abi(my_hstring));`若要获取 HSTRING *。
- **重大更改**。 现在作为**winrt::hresult**投影的 HRESULT。 如果你需要的 HRESULT （若要执行此类型检查，或支持类型特征），则可以`static_cast` **winrt::hresult**。 否则， **winrt::hresult**将转换为 HRESULT，只要你包含`unknwn.h`之前你包括了任何 C + + /winrt 标头。
- **重大更改**。 现在作为**winrt::guid**投影 GUID。 为你实现的 Api，你必须为 GUID 参数使用**winrt::guid** 。 否则， **winrt::hresult**将转换为 GUID，只要你包含`unknwn.h`之前你包括了任何 C + + /winrt 标头。
- **重大更改**。 [**Winrt::handle_type 构造函数**](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor)具有已强化，从而显式 （它现在是很难编写不正确的代码与它）。 如果你需要将分配原始句柄值，请改为调用[**handle_type::attach 函数**](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function)。
- **重大更改**。 **WINRT_CanUnloadNow**和**WINRT_GetActivationFactory**的签名发生了更改。 你不完全声明这些功能。 相反，包括`winrt/base.h`(这是自动包含在内，如果你包括了任何 C + + /winrt Windows 命名空间标头文件) 包含这些函数的声明。
- 对于[**winrt::clock 结构**](/uwp/cpp-ref-for-winrt/clock)， **from_FILETIME/to_FILETIME**被弃用，以**from_file_time/to_file_time**支持。
- 预计**IBuffer**参数的 Api 进行过简化有关。 虽然集合或数组倾向于大多数 Api，足够 Api 将依赖于需要更易于使用 c + + 中的此类 Api 的**IBuffer** 。 此更新提供直接访问**IBuffer**实现的使用 c + + 标准库容器使用的相同数据命名约定背后的数据。 这还可以避免逆开头的大写字母的元数据名称与产生冲突。
- 改进了代码生成： 各种改进，以减少代码大小改进内联，并优化工厂缓存。
- 删除不必要的递归。 当命令行引用到某个文件夹，而不是特定于`.winmd`、`cppwinrt.exe`工具不会再递归搜索`.winmd`文件。 `cppwinrt.exe`工具现在还处理重复更加智能化，从而使其更具复原能力用户错误，并且为不当正确`.winmd`文件。
- 强化的智能指针。 以前，失败时撤销事件 revokers 移动分配一个新值。 这有助于发现智能指针类未可靠地处理自我分配; 问题[**winrt:: com_ptr 结构模板**](/uwp/cpp-ref-for-winrt/com-ptr)中的根。 **winrt:: com_ptr**已修复，并固定来处理事件 revokers 移动语义正确，以便它们分配时撤销。

> [!NOTE]
> 使用版本 1.0.181002.2 （或更高版本） 的[C + + /winrt Visual Studio 扩展 (VSIX)](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix)安装，创建新的 C + + WinRT 项目会自动安装该项目的[Microsoft.Windows.CppWinRT NuGet 程序包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)。 Microsoft.Windows.CppWinRT NuGet 程序包提供改进的 C + + WinRT 项目生成支持，使你的项目之间的开发计算机和生成代理 （仅 NuGet 程序包和 VSIX 不安装的） 移植。
>
> 现有项目的&mdash;已安装版本 1.0.181002.2 后 （或更高版本） 的 VSIX&mdash;我们建议你在 Visual Studio 中打开项目，单击**项目** \> **管理 NuGet 程序包...** \> **浏览**，键入或将**Microsoft.Windows.CppWinRT**粘贴在搜索框中，选择搜索结果中的项，然后单击**安装**安装该项目的程序包。


## <a name="isolation-from-windows-sdk-header-files"></a>Windows SDK 标头文件中的隔离

这可能是你的代码的重大更改。

为其进行编译，C + + WinRT 不再依赖于 Windows SDK 中的标头文件。 C 运行时库 (CRT) 和 c + + 标准模板库 (STL) 中的头文件还不包含任何 Windows SDK 标头。 以及提高标准合规性，可以避免无意依赖项，可以大大减少你需要防御的宏。

这意味着，C + + WinRT 现在是更多笔记本和标准符合条件，并且它将促进有可能成为跨编译器和跨平台的库。 它还意味着，C + + /winrt 标头不会产生负面影响的宏。

如果你以前离开它为 C + + WinRT，则你现在需要自行包括在项目中，包括任何 Windows 标头。 是，在任何情况下，始终最佳做法明确包括你依赖的标头和不到另一个库，以包含他们为你保留它。

目前，Windows SDK 标头文件隔离的唯一例外是针对内部函数和数字。 没有这些最后的剩余依赖关系的已知的问题。

在项目中，你可以在需要时重新启用与 Windows SDK 标头互操作。 例如，你可能希望实现 COM 接口 （已植入中[**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)）。 对于该示例中，包括`unknwn.h`之前你包括了任何 C + + /winrt 标头。 执行此操作将导致 C + + /winrt 基础库，若要启用各种挂钩，从而支持传统的 COM 接口。 有关代码示例，请参阅[创作 COM 组件与 C + + WinRT](author-coclasses.md)。 同样，明确包括声明类型和/或你想要调用的函数任何其他 Windows SDK 标头。

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>如何重定目标 C + + WinRT 到更高版本的 Windows SDK 的项目

重定目标项目可能会导致最少的编译器和链接器问题的方法也是工作量最大。 该方法需要创建新项目 （面向你选择的 Windows SDK 版本），然后通过中将文件复制到新项目中，从你的旧。 将旧的部分`.vcxproj`和`.vcxproj.filters`文件，你可以只复制超过保存你在 Visual Studio 中添加文件。

但是，有两种其他方法，以重新定位你在 Visual Studio 中的项目。

- 转到项目属性**常规** \> **Windows SDK 版本**，并选择**所有配置**及其**所有平台**。 设置为你想要面向的版本的**Windows SDK 版本**。
- 在**解决方案资源管理器**中，右键单击项目节点单击**重定目标项目**，选择你想要面向，版本而定，然后单击**确定**。

如果使用这两种方法，其中任一后遇到任何编译器或链接器错误，则你可以尝试清理解决方案 (**生成** > **全新解决方案**和/或手动删除所有临时文件夹和文件) 之前尝试再次生成。

如果 c + + 编译器产生"*错误 C2039: IUnknown': 不是成员 \'global 命名空间 '*"，然后添加`#include <unknwn.h>`到顶部你`pch.h`文件 (之前你包括了任何 C + + /winrt 标头)。

你可能还需要添加`#include <hstring.h>`在此之后。

如果 c + + 链接器产生"*错误 LNK2019： 无法解析的外部符号_WINRT_CanUnloadNow@0函数中引用_VSDesignerCanUnloadNow@0*"，然后你可以通过添加解决的`#define _VSDESIGNER_DONT_LOAD_AS_DLL`到你`pch.h`文件。
