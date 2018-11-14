---
title: WinRT API 错误处理
author: KevinAsgari
description: 了解在通过 WinRT API 进行 Xbox Live 服务调用时如何处理错误。
ms.assetid: b4f04798-df91-430e-90f0-83b5a48b9ab2
ms.author: kevinasg
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, 游戏, uwp, windows 10, xbox one, 错误, 处理
ms.localizationpriority: medium
ms.openlocfilehash: d1127811e44eecb03cfc9a9818733a2b2234d2c1
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2018
ms.locfileid: "6264048"
---
# <a name="winrt-api-error-handling"></a>WinRT API 错误处理

在可通过 C++/CX、C# 或 Javascript 调用的 WinRT API 中，使用异常报告错误，因此，必须使用 try/catch 块处理错误。

请注意，杜宇异步调用，需要使用两个 try/catch 块，如下所示：

```cpp
    try
    {
        auto asyncOp = xboxLiveContext->TitleStorageService->UploadBlobAsync(
            blobMetadata,
            blobBuffer,
            TitleStorageETagMatchCondition::NotUsed,
            DEFAULT_UPLOAD_BLOCK_SIZE
            );

        create_task(asyncOp)
        .then([this, ui]( task<TitleStorageBlobMetadata^> t )
        {
            try
            {
                TitleStorageBlobMetadata^ blobMetadata = t.get();

                ui->Log(L"UploadBlobAsync succeeded");
                PrintBlobMetadata(ui, blobMetadata);

                SaveNewETag(blobMetadata->ETag);
            }
            catch (Platform::Exception^ ex)
            {
                // This could happen if there's a network error or the service returns an error
                ui->Log("The async task of UploadBlobAsync failed with", ex->ToString());
            }
        });
    }
    catch (Platform::Exception^ ex)
    {
        // This could happen if there's invalid args sent to the API
        ui->Log("The API call to UploadBlobAsync failed with", ex->ToString());
    }
```

XSAPI 异步方法具有一些在调用方法时将会同步运行的代码。  同步代码用于执行诸如验证输入参数和启动异步操作之类的任务。  因此，即便是调用异步代码也可能会导致出现异常，尽管这在正常情况下应该不会发生（即程序员错误，而不是网络错误）。  请意识到存在这种可能性，并在开发过程中彻底验证输入和测试你的代码，以做好防御性编程。  无论你是在调用本身周围放置一个 try/catch，还是将其置于调用堆栈中的较高级别，重要的是要拥有它。

## <a name="help-my-game-crashes-when-i-call-xyz-xbox-service-api"></a>请求帮助！ 在我调用 XYZ Xbox 服务 API 时，我的游戏崩溃

因此，你的游戏崩溃且调用堆栈表示这是一个 Xbox 服务 API 调用，但确实是这样吗？

```cpp
msvcr110.dll!Concurrency::details::_ReportUnobservedException() Line 2455    C++
Social110Release.exe!Concurrency::details::_ExceptionHolder::~_ExceptionHolder() Line 915    C++
Social110Release.exe!Concurrency::details::_Task_impl_base::~_Task_impl_base() Line 1488    C++
Social110Release.exe!Concurrency::details::_Task_impl<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::`scalar deleting destructor'(unsigned int)    C++
Social110Release.exe!Concurrency::task<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64>::_ContinuationTaskHandle<Microsoft::Xbox::Services::Social::XboxUserProfile ^ __ptr64,void,<lambda_8571b6148830c0805feee6ba9e76a692>,std::integral_constant<bool,1>,Concurrency::details::_TypeSelectorNoAsync>::`scalar deleting destructor'(unsigned int)    C++
msvcr110.dll!Concurrency::details::_TaskCollection::_NotifyCompletedChoreAndFree(Concurrency::details::_UnrealizedChore * pChore) Line 1171    C++
msvcr110.dll!Concurrency::details::_UnrealizedChore::_UnstructuredChoreWrapper(Concurrency::details::_UnrealizedChore * pChore) Line 373    C++
msvcr110.dll!Concurrency::details::InternalContextBase::Dispatch(Concurrency::DispatchState * pDispatchState) Line 1716    C++
msvcr110.dll!Concurrency::details::FreeThreadProxy::Dispatch() Line 203    C++
msvcr110.dll!Concurrency::details::ThreadProxy::ThreadProxyMain(void * lpParameter) Line 174    C++
ntdll.dll!RtlUserThreadStart(long (void *) * StartAddress, void * Argument) Line 822    C++
```

这些调用堆栈非常混乱。  这也并发运行，那也并发运行…  噢，Microsoft::Xbox::Services::Social::XboxUserProfile 位于调用堆栈中，因此它必定是原因所在。  实际上，这是为无效 Xbox 用户 ID 调用 GetUserProfileAsync 生成的调用堆栈。生成此调用堆栈的示例代码如下所示：

```cpp
    auto pAsyncOp = requester->ProfileService->GetUserProfileAsync("abc123"); //passing invalid Xbox User Id;

    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)
    {
        // Oops, I forgot my exception handling code here.
        // If I don't call resultTask.get() and catch any potential exception it may throw,
        // then PPL will report an unobserved exception.  That unobserved exception will cause your
        // app to crash.
    });
```

你的调用堆栈是否包含 Concurrency::_ReportUnobservedException()？  再看一看以上调用堆栈。  这非常重要。  如果你的调用堆栈包含 Concurrency::_ReportUnobservedException()，那么你将会在游戏代码中发现一个 bug。

