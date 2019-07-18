---
description: 本主题介绍了你可通过 C++/WinRT 创建和使用 Windows 运行时异步对象的方式。
title: 使用 C++/WinRT 执行并发和异步操作
ms.date: 07/08/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 并发, async, 异步
ms.localizationpriority: medium
ms.openlocfilehash: cbabf38f41ae940f5c92944154638eae7016e043
ms.sourcegitcommit: 7585bf66405b307d7ed7788d49003dc4ddba65e6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2019
ms.locfileid: "67660089"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>使用 C++/WinRT 执行并发和异步操作

本主题介绍如何通过 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 以不同的方式创建和使用 Windows 运行时异步对象。

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>异步操作和 Windows 运行时“Async”函数

有可能需要超过 50 毫秒才能完成的任何 Windows 运行时 API 将实现为异步函数（具有一个以“Async”结尾的名称）。 异步函数的实现将启动另一线程上的工作，并且立即返回表示异步操作的对象。 在异步操作完成后，返回的对象会包含从该工作中生成的任何值。 **Windows::Foundation** Windows 运行时命名空间包含四种类型的异步操作对象。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)；
- [**IAsyncActionWithProgress&lt;TProgress&gt;** ](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)；
- [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_)；
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。

每种异步操作类型都将投影到 **winrt::Windows::Foundation** C++/WinRT 命名空间中的相应类型。 C++/WinRT 还包含内部 await 适配器结构。 不要直接使用它，但借助该结构，可以编写 `co_await` 语句以协作等待返回其中一种异步操作类型的任何函数的结果。 然后，可以自行创作返回这些类型的协同例程。

异步 Windows 函数的示例是 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)，其返回类型 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 的异步操作对象。 让我们来看一些阻塞和不阻塞使用 C++/WinRT 来调用类似 API 的方法。

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

## <a name="asychronously-return-a-windows-runtime-type"></a>异步返回 Windows 运行时类型

在下一个示例中，我们将针对特定 URI 封装对 **RetrieveFeedAsync** 的调用，以为我们提供异步返回 [**SyndicationFeed**](/uwp/api/windows.web.syndication.syndicationfeed) 的 **RetrieveBlogFeedAsync** 函数。

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

如果要异步返回 Windows 运行时类型，则应返回 [**IAsyncOperation&lt;TResult&gt;** ](/uwp/api/windows.foundation.iasyncoperation_tresult_) 或 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** ](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。 任何第一方或第三方运行时类或可以传入/传出 Windows 运行时函数的任何类型（例如 `int` 或 **winrt::hstring**）都符合条件。 如果尝试对非 Windows 运行时类型使用其中一种异步操作类型，编译器可帮助你处理“必须为 WinRT 类型”错误  。

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

## <a name="asychronously-return-a-non-windows-runtime-type"></a>异步返回非 Windows 运行时类型

如果要异步返回非 Windows 运行时类型的类型，则应返回并行模式库 (PPL) [**concurrency::task**](/cpp/parallel/concrt/reference/task-class)。  建议使用 **concurrency::task**，因为它将提供比 **std::future** 更好的性能（以及更好的兼容性）。

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

    co_await DoOtherWorkAsync();

    // ...accessing value here carries no guarantees of safety.
}
```

在协同例程中，在第一个暂停点之前，执行是同步的；到达第一个暂停点时，控制返回到调用方。 在协同例程恢复时，引用参数引用的源值可能已发生更改。 从协同例程的角度来看，引用参数具有不受控制的生命周期。 因此，在上面的示例中，在 `co_await` 之前，我们可以安全地访问 *value*，但之后就无法保证安全了。 如果调用方销毁了 *value*，则尝试在协同例程中访问它会导致内存损坏。 如果 **DoOtherWorkAsync** 函数有可能暂停并在恢复后尝试使用 *value*，我们也无法安全地将 *value* 传递给 DoOtherWorkAsync。

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

## <a name="offloading-work-onto-the-windows-thread-pool"></a>将工作卸载到 Windows 线程池

协同例程与任何其他函数的类似之处在于，调用方将会阻塞到某个函数向其返回了执行为止。 另外，协同例程返回的第一个机会是第一个 `co_await`、`co_return` 或 `co_yield`。

因此，在协同例程中执行受计算限制的工作之前，需要将执行返回给调用方（换句话说，引入暂停点），使调用方不被阻塞。 如果还没有对其他某个操作运行 `co_await` 来做到这一点，则可以对 [**winrt::resume_background**](/uwp/cpp-ref-for-winrt/resume-background) 函数运行 `co_await`。 这会将控制权返回给调用方，然后立即在某个线程池线程上恢复执行。

实现中使用的线程池是底层 [Windows 线程池](https://docs.microsoft.com/windows/desktop/ProcThread/thread-pool-api)，因此具有极高的效率。

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

该方案继续对上一个方案进行扩展。 你已将一些工作卸载到线程池，但希望在用户界面 (UI) 中显示进度。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    textblock.Text(L"Done!"); // Error: TextBlock has thread affinity.
}
```

