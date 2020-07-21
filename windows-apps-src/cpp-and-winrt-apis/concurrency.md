---
description: 本主题介绍了你可通过 C++/WinRT 创建和使用 Windows 运行时异步对象的方式。
title: 使用 C++/WinRT 执行并发和异步操作
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 并发, async, 异步
ms.localizationpriority: medium
ms.openlocfilehash: 048d6fe455f7c3e77922ef8b937a9cb1d6cbb21c
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "81266895"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>使用 C++/WinRT 执行并发和异步操作

> [!IMPORTANT]
> 本主题介绍协同例程  和 `co_await` 的概念，我们建议你在 UI 应用程序和非 UI 应用程序中使用它们。  为了简单起见，本介绍主题中的大多数代码示例演示了 **Windows 控制台应用程序 (C++/WinRT)** 项目。 本主题中后面的代码示例使用协同例程，但为方便起见，控制台应用程序示例还会在退出前继续使用阻止性的 **get** 函数调用，这样应用程序就不会在显示其输出之前退出。 不要通过 UI 线程这样做（调用阻止性的 **get** 函数）， 而应使用 `co_await` 语句。 [更高级的并发和异步](concurrency-2.md)主题介绍了将要在 UI 应用程序中使用的技术。

本简介性主题介绍了可通过 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 创建和使用 Windows 运行时异步对象的部分方式。 阅读本主题后，如需其他技术，尤其是将要在 UI 应用程序中使用的技术，另请参阅[更高级的并发和异步](concurrency-2.md)。

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>异步操作和 Windows 运行时“Async”函数

有可能需要超过 50 毫秒才能完成的任何 Windows 运行时 API 将实现为异步函数（具有一个以“Async”结尾的名称）。 异步函数的实现会启动另一线程上的工作，并且会立即返回表示异步操作的对象。 在异步操作完成后，返回的对象会包含从该工作中生成的任何值。 **Windows::Foundation** Windows 运行时命名空间包含四种类型的异步操作对象。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)；
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress-1)；
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation-1)；
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)。

每种异步操作类型都将投影到 **winrt::Windows::Foundation** C++/WinRT 命名空间中的相应类型。 C++/WinRT 还包含内部 await 适配器结构。 不要直接使用它，但借助该结构，可以编写 `co_await` 语句以协作等待返回其中一种异步操作类型的任何函数的结果。 然后，可以自行创作返回这些类型的协同例程。

异步 Windows 函数的示例是 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)，其返回类型 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress-2) 的异步操作对象。

让我们来看一些阻塞和不阻塞使用 C++/WinRT 来调用类似 API 的方法。 我们将在接下来的几个代码示例中使用 **Windows 控制台应用程序 (C++/WinRT)** 项目，只为说明基本的概念。 更适用于 UI 应用程序的技术在[更高级的并发和异步](concurrency-2.md)中讨论。

## <a name="block-the-calling-thread"></a>阻塞调用线程

以下代码示例接收来自 **RetrieveFeedAsync** 的异步操作对象，并且在该对象上调用 **get** 以阻塞调用线程，直到异步操作的结果可用。

若要将此示例直接复制并粘贴到 **Windows 控制台应用程序 (C++/WinRT)** 项目的主源代码文件中，请先在项目属性中设置“不使用预编译的标头”。 

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeed()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
    // use syndicationFeed.
}

int main()
{
    winrt::init_apartment();
    ProcessFeed();
}
```

调用 **get** 可以方便编写代码，对于出于任何原因不想使用协同例程的控制台应用或后台线程来说，这是一种理想选择。 但这既不是并发也不是异步操作，因此不适合 UI 线程（如果试图在 UI 线程上使用它，会在未优化的版本中触发断言）。 为了避免占用 OS 线程执行其他有用的工作，我们需要另一种方法。

## <a name="write-a-coroutine"></a>编写协同例程

C++/WinRT 将 C++ 协同例程集成到编程模型中以提供协作等待结果的自然方式。 可以通过编写协同例程来生成自己的 Windows 运行时异步操作。 在以下代码示例中，**ProcessFeedAsync** 是协同例程。

> [!NOTE]
> **get** 函数位于 C++/WinRT 投影类型 **winrt::Windows::Foundation::IAsyncAction** 中，因此你可以从任意 C++/WinRT 项目内部调用该函数。 你将找不到列为 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) 接口成员的函数，因为 **get** 不属于实际 Windows 运行时类型 **IAsyncAction** 的应用程序二进制接口 (ABI) 设计面。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    PrintFeed(syndicationFeed);
}

int main()
{
    winrt::init_apartment();

    auto processOp{ ProcessFeedAsync() };
    // do other work while the feed is being printed.
    processOp.get(); // no more work to do; call get() so that we see the printout before the application exits.
}
```