PPL 创建任务，然后接着创建其他任务。  在以上示例中，create_task() 将创建任务以调用 GetUserProfileAsync()，而 .then() 将创建后续任务。  这些通常被称为先行任务（第一个任务）和延续任务（第二个任务）。  在本示例中，延续任务缺少错误处理。  如果任务发生异常并且该异常未被任务或其延续任务之一捕获，则运行时将终止应用。

涉及到延续任务时，请注意，实际上有两种不同类型的延续任务。  第一种是基于任务的延续任务，它采用与输入参数一样的前一项任务。  此任务始终运行，即便先行任务发生异常。  若要获得先行任务的结果，你必须调用参数上的 .get()。  第二种是基于值的延续任务，它直接接收前一项任务的输出 – 当先行任务发生异常时，基于值的延续任务完全不会运行，除此以外，它是真正的语法糖。

建议：为了防止崩溃，请在延续任务链结束时使用基于任务的延续任务，并包围 try/catch 块中的所有 concurrency::task::get() or concurrency::task::wait() 调用，以处理可恢复的错误。

我们来看几个示例：

基于任务的延续任务示例

```cpp
    create_task( pAsyncOp )
    .then( [this] (task<XboxUserProfile^> resultTask)   // Task-based continuation
    {
        try
        {
            XboxUserProfile^ result = resultTask.get();

            // success, do something
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
        }
    });
```

基于值的延续任务示例

```cpp
    create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
        // if the task didn't complete successfully, you'd better have a task-based
        // continuation at the end of the continuation chain or the app will crash.
    })
    .then( [this] (task<void> previousTask) // Task-based continuation
    {
        try
        {
            // DO NOT IGNORE THIS THINKING IT'S NOT IMPORTANT.

            // call concurrency::task::get and handle any unobserved exception
            // so the application doesn't crash.
            previousTask.get();

            // success, continue
        }
        catch (Platform::Exception^ ex)
        {
            // concurrency::task::get threw an exception
            // safely handle the error here
            // By handling this exception, you ensure your application will not
            // crash when calling Xbox Service APIs
        }
    });
```

可使用第三个解决方案 – 完全使用基于值的延续任务，但在其他线程上调用 .get() 或 .wait() 和捕获上面的异常。  下面是一个简单示例：

```cpp
    auto getProfileTask = create_task( pAsyncOp )
    .then( [this] (XboxUserProfile^ result) // Value-based continuation
    {
        // The task completed successfully, do something here.
    });
    // Note the lack of a task-based continuation with error handling at the end

    // You may call .get() or .wait() on a value-based only chain, but
    // must ensure you surround the call in a try/catch block and handle errors
    try
    {
        getProfileTask.get();     // or getProfileTask.wait();
    }
    catch (Platform::Exception^ ex)
    {
        // concurrency::task::get threw an exception
        // safely handle the error here
        // By handling this exception, you ensure your application will not
        // crash when calling Xbox Service APIs
    }
```

**如果不使用 PPL** 如果你使用 AsyncOperationCompletionHandler 或 AsyncActionCompletionHandler 而不是 PPL，那么，你也需要进行一些错误处理，否则可能会导致应用程序崩溃。  以下示例展示了如何处理错误

```cpp
    try
    {
        // Example is making a service call with an invalid XboxUserId which will result in an error.
        // The completion handler properly detects the error and does not crash the app.
        requester->ProfileService->GetUserProfileAsync("abc123")->Completed
            = ref new AsyncOperationCompletedHandler<XboxUserProfile^>([=] (IAsyncOperation<XboxUserProfile^>^ operation, Windows::Foundation::AsyncStatus status)
        {
            if( status == Windows::Foundation::AsyncStatus::Completed)
            {
                // Always check the AsyncStatus before calling GetResults().
                // If status is not AsyncStatus::Completed, calls to operation->GetResults()
                // may throw an exception.
                // You can also surround this call in a try/catch block for added safety.

                XboxUserProfile^ result = operation->GetResults();

                // success, do something with the result
            }
            else if( status == Windows::Foundation::AsyncStatus::Error )
            {
                // Failed
            }
        });
    }
    catch ( Platform::COMException^ ex )
    {
        // What is this try/catch block for?
        //
        // Xbox Service APIs do have some code that runs synchronously and errors need
        // to be safely handled.  In this example, if “” was passed instead of “abc123”,
        // then an invalid argument exception would be thrown when calling GetUserProfileAsync
    // See the next section for more a more detailed explanation.
        //
        // Note: this catch block will NOT catch exceptions thrown within the completion handler.
    }
```

**GetNextAsync() and exceptions** 使用分页 API？  AchievementResult、LeaderboardResult、InventoryItemResult 和 TitleStorageBlobMetadataResult 对象均包含 GetNextAsync() 方法，用于请求下一页结果。  还有一种特殊情况，那就是无其他数据可用，在此情况下，调用 GetNextAsync() 将会触发异常。  在同步执行 GetNextAsync() 时将会发生此异常。  在此情况下，GetNextAsync 方法将会发出 INET_E_DATA_NOT_AVAILABLE (0x800C0007)。

建议：请确保包围 try/catch 块中的 GetNextAsync() 方法调用并妥善处理 INET_E_DATA_NOT_AVAILABLE 情况。

```cpp
    try
    {
        // AchievementResult^ LastResult

        // Get next page of achievement results
        if(LastResult != nullptr)
        {
            auto getNextPage = LastResult->GetNextAsync(10);

            // create_task( getNextPage ) ...
        }
    }
    catch (Platform::Exception^ ex)
    {
        if (ex->HResult == INET_E_DATA_NOT_AVAILABLE)
        {
                // we hit the end of the achievements
        }
        else
        {
            // failed for unexpected reason
        }
    }
```
