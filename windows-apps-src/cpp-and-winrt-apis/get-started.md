---
description: 为了帮助你更快地开始使用 C++/WinRT，本主题详细介绍了一个简单的代码示例。
title: C++/WinRT 入门
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 入门, 开始使用
ms.localizationpriority: medium
ms.openlocfilehash: c058a727e09f00e01664c314d8c198f3f25e841e
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "74255132"
---
# <a name="get-started-with-cwinrt"></a>C++/WinRT 入门

为帮助你快速掌握 [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 的用法，本主题将会根据新的 **Windows 控制台应用程序 (C++/WinRT)** 项目演练一个简单的代码示例。 此外，本主题将会介绍如何[将 C++/WinRT 支持添加到 Windows 桌面应用程序项目](#modify-a-windows-desktop-application-project-to-add-cwinrt-support)。

> [!NOTE]
> 我们建议使用最新版本的 Visual Studio 和 Windows SDK 进行开发。如果你使用的是 Visual Studio 2017（15.8.0 或更高版本）并且面向 Windows SDK 版本 10.0.17134.0（Windows 10 版本 1803），则新建的 C++/WinRT 项目可能无法编译并出现错误“错误 C3861: 'from_abi': 找不到标识符”，以及源自 *base.h* 的其他错误。  解决方法是要么面向 Windows SDK 的更高（更相符）版本，要么设置项目属性“C/C++” > “语言” > “一致性模式:    否”（此外，如果 **/permissive-** 显示在“其他选项”下的项目属性“C/C++” > “语言” > “命令行”中，请将其删除）。    

## <a name="a-cwinrt-quick-start"></a>C++/WinRT 快速入门

> [!NOTE]
> 有关设置 Visual Studio 以进行 C++/WinRT 部署的信息 &mdash; 包括安装和使用 C++/WinRT Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息 &mdash; 请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

创建一个新的 **Windows 控制台应用程序(C++/WinRT)** 项目。

按如下所示编辑 `pch.h` 和 `main.cpp`。

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
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

我们将分析上述简短代码示例的每个片段，并解释每个部分的作用。

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>
```

包含的标头采用默认项目设置，来自 Windows SDK 的 `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt` 文件夹。 Visual Studio 将该路径包含在其 *IncludePath* 宏中。 但这些标头并不严格依赖于 Windows SDK，因为项目会（通过 `cppwinrt.exe` 工具）在项目的 *$(GeneratedFilesDir)* 文件夹中生成与此相同的标头。 如果在其他位置找不到这些标头，或者你更改了项目设置，则会从该文件夹中加载这些标头。

这些标头包含投影到 C++/WinRT 的 Windows API。 换言之，对于每个 Windows 类型，C++/WinRT 都会定义 C++ 友好等效项（称为“投影类型”）  。 投影类型具有与 Windows 类型相同的完全限定名称，但放置于 C++ **winrt** 命名空间中。 将这些内容放置在预编译标头中将减少增量生成时间。

> [!IMPORTANT]
> 如果希望使用来自 Windows 命名空间的类型，请包括对应的 C++/WinRT Windows 命名空间头文件，如上所示。 对应的标头是与该类型的命名空间具有相同名称的标头  。 例如，若要为 [**Windows::Foundation::Collections::PropertySet**](/uwp/api/windows.foundation.collections.propertyset) 运行时类使用 C++/WinRT 投影，则应包含 `#include <winrt/Windows.Foundation.Collections.h>`。 如果包含 `winrt/Windows.Foundation.Collections.h`，则不需要同时包含 `winrt/Windows.Foundation.h`。  每个 C++/WinRT 投影标头将自动包括其父命名空间头文件；因此你不需要显式包含它  。 不过，这样做也不会出现错误。

```cppwinrt
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;
```

`using namespace` 指令是可选的，不过这种指令很方便。 上方显示的此类指令的模式（允许查找 **winrt** 命名空间中任何项目的非限定名称）适用于当你开始新项目且 C++/WinRT 是你在该项目内使用的唯一语言投影的情况。 另一方面，如果你在将 C++/WinRT 代码与 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 和/或 SDK 应用程序二进制接口 (ABI) 代码混合（从其移植或与其互操作，其中一个模型或全部两个模型），则请参阅主题[实现 C++/WinRT 与 C++/CX 之间的互操作](interop-winrt-cx.md)、[从 C++/CX 移动到 C++/WinRT](move-to-winrt-from-cx.md) 和[实现 C++/WinRT 与 ABI 之间的互操作](interop-winrt-abi.md)。

```cppwinrt
winrt::init_apartment();
```

调用 **winrt::init_apartment** 会初始化 Windows 运行时中（默认在多线程单元中）的线程。 该调用还会初始化 COM。

```cppwinrt
Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
SyndicationClient syndicationClient;
```

堆栈分配两个对象：它们表示 Windows 博客的 URI 和联合客户端。 我们将使用具有简单的宽字符串参数的 uri（请参阅 [C++/WinRT 中的字符串处理](strings.md)了解使用字符串的更多方法）。

```cppwinrt
SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
```

[**SyndicationClient::RetrieveFeedAsync**](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) 是异步 Windows 运行时函数的示例。 该代码示例将接收来自 **RetrieveFeedAsync** 的异步操作对象，然后对该对象调用 **get** 以阻止调用线程并等待结果（在此例中为联合源）。 要获得有关并发的详细信息和了解非阻止性技术，请参阅 [C++/WinRT 的并发和异步操作](concurrency.md)。

```cppwinrt
for (const SyndicationItem syndicationItem : syndicationFeed.Items()) { ... }
```

[**SyndicationFeed.Items**](/uwp/api/windows.web.syndication.syndicationfeed.items) 是一个范围，由从 **begin** 和 **end** 函数（或其常量、反向和常量-反向变体）返回的迭代程序定义。 因此，可以使用基于范围的 `for` 语句或使用 **std::for_each** 模板函数枚举**项**。 循环访问此类 Windows 运行时集合时，需要指定 `#include <winrt/Windows.Foundation.Collections.h>`。

```cppwinrt
winrt::hstring titleAsHstring = syndicationItem.Title().Text();
std::wcout << titleAsHstring.c_str() << std::endl;
```

获取源的标题文本以作为 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 对象（有关更多详细信息，请参阅 [C++/WinRT 中的字符串处理](strings.md)）。 然后，**hstring** 通过 **c_str** 函数输出，这反映使用 C++ 标准库字符串的模式。

可以看到，C++/WinRT 鼓励使用类似于类的新式 C++ 表达式，例如 `syndicationItem.Title().Text()`。 这是与传统的 COM 编程不同的更简洁的编程风格。 无需直接初始化 COM，也无需处理 COM 指针。

也不需要处理 HRESULT 返回代码。 C++/WinRT 会将错误 HRESULT 转换为异常（如 [**winrt::hresult-error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)）以实现自然、现代化的编程风格。 有关错误处理以及代码示例的详细信息，请参阅 [C++/WinRT 的错误处理](error-handling.md)。

## <a name="modify-a-windows-desktop-application-project-to-add-cwinrt-support"></a>修改 Windows 桌面应用程序项目以添加 C++/WinRT 支持

本部分介绍如何将 C++/WinRT 支持添加到 Windows 桌面应用程序项目。 如果你没有 Windows 桌面应用程序项目，可以先遵循以下步骤创建一个。 例如，打开 Visual Studio 并选择“Visual C++”\>“Windows 桌面”\>“Windows 桌面应用程序”来创建一个项目。   

可以选择性地安装 [C++/WinRT Visual Studio 扩展 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) 和 NuGet 包。 有关详细信息，请参阅 [C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

### <a name="set-project-properties"></a>设置项目属性

转到项目属性“常规”\>“Windows SDK 版本”，然后选择“所有配置”和“所有平台”。     确保“Windows SDK 版本”设置为 10.0.17134.0（Windows 10 版本 1803）或更高。 

确认你没有遇到[为何我的新项目不能编译？](/windows/uwp/cpp-and-winrt-apis/faq)的问题。

由于 C++/WinRT 使用 C++17 标准版中的功能，请将项目属性“C/C++” > “语言” > “C++ 语言标准版”设置为“ISO C++17 标准版(/std:c++17)”。    

### <a name="the-precompiled-header"></a>预编译的标头

默认项目模板将为你创建名为 `framework.h` 或 `stdafx.h` 的预编译标头。 请将它重命名为 `pch.h`。 如果已有一个 `stdafx.cpp` 文件，请将它重命名为 `pch.cpp`。 将项目属性“C/C++” > “预编译标头” > “预编译标头”设置为“创建(/Yc)”，将“预编译头文件”设置为“pch.h”。      

查找所有 `#include "framework.h"`（或 `#include "stdafx.h"`）并将其替换为 `#include "pch.h"`。

在 `pch.h` 中包含 `winrt/base.h`。

```cppwinrt
// pch.h
...
#include <winrt/base.h>
```

### <a name="linking"></a>链接

C++/WinRT 语言投影依赖于某些 Windows 运行时自由（非成员）函数和入口点，需要链接到 [WindowsApp.lib](/uwp/win32-and-com/win32-apis) 伞型库。 本部分介绍满足链接器要求的三种方式。

第一种做法是将所有 C++/WinRT MSBuild 属性和目标添加到 Visual Studio 项目。 为此，请在项目中安装 [Microsoft.Windows.CppWinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)。 在 Visual Studio 中打开项目，然后单击“项目”\>“管理 NuGet 包...”   \>“浏览”，在搜索框中键入或粘贴 Microsoft.Windows.CppWinRT  ，在搜索结果中选择该项，然后单击“安装”以安装该项目的包。  

也可以使用项目链接设置来显式链接 `WindowsApp.lib`。 或者，可以在源代码中（例如，在 `pch.h` 中）按如下所示执行此操作。

```cppwinrt
#pragma comment(lib, "windowsapp")
```

现在，可以编译、链接 C++/WinRT 代码并将其添加到项目（例如，类似于前面 [C++/WinRT 快速入门](#a-cwinrt-quick-start)部分所示的代码）。

## <a name="the-three-main-scenarios-for-cwinrt"></a>C++/WinRT 的三大应用方案

在使用和熟悉 C++/WinRT 的过程中，以及在阅读本文档余下内容的过程中，你可能会注意到有三大应用方案，详见后面部分的介绍。

### <a name="consuming-windows-runtime-apis-and-types"></a>使用 Windows 运行时 API 和类型

也就是说，使用或调用 API。   例如，通过 API 调用使用蓝牙进行通信、流式传输和提供视频、与 Windows shell 集成，等等。 C++/WinRT 完全支持此类方案。 有关详细信息，请参阅[通过 C++/WinRT 使用 API](/windows/uwp/cpp-and-winrt-apis/consume-apis)。

### <a name="authoring-windows-runtime-apis-and-types"></a>创作 Windows 运行时 API 和类型

也就是说，生成 API 和类型。  例如，生成上一部分介绍的 API 类型、图形 API、存储和文件系统 API、网络 API 等。 有关详细信息，请参阅[使用 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis)。

使用 C++/WinRT 创作 API 涉及的事项要稍多于使用 API 的情况，因为你必须使用 IDL 来定义 API 的形状，然后才能实现它。 [XAML 控件；绑定到 C++/WinRT 属性](/windows/uwp/cpp-and-winrt-apis/binding-property)中详述了此方面的操作。

### <a name="xaml-applications"></a>XAML 应用程序

此方案涉及在 XAML UI 框架上构建应用程序和控件。 在 XAML 应用程序中工作相当于既要使用，又要创作。 但是，由于 XAML 是当今的 Windows 主流 UI 框架，其对 Windows 运行时的影响也同样很大，因此有必要专门设置一个它的应用方案类别。

请注意，XAML 最适用于提供反射的编程语言。 在 C++/WinRT 中，有时需要做一些额外的工作才能与 XAML 框架互操作。 所有这些情况均在相应文档中进行了介绍。 可以从 [XAML 控件；绑定到 C++/WinRT 属性](/windows/uwp/cpp-and-winrt-apis/binding-property)和 [XAML 自定义（模板化）控件与 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl) 着手。

## <a name="important-apis"></a>重要的 API
* [SyndicationClient::RetrieveFeedAsync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed.Items 属性](/uwp/api/windows.web.syndication.syndicationfeed.items)
* [winrt::hstring 结构](/uwp/cpp-ref-for-winrt/hstring)
* [winrt::hresult-error 结构](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C++/WinRT 的错误处理](error-handling.md)
* [实现 C++/WinRT 与 C++/CX 之间的互操作](interop-winrt-cx.md)
* [实现 C++/WinRT 与 ABI 之间的互操作](interop-winrt-abi.md)
* [从 C++/CX 移动到 C++/WinRT](move-to-winrt-from-cx.md)
* [C++/WinRT 中的字符串处理](strings.md)
