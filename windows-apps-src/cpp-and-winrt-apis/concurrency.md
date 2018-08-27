---
author: stevewhims
description: 本主题介绍你可通过 C++/WinRT 创建和使用 Windows 运行时异步对象的方式。
title: 通过 C++/WinRT 的并发和异步操作
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 并发, 异步, 异步的, 异步
ms.localizationpriority: medium
ms.openlocfilehash: fe43eaa233d3384eecb5e8755190efc1a109bbb9
ms.sourcegitcommit: 753dfcd0f9fdfc963579dd0b217b445c4b110a18
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2018
ms.locfileid: "2855853"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrtwindowsuwpcpp-and-winrt-apisintro-to-using-cpp-with-winrt"></a>通过 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 的并发和异步操作
> [!NOTE]
> **与在商业发行之前可能会进行实质性修改的预发布产品相关的一些信息。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。**

本主题介绍你可通过 C++/WinRT 创建和使用 Windows 运行时异步对象的方式。

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>异步操作和 Windows 运行时“异步”函数
有可能需要超过 50 毫秒才能完成的任何 Windows 运行时 API 将实现为异步函数（具有一个以“Async”结尾的名称）。 异步函数的实现将启动另一线程上的工作，并且立即返回表示异步操作的对象。 在异步操作完成后，返回的对象会包含从该工作中生成的任何值。 **Windows::Foundation** Windows 运行时命名空间包含四种类型的异步操作对象。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)、
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)、
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) 和
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。

每种异步操作类型都将投影到 **winrt::Windows::Foundation** C++/WinRT 命名空间中的相应类型。 C++/WinRT 还包含内部 await 适配器结构。 不要直接使用它，但借助该结构，可以编写 co_await 语句以协作等待返回其中一种异步操作类型的任何函数的结果。 然后，你可以自行创作返回这些类型的协同程序。

异步 Windows 函数的示例是 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)，其返回类型 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 的异步操作对象。 让我们来看一些阻止和不阻止使用 C++/WinRT 来调用类似 API 的方法。

## <a name="block-the-calling-thread"></a>阻止调用线程
以下代码示例接收来自 **RetrieveFeedAsync** 的异步操作对象，并且在该对象上调用 **get** 以阻止调用线程，直到异步操作的结果可用。

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

void ProcessFeed()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;
    SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
    // use syndicationFeed.
}

int main()
{
    winrt::init_apartment();
    ProcessFeed();
}
```

调用 **get** 可以方便编码，对于出于任何原因不想使用协同程序的控制台应用或后台线程来说，这是一种理想选择。 但这既不是并发也不是异步操作，因此不适合 UI 线程（如果试图在 UI 线程上使用它，会在未优化的版本中触发断言）。 为了避免占用 OS 线程执行其他有用的工作，我们需要另一种方法。

## <a name="write-a-coroutine"></a>编写协调程序
C++/WinRT 将 C++ 协同程序集成到编程模型中以提供协作等待结果的自然方式。 你可以通过编写协同程序来生成自己的 Windows 运行时异步操作。 在以下代码示例中，**ProcessFeedAsync** 是协同程序。

> [!NOTE]
> **获取**函数存在于 C + + / WinRT 投影键入**winrt::Windows::Foundation::IAsyncAction**，以便您可以调用该功能中任何 C + + / WinRT 项目。 因为**获取**不是实际的 Windows Runtime 类型**IAsyncAction**的应用程序二进制接口 (ABI) 表面的一部分，您将无法找到列出该[**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)接口的成员身份的函数。

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

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

    auto processOp = ProcessFeedAsync();
    // do other work while the feed is being printed.
    processOp.get(); // no more work to do; call get() so that we see the printout before the application exits.
}
```

协同程序是可暂停和恢复的函数。 在上述 **ProcessFeedAsync** 协同程序中，当达到 `co_await` 语句时，该协同程序会异步启动 **RetrieveFeedAsync** 调用，然后立即暂停自身并将控件返回到调用方（上述示例中为 **main**）。 然后，**main** 可以继续执行工作，同时将检索并打印提要。 完成该操作（**RetrieveFeedAsync** 调用完成）后，**ProcessFeedAsync** 协同程序将在下一个语句中恢复。

