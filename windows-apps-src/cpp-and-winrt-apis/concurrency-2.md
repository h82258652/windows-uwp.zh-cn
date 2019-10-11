---
description: 使用 C++/WinRT 的具有并发性和异步性的更高级方案。
title: C++/WinRT 的更高级并发和异步
ms.date: 07/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 并发, async, 异步
ms.localizationpriority: medium
ms.openlocfilehash: 9484b61aae91ae426efb1963cd37ebf276ef7c6c
ms.sourcegitcommit: f8634aad3a3675c2f0eac62f56df3def4285a7b0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2019
ms.locfileid: "71720434"
---
# <a name="more-advanced-concurrency-and-asynchrony-with-cwinrt"></a>C++/WinRT 的更高级并发和异步

本主题介绍使用 C++/WinRT 的具有并发性和异步性的更高级方案。

有关此主题的简介，请首先阅读[并发和异步运算](concurrency.md)。

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
using namespace std::chrono_literals;
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

## <a name="a-deeper-dive-into-winrtresume_foreground"></a>深入了解 **winrt::resume_foreground**

从 [C++/WinRT 2.0](/windows/uwp/cpp-and-winrt-apis/newsnews#news-and-changes-in-cwinrt-20) 开始，[**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground) 函数会暂停，即使是从调度程序线程来调用它（在以前的版本中，它可能会在某些情况下引入死锁，因为它暂停的前提是尚未位于调度程序线程上）。

当前的行为意味着，你可以依赖于堆栈展开和进行的重新排队；这对于系统稳定很重要，尤其是在低级别系统代码中。 在上面的[编程时仔细考虑线程相关性](#programming-with-thread-affinity-in-mind)部分列出的最后的代码演示了如何在后台线程上执行某些复杂的计算，然后切换到相应的 UI 线程，以便更新用户界面 (UI)。

下面是 **winrt::resume_foreground** 的具体代码。

```cppwinrt
auto resume_foreground(...) noexcept
{
    struct awaitable
    {
        bool await_ready() const
        {
            return false; // Queue without waiting.
            // return m_dispatcher.HasThreadAccess(); // The C++/WinRT 1.0 implementation.
        }
        void await_resume() const {}
        void await_suspend(coroutine_handle<> handle) const { ... }
    };
    return awaitable{ ... };
};
```

这种当前行为与过去行为的对比类似于 Win32 应用程序开发过程中出现的 [**PostMessage**](/windows/win32/api/winuser/nf-winuser-postmessagew) 和 [**SendMessage**](/windows/win32/api/winuser/nf-winuser-sendmessage) 之间的差异。 **PostMessage** 先将工作排队，然后在不等待工作完成的情况下展开堆栈。 堆栈展开有时候很重要。

**winrt::resume_foreground** 函数一开始也只支持在 Windows 10 之前引入的 [**CoreDispatcher**](/uwp/api/windows.ui.core.coredispatcher)（绑定到 [**CoreWindow**](/uwp/api/windows.ui.core.corewindow)）。 自那以后，我们引入了更灵活高效的调度程序：[**DispatcherQueue**](/uwp/api/windows.system.dispatcherqueue)。 你可以创建适合自己使用的 **DispatcherQueue**。 让我们考虑一下这个简单的控制台应用程序。

```cppwinrt
using namespace Windows::System;

winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    auto controller{ DispatcherQueueController::CreateOnDedicatedThread() };
    RunAsync(controller.DispatcherQueue());
    getchar();
}
```

上面的示例在专用线程上创建一个队列（包含在控制器中），然后将控制器传递给协同程序。 协同程序可以使用队列在专用线程上等待（先暂停再继续）。 **DispatcherQueue** 的另一常见用法是在传统桌面或 Win32 应用的当前 UI 线程上创建一个队列。

```cppwinrt
DispatcherQueueController CreateDispatcherQueueController()
{
    DispatcherQueueOptions options
    {
        sizeof(DispatcherQueueOptions),
        DQTYPE_THREAD_CURRENT,
        DQTAT_COM_STA
    };
 
    ABI::Windows::System::IDispatcherQueueController* ptr{};
    winrt::check_hresult(CreateDispatcherQueueController(options, &ptr));
    return { ptr, take_ownership_from_abi };
}
```

上面的代码演示了如何调用 Win32 函数并将其纳入 C++/WinRT 项目中，方法是：直接调用 Win32 样式的 [**CreateDispatcherQueueController**](/windows/win32/api/dispatcherqueue/nf-dispatcherqueue-createdispatcherqueuecontroller) 函数来创建控制器，然后将生成的队列控制器的所有权以 WinRT 对象形式移交给调用方。 这也正是你能够在现有的 Petzold 样式 Win32 桌面应用程序上为高效无缝排队提供支持的方式。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue);
 
int main()
{
    Window window;
    auto controller{ CreateDispatcherQueueController() };
    RunAsync(controller.DispatcherQueue());
    MSG message;
 
    while (GetMessage(&message, nullptr, 0, 0))
    {
        DispatchMessage(&message);
    }
}
```

上面这个简单的 **main** 函数一开始就创建一个窗口。 你可以想象一下，这样会注册一个 window 类，然后调用 [**CreateWindow**](/windows/win32/api/winuser/nf-winuser-createwindoww) 来创建顶级桌面窗口。 然后调用 **CreateDispatcherQueueController** 函数来创建队列控制器，再通过该控制器拥有的调度程序队列调用某个协同程序。 然后进入传统的消息泵，在其中的此线程上自然而然地恢复协同程序。 然后，你可以回到协同程序所在的位置，在应用程序中完成异步的或基于消息的工作流。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ... // Begin on the calling thread...
 
    co_await winrt::resume_foreground(queue);
 
    ... // ...resume on the dispatcher thread.
}
```

调用 **winrt::resume_foreground** 时会始终先排队，然后展开堆栈。  也可选择设置恢复优先级。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await winrt::resume_foreground(queue, DispatcherQueuePriority::High);
 
    ...
}
```

或者，使用默认的排队顺序。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    co_await queue;
 
    ...
}
```

