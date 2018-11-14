---
title: 异步 C API 调用模式
author: aablackm
description: 了解 XSAPI C API 的异步 C API 调用模式
ms.author: aablackm
ms.date: 06/10/2018
ms.topic: article
keywords: 'xbox live, xbox, 游戏, uwp, windows 10, xbox one, 开发人员计划, '
ms.localizationpriority: medium
ms.openlocfilehash: b247a69e0def8a2e3a62a8c05a8fac3106bded35
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2018
ms.locfileid: "6203718"
---
# <a name="calling-pattern-for-xsapi-flat-c-layer-async-calls"></a>XSAPI 平面 C 层异步调用的调用模式

**异步 API** 指 API 快速返回，但会启动一个**异步任务**，待任务完成后才返回结果。

过去，游戏很少控制由哪个线程执行**异步任务**，以及在使用**完成回调**时由哪个线程返回结果。  有些游戏的设计方式是让某个堆集部分仅供某一个线程访问，以避免线程同步的需要。 如果**完成回调**不是从游戏所控制的线程调用的，则使用**异步任务**结果更新共享状态时需要进行线程同步。

XSAPI C API 公开了一个新的异步 C API，为开发人员在执行**异步 API** 调用（如 **XblSocialGetSocialRelationshipsAsync()**、**XblProfileGetUserProfileAsync()** 和 **XblAchievementsGetAchievementsForTitleIdAsync()**）时提供了直接的线程控制。

下面是调用 **XblProfileGetUserProfileAsync** API 的一个基本示例。

```cpp
AsyncBlock* asyncBlock = new AsyncBlock {};
asyncBlock->queue = asyncQueue;
asyncBlock->context = customDataForCallback;
asyncBlock->callback = [](AsyncBlock* asyncBlock)
{
    XblUserProfile profile;
    if( SUCCEEDED( XblProfileGetUserProfileResult(asyncBlock, &profile) ) )
    {
        printf("Profile retrieved successfully\r\n");
    }
    delete asyncBlock;
};
XblProfileGetUserProfileAsync(asyncBlock, xboxLiveContext, xuid);
```

若要理解此调用模式，你需要了解如何使用 **AsyncBlock** 和 **AsyncQueue**。

* **AsyncBlock** 将携带与**异步任务**和**完成回调**相关的所有信息。

* **AsyncQueue** 可用于确定哪个线程执行**异步任务**，哪个线程调用 AsyncBlock 的**完成回调**。

## <a name="the-asyncblock"></a>**AsyncBlock**

我们细看一下 **AsyncBlock**。 它是一个定义如下的结构：

```cpp
typedef struct AsyncBlock
{
    AsyncCompletionRoutine* callback;
    void* context;
    async_queue_handle_t queue;
} AsyncBlock;
```

**AsyncBlock** 中包含：

* *callback* - 一个将在异步工作完成后调用的可选回调函数。  如果不指定回调，可以等待 **AsyncBlock** 完成并显示 **GetAsyncStatus**，然后获取结果。
* *context* - 用于向回调函数传递数据。
* *queue* - 一个 async_queue_handle_t，作为指定 **AsyncQueue** 的句柄。 如果未设置此队列，将使用默认队列。

你应该在每个异步调用的 API 在堆栈上创建新 AsyncBlock。  AsyncBlock 必须位于之前称为 AsyncBlock 的完成回调，然后可以将其删除。

> [!IMPORTANT]
> **AsyncBlock** 必须一直保留在内存中，直到**异步任务**完成。 如果是动态分配的，可以在 AsyncBlock 的**完成回调**内部将其删除。

### <a name="waiting-for-asynchronous-task"></a>等待**异步任务**

你可以通过几种不同的方式获知**异步任务**已完成：

* 调用了 AsyncBlock 的**完成回调**
* 通过 true 值调用 **GetAsyncStatus** 一直等到它完成。
* 在 **AsyncBlock** 中设置一个 waitEvent，并等待发出事件信号

借助**GetAsyncStatus**和 waitEvent，**异步任务**被视为完成后 AsyncBlock 的**完成回调**执行但 AsyncBlock 的**完成回调**是可选的。

一旦**异步任务**完成，你就可以获取结果。

### <a name="getting-the-result-of-the-asynchronous-task"></a>获取**异步任务**的结果

