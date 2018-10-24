---
author: stevewhims
description: 本主题介绍你可通过 C++/WinRT 创建和使用 Windows 运行时异步对象的方式。
title: 通过 C++/WinRT 的并发和异步操作
ms.author: stwhi
ms.date: 10/21/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 并发, 异步, 异步的, 异步
ms.localizationpriority: medium
ms.openlocfilehash: 0767f8c1ca0fb80ff8c7b033832ffccd61aeabfc
ms.sourcegitcommit: 82c3fc0b06ad490c3456ad18180a6b23ecd9c1a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5472591"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>通过 C++/WinRT 的并发和异步操作

本主题介绍了可以这两种方式创建和使用 Windows 运行时异步对象[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>异步操作和 Windows 运行时“异步”函数

有可能需要超过 50 毫秒才能完成的任何 Windows 运行时 API 将实现为异步函数（具有一个以“Async”结尾的名称）。 异步函数的实现将启动另一线程上的工作，并且立即返回表示异步操作的对象。 在异步操作完成后，返回的对象会包含从该工作中生成的任何值。 **Windows::Foundation** Windows 运行时命名空间包含四种类型的异步操作对象。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)、
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)、
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_) 和
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。

每种异步操作类型都将投影到 **winrt::Windows::Foundation** C++/WinRT 命名空间中的相应类型。 C++/WinRT 还包含内部 await 适配器结构。 直接但借助该结构，不要使用它，你可以编写`co_await`语句以协作等待返回其中一种异步操作类型的任何函数的结果。 然后，你可以自行创作返回这些类型的协同程序。

异步 Windows 函数的示例是 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)，其返回类型 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 的异步操作对象。 让我们来看看一些方法&mdash;第一个阻止，然后非阻止&mdash;使用 C + + /winrt 来调用类似 API 的。

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
    SyndicationFeed syndicationFeed{ syndicationClient.RetrieveFeedAsync(rssFeedUri).get() };
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
> **获取**函数存在于 C + + /winrt 投影类型**winrt::Windows::Foundation::IAsyncAction**，因此你可以调用该功能在任何 C + + WinRT 项目。 因为**获取**不是实际的 Windows 运行时类型**IAsyncAction**的应用程序二进制接口 (ABI) 表面的一部分，不会找到列为[**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)接口的成员函数。

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

    auto processOp{ ProcessFeedAsync() };
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

    auto feedOp{ RetrieveBlogFeedAsync() };
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

在协同程序中执行受计算限制的工作之前，需要将执行返回给调用方，以便调用方不被阻塞（换句话说，引入暂停点）。 如果你正在尚未执行此操作`co-await`运算某些其他操作，则可以`co-await` [**winrt:: resume_background**](/uwp/cpp-ref-for-winrt/resume-background)函数。 这将控制权返回给调用方，然后立即在某个线程池线程上恢复执行。

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

上面的代码抛出一个 [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/hresult-wrong-thread) 异常，因为必须从创建 **TextBlock** 的线程（即 UI 线程）更新 TextBlock。 一种解决方案是捕获最初调用协同程序的线程上下文。 若要执行该操作，实例化[**winrt:: apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context)对象，执行后台任务，然后`co_await` **apartment_context**若要切换回调用上下文。

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

更多常规解决方案更新 UI，涵盖调用线程不确定的情况下，你可以`co-await` [**winrt:: resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)函数以切换到特定的前台线程。 在下面的代码示例中，我们通过传递与 **TextBlock** 关联的调度程序对象（通过访问其 [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher) 属性）来指定前台线程。 **winrt::resume_foreground** 实现对该调度程序对象调用 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)，以执行协同程序中该调度程序对象之后的工作。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>取消异步操作，并取消回调

