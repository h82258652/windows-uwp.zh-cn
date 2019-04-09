---
description: 为了帮助你更快地开始使用 C++/WinRT，本主题将详细介绍一个简单的代码示例。
title: C++/WinRT 入门
ms.date: 04/03/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 获取, 获得, 开始
ms.localizationpriority: medium
ms.openlocfilehash: 4928540d9b6e7e1c3df67f7c247aa3664618a65c
ms.sourcegitcommit: c315ec3e17489aeee19f5095ec4af613ad2837e1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/04/2019
ms.locfileid: "58921683"
---
# <a name="get-started-with-cwinrt"></a>C++/WinRT 入门

若要获取你掌握使用[ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)，本主题将指导完成一个简单的代码示例基于新**Windows 控制台应用程序 (C++/WinRT)** 项目。 本主题演示如何[添加C++/WinRT 支持添加到 Windows 桌面应用程序项目](#modify-a-windows-desktop-application-project-to-add-cwinrt-support)。

> [!IMPORTANT]
> 如果您使用的 Visual Studio 2017 (版本 15.8.0 或更高版本)，以及针对 Windows SDK 版本 10.0.17134.0 (Windows 10，版本 1803年)，然后对新创建C++/WinRT 项目可能无法编译出现错误"*错误 C3861: from_abi:找不到标识符*"，并与来自其他错误同时*base.h*。 解决方法到任一目标是更高版本的 （更符合） 版本的 Windows SDK 或设置项目属性**C /C++** > **语言** >  **符合模式：否**(此外，如果**触发-** 出现在项目属性**C /C++** > **语言** >  **Command Line**下**其他选项**，然后将其删除)。

## <a name="a-cwinrt-quick-start"></a>C++/WinRT 快速入门

> [!NOTE]
> 有关如何安装和使用信息C++WinRT Visual Studio 扩展 (VSIX) 和 NuGet 包 （该一起提供项目模板，并生成支持），请参阅[适用于 Visual Studio 支持C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

创建一个新的 **Windows 控制台应用程序(C++/WinRT)** 项目。

按下面所示编辑 `pch.h` 和 `main.cpp`。

```cppwinrt
// pch.h
...
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
...
```

```cppwinrt
// main.cpp
#include "pch.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

int main()
{
    winrt::init_apartment();

    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
    for (const SyndicationItem syndicationItem : syndicationFeed.Items())
    {
        winrt::hstring titleAsHstring = syndicationItem.Title().Text();
        std::wcout << titleAsHstring.c_str() << std::endl;
    }
}
```

我们来逐步了解上述简短代码示例，并说明每一部分的内容。

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
```

我们包含的标头是 SDK 的一部分，位于文件夹 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 内。 Visual Studio 将该路径包含在其 *IncludePath* 宏中。 这些标头包含投影到 C++/WinRT 中的 Windows API。 换言之，对于每个 Windows 类型，C++/WinRT 都会定义 C++ 友好等效项（称为*投影类型*）。 投影类型具有与 Windows 类型相同的完全限定名称，但放置于 C++ **winrt** 命名空间中。 将这些内容放置在预编译标头中将减少增量生成时间。

> [!IMPORTANT]
> 如果希望使用来自 Windows 命名空间的类型，请包括对应的 C++/WinRT Windows 命名空间标头文件，如下所示。 *对应*标头是与该类型的命名空间具有相同名称的标头。 例如，若要为 [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset) 运行时类使用 C++/WinRT 投影，则应 `#include <winrt/Windows.Foundation.Collections.h>`。

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

`using namespace` 指令是可选的，不过这种指令很方便。 上方显示的此类指令的模式（允许查找 **winrt** 命名空间中任何项目的非限定名称）适用于当你开始新项目且 C++/WinRT 是你在该项目内使用的唯一语言投影的情况。 另一方面，如果你在将 C++/WinRT 代码与 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 和/或 SDK 应用程序二进制接口 (ABI) 代码混合（从其移植或与其互操作，其中一个模型或全部两个模型），则请参阅主题[实现 C++/WinRT 与 C++/CX 之间的互操作](interop-winrt-cx.md)、[从 C++/CX 移动到 C++/WinRT](move-to-winrt-from-cx.md) 和[实现 C++/WinRT 与 ABI 之间的互操作](interop-winrt-abi.md)。

```cppwinrt
winrt::init_apartment();
```

调用 **winrt::init_apartment** 将初始化 COM；默认情况下，使用多线程单元。

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

堆叠分配两个对象：它们表示 Windows 博客的 uri 和联合客户端。 我们使用具有简单的宽字符串参数的 uri（请参阅 [C++/WinRT 中的字符串处理](strings.md)了解使用字符串的更多方法）。

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync** ](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)是异步 Windows 运行时函数的示例。 该代码示例将接收来自 **RetrieveFeedAsync** 的异步操作对象，然后对该对象调用 **get** 以阻止调用线程并等待结果（在此例中为联合源）。 要获得有关并发的详细信息和了解非阻止性技术，请参阅 [C++/WinRT 的并发和异步操作](concurrency.md)。

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items** ](/uwp/api/windows.web.syndication.syndicationfeed.items)是由迭代器返回的定义的范围内**开始**并**最终**函数 （或其常量、 反向，和常量反向变体）。 因此，你可以使用基于范围的 `for` 语句或使用 **std::for_each** 模板函数枚举**项目**。

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