为了获取结果，大多数**异步 API** 函数都有相应的 \[Name of Function\]Result 函数用来接收异步调用的结果。 在我们的示例代码中，**XblProfileGetUserProfileAsync** 有一个相应的 **XblProfileGetUserProfileResult** 函数。 你可以使用此函数返回函数结果并相应执行操作。  请参阅每个**异步 API** 函数的文档，了解接收结果的详尽信息。

## <a name="the-asyncqueue"></a>**AsyncQueue**

**AsyncQueue** 可用于确定哪个线程执行**异步任务**，哪个线程调用 AsyncBlock 的**完成回调**。

你可以通过设置*调度模式*控制由哪个线程执行这些操作。 有以下三种调度模式：

* *手动* - 不自动调度手动队列。  由开发人员负责将它们调度到所需的任何线程。 此模式可用于将异步调用的工作端或回调端分配到特定线程。  下面对此有详细讨论。

* *线程池* - 调度使用线程池。  线程池并行调取调用，然后，当线程池可用时，从队列执行调用。  此模式最易使用，但对于使用哪个线程控制力度最低。

* *固定线程* - 调度在创建异步队列的线程上使用 QueueUserAPC。 将用户模式 APC 排入队列后，线程不会被引导至调用 APC 函数，除非此函数处于可警告状态。 线程通过使用 **SleepEx**、**SignalObjectAndWait**、**WaitForSingleObjectEx**、**WaitForMultipleObjectsEx** 或 **MsgWaitForMultipleObjectsEx** 执行可警告等待操作来进入可警告状态。

若要创建新的 **AsyncQueue**，需要调用 **CreateAsyncQueue**。

```cpp
STDAPI CreateAsyncQueue(
    _In_ AsyncQueueDispatchMode workDispatchMode,
    _In_ AsyncQueueDispatchMode completionDispatchMode,
    _Out_ async_queue_handle_t* queue);
```

其中 AsyncQueueDispatchMode 包括前面介绍的三种调度模式：

```cpp
typedef enum AsyncQueueDispatchMode
{
    /// <summary>
    /// Async callbacks are invoked manually by DispatchAsyncQueue
    /// </summary>
    AsyncQueueDispatchMode_Manual,

    /// <summary>
    /// Async callbacks are queued to the thread that created the queue
    /// and will be processed when the thread is alertable.
    /// </summary>
    AsyncQueueDispatchMode_FixedThread,

    /// <summary>
    /// Async callbacks are queued to the system thread pool and will
    /// be processed in order by the threadpool.
    /// </summary>
    AsyncQueueDispatchMode_ThreadPool

} AsyncQueueDispatchMode;
```

**workDispatchMode** 确定处理异步工作的线程的调度模式，**completionDispatchMode** 确定处理异步操作完成的线程的调度模式。

你还可以调用 **CreateSharedAsyncQueue**，通过为队列指定 ID 创建具有相同队列类型的 **AsyncQueue**。

```cpp
STDAPI CreateSharedAsyncQueue(
    _In_ uint32_t id,
    _In_ AsyncQueueDispatchMode workerMode,
    _In_ AsyncQueueDispatchMode completionMode,
    _Out_ async_queue_handle_t* aQueue);
```

> [!NOTE]
> 如果已存在具有此 ID 和调度模式的队列，将引用该队列。  否则，将创建新的队列。

创建 **AsyncQueue** 之后，请直接将其添加到 **AsyncBlock** 以控制工作和完成函数的线程处理。
当你完成使用**AsyncQueue**时，通常时即将结束游戏，你可以关闭它与**CloseAsyncQueue**:

```cpp
STDAPI_(void) CloseAsyncQueue(
    _In_ async_queue_handle_t aQueue);
```

### <a name="manually-dispatching-an-asyncqueue"></a>手动调度 **AsyncQueue**

如果为 **AsyncQueue** 工作或完成队列使用了手动队列调度模式，将需要执行手动调度。
假设创建了一个 **AsyncQueue**，其中工作队列和完成队列均如下设置为手动调度：

```cpp
CreateAsyncQueue(
    AsyncQueueDispatchMode_Manual,
    AsyncQueueDispatchMode_Manual,
    &queue);
```