你可以将一个协同程序聚合到其他协同程序中。 或者，也可以调用 **get** 以阻止和等待其完成（以及获得结果，如果有）。 或者，可以将其传递到支持 Windows 运行时的其他编程语言。

也可以通过使用委托来处理异步操作的已完成和/或正在进行中的事件。 有关详细信息和代码示例，请参阅[异步操作的委托类型](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)。

## <a name="asychronously-return-a-windows-runtime-type"></a>异步返回 Windows 运行时类型
在下一个示例中，我们将针对特定 URI 封装对 **RetrieveFeedAsync** 的调用，以为我们提供异步返回 [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed) 的 **RetrieveBlogFeedAsync** 函数。

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>

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

    auto feedOp = RetrieveBlogFeedAsync();
    // do other work.
    PrintFeed(feedOp.get());
}
```

在上述示例中，**RetrieveBlogFeedAsync** 返回 **IAsyncOperationWithProgress**，其具有进度值和返回值。 我们可以在 **RetrieveBlogFeedAsync** 执行其操作并检索提要的同时进行其他工作。 然后，我们在该异步操作对象上调用 **get**，以阻止、等待其完成，然后获取该操作的结果。

如果要异步返回 Windows 运行时类型，则应返回 [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) 或 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。 任何第一方或第三方运行时类或可以传入/传出 Windows 运行时函数的任何类型（例如 `int` 或 **winrt::hstring**）都符合资格。 如果你尝试对非 Windows 运行时类型使用其中一种异步操作类型，编译器将帮助你处理“*必须为 WinRT 类型*”错误。

如果协同程序没有至少一条 `co_await` 语句，则为了符合成为协同程序的资格，它必须至少有一条 `co_return` 或一条 `co_yield` 语句。 在某些情况下，协同程序可以返回值而不引入任何异步，因此不阻塞也不切换上下文。 下面是一个通过缓存值来实现上述功能（第二次及后续调用时）的示例。

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

## <a name="asychronously-return-a-non-windows-runtime-type"></a>异步返回非 Windows 运行时类型
如果要异步返回*非* Windows 运行时类型的类型，则应返回并行模式库 (PPL) [**concurrency::task**](/cpp/parallel/concrt/reference/task-class)。 建议使用 **concurrency::task**，因为它将提供比 **std::future** 更好的性能（以及更好的兼容性）。

> [!TIP]
> 如果包含 `<pplawait.h>`，则可以使用 **concurrency::task** 作为协同程序类型。

```cppwinrt
// main.cpp

#include "pch.h"
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Syndication.h>
#include <iostream>
#include <ppltasks.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Web::Syndication;

concurrency::task<std::wstring> RetrieveFirstTitleAsync()
{
    return concurrency::create_task([]
    {
        Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
        SyndicationClient syndicationClient;
        SyndicationFeed syndicationFeed = syndicationClient.RetrieveFeedAsync(rssFeedUri).get();
        return std::wstring{ syndicationFeed.Items().GetAt(0).Title().Text() };
    });
}