或者，检测队列关闭情况并对其进行适当处理，如以下示例所示。

```cppwinrt
winrt::fire_and_forget RunAsync(DispatcherQueue queue)
{
    ...
 
    if (co_await queue)
    {
        ... // Resume on dispatcher thread.
    }
    else
    {
        ... // Still on calling thread.
    }
}
```

`co_await` 表达式返回 `true`，表明会在调度程序线程上进行恢复。 换而言之，该排队操作是成功的。 与之相反的是返回 `false`，这表明执行仍保留在调用线程上，因为队列的控制器关闭，再也不能处理队列请求。

因此，在将 C++/WinRT 与协同程序配合使用的情况下，尤其是在进行一些传统的 Petzold 样式桌面应用程序开发的情况下，你拥有很大的控制权限。

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

第一个参数 (*sender*) 未命名，因为我们从未使用它。 因此，可以安全地将它保留为引用。 但请注意，*args* 是按值传递的。 请参阅上面的[参数传递](concurrency.md#parameter-passing)部分。

## <a name="awaiting-a-kernel-handle"></a>等待内核句柄

C++/WinRT 提供一个 **resume_on_signal** 类，用于在收到内核事件信号之前暂停。 你有责任确保在 `co_await resume_on_signal(h)` 返回之前句柄始终有效。 **resume_on_signal** 本身不能为你执行该操作，因为在 **resume_on_signal** 开始之前，你就可能已失去句柄，如此示例（第一个示例）所示。

```cppwinrt
IAsyncAction Async(HANDLE event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle is not valid here.
}
```

传入的**句柄**仅在函数返回之前有效，该函数（为协同程序）在第一个暂停点（在此示例中为第一个 `co_await`）返回。 在等待 **DoWorkAsync** 时，控制返回到调用方，调用帧超出范围，你再也无法知道在协同程序继续时句柄是否会有效。

从技术上来说，我们的协同程序在按值接收其参数，这符合预期（请参阅上面的[参数传递](concurrency.md#parameter-passing)）。 但在此示例中，我们需要更进一步，因此我们将遵循该指南的精神  （而不仅仅是字面涵义）。 除了句柄，我们还需要传递强引用（换句话说，所有权）。 操作方法如下。

```cppwinrt
IAsyncAction Async(winrt::handle event)
{
    co_await DoWorkAsync();
    co_await resume_on_signal(event); // The incoming handle *is* not valid here.
}
```

通过值传递 [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) 可以提供所有权语义，确保内核句柄在协同程序生存期内始终有效。

下面介绍如何调用该协同程序。

```cppwinrt
namespace
{
    winrt::handle duplicate(winrt::handle const& other, DWORD access)
    {
        winrt::handle result;
        if (other)
        {
            winrt::check_bool(::DuplicateHandle(::GetCurrentProcess(),
                other.get(), ::GetCurrentProcess(), result.put(), access, FALSE, 0));
        }
        return result;
    }

    winrt::handle make_manual_reset_event(bool initialState = false)
    {
        winrt::handle event{ ::CreateEvent(nullptr, true, initialState, nullptr) };
        winrt::check_bool(static_cast<bool>(event));
        return event;
    }
}

IAsyncAction SampleCaller()
{
    handle event{ make_manual_reset_event() };
    auto async{ Async(duplicate(event)) };

    ::SetEvent(event.get());
    event.close(); // Our handle is closed, but Async still has a valid handle.

    co_await async; // Will wake up when *event* is signaled.
}
```

## <a name="asynchronous-timeouts-made-easy"></a>异步超时变得很简单

我们对 C++/WinRT 的 C++ 协同程序的投入很大。 协同程序对编写并发代码的影响是变革性的。 就此部分讨论的示例来说，异步的详情并不重要，重要的是当场获得的结果。 因此，C++/WinRT 在实现 [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction) Windows 运行时异步操作接口时会使用一个 **get** 函数，这与 **std::function** 提供的类似。

```cppwinrt
using namespace winrt::Windows::Foundation;
int main()
{
    IAsyncAction async = ...
    async.get();
    puts("Done!");
}
```

当异步对象正在完成其操作时，**get** 函数会实施无限期的阻止。 异步对象的生存期往往很短，因此通常情况下这正是你所需要的。

但有时候，你还有其他要求：在超过一定的时间后，你不能再等待。 由于 Windows 运行时提供了构建基块，因此始终可以编写那样功能的代码。 不过，现在 C++/WinRT 提供的 **wait_for** 函数使之变得容易得多。 它还在 **IAsyncAction** 上实现。同样，它与 **std::function** 提供的类似。

```cppwinrt
using namespace std::chrono_literals;
int main()
{
    IAsyncAction async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        puts("done");
    }
}
```

> [!NOTE]
> **wait_for** 在接口上使用 **std::chrono::duration**，但它有一个受限范围，该范围小于 **std::chrono::duration** 提供的值（大约为 49.7 天）。

下面这个示例中的 **wait_for** 会先等待约五秒钟，然后检查完成情况。 如果比较后得出的结果良好，则表明异步对象已成功完成，你的任务完成。 如果你在等待某个结果，则可随后调用 **get** 函数来检索结果。

```cppwinrt
int main()
{
    IAsyncOperation<int> async = ...
 
    if (async.wait_for(5s) == AsyncStatus::Completed)
    {
        printf("result %d\n", async.get());
    }
}
```

由于异步对象其时已经完成，因此 **get** 函数会立即返回结果，不需进一步的等待。 可以看到，**wait_for** 返回了异步对象的状态。 因此，你可以将它用于更精细的控制，如下所示。

```cppwinrt
switch (async.wait_for(5s))
{
case AsyncStatus::Completed:
    printf("result %d\n", async.get());
    break;
case AsyncStatus::Canceled:
    puts("canceled");
    break;
case AsyncStatus::Error:
    puts("failed");
    break;
case AsyncStatus::Started:
    puts("still running");
    break;
}
```

- 记住，**AsyncStatus::Completed** 意味着异步对象成功完成，你可以调用 **get** 函数检索任何结果。
- **AsyncStatus::Canceled** 意味着异步对象已取消。 取消通常是由调用方请求的，因此需要处理该状态的情况很罕见。 通常会直接将已取消的异步对象丢弃。
- **AsyncStatus::Error** 意味着，从某些方面来看，异步对象已失败。 可以根据需要通过 **get** 重新引发异常。
- **AsyncStatus::Started** 意味着异步对象仍在运行。 Windows 运行时异步模式不允许多个等待，也不允许多个等待程序。 这意味着不能以循环方式调用 **wait_for**。 如果等待实际上已超时，你可以有多个选择。 可以放弃该对象，也可以轮询其状态，然后调用 **get** 来检索任何结果。 不过，此时最佳选择是直接丢弃该对象。

## <a name="returning-an-array-asynchronously"></a>异步返回数组

以下是 [MIDL 3.0](/uwp/midl-3/) 示例，它生成*错误 MIDL2025: [msg]syntax error [context]: expecting > or, near "["* 。

```idl
Windows.Foundation.IAsyncOperation<Int32[]> RetrieveArrayAsync();
```

原因在于将数组用作参数化接口的形参类型实参无效。 因此，我们需要一种不太明显的方式来实现通过运行时类方法以异步方式传递回数组的目标。 

可以将装箱的数组返回到 [PropertyValue](/uwp/api/windows.foundation.propertyvalue) 对象。 然后，调用代码会取消装箱。 以下是一个代码示例，在此示例中，可以将 SampleComponent 运行时类添加到 Windows Runtime Component (C++/WinRT) 项目，然后从（例如） Core App (C++/WinRT) 项目使用它    。

```cppwinrt
// SampleComponent.idl
namespace MyComponentProject
{
    runtimeclass SampleComponent
    {
        Windows.Foundation.IAsyncOperation<IInspectable> RetrieveCollectionAsync();
    };
}

// SampleComponent.h
...
struct SampleComponent : SampleComponentT<SampleComponent>
{
    ...
    Windows::Foundation::IAsyncOperation<Windows::Foundation::IInspectable> RetrieveCollectionAsync()
    {
        co_return Windows::Foundation::PropertyValue::CreateInt32Array({ 99, 101 }); // Box an array into a PropertyValue.
    }
}
...

// SampleCoreApp.cpp
...
MyComponentProject::SampleComponent m_sample_component;
...
auto boxed_array{ co_await m_sample_component.RetrieveCollectionAsync() };
auto property_value{ boxed_array.as<winrt::Windows::Foundation::IPropertyValue>() };
winrt::com_array<int32_t> my_array;
property_value.GetInt32Array(my_array); // Unbox back into an array.
...
```

## <a name="important-apis"></a>重要的 API
* [IAsyncAction 接口](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt; 接口](/uwp/api/windows.foundation.iasyncactionwithprogress_tprogress_)
* [IAsyncOperation&lt;TResult&gt; 接口](/uwp/api/windows.foundation.iasyncoperation_tresult_)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt; 接口](/uwp/api/windows.foundation.iasyncoperationwithprogress_tresult_tprogress_)
* [SyndicationClient::RetrieveFeedAsync 方法](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::get_cancellation_token](/uwp/cpp-ref-for-winrt/get-cancellation-token)
* [winrt::get_progress_token](/uwp/cpp-ref-for-winrt/get-progress-token)
* [winrt::resume_foreground](/uwp/cpp-ref-for-winrt/resume-foreground)

## <a name="related-topics"></a>相关主题
* [并发和异步操作](concurrency.md)
* [在 C++/WinRT 中使用委托处理事件](handle-events.md)