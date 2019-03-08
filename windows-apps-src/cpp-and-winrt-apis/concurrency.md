---
description: 本主题介绍你可通过 C++/WinRT 创建和使用 Windows 运行时异步对象的方式。
title: 通过 C++/WinRT 的并发和异步操作
ms.date: 10/27/2018
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 并发, 异步, 异步的, 异步
ms.localizationpriority: medium
ms.openlocfilehash: f3283ffa5fa047806befa2712301c25a7d07af8e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57611292"
---
# <a name="concurrency-and-asynchronous-operations-with-cwinrt"></a>通过 C++/WinRT 的并发和异步操作

本主题介绍的方法均可在其中创建和使用 Windows 运行时异步对象与[C + + WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)。

## <a name="asynchronous-operations-and-windows-runtime-async-functions"></a>异步操作和 Windows 运行时“异步”函数

有可能需要超过 50 毫秒才能完成的任何 Windows 运行时 API 将实现为异步函数（具有一个以“Async”结尾的名称）。 异步函数的实现将启动另一线程上的工作，并且立即返回表示异步操作的对象。 在异步操作完成后，返回的对象会包含从该工作中生成的任何值。 **Windows::Foundation** Windows 运行时命名空间包含四种类型的异步操作对象。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction),
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_),
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation_tresult_)，和
- [**IAsyncOperationWithProgress&lt;TResult，TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)。

每种异步操作类型都将投影到 **winrt::Windows::Foundation** C++/WinRT 命名空间中的相应类型。 C++/WinRT 还包含内部 await 适配器结构。 直接，但，得益于该结构，不使用它，您可以编写`co_await`语句以协作方式等待返回这些异步操作类型之一的任何函数的结果。 然后，你可以自行创作返回这些类型的协同程序。