协同例程是可以暂停和恢复的函数。 在上述 **ProcessFeedAsync** 协同例程中，当达到 `co_await` 语句时，该协同例程会异步启动 **RetrieveFeedAsync** 调用，然后立即暂停自身并将控件返回到调用方（上述示例中为 **main**）。 然后，**main** 可以继续执行工作，同时将检索并打印提要。 完成该操作（**RetrieveFeedAsync** 调用完成）后，**ProcessFeedAsync** 协同例程将在下一个语句中恢复。

可以将一个协同例程聚合到其他协同例程中。 或者，也可以调用 **get** 以阻塞和等待其完成（以及获得结果，如果有）。 或者，可以将其传递到支持 Windows 运行时的其他编程语言。

也可以通过使用委托来处理异步操作的已完成和/或正在进行中的事件。 有关详细信息和代码示例，请参阅[异步操作的委托类型](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)。

正如你所看到的，在上面的代码示例中，我们在退出 **main** 之前继续使用阻止性的 **get** 函数调用。 但是，这只是为了让应用程序不会在显示其输出之前退出。

## <a name="asynchronously-return-a-windows-runtime-type"></a>异步返回 Windows 运行时类型

在下一个示例中，我们将针对特定 URI 封装对 **RetrieveFeedAsync** 的调用，以为我们提供异步返回  SyndicationFeed[**的**RetrieveBlogFeedAsync](/uwp/api/windows.web.syndication.syndicationfeed) 函数。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void PrintFeed(SyndicationFeed const& syndicationFeed)
{
    for (SyndicationItem const& syndicationItem : syndicationFeed.Items())
    {
        std::wcout << syndicationItem.Title().Text().c_str() << std::endl;
    }
}

IAsyncOperationWithProgress<SyndicationFeed, RetrievalProgress> RetrieveBlogFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    return syndicationClient.RetrieveFeedAsync(rssFeedUri);
}

int main()
{
    winrt::init_apartment();

    auto feedOp{ RetrieveBlogFeedAsync() };
    // do other work.
    PrintFeed(feedOp.get());
}
```

在上述示例中，**RetrieveBlogFeedAsync** 返回 **IAsyncOperationWithProgress**，其具有进度值和返回值。 我们可以在 **RetrieveBlogFeedAsync** 执行其操作并检索提要的同时进行其他工作。 然后，在该异步操作对象上调用 **get**，以阻塞、等待其完成，然后获取该操作的结果。

如果要异步返回 Windows 运行时类型，则应返回 [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation-1) 或 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)。 任何第一方或第三方运行时类或可以传入/传出 Windows 运行时函数的任何类型（例如 `int` 或 **winrt::hstring**）都符合条件。 如果尝试对非 Windows 运行时类型使用其中一种异步操作类型，编译器可帮助你处理“必须为 WinRT 类型”错误  。

如果协同例程没有至少一条 `co_await` 语句，则为了符合成为协同例程的资格，它必须至少有一条 `co_return` 或一条 `co_yield` 语句。 在某些情况下，协同例程可以返回值而不引入任何异步，因此不阻塞也不切换上下文。 下面是一个通过缓存值来实现上述功能（第二次及后续调用时）的示例。

```cppwinrt
winrt::hstring m_cache;