上述代码抛出一个 [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) 异常，因为必须从创建 **TextBlock** 的线程（即 UI 线程）更新 TextBlock。 一种解决方案是捕获最初调用协同例程的线程上下文。 为此，请实例化 [**winrt::apartment_context**](/uwp/cpp-ref-for-winrt/apartment-context) 对象，执行后台工作，然后对 **apartment_context** 运行 `co_await` 以切回到调用上下文。

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

只要上面的协同例程是从创建 **TextBlock** 的 UI 线程调用的，这种方法是可行的。 在应用中，有很多时候都是可以保证这一点的。

若要通过某种更通用的解决方案来更新 UI（包括不确定调用线程的情况）可以对 [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 函数运行 `co_await`，以切换到特定的前台线程。 在以下代码示例中，我们通过传递与 **TextBlock** 关联的调度程序对象（通过访问其 [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher) 属性）来指定前台线程。 **winrt::resume_foreground** 实现对该调度程序对象调用 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)，以执行协同例程中该调度程序对象之后的工作。

```cppwinrt
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>协同例程中的执行上下文、恢复和切换

概括地说，在协同例程中某个暂停点之后，原始执行线程可能会消失，而恢复可能会在任何线程上发生（换而言之，任何线程都可以针对异步操作调用 **Completed** 方法）。

但是，如果对四个 Windows 运行时异步操作类型 (**IAsyncXxx**) 中的任何一个运行 `co_await`，则 C++/WinRT 会在运行 `co_await` 时捕获调用上下文。 另外，它可以当延续操作恢复时，你仍处于该上下文中。 为此，C++/WinRT 会检查你是否已进入调用上下文，如果没有，则切换到该上下文。 如果在运行 `co_await` 之前你处于单线程单元 (STA) 线程中，则运行之后你仍处于相同的线程中；如果在运行 `co_await` 之前你处于多线程单元 (MTA) 线程中，则运行之后你将处于不同的线程中。

```cppwinrt
IAsyncAction ProcessFeedAsync()
{
    Uri rssFeedUri{ L"https://blogs.windows.com/feed" };
    SyndicationClient syndicationClient;

    // The thread context at this point is captured...
    SyndicationFeed syndicationFeed{ co_await syndicationClient.RetrieveFeedAsync(rssFeedUri) };
    // ...and is restored at this point.
}
```

可以依赖此行为的原因在于，C++/WinRT 提供相应的代码，使这些 Windows 运行时异步操作类型能够适应 C++ 协同例程语言支持（这些代码片段称为等待适配器）。 C++/WinRT 中剩余的可等待类型只是一些线程池包装器和/或帮助器；因此它们会在线程池中完成。

```cppwinrt
using namespace std::chrono;
IAsyncOperation<int> return_123_after_5s()
{
    // No matter what the thread context is at this point...
    co_await 5s;
    // ...we're on the thread pool at this point.
    co_return 123;
}
```

如果对其他某个类型运行 `co_await` &mdash; 即使是在 C++/WinRT 协同例程实现中 &mdash; 则另一个库会提供适配器，你需要了解这些适配器在恢复和上下文方面的作用。

为了尽量减少上下文切换次数，可以使用本主题所述的某些方法。 让我们看看该操作的几个图示。 以下伪代码示例演示了一个事件处理程序的大纲。该处理程序调用 Windows 运行时 API 来加载图像，切换到后台线程来处理该图像，然后返回到 UI 线程以在 UI 中显示该图像。

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    // Call StorageFile::OpenAsync to load an image file.

    // The call to OpenAsync occurred on a background thread, but C++/WinRT has restored us to the UI thread by this point.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

在此方案中，调用 **StorageFile::OpenAsync** 会使效率略微下降。 恢复时有必要将上下文切换到后台线程（这样，处理程序便可以将执行返回给调用方），然后 C++/WinRT 会还原 UI 线程上下文。 但是，在此情况下，在我们即将更新 UI 之前，没有必要处于 UI 线程中。 在调用 **winrt::resume_background** 之前调用的 Windows 运行时 API 越多，发生的不必要往返上下文切换也越多。  解决方法是在此之前不要调用任何 Windows 运行时 API。  将所有此类调用移到 **winrt::resume_background** 的后面。

```cppwinrt
IAsyncAction MainPage::ClickHandler(IInspectable /* sender */, RoutedEventArgs /* args */)
{
    // We begin in the UI context.

    co_await winrt::resume_background();

    // We're now on a background thread.

    // Call StorageFile::OpenAsync to load an image file.

    // Process the image.

    co_await winrt::resume_foreground(this->Dispatcher());

    // We're back on MainPage's UI thread.

    // Display the image in the UI.
}
```

如果需要提前执行某些调用，可以编写自己的 await 适配器。 例如，若要运行 `co_await` 以便在完成异步操作所在的同一线程上进行恢复（因此不会发生上下文切换），可以先编写如下所示的 await 适配器。

> [!NOTE]
> 以下代码示例仅用于培训目的，可帮助你开始了解 await 适配器的工作原理。 若要在自己的基代码中使用此方法，我们建议你开发并测试自己的 await 适配器结构。 例如，可以编写 **complete_on_any**、**complete_on_current** 和 **complete_on(dispatcher)** 。 另请考虑将这些结构设置为使用 **IAsyncXxx** 类型作为模板参数的模板。

```cppwinrt
struct no_switch
{
    no_switch(Windows::Foundation::IAsyncAction const& async) : m_async(async)
    {
    }