获取源的标题文本以作为 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 对象（更多详细信息请参阅 [C++/WinRT 中的字符串处理](strings.md)）。 **hstring** 然后通过 **c_str** 函数输出，这反映使用 C++ 标准库字符串的模式。

如你所见，C++/WinRT 鼓励使用与类相似的新式 C++ 表达式，例如 `syndicationItem.Title().Text()`。 这是与传统的 COM 编程不同的更简洁的编程风格。 你无需直接初始化 COM、处理 COM 指针。

也不需要处理 HRESULT 返回代码。 C++/WinRT 会将错误 HRESULT 转换为异常（如 [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)）以实现自然、现代化的编程风格。 有关错误处理以及代码示例的详细信息，请参阅 [C++/WinRT 的错误处理](error-handling.md)。

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>修改 Windows 桌面应用程序项目，以添加C++/WinRT 支持

本部分演示如何添加C++/WinRT 支持添加到可能有一个 Windows 桌面应用程序项目。 如果没有现有的 Windows 桌面应用程序项目，然后你可以遵循这些步骤按第一个创建一个。 例如，打开 Visual Studio 并创建**可视化C++**  \> **Windows 桌面** \> **Windows 桌面应用程序**项目。

您可以选择安装[ C++WinRT Visual Studio 扩展 (VSIX)](https://aka.ms/cppwinrt/vsix)和 NuGet 包。 有关详细信息，请参阅[适用于 Visual Studio 支持C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

### <a name="set-project-properties"></a>设置项目属性

转到项目属性**常规** \> **Windows SDK 版本**，然后选择**所有配置**并**所有平台**。 絋粄**Windows SDK 版本**设置为 10.0.17134.0 (Windows 10，版本 1803年) 或更高版本。

确认您不受[我的新项目将不会为什么编译？](/windows/uwp/cpp-and-winrt-apis/faq)。

因为C++/WinRT 使用 C + + 17 标准，将项目属性中的功能**C /C++** > **语言** >   **C++语言标准**到*ISO C + + 17 标准 (/ /std: c + + 17)*。

### <a name="the-precompiled-header"></a>预编译标头

默认项目模板为你命名为创建预编译标头`framework.h`，或`stdafx.h`。 重命名为`pch.h`。 如果有`stdafx.cpp`文件，然后重命名为`pch.cpp`。 设置项目属性**C /C++** > **预编译标头** > **预编译头文件**到*pch.h*.

查找和替换所有`#include "framework.h"`(或`#include "stdafx.h"`) 与`#include "pch.h"`。

在中`pch.h`，包括`winrt/base.h`。

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>链接

C++/WinRT 语言投影依赖于某些 Windows 运行时可用 （非成员） 函数和入口点，需要将链接到[WindowsApp.lib](/uwp/win32-and-com/win32-apis)涵盖性库。 本部分介绍三种方法满足链接器。

第一个选项是将添加到您的 Visual Studio 项目的所有C++WinRT MSBuild 属性和目标。 若要执行此操作，安装[Microsoft.Windows.CppWinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)到你的项目。 打开 Visual Studio 项目中，单击**项目** \> **管理 NuGet 包...**\> **浏览**，键入或粘贴**Microsoft.Windows.CppWinRT**在搜索框中，在搜索结果中选择的项，然后单击**安装**若要安装该项目的包。

此外可以使用项目链接设置为显式链接`WindowsApp.lib`。 或者，你可以执行此操作在源代码中 (在`pch.h`，例如) 如下所示。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

现在可以编译和链接，并添加C++到你的项目的 /WinRT 代码 (例如，如中所示类似的代码[A C++/WinRT 快速入门](#a-cwinrt-quick-start)部分中，更高版本)。

## <a name="important-apis"></a>重要的 API
* [SyndicationClient::RetrieveFeedAsync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items 属性](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring 结构](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::hresult 错误结构](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT 的错误处理](error-handling.md)
* [实现 C++/WinRT 与 C++/CX 之间的互操作](interop-winrt-cx.md)
* [实现 C++/WinRT 与 ABI 之间的互操作](interop-winrt-abi.md)
* [从 C++/CX 移动到 C++/WinRT](move-to-winrt-from-cx.md)
* [C++/WinRT 中的字符串处理](strings.md)