IAsyncOperation<winrt::hstring> ReadAsync()
{
    if (m_cache.empty())
    {
        // Asynchronously download and cache the string.
    }
    co_return m_cache;
}
``` 

## <a name="asynchronously-return-a-non-windows-runtime-type"></a>异步返回非 Windows 运行时类型

如果要异步返回非 Windows 运行时类型的类型，则应返回并行模式库 (PPL)  concurrency::task[ **。** ](/cpp/parallel/concrt/reference/task-class) 建议使用 **concurrency::task**，因为它将提供比 **std::future** 更好的性能（以及更好的兼容性）。

> [!TIP]
> 如果包含 `<pplawait.h>`，则可以使用 **concurrency::task** 作为协同例程类型。

```cppwinrt
// main.cpp
#include <iostream>
#include <ppltasks.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

concurrency::task<std::wstring> RetrieveFirstTitleAsync()
{
    return concurrency::create_task([]
        {
            Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
            SyndicationClient syndicationClient;
            SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
            return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
        });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp{ RetrieveFirstTitleAsync() };
    // Do other work here.
    std::wcout << firstTitleOp.get() << std::endl;
}
```

## <a name="parameter-passing"></a>参数传递

对于同步函数，默认情况下应该使用 `const&` 参数。 这将避免复制开销（涉及引用计数，意味着互锁的增加和减少）。

```cppwinrt
// Synchronous function.
void DoWork(Param const& value);
```

但如果向协同例程传递引用参数，可能会遇到问题。

```cppwinrt
// NOT the recommended way to pass a value to a coroutine!
IASyncAction DoWorkAsync(Param const& value)
{
    // While it's ok to access value here...

    co_await DoOtherWorkAsync(); // (this is the first suspension point)...

    // ...accessing value here carries no guarantees of safety.
}
```

在协同程序中，在第一个暂停点之前，执行是同步的；到达第一个暂停点时，控制返回到调用方，调用帧超出范围。 在协同例程恢复时，引用参数引用的源值可能已发生更改。 从协同例程的角度来看，引用参数具有不受控制的生命周期。 因此，在上面的示例中，在 *之前，我们可以安全地访问*value`co_await`，但之后就无法保证安全了。 如果调用方销毁了 *value*，则尝试在协同例程中访问它会导致内存损坏。 如果 *DoOtherWorkAsync* 函数有可能暂停并在恢复后尝试使用 **value**，我们也无法安全地将 *value* 传递给 DoOtherWorkAsync。

为了能够在暂停和恢复后安全地使用参数，默认情况下，协同例程应使用按值传递，以确保按值进行捕获并避免生命周期问题。 确信不遵从该指引也能安全进行操作的情况是很少见的。

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value); // not const&
```

按值传递要求参数的移动或复制开销不高，智能指针通常就是这样的。

传递 const 值是否是一个好的做法也还存在争议（除非你想移动值）。 它不会对要复制的源值产生任何影响，但有助于表明意图，并避免你无意间修改副本。

```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

另请参阅[标准数组和向量](std-cpp-data-types.md#standard-arrays-and-vectors)，其中介绍了如何将标准向量传递到异步被调用方。

如果不能更改协同例程的签名，但是能够更改实现，则可在首次执行 `co_await` 之前进行本地复制。

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_value = value;
    // It's ok to access both safe_value and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_value here (not value).
}
```

如果 `Param` 复制起来开销很大，则在首次执行 `co_await` 之前只提取所需的片段。

```cppwinrt
IASyncAction DoWorkAsync(Param const& value)
{
    auto safe_data = value.data;
    // It's ok to access safe_data, value.data, and value here.

    co_await DoOtherWorkAsync();

    // It's ok to access only safe_data here (not value.data, nor value).
}
```

## <a name="safely-accessing-the-this-pointer-in-a-class-member-coroutine"></a>在类成员协同例程中安全访问 *this* 指针

请参阅 [C++/WinRT 中的强引用和弱引用](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)。

## <a name="important-apis"></a>重要的 API
* [concurrency::task 类](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction 接口](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; 接口](/uwp/api/windows.foundation.iasyncactionwithprogress-1)
* [IAsyncOperation&lt;TResult&gt; 接口](/uwp/api/windows.foundation.iasyncoperation-1)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 接口](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)
* [SyndicationClient::RetrieveFeedAsync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed 类](/uwp/api/windows.web.syndication.syndicationfeed)

## <a name="related-topics"></a>相关主题
* [更高级的并发和异步](concurrency-2.md)
* [在 C++/WinRT 中使用委托处理事件](handle-events.md)
* [标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)