    bool await_ready() const
    {
        return m_async.Status() == Windows::Foundation::AsyncStatus::Completed;
    }

    void await_suspend(std::experimental::coroutine_handle<> handle) const
    {
        m_async.Completed([handle](Windows::Foundation::IAsyncAction const& /* asyncInfo */, Windows::Foundation::AsyncStatus const& /* asyncStatus */)
        {
            handle();
        });
    }

    auto await_resume() const
    {
        return m_async.GetResults();
    }

private:
    Windows::Foundation::IAsyncAction const& m_async;
};
```

若要了解如何使用 **no_switch** await 适配器，首先需要知道，当 C++ 编译器遇到 `co_await` 表达式时，它会查找名为 **await_ready**、**await_suspend** 和 **await_resume** 的函数。 C++/WinRT 库提供了这些函数，使你在默认情况下能够获得合理行为，如下所示。

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

若要使用 **no_switch** await 适配器，只需将该 `co_await` 表达式的类型从 **IAsyncXxx** 更改为 **no_switch**，如下所示。

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

然后，C++ 编译器不会查找与 **IAsyncXxx** 匹配的三个 **await_xxx** 函数，而是查找与 **no_switch** 匹配的函数。

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>取消异步操作和取消回调

使用 Windows 运行时的异步编程功能可以取消正在进行的异步操作或运算。 以下示例调用 [**StorageFolder::GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync) 来检索可能较大的文件集合，并将生成的异步操作对象存储在数据成员中。 用户可以选择取消该操作。

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.Search.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::Storage;
using namespace Windows::Storage::Search;
using namespace Windows::UI::Xaml;
...
struct MainPage : MainPageT<MainPage>
{
    MainPage()
    {
        InitializeComponent();
    }

    IAsyncAction OnWork(IInspectable /* sender */, RoutedEventArgs /* args */)
    {
        workButton().Content(winrt::box_value(L"Working..."));

        // Enable the Pictures Library capability in the app manifest file.
        StorageFolder picturesLibrary{ KnownFolders::PicturesLibrary() };

        m_async = picturesLibrary.GetFilesAsync(CommonFileQuery::OrderByDate, 0, 1000);

        IVectorView<StorageFile> filesInFolder{ co_await m_async };

        workButton().Content(box_value(L"Done!"));

        // Process the files in some way.
    }

    void OnCancel(IInspectable const& /* sender */, RoutedEventArgs const& /* args */)
    {
        if (m_async.Status() != AsyncStatus::Completed)
        {
            m_async.Cancel();
            workButton().Content(winrt::box_value(L"Canceled"));
        }
    }

private:
    IAsyncOperation<::IVectorView<StorageFile>> m_async;
};
...
```

让我们通过一个简单的示例开始了解取消的实现端。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

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

如果运行上述示例，则你会看到，**ImplicitCancellationAsync** 在三秒钟内每秒输出一条消息，然后，它会由于执行了取消操作而自动终止。 之所以此行为是可行的，是因为在遇到 `co_await` 表达式时，协同例程会检查它是否已取消。 如果已取消，则它会短路掉，否则它会像正常情况下一样暂停。

当然，取消也可以在协同例程暂停时发生。 仅当协同例程恢复或者遇到另一个 `co_await` 时，它才会检查取消状态。 问题在于，在响应取消时可能会出现过于粗糙粒度的延迟。

