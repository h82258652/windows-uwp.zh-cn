---
description: 新闻和更改 C + + WinRT。
title: 什么是新在 C + + WinRT
ms.date: 01/29/2019
ms.topic: article
keywords: windows 10、 uwp、 标准版、 c + +、 cpp、 winrt、 投影、 新闻、 什么的新
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cb624a93a010dfe9784cf8c26beed12c6cf2f77d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639952"
---
# <a name="whats-new-in-cwinrt"></a>什么是新在 C + + WinRT

下表包含新闻，并将更改为[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)在最新公开发布版本的 Windows SDK，这是 10.0.17763.0 (Windows 10，版本 1809年)。 这些更改还可能会更高版本的 SDK Insider Preview 版本中存在。

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>新闻和更改，请在 Windows SDK 版本 10.0.17763.0 (Windows 10，版本 1809年)

| 新的或已更改功能 | 详细信息 |
| - | - |
| **重大更改**。 为其进行编译，C + + WinRT 不依赖于从 Windows SDK 标头。 | 请参阅[与 Windows SDK 标头文件隔离](#isolation-from-windows-sdk-header-files)下文。 |
| Visual Studio 项目系统格式已更改。 | 请参阅[如何重定目标，你的 C + + WinRT 项目到更高版本的 Windows SDK](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)下文。 |
| 有新的函数和基类，这些类将帮助你将集合对象传递给 Windows 运行时函数，或实现自己的集合属性和集合类型。 | 请参阅[集合使用 C + + WinRT](collections.md)。 |
| 可以使用[{Binding}](/windows/uwp/xaml-platform/binding-markup-extension)标记扩展使用 C + + WinRT 运行时类。 | 有关详细信息和代码示例，请参阅[数据绑定概述](/windows/uwp/data-binding/data-binding-quickstart)。 |
| 支持的取消协同程序，可注册取消回调。 | 有关详细信息和代码示例，请参阅[取消异步操作和取消回调](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks)。 |
| 在创建指向成员函数的委托，可以建立具有强名称或对当前对象的弱引用 (而不是原始*这*指针) 在处理程序注册所在的点。 | 有关详细信息和代码示例，请参阅**如果使用成员函数作为委托**部分中的子部分[安全地访问*这*一个事件处理委托的指针](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| 已修复了由 Visual Studio 的 c + + 标准改进了符合发现 bug。 LLVM 和 Clang 工具链也更好地利用来验证 C + + WinRT 的标准符合性。 | 您将不会再遇到中所述的问题[我的新项目将不会为什么编译？我使用 Visual Studio 2017 (版本 15.8.0 或更高版本)，和 SDK 版本版本 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

其他更改。

- **重大更改**。 [**winrt::get_abi(winrt::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi)现在将返回`void*`而不是`HSTRING`。 可以使用`static_cast<HSTRING>(get_abi(my_hstring));`获取 HSTRING。
- **重大更改**。 [**winrt::put_abi(winrt::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi)现在将返回`void**`而不是`HSTRING*`。 可以使用`reinterpret_cast<HSTRING*>(put_abi(my_hstring));`获取 HSTRING *。
- **重大更改**。 HRESULT 现在作为投影**winrt::hresult**。 如果您需要的 HRESULT （若要执行类型检查，或支持类型特征），然后，你可以`static_cast` **winrt::hresult**。 否则为**winrt::hresult**将转换为 HRESULT，只要您包括`unknwn.h`之前包括任何 C + + WinRT 标头。
- **重大更改**。 GUID 现在作为投影**winrt::guid**。 对于您实现的 Api，必须使用**winrt::guid**的 GUID 参数。 否则为**winrt::hresult**将转换为 GUID，只要您包括`unknwn.h`之前包括任何 C + + WinRT 标头。
- **重大更改**。 [ **Winrt::handle_type 构造函数**](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor)已经强制写入通过使显式 （它是现在更难编写与其不正确的代码）。 如果需要分配原始句柄值，调用[ **handle_type::attach 函数**](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function)相反。
- **重大更改**。 签名**WINRT_CanUnloadNow**并**WINRT_GetActivationFactory**已更改。 您根本不能声明这些函数。 相反，包括`winrt/base.h`(这是自动包含的如果包括所有 C + + WinRT Windows 命名空间的标头文件) 来都包含这些函数的声明。
- 有关[ **winrt::clock 结构**](/uwp/cpp-ref-for-winrt/clock)， **from_FILETIME/to_FILETIME**支持的弃用**from_file_time/to_file_time**。
- 需要 Api **IBuffer**参数进行了简化。 尽管大多数 Api 首选集合或数组，但足够 Api 依赖**IBuffer**它需要要更轻松地使用此类 Api 通过 c + +。 此更新提供了直接访问数据背后**IBuffer**实现中，使用相同的 c + + 标准库容器使用的数据命名约定。 这样还可以避免冲突元数据名称通常以大写字母开头。
- 改进了代码生成： 各种改进来减小代码大小，提高内联，并优化工厂缓存。
- 删除不必要的递归。 当命令行引用到一个文件夹中，而不是特定于`.winmd`，则`cppwinrt.exe`工具不再以递归方式搜索`.winmd`文件。 `cppwinrt.exe`工具现在还处理重复项更加智能化，使其更具弹性用户错误，并且为得不好正确`.winmd`文件。
- 强化的智能指针。 以前，无法撤消时事件 revokers 移动分配新值。 这有助于揭示了智能指针类不可靠地处理自我赋值; 的问题针对根部位于[ **winrt::com_ptr 结构模板**](/uwp/cpp-ref-for-winrt/com-ptr)。 **winrt::com_ptr**已解决和修复，可处理事件 revokers 移动语义正确，以便它们会吊销分配时。

> [!IMPORTANT]
> 对进行了重要更改[C + + WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix)，同时在版本 1.0.181002.2，并且以后版本 1.0.190128.4 中。 有关这些更改，以及它们如何影响现有项目的详细信息[Visual Studio 支持 C + + WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)并[早期版本的 VSIX 扩展](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension)。

## <a name="isolation-from-windows-sdk-header-files"></a>从 Windows SDK 标头文件隔离

这可能是一项重大更改为你的代码。

为其进行编译，C + + WinRT 不再依赖于从 Windows SDK 标头文件。 C 运行时库 (CRT) 和 c + + 标准模板库 (STL) 中的标头文件还不包括任何 Windows SDK 标头。 并且改进了标准符合性、 可避免意外的依赖项，并极大地减少了的宏，您必须防止出现。

这种独立性意味着 C + + / WinRT 是现在更多的可移植且符合，标准，它进一步增强能力就有可能成为跨编译器和跨平台库。 这也意味着，C + + WinRT 标题并不是受产生负面影响的宏。

如果它之前保留与 C + + WinRT 以在项目中，包括任何 Windows 标头，则现在需要将其包含自己。 它是，在任何情况下，始终最佳做法来显式包括所依赖的标头并不将其留到另一个库，以便将它们包含。

目前，Windows SDK 标头文件隔离到唯一的例外是内部函数，和的数字。 没有这些最后一个剩余依赖关系与任何已知的问题。

在项目中，您可以在需要时重新启用与 Windows SDK 标头进行互操作。 您可能，例如，想要实现 COM 接口 (来源于[ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509))。 对于该示例，包括`unknwn.h`之前包括任何 C + + WinRT 标头。 执行操作会导致 C + + WinRT 基库启用各种挂钩，以支持经典 COM 接口。 有关代码示例，请参阅[作者 COM 组件使用 C + + WinRT](author-coclasses.md)。 同样，显式包含类型和/或你想要调用的函数声明任何其他 Windows SDK 标头。

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>如何重定目标，你的 C + + WinRT 项目到更高版本的 Windows SDK

重定目标的项目中可能会导致最少的编译器和链接器问题的方法也是最费力的。 该方法包括创建 （面向所选的 Windows SDK 版本） 的新项目，然后通过中将文件复制到新项目中，从你的旧版。 将你的旧版的部分`.vcxproj`和`.vcxproj.filters`文件，则可仅将超过复制以节省你在 Visual Studio 中添加文件。

但是，有两种方法可重定目标，Visual Studio 中的项目。

- 转到项目属性**常规** \> **Windows SDK 版本**，然后选择**所有配置**并**所有平台**。 设置**Windows SDK 版本**到想要面向的版本。
- 在中**解决方案资源管理器**，右键单击项目节点，单击**重定目标项目**，选择你想要为目标，然后依次的版本**确定**。

如果使用这两种方法之一后遇到任何编译器或链接器错误，则可以尝试清理解决方案 (**构建** > **清理解决方案**和/或手动删除所有临时文件夹和文件） 然后再尝试重新生成。

如果 c + + 编译器会生成"*错误 C2039:IUnknown： 不是成员的 '\`全局命名空间'*"，然后添加`#include <unknwn.h>`到顶部你`pch.h`文件 (之前包括任何 C + + WinRT 标头)。

您可能还需要添加`#include <hstring.h>`之后。

如果生成的 c + + 链接器"*错误 LNK2019： 无法解析的外部符号_WINRT_CanUnloadNow@0函数中引用_VSDesignerCanUnloadNow@0* "，则可以通过添加解析它`#define _VSDESIGNER_DONT_LOAD_AS_DLL`到你`pch.h`文件。