为了调度已指定 **AsyncQueueDispatchMode_Manual** 的调度工作，必须使用 **DispatchAsyncQueue** 函数调度该工作。

```cpp
STDAPI_(bool) DispatchAsyncQueue(
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type,
    _In_ uint32_t timeoutInMs);
```

* *queue* - 在哪个队列上调度工作
* *type* - **AsyncQueueCallbackType** 枚举的实例
* *timeoutInMs* - 表示毫秒超时的 uint32_t 值。

**AsyncQueueCallbackType** 枚举定义了两种回调类型：

```cpp
typedef enum AsyncQueueCallbackType
{
    /// <summary>
    /// Async work callbacks
    /// </summary>
    AsyncQueueCallbackType_Work,

    /// <summary>
    /// Completion callbacks after work is done
    /// </summary>
    AsyncQueueCallbackType_Completion
} AsyncQueueCallbackType;
```

### <a name="when-to-call-dispatchasyncqueue"></a>何时调用 **DispatchAsyncQueue**

为了检查队列何时收到了新项目，可以调用 **AddAsyncQueueCallbackSubmitted** 设置一个事件处理程序，以告知代码工作或完成已准备好接受调度。

```cpp
STDAPI AddAsyncQueueCallbackSubmitted(
    _In_ async_queue_handle_t queue,
    _In_opt_ void* context,
    _In_ AsyncQueueCallbackSubmitted* callback,
    _Out_ uint32_t* token);
```

**AddAsyncQueueCallbackSubmitted** 使用以下参数：

* *queue* - 为其提交回调的异步队列。
* *context* - 一个指向应传递给提交回调的数据的指针。
* *callback* - 将新的回调提交给队列时将要调用的函数。
* *token* - 一个在后面调用 **RemoveAsynceCallbackSubmitted** 删除回调时将要使用的令牌。

例如，下面是对 **AddAsyncQueueCallbackSubmitted ** 的调用：

`AddAsyncQueueCallbackSubmitted(queue, nullptr, HandleAsyncQueueCallback, &m_callbackToken);`

相应 **AsyncQueueCallbackSubmitted** 回调的可能实现如下：

```cpp
void CALLBACK HandleAsyncQueueCallback(
    _In_ void* context,
    _In_ async_queue_handle_t queue,
    _In_ AsyncQueueCallbackType type)
{
    switch (type)
    {
    case AsyncQueueCallbackType::AsyncQueueCallbackType_Work:
        ReleaseSemaphore(g_workReadyHandle, 1, nullptr);
        break;
    }
}
```

然后在后台线程中你可以侦听此信号可以唤醒并调用**DispatchAsyncQueue**。

```cpp
DWORD WINAPI BackgroundWorkThreadProc(LPVOID lpParam)
{
    HANDLE hEvents[2] =
    {
        g_workReadyHandle.get(),
        g_stopRequestedHandle.get()
    };

    async_queue_handle_t queue = static_cast<async_queue_handle_t>(lpParam);

    bool stop = false;
    while (!stop)
    {
        DWORD dwResult = WaitForMultipleObjectsEx(2, hEvents, false, INFINITE, false);
        switch (dwResult)
        {
        case WAIT_OBJECT_0: 
            // Background work is ready to be dispatched
            DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);
            break;

        case WAIT_OBJECT_0 + 1:
        default:
            stop = true;
            break;
        }
    }

    CloseAsyncQueue(queue);
    return 0;
}
```

它是最佳做法，用于实现与 Win32 信号灯对象。  如果改为实现使用 Win32 事件对象，然后你将需要确保不会错过代码的任何事件如：

```cpp
    case WAIT_OBJECT_0: 
        // Background work is ready to be dispatched
        DispatchAsyncQueue(queue, AsyncQueueCallbackType_Work, 0);        
        
        if (!IsAsyncQueueEmpty(queue, AsyncQueueCallbackType_Work))
        {
            // If there's more pending work, then set the event to process them
            SetEvent(g_workReadyHandle.get());
        }
        break;
```


你可以在[社交 C 示例 AsyncIntegration.cpp](https://github.com/Microsoft/xbox-live-api/blob/master/InProgressSamples/Social/Xbox/C/AsyncIntegration.cpp)查看异步集成的最佳做法的示例