因此，另一种做法是从协同例程内部显式轮询取消。 使用以下列表中的代码更新上述示例。 在此新示例中，**ExplicitCancellationAsync** 检索 [**winrt::get_cancellation_token**](/uwp/cpp-ref-for-winrt/get-cancellation-token) 函数返回的对象，并使用该对象定期检查是否已取消协同例程。 只要尚未取消，协同例程就会无限循环；一旦取消，循环和函数就会正常退出。 结果与前一个示例相同，不过，在此示例中，退出是显式发生的并且受控。

```cppwinrt
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

等待 **winrt::get_cancellation_token** 时会使用协同例程代表你生成的 **IAsyncAction** 信息检索取消标记。 可以针对该标记使用函数调用运算符以查询取消状态 &mdash; 实质上是轮询取消。 如果执行某项计算资源受限的操作，或循环访问大型集合，则这是一种合理的方法。

### <a name="register-a-cancellation-callback"></a>注册取消回调

Windows 运行时的取消不会自动流向其他异步对象。 但是，可以注册取消回调 &mdash; 这是 Windows SDK 版本 10.0.17763.0（Windows 10 版本 1809）中引入的功能。 这是一种先发性的挂钩，可据此传播取消，以及与现有的并发库集成。

在以下代码示例中，**NestedCoroutineAsync** 将执行工作，但其中不包含特殊的取消逻辑。 **CancellationPropagatorAsync** 实质上是协同例程中的一个包装器；该包装器提前转发取消。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

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

    cancellation_token.callback([=]
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

**CancellationPropagatorAsync** 为自身的取消回调注册一个 lambda 函数，然后等待（此时会暂停）嵌套的工作完成。 如果 **CancellationPropagatorAsync** 已取消，则会将取消传播到嵌套的协同例程。 无需轮询取消；取消不会无限期阻塞。 此机制足够灵活，使用它可以与完全不了解 C++/WinRT 的协同例程或并发库互操作。

## <a name="reporting-progress"></a>报告进度

如果协同例程返回 [**IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_) 或 [**IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)，则你可以检索 [**winrt::get_progress_token**](/uwp/cpp-ref-for-winrt/get-progress-token) 函数返回的对象，并使用该对象将进度报告给进度处理程序。 下面是代码示例。

```cppwinrt
// main.cpp
#include <iostream>
#include <winrt/Windows.Foundation.h>

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
> 对一个异步操作或运算实现多个完成处理程序是错误的做法。  可对其已完成的事件使用单个委托，或者可对其运行 `co_await`。 如果同时采用这两种方法，则第二种方法会失败。 以下两种完成处理程序都是适当的；但不能同时对同一个异步对象使用两者。

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

## <a name="fire-and-forget"></a>发后不理

有时，某个任务可与其他工作并发执行，你无需等待该任务完成（因为没有其他工作依赖于它），也无需该任务返回值。 在这种情况下，可以激发该任务，然后忘记它。 为此，可以编写返回类型为 [**winrt::fire_and_forget**](/uwp/cpp-ref-for-winrt/fire-and-forget)（而不是某个 Windows 运行时异步操作类型，或 **concurrency:: task**）的协同例程。

```cppwinrt
// main.cpp
#include <winrt/Windows.Foundation.h>

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

**winrt::fire_and_forget** 也可用作事件处理程序的返回类型，前提是需在其中执行异步操作。 下面是一个示例（另请参阅 [C++/WinRT 中的强引用和弱引用](/windows/uwp/cpp-and-winrt-apis/weak-references#safely-accessing-the-this-pointer-in-a-class-member-coroutine)）。

```cppwinrt
winrt::fire_and_forget MyClass::MyMediaBinder_OnBinding(MediaBinder const&, MediaBindingEventArgs args)
{
    auto lifetime{ get_strong() }; // Prevent *this* from prematurely being destructed.
    auto ensure_completion{ unique_deferral(args.GetDeferral()) }; // Take a deferral, and ensure that we complete it.

    auto file{ co_await StorageFile::GetFileFromApplicationUriAsync(Uri(L"ms-appx:///video_file.mp4")) };
    args.SetStorageFile(file);

    // The destructor of unique_deferral completes the deferral here.
}
```

第一个参数 (*sender*) 未命名，因为我们从未使用它。 因此，可以安全地将它保留为引用。 但请注意，*args* 是按值传递的。 请参阅上面的[参数传递](#parameter-passing)部分。

## <a name="important-apis"></a>重要的 API
* [concurrency::task 类](/cpp/parallel/concrt/reference/task-class)
* [IAsyncAction 接口](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; 接口](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; 接口](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 接口](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed 类](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>相关主题
* [在 C++/WinRT 中使用委托处理事件](handle-events.md)
* [标准 C++ 数据类型和 C++/WinRT](std-cpp-data-types.md)