int main()
{
    winrt::init_apartment();

    auto firstTitleOp = RetrieveFirstTitleAsync();
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

但如果向协同程序传递引用参数，可能会遇到问题。

```cppwinrt
// NOT the recommended way to pass a value to a coroutine!
IASyncAction DoWorkAsync(Param const& value)
{
    // While it's ok to access value here...
    
    co_await DoOtherWorkAsync();

    // ...accessing value here carries no guarantees of safety.
}
```

在协同程序中，在第一个暂停点之前，执行是同步的；到达第一个暂停点时，控制返回到调用方。 在协同程序恢复时，引用参数引用的源值可能已发生更改。 从协同程序的角度来看，引用参数具有不受控制的生命周期。 因此，在上面的示例中，在 `co_await` 之前，我们可以安全地访问 *value*，但之后就无法保证安全了。 如果 **DoOtherWorkAsync** 有可能暂停并在恢复后尝试使用 *value*，我们也无法安全地将 *value* 传递给 DoOtherWorkAsync。 为了能够在暂停和恢复后安全地使用参数，默认情况下，协同程序应使用按值传递，以确保捕获参数的值并避免生命周期问题。 确信不遵从该指引也能安全进行操作的情况是很少见的。

```cppwinrt
// Coroutine
IASyncAction DoWorkAsync(Param value);
```

传递 const 值是否是一个好的做法也还存在争议（除非你想移动值）。 它不会对要复制的源值产生任何影响，但有助于表明意图，并避免你无意间修改副本。
    
```cppwinrt
// coroutine with strictly unnecessary const (but arguably good practice).
IASyncAction DoWorkAsync(Param const value);
```

另请参阅[标准数组和向量](std-cpp-data-types.md#standard-arrays-and-vectors)，它介绍如何将标准向量传递到异步被调用方。

## <a name="offloading-work-onto-the-windows-thread-pool"></a>将工作卸载到 Windows 线程池
在协同程序中执行受计算限制的工作之前，需要将执行返回给调用方，以便调用方不被阻塞（换句话说，引入暂停点）。 如果还没有通过 `co-await`（协作等待）某些其他操作来实现这一点，则可以 `co-await` **winrt::resume_background** 函数。 这将控制权返回给调用方，然后立即在某个线程池线程上恢复执行。

实现中使用的线程池是底层 [Windows 线程池](https://msdn.microsoft.com/library/windows/desktop/ms686766)，因此具有极高的效率。

```cppwinrt
IAsyncOperation<uint32_t> DoWorkOnThreadPoolAsync()
{
    co_await winrt::resume_background(); // Return control; resume on thread pool.

    uint32_t result;
    for (uint32_t y = 0; y < height; ++y)
    for (uint32_t x = 0; x < width; ++x)
    {
        // Do compute-bound work here.
    }
    co_return result;
}
```

## <a name="programming-with-thread-affinity-in-mind"></a>编程时仔细考虑线程相关性
该方案继续对上一个方案进行扩展。 你将一些工作卸载到线程池，但希望在用户界面 (UI) 中显示进度。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

上面的代码抛出一个 [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/hresult-wrong-thread) 异常，因为必须从创建 **TextBlock** 的线程（即 UI 线程）更新 TextBlock。 一种解决方案是捕获最初调用协同程序的线程上下文。 实例化一个 **winrt::apartment_context** 对象，然后 `co_await` 它。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    winrt::apartment_context ui_thread; // Capture calling context.

    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await ui_thread; // Switch back to calling context.

    textblock.Text(L"Done!"); // Ok if we really were called from the UI thread.
}
```

只要上面的协同程序是从创建 **TextBlock** 的 UI 线程调用的，这种技术就有效。 在你的应用中，有很多时候都是可以保证这一点的。

> [!NOTE]
> **以下代码示例与在商业发行之前可能会进行实质性修改的预发布产品相关。 Microsoft 对于此处提供的信息不作任何明示或默示的担保。**

要采用更通用的 UI 更新解决方案（涵盖调用线程不确定的情况），你可以安装 [Windows 10 SDK 预览版 17661](https://www.microsoft.com/software-download/windowsinsiderpreviewSDK) 或更高版本。 然后，你可以 `co-await` **winrt::resume_foreground** 函数以切换到特定的前台线程。 在下面的代码示例中，我们通过传递与 **TextBlock** 关联的调度程序对象（通过访问其 [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher) 属性）来指定前台线程。 **winrt::resume_foreground** 实现对该调度程序对象调用 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)，以执行协同程序中该调度程序对象之后的工作。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="important-apis"></a>重要的 API
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed](/uwp/api/windows.web.syndication.syndicationfeed)

## <a name="related-topics"></a>相关主题
* [在 C++/WinRT 中使用委托处理事件](handle-events.md)
* [标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)