异步编程的 Windows 运行时的功能，可以取消正在进行的异步操作。 让我们开始一个简单的示例。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction ImplicitCancellationAsync()
{
    while (true)
    {
        std::cout << "ImplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto implicit_cancellation{ ImplicitCancellationAsync() };
    co_await 3s;
    implicit_cancellation.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

如果你运行上述示例，则你将看到**ImplicitCancellationAsync**打印一条消息每秒三秒之后, 自动时间终止由于被取消。 这样做的原因，在遇到`co_await`表达式，协同程序会检查是否已被取消。 如果有，则它会使短路并且，如果没有，则它会暂停为正常。

暂停协同程序时，取消，当然，会发生。 仅协同程序恢复时，或点击另一个`co_await`，它将检查取消。 此问题是可能太粗糙的粒度延迟以响应取消之一。

因此，另一个选项是显式轮询从协同程序中取消。 使用以下列表中的代码更新上面的示例。 在此新的示例中， **ExplicitCancellationAsync**检索[**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token)函数中，返回的对象，并使用它来定期检查是否已取消协同程序。 只要它不会取消，该协同程序之间无限;一旦其被取消后，该循环并将该函数正常退出。 如前面的示例，但此处退出发生显式方法，并控制，结果都是相同的。

```cppwinrt
...
IAsyncAction ExplicitCancellationAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };

    while (!cancellation_token())
    {
        std::cout << "ExplicitCancellationAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction MainCoroutineAsync()
{
    auto explicit_cancellation{ ExplicitCancellationAsync() };
    co_await 3s;
    explicit_cancellation.Cancel();
}
...
```

等待**winrt::get_cancellation_token**检索取消令牌代表你的协同程序生成**IAsyncAction**知识。 你可以使用该令牌函数调用运算符查询取消状态&mdash;本质上轮询取消。 如果你正在执行一些计算密集型操作，或循环访问较大的集合，这是合理的技术。

### <a name="register-a-cancellation-callback"></a>注册取消回调

在 Windows 运行时取消不会自动流向其他异步对象。 但是&mdash;在 Windows SDK 版本 10.0.17763.0 (Windows 10 版本 1809年) 中引入&mdash;你可以注册取消回调。 这是性挂钩的取消程度可以传播，并使其可以与现有的并发库集成。

在下一个代码示例， **NestedCoroutineAsync**执行的工作，但它没有特殊取消逻辑中。 **CancellationPropagatorAsync**是本质上的嵌套协同程序; 在包装器包装器将优先转发取消。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncAction NestedCoroutineAsync()
{
    while (true)
    {
        std::cout << "NestedCoroutineAsync: do some work for 1 second" << std::endl;
        co_await 1s;
    }
}

IAsyncAction CancellationPropagatorAsync()
{
    auto cancellation_token{ co_await winrt::get_cancellation_token() };
    auto nested_coroutine{ NestedCoroutineAsync() };

    cancellation_token.callback([&]
    {
        nested_coroutine.Cancel();
    });

    co_await nested_coroutine;
}

IAsyncAction MainCoroutineAsync()
{
    auto cancellation_propagator{ CancellationPropagatorAsync() };
    co_await 3s;
    cancellation_propagator.Cancel();
}

int main()
{
    winrt::init_apartment();
    MainCoroutineAsync().get();
}
```

**CancellationPropagatorAsync**注册其自己的取消回调，lambda 函数，然后等待 （它会暂停） 嵌套的工作完成后才。 时或**CancellationPropagatorAsync**被取消后，它会传播到嵌套协同程序取消。 若要轮询取消; 无需也不会被取消阻止无限期。 此机制非常灵活，可用于互操作具有协同程序或并发库知道任何 C + WinRT。

## <a name="reporting-progress"></a>报告进度

如果你的协同程序返回[**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)或[**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)，然后你可以检索[**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token)函数中，返回的对象并使用它返回到进度报告进度处理程序。 下面是代码示例。

```cppwinrt
// pch.h
#pragma once
#include <iostream>
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
using namespace winrt;
using namespace Windows::Foundation;
using namespace std::chrono_literals;

IAsyncOperationWithProgress<double, double> CalcPiTo5DPs()
{
    auto progress{ co_await winrt::get_progress_token() };

    co_await 1s;
    double pi_so_far{ 3.1 };
    progress(0.2);

    co_await 1s;
    pi_so_far += 4.e-2;
    progress(0.4);

    co_await 1s;
    pi_so_far += 1.e-3;
    progress(0.6);

    co_await 1s;
    pi_so_far += 5.e-4;
    progress(0.8);

    co_await 1s;
    pi_so_far += 9.e-5;
    progress(1.0);
    co_return pi_so_far;
}

IAsyncAction DoMath()
{
    auto async_op_with_progress{ CalcPiTo5DPs() };
    async_op_with_progress.Progress([](auto const& /* sender */, double progress)
    {
        std::wcout << L"CalcPiTo5DPs() reports progress: " << progress << std::endl;
    });
    double pi{ co_await async_op_with_progress };
    std::wcout << L"CalcPiTo5DPs() is complete !" << std::endl;
    std::wcout << L"Pi is approx.: " << pi << std::endl;
}

int main()
{
    winrt::init_apartment();
    DoMath().get();
}
```

> [!NOTE]
> 不正确实现为异步操作或操作的多个*完成处理程序*。 你可以让任一单个为其已完成的事件，委托，或者你可以`co_await`它。 如果你有两者，则第二个将失败。 任何一种以下两种类型的完成处理程序之一是相应;不能两者都相同的异步对象。

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
async_op_with_progress.Completed([](auto const& sender, AsyncStatus /* status */)
{
    double pi{ sender.GetResults() };
});
```

```cppwinrt
auto async_op_with_progress{ CalcPiTo5DPs() };
double pi{ co_await async_op_with_progress };
```

有关完成处理程序的详细信息，请参阅[异步操作和运算的委托类型](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)。

## <a name="fire-and-forget"></a>触发，并且忘记

有时，你的任务可与其他工作，同时完成并且你无需等待完成该任务 （任何其他工作取决于它），你需要返回一个值，也不会。 在此情况下，你可以关闭该任务触发，并且忘记其。 你可以通过编写协同程序其返回类型是[**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget) （而不是 Windows 运行时异步操作类型或**concurrency:: task**之一） 来执行该操作。

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
using namespace winrt;
using namespace std::chrono_literals;

winrt::fire_and_forget CompleteInFiveSeconds()
{
    co_await 5s;
}

int main()
{
    winrt::init_apartment();
    CompleteInFiveSeconds();
    // Do other work here.
}
```

## <a name="important-apis"></a>重要的 API
* [concurrency:: task 类](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction 接口](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;接口](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;接口](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult，TProgress&gt;接口](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [Syndicationclient:: Retrievefeedasync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed 类](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>相关主题
* [在 C++/WinRT 中使用委托处理事件](handle-events.md)
* [标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)