异步 Windows 函数的示例是 [**SyndicationClient::RetrieveFeedAsync**](https://docs.microsoft.com/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)，其返回类型 [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_) 的异步操作对象。 让我们看一下某些方面&mdash;第一个阻塞，然后非阻塞&mdash;的使用 C + + WinRT 调用诸如的 API。

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
> **获取**函数存在于上 C + + WinRT 投影类型**winrt::Windows::Foundation::IAsyncAction**，因此可以调用的函数在任何 C + + WinRT 项目。 您将找不到列出的成员函数[ **IAsyncAction** ](/uwp/api/windows.foundation.iasyncaction)接口，因为**获取**不属于的应用程序二进制接口 (ABI) 图面实际的 Windows 运行时类型**IAsyncAction**。

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

协同程序的任何其他函数在于调用方被阻止，直到函数执行返回给它。 和协同程序返回的第一个机会是第一个`co_await`， `co_return`，或`co_yield`。

因此，在执行操作之前的协同程序中的计算密集型工作，您需要返回到调用方的执行 （即，引入挂起点），以便不会阻止调用方。 如果尚不存在要执行该操作`co_await`-ing 某个其他操作，则你可以`co_await` [ **winrt::resume_background** ](/uwp/cpp-ref-for-winrt/resume-background)函数。 这将控制权返回给调用方，然后立即在某个线程池线程上恢复执行。

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

上面的代码抛出一个 [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) 异常，因为必须从创建 **TextBlock** 的线程（即 UI 线程）更新 TextBlock。 一种解决方案是捕获最初调用协同程序的线程上下文。 为此，请实例化[ **winrt::apartment_context** ](/uwp/cpp-ref-for-winrt/apartment-context)对象，请执行后台工作，然后`co_await` **apartment_context**若要切换回调用上下文。

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

对于更新 UI，涵盖你是确定调用线程的情况下，更通用的解决方案，可`co_await` [ **winrt::resume_foreground** ](/uwp/cpp-ref-for-winrt/resume-foreground)要切换到特定函数前台线程。 在下面的代码示例中，我们通过传递与 **TextBlock** 关联的调度程序对象（通过访问其 [**Dispatcher**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher#Windows_UI_Xaml_DependencyObject_Dispatcher) 属性）来指定前台线程。 **winrt::resume_foreground** 实现对该调度程序对象调用 [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync)，以执行协同程序中该调度程序对象之后的工作。

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
IAsyncAction DoWorkAsync(TextBlock textblock)
{
    co_await winrt::resume_background();
    // Do compute-bound work here.

    co_await winrt::resume_foreground(textblock.Dispatcher()); // Switch to the foreground thread associated with textblock.

    textblock.Text(L"Done!"); // Guaranteed to work.
}
```

## <a name="execution-contexts-resuming-and-switching-in-a-coroutine"></a>恢复和协同程序中切换执行上下文

概括地说，暂停时间点后的协同程序中，执行原始线程可能会消失，恢复可能会出现在任何线程上 (即，任何线程都可以调用**已完成**异步操作方法)。

但是，如果您`co_await`任意四个 Windows 运行时异步操作类型 (**IAsyncXxx**)，然后 C + + WinRT 捕获点处的调用上下文您`co_await`。 它可确保您仍在使用该上下文延续恢复运行时。 C + + WinRT 执行此通过检查你是否已将在调用上下文，以及如果没有，请切换到它。 如果您是在单线程单元 (STA) 线程之前`co_await`，然后您就会在同一个之后; 如果您是之前在多线程的单元 (MTA) 线程上`co_await`，然后您就会在一个之后。

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

你可以依赖此行为的原因是因为 C + + WinRT 提供了代码以适应这些 c + + 协同程序语言支持 （这些代码段称为等待适配器） 的 Windows 运行时异步操作类型。 剩余的可等待类型在 C + + / WinRT 是只需线程池包装器和/或帮助程序;因此他们完成线程池上。

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

如果您`co_await`某些其他类型&mdash;甚至在 C + + WinRT 协同例程实现&mdash;则另一个库提供了适配器，并将需要了解这些适配器执行操作恢复和上下文。

若要使下一个最少的上下文切换，可以使用一些我们已了解本主题中的技术。 我们来看，这样某些图例。 在此下一步的伪代码示例中，我们显示的事件处理程序调用一个 Windows 运行时 API，可将图像加载、 到后台线程来处理该映像上删除，然后返回到 UI 线程在 UI 中显示图像的轮廓。

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
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

对于此方案中，没有很少的围绕对调用 ineffiency **StorageFile::OpenAsync**。 没有必要的上下文切换到后台线程 （因此，该处理程序可以返回到调用方执行），在恢复后的 C + + WinRT 还原 UI 线程上下文。 但是，在这种情况下，不需要是 UI 线程上，直到我们想要更新 UI。 我们调用多个 Windows 运行时 Api*之前*我们调用**winrt::resume_background**，我们会产生更多不必要和-往返上下文切换。 解决方案是不调用*任何*在此之前先 Windows 运行时 Api。 所有后将它们移**winrt::resume_background**。

```cppwinrt
#include <winrt/Windows.UI.Core.h> // necessary in order to use winrt::resume_foreground.
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

如果你想要执行一些更高级的则可以编写您自己，await 适配器。 例如，如果你想`co_await`完成的异步操作在同一线程上恢复 （因此，没有任何上下文切换），然后您可以开始通过编写 await 适配器类似于如下所示。

> [!NOTE]
> 下面的代码示例提供; 仅供学习它是为了便于您开始了解如何 await 适配器工作。 如果你想要使用此方法在你自己的基本代码，则我们建议在开发和测试您自己 await 适配器 struct(s)。 例如，可以编写**complete_on_any**， **complete_on_current**，并**complete_on(dispatcher)**。 此外请考虑使它们需要的模板**IAsyncXxx**与模板参数的类型。

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

若要了解如何使用**no_switch** await 适配器，您首先需要知道，当 c + + 编译器遇到`co_await`表达式调用函数看起来**await_ready**， **await_suspend**，并**await_resume**。 C + + WinRT 库提供了这些函数，以便默认情况下，此类获得合理的行为。

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await async;
```

若要使用**no_switch** await 适配器时，只需更改的类型`co_await`表达式从**IAsyncXxx**到**no_switch**，如下所示。

```cppwinrt
IAsyncAction async{ ProcessFeedAsync() };
co_await static_cast<no_switch>(async);
```

然后，而不是三种查看**await_xxx**函数匹配**IAsyncXxx**，c + + 编译器查找匹配的函数**no_switch**。

## <a name="canceling-an-asychronous-operation-and-cancellation-callbacks"></a>取消异步操作和取消回调

进行异步编程的 Windows 运行时的功能，可以取消正在进行的异步动作或操作。 下面是调用示例[ **StorageFolder::GetFilesAsync** ](/uwp/api/windows.storage.storagefolder.getfilesasync)检索可能很大的文件和其集合存储所生成的异步操作对象中的数据成员。 用户可以选择取消该操作。

```cppwinrt
// MainPage.xaml
...
<Button x:Name="workButton" Click="OnWork">Work</Button>
<Button x:Name="cancelButton" Click="OnCancel">Cancel</Button>
...

// MainPage.h
...
#include <winrt/Windows.Storage.Search.h>
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
```

取消的实现端，让我们先简单示例。

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

如果运行上述示例中，则你将看到**ImplicitCancellationAsync**打印每秒的三秒之后, 它将自动终止的时间作为结果的正在取消一条消息。 这样做的原因，在遇到`co_await`表达式，协同例程将检查是否已取消。 如果已更改，然后它会短路如果没有，然后它将挂起作为普通。

当挂起协同例程时，可能会当然，发生取消。 仅当在协同程序重新开始，或到达另一个`co_await`，它将检查取消。 问题在于其中一个可能太粗糙的粒度响应取消操作的延迟。

因此，另一种方法是显式轮询取消在协同程序中。 使用下面的列表中的代码更新上面的示例。 在此新的示例中， **ExplicitCancellationAsync**返回的对象中检索[ **winrt::get_cancellation_token** ](/uwp/cpp-ref-for-winrt/get-cancellation-token)函数，并定期使用检查是否已取消协同例程。 只要未取消，协同例程将陷入死循环;一旦取消，循环和函数通常情况下退出。 因为上一示例中，但此处退出发生显式，并控制下，结果都是相同的。

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

等待**winrt::get_cancellation_token**检索具有的知识的取消令牌**IAsyncAction**协同程序生成你的名义。 可以在该令牌使用函数调用运算符进行查询的取消状态&mdash;实质上轮询取消。 如果要执行某些计算密集型操作，或循环访问大型集合，这是一种合理的技术。

### <a name="register-a-cancellation-callback"></a>注册取消回调

Windows 运行时的取消不会自动流动到其他异步对象。 但是&mdash;10.0.17763.0 (Windows 10，版本 1809年) 版本的 Windows SDK 中引入&mdash;可注册一个取消回调。 这是依据取消可以传播，并使其可以与现有的并发库集成的强占式挂钩。

在此下一步的代码示例**NestedCoroutineAsync**执行工作，但它对任何特殊的取消逻辑。 **CancellationPropagatorAsync**是实质上是嵌套的协同例程; 上的包装器包装器将提前转发取消。

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

**CancellationPropagatorAsync**寄存器 lambda 函数为其自己的取消回调，然后等待 （暂停） 的嵌套的工作完成之前。 或者如果**CancellationPropagatorAsync**是取消，将传播到嵌套的协同例程的取消操作。 若要轮询取消; 无需也不会取消阻止无限期。 此机制的灵活程度足以供你使用与协同程序或并发库一无所知的 C + + WinRT。

## <a name="reporting-progress"></a>报告进度

如果在协同程序返回[ **IAsyncActionWithProgress**](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)，或[ **IAsyncOperationWithProgress**](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)，则可以检索返回的对象[ **winrt::get_progress_token** ](/uwp/cpp-ref-for-winrt/get-progress-token)函数，并使用回进度处理程序报告进度。 下面是代码示例。

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
> 它不是正确实现多个*完成处理程序*为异步操作或操作。 你可以委托任一单个的已完成事件，或者可以`co_await`它。 如果你有这种，则第二个将失败。 任一以下两种完成处理程序之一是相应;不能同时为同一异步对象。

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

有关完成处理程序的详细信息，请参阅[委托的异步操作和操作的类型](handle-events.md#delegate-types-for-asynchronous-actions-and-operations)。

## <a name="fire-and-forget"></a>发后不理

有时，有的任务可以与其他工作，同时进行，无需等待该任务完成 （没有其他工作依赖于它），也不需要它返回一个值。 在这种情况下，您可以触发该任务不用再管。 是通过编写其返回类型的协同程序，便可做到[ **winrt::fire_and_forget** ](/uwp/cpp-ref-for-winrt/fire-and-forget) (而不是一种 Windows 运行时异步操作类型，或**concurrency:: task**).

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
* [IAsyncActionWithProgress&lt;TProgress&gt; interface](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt;接口](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult，TProgress&gt;接口](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [SyndicationFeed 类](/uwp/api/windows.web.syndication.syndicationfeed)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)

## <a name="related-topics"></a>相关主题
* [处理事件，通过使用委托中 C + + WinRT](handle-events.md)
* [标准 c + + 数据类型和 C + + WinRT](std-cpp-data-types.md)
