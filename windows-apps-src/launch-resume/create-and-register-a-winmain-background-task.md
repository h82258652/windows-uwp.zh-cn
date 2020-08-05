---
title: 创建并注册 winmain 后台任务
description: 创建一个 COM 后台任务，它可以在你的主进程中运行，也可以在你的打包 winmain 应用程序无法运行时在进程外运行。
ms.assetid: 8CBD4986-6E65-4374-BC7C-C38908E417E1
ms.date: 03/27/2020
ms.topic: article
keywords: windows 10，桌面桥，稀疏签名打包，winmain，后台任务
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 1e06a87ce771f603721c928b984d0f57d8e45013
ms.sourcegitcommit: 1d53d89bd3d044f4a2dc290b93c1ad15a088b361
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87547309"
---
# <a name="create-and-register-a-winmain-com-background-task"></a>创建并注册 winmain COM 后台任务

> [!TIP]
> 从 Windows 10 版本2004开始提供 BackgroundTaskBuilder. SetTaskEntryPointClsid 方法。

> [!NOTE]
> 此方案仅适用于打包的 WinMain 应用。 UWP 应用程序在尝试实现此方案时将遇到错误。

**重要的 API**

-   [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)

创建一个 COM 后台任务类，并将其注册为在完全信任的打包 winmain 应用中运行，以响应触发器。 你可以使用后台任务在应用暂停或未运行时提供功能。 本主题演示如何创建和注册可在前台应用进程或其他进程中运行的后台任务。

## <a name="create-the-background-task-class"></a>创建后台任务类

你可以通过编写用于实现 [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 接口的类来在后台运行代码。 此代码在使用触发特定事件时运行，例如， [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)或[**TimeTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.TimeTrigger)。

以下步骤说明如何编写实现[**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)接口的新类并将其添加到主进程。

1.  [**请参阅这些说明**](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-enhance)，以在打包的 WinMain 应用程序解决方案中引用 WinRT api。 这是使用 IBackgroundTask 和相关 Api 所必需的。
2.  在该新类中，实现[**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)接口。 [**IBackgroundTask**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)方法是在触发指定事件时将调用的必需入口点;每个后台任务都需要此方法。

> [!NOTE]
> 后台任务类本身 &mdash; 以及后台任务项目中的所有其他类都 &mdash; 需要是**公共**的。

下面的示例代码演示了一个基本的基本任务类，该类对 primes 进行计数，并将其写入文件，直到请求取消为止。

C + +/WinRT 示例将后台任务类作为[**COM coclass**](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/author-coclasses#implement-the-coclass-and-class-factory)实现。


<details>
<summary>后台任务代码示例</summary>
<p>

```csharp

using System;
using System.IO; // Path
using System.Threading; // EventWaitHandle
using System.Collections.Generic; // Queue
using System.Runtime.InteropServices; // Guid, RegistrationServices
using Windows.ApplicationModel.Background; // IBackgroundTask

namespace PackagedWinMainBackgroundTaskSample
{
    // {14C5882B-35D3-41BE-86B2-5106269B97E6} is GUID to register this task with BackgroundTaskBuilder. Generate a random GUID before implementing.
    [ComVisible(true)]
    [ClassInterface(ClassInterfaceType.None)]
    [Guid("14C5882B-35D3-41BE-86B2-5106269B97E6")]
    [ComSourceInterfaces(typeof(IBackgroundTask))]
    public class SampleTask : IBackgroundTask
    {
        private volatile int cleanupTask; // flag used to indicate to Run method that it should exit
        private Queue<int> numbersQueue; // the data structure holding the set of primes in memory

        private const int maxPrimeNumber = 1000000000; // the number up to which task will attempt to calculate primes
        private const int queueDepthToWrite = 10; // how frequently this task should flush its queue of primes
        private const string numbersQueueFile = "numbersQueue.log"; // the file to write to relative to AppData

        public SampleTask()
        {
            cleanupTask = 0;
            numbersQueue = new Queue<int>(queueDepthToWrite);
        }

        /// <summary>
        /// This method writes all the numbers in the current queue to the specified file.
        /// </summary>
        private void FlushNumbersToFile(Queue<int> queueToWrite)
        {
            string logPath = Path.Combine(ApplicationData.Current.LocalFolder.Path,
                                        System.Diagnostics.Process.GetCurrentProcess().ProcessName);

            if (!Directory.Exists(logPath))
            {
                Directory.CreateDirectory(logPath);
            }

            logPath = Path.Combine(logPath, numbersQueueFile);

            const string delimiter = ", ";
            UnicodeEncoding unicodeEncoding = new UnicodeEncoding();
            // convert the queue to a list of comma separated values.
            string stringToWrite = String.Join(delimiter, queueToWrite);
            // Add the comma at the end.
            stringToWrite += delimiter;

            File.AppendAllText(logPath, stringToWrite);
        }

        /// <summary>
        /// This method determines if the specified number is a prime number.
        /// </summary>
        private bool IsPrimeNumber(int dividend)
        {
            bool isPrime = true;
            for (int divisor = dividend - 1; divisor > 1; divisor -= 1)
            {
                if ((dividend % divisor) == 0)
                {
                    isPrime = false;
                    break;
                }
            }

            return isPrime;
        }

        /// <summary>
        /// Given the current number, this method calculates the next prime number (excluding the specified number).
        /// </summary>
        private int GetNextPrime(int previousNumber)
        {
            int currentNumber = previousNumber + 1;
            while (!IsPrimeNumber(currentNumber))
            {
                currentNumber += 1;
            }

            return currentNumber;
        }

        /// <summary>
        /// This method is the main entry point for the background task. The system will believe this background task
        /// is complete when this method returns.
        /// </summary>
        [MTAThread]
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Start with the first applicable number.
            int currentNumber = 1;

            taskDeferral = taskInstance.GetDeferral();

            // Wire the cancellation handler.
            taskInstance.Canceled += this.OnCanceled;

            // Set the progress to indicate this task has started
            taskInstance.Progress = 10;

            // Calculate primes until a cancellation has been requested or until
            // the maximum number is reached.
            while ((cleanupTask == 0) && (currentNumber < maxPrimeNumber)) {
                // Compute the next prime number and add it to our queue.
                currentNumber = GetNextPrime(currentNumber);
                numbersQueue.Enqueue(currentNumber);
                // Once the queue is filled to its max size, flush the numbers to the file.
                if (numbersQueue.Count >= queueDepthToWrite)
                {
                    FlushNumbersToFile(numbersQueue);
                    numbersQueue.Clear();
                }
            }

            // Flush any remaining numbers to the file as part of cleanup.
            FlushNumbersToFile(numbersQueue);

            if (taskDeferral != null)
            {
                taskDeferral.Complete();
            }
        }

        /// <summary>
        /// This method is signaled when the system requests the background task be canceled. This method will signal
        /// to the Run method to clean up and return.
        /// </summary>
        [MTAThread]
        public void OnCanceled(IBackgroundTaskInstance taskInstance, BackgroundTaskCancellationReason cancellationReason)
        {
            cleanupTask = 1;
        }
    }
}

```

```cppwinrt

#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.ApplicationModel.Background.h>

using namespace winrt;
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::Foundation::Collections;
using namespace winrt::Windows::ApplicationModel::Background;

namespace PackagedWinMainBackgroundTaskSample {

    // Note insert unique UUID.
    struct __declspec(uuid("14C5882B-35D3-41BE-86B2-5106269B97E6"))
    SampleTask : implements<SampleTask, IBackgroundTask>
    {
        const unsigned int MaximumPotentialPrime = 1000000000;
        volatile bool isCanceled = false;
        BackgroundTaskDeferral taskDeferral = nullptr;

        void __stdcall Run (_In_ IBackgroundTaskInstance taskInstance)
        {
            taskInstance.Canceled({ this, &SampleTask::OnCanceled });

            taskDeferral = taskInstance.GetDeferral();

            unsigned int currentPrimeNumber = 1;
            while (!isCanceled && (currentPrimeNumber < MaximumPotentialPrime))
            {
                currentPrimeNumber = GetNextPrime(currentPrimeNumber);
            }

            taskDeferral.Complete();
        }

        void __stdcall OnCanceled (_In_ IBackgroundTaskInstance, _In_ BackgroundTaskCancellationReason)
        {
            isCanceled = true;
        }
    };

    struct TaskFactory : implements<TaskFactory, IClassFactory>
    {
        HRESULT __stdcall CreateInstance (_In_opt_ IUnknown* aggregateInterface, _In_ REFIID interfaceId, _Outptr_ VOID** object) noexcept final
        {
            if (aggregateInterface != NULL) {
                return CLASS_E_NOAGGREGATION;
            }

            return make<SampleTask>().as(interfaceId, object);
        }

        HRESULT __stdcall LockServer (BOOL) noexcept final
        {
            return S_OK;
        }
    };
}

```

</p>
</details>


## <a name="add-the-support-code-to-instantiate-the-com-class"></a>添加支持代码以实例化 COM 类

为了使后台任务激活到完全信任的 winmain 应用程序中，后台任务类必须具有支持代码，以便 COM 了解如何启动应用程序进程（如果它未在运行），然后了解进程的哪个实例当前是用于处理该后台任务的新激活的服务器。

1.  如果应用程序尚未运行，COM 需要了解如何启动该应用程序。 承载后台任务代码的应用进程需要在包清单中声明。 下面的示例代码演示了如何在**SampleBackgroundApp.exe**中承载**SampleTask** 。 当后台任务在没有进程正在运行时启动时，将用进程参数 **"-StartSampleTaskServer"** 启动**SampleBackgroundApp.exe** 。

```xml

<Extensions>
  <com:Extension Category="windows.comServer">
    <com:ComServer>
      <com:ExeServer Executable="SampleBackgroundApp\SampleBackgroundApp.exe" DisplayName="SampleBackgroundApp" Arguments="-StartSampleTaskServer">
        <com:Class Id="14C5882B-35D3-41BE-86B2-5106269B97E6" DisplayName="Sample Task" />
      </com:ExeServer>
    </com:ComServer>
  </com:Extension>
</Extensions>

```

2.  当你的进程通过正确的参数启动后，它应告诉 COM 它是当前 SampleTask 的新实例的 COM 服务器。 下面的示例代码演示如何将应用程序进程注册到 COM。 请注意，这些示例指示在退出之前，该过程如何将自身声明为 SampleTask 的 COM 服务器来完成至少一个实例。 这是可选的，处理后台任务可能会启动您的主要进程函数。

```csharp

class SampleTaskServer
{
    SampleTaskServer()
    {
        comRegistrationToken = 0;
        waitHandle = new EventWaitHandle(false, EventResetMode.AutoReset);
    }

    ~SampleTaskServer()
    {
        Stop();
    }

    public void Start()
    {
        RegistrationServices registrationServices = new RegistrationServices();
        comRegistrationToken = registrationServices.RegisterTypeForComClients(typeof(SampleTask), RegistrationClassContext.LocalServer, RegistrationConnectionType.MultipleUse);

        // Either have the background task signal this handle when it completes, or never signal this handle to keep this
        // process as the COM server until the process is closed.
        waitHandle.WaitOne();
    }

    public void Stop()
    {
        if (comRegistrationToken != 0)
        {
            RegistrationServices registrationServices = new RegistrationServices();
            registrationServices.UnregisterTypeForComClients(registrationCookie);
        }

        waitHandle.Set();
    }

    private int comRegistrationToken;
    private EventWaitHandle waitHandle;
}

var sampleTaskServer = new SampleTaskServer();
sampleTaskServer.Start();

```

```cppwinrt

class SampleTaskServer
{
public:
    SampleTaskServer()
    {
        waitHandle = EventWaitHandle(false, EventResetMode::AutoResetEvent);
        comRegistrationToken = 0;
    }

    ~SampleTaskServer()
    {
        Stop();
    }

    void Start()
    {
        try
        {
            com_ptr<IClassFactory> taskFactory = make<TaskFactory>();

            winrt::check_hresult(CoRegisterClassObject(__uuidof(SampleTask),
                                                       taskFactory.get(),
                                                       CLSCTX_LOCAL_SERVER,
                                                       REGCLS_MULTIPLEUSE,
                                                       &comRegistrationToken));

            // Either have the background task signal this handle when it completes, or never signal this handle to
            // keep this process as the COM server until the process is closed.
            waitHandle.WaitOne();

        }
        catch (...)
        {
            // Indicate an error has been encountered.
        }
    }

    void Stop()
    {
        if (comRegistrationToken != 0)
        {
            CoRevokeClassObject(comRegistrationToken);
        }

        waitHandle.Set();
    }

private:
    DWORD comRegistrationToken;
    EventWaitHandle waitHandle;
};

SampleTaskServer sampleTaskServer;
sampleTaskServer.Start();

```


## <a name="register-the-background-task-to-run"></a>注册要运行的后台任务

1.  通过循环访问[**BackgroundTaskRegistration. AllTasks**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks)属性，确定是否已注册了后台任务。 *此步骤非常重要*;如果你的应用程序不检查现有的后台任务注册，它可以轻松地多次注册任务，从而导致性能问题，并支持任务在工作完成前的可用 CPU 时间。 应用程序可以自由使用同一入口点来处理所有后台任务，并使用其他属性（例如，分配给[**BackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration)的[**名称**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.name#Windows_ApplicationModel_Background_BackgroundTaskRegistration_Name)或[**TaskId**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.taskid#Windows_ApplicationModel_Background_BackgroundTaskRegistration_TaskId) ）来决定应执行的操作。

下面的示例将循环访问**AllTasks**属性，并将一个标志变量设置为 true （如果该任务已注册）。

```csharp

var taskRegistered = false;
var sampleTaskName = "SampleTask";

foreach (var task in BackgroundTaskRegistration.AllTasks)
{
    if (task.Value.Name == sampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.

```

```cppwinrt

bool taskRegistered = false;
std::wstring sampleTaskName = L"SampleTask";
auto allTasks = BackgroundTaskRegistration::AllTasks();

for (auto const& task : allTasks)
{
    if (task.Value().Name() == sampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.

```

1.  如果后台任务尚未注册，则使用 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 创建你的后台任务的一个实例。 任务入口点应为命名空间为前缀的后台任务的名称。

后台任务触发器控制后台任务何时运行。 有关可能的触发器的列表，请参阅[**Windows.applicationmodel.resources.core**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background)命名空间。

> [!NOTE]
> 对于打包的 winmain 后台任务，仅支持一部分触发器。

例如，以下代码将创建一个新的后台任务，并将其设置为在15分钟的周期性[**TimeTrigger**]()上运行：

```csharp

if (!taskRegistered)
{
    var builder = new BackgroundTaskBuilder();

    builder.Name = sampleTaskName;
    builder.SetTaskEntryPointClsid(typeof(SampleTask).GUID);
    builder.SetTrigger(new TimeTrigger(15, false));
}

// The code in the next step goes here.

```

```cppwinrt

if (!taskRegistered)
{
    BackgroundTaskBuilder builder;

    builder.Name(sampleTaskName);
    builder.SetTaskEntryPointClsid(__uuidof(SampleTask));
    builder.SetTrigger(TimeTrigger(15, false));
}

// The code in the next step goes here.

```

1.  （可选）在触发器事件发生后，你可以添加条件控制任务何时运行。 例如，如果你不希望在 internet 可用之前运行任务，请使用条件**InternetAvailable**。 有关可能条件的列表，请参阅 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)。

以下示例代码分配需要用户存在的条件：

```csharp
builder.AddCondition(new SystemCondition(SystemConditionType.InternetAvailable));
// The code in the next step goes here.
```

```cppwinrt
builder.AddCondition(SystemCondition{ SystemConditionType::InternetAvailable });
// The code in the next step goes here.
```

4.  通过在 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 对象上调用 Register 方法来注册后台任务。 存储 [**BackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) 结果，以便可以在下一步中使用该结果。 请注意，register 函数可能会以异常形式返回错误。 请确保在 try-catch 中调用 Register。

以下代码注册后台任务并存储结果：

```csharp

try
{
    var task = builder.Register();
}
catch (...)
{
    // Indicate an error was encountered.
}

```

```cppwinrt

try
{
    auto task = builder.Register();
}
catch (...)
{
    // Indicate an error was encountered.
}

```

## <a name="bringing-it-all-together"></a>全部整合

下面的代码示例演示运行和注册 COM winmain 后台任务所需的完整代码：

<details>
<summary>完成 winmain 应用程序包清单</summary>
<p>

```xml

<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10"
  IgnorableNamespaces="uap rescap com">

  <Identity
    Name="SamplePackagedWinMainBackgroundApp"
    Publisher="CN=Contoso"
    Version="1.0.0.0" />

  <Properties>
    <DisplayName>SamplePackagedWinMainBackgroundApp</DisplayName>
    <PublisherDisplayName>Contoso</PublisherDisplayName>
    <Logo>Images\StoreLogo.png</Logo>
  </Properties>

  <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.19041.0" MaxVersionTested="10.0.19041.0" />
  </Dependencies>

  <Resources>
    <Resource Language="x-generate"/>
  </Resources>

  <Applications>
    <Application Id="App"
                 Executable="SampleBackgroundApp\$targetnametoken$.exe"
                 EntryPoint="$targetentrypoint$">

      <uap:VisualElements
        DisplayName="SampleBackgroundApp"
        Description="SampleBackgroundApp"
        BackgroundColor="transparent"
        Square150x150Logo="Images\Square150x150Logo.png"
        Square44x44Logo="Images\Square44x44Logo.png">
        <uap:DefaultTile Wide310x150Logo="Images\Wide310x150Logo.png" />
        <uap:SplashScreen Image="Images\SplashScreen.png" />
      </uap:VisualElements>

      <Extensions>
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:ExeServer Executable="SampleBackgroundApp\SampleBackgroundApp.exe" DisplayName="SampleBackgroundApp" Arguments="-StartSampleTaskServer">
              <com:Class Id="14C5882B-35D3-41BE-86B2-5106269B97E6" DisplayName="Sample Task" />
            </com:ExeServer>
          </com:ComServer>
        </com:Extension>
      </Extensions>
    </Application>
  </Applications>

  <Capabilities>
  <rescap:Capability Name="runFullTrust" />
  </Capabilities>
</Package>

```

</p>
</details>


<details>
<summary>完成后台任务代码示例</summary>
<p>

```csharp

using System;
using System.IO; // Path
using System.Threading; // EventWaitHandle
using System.Collections.Generic; // Queue
using System.Runtime.InteropServices; // Guid, RegistrationServices
using Windows.ApplicationModel.Background; // IBackgroundTask

namespace PackagedWinMainBackgroundTaskSample
{
    // Background task implementation.
    // {14C5882B-35D3-41BE-86B2-5106269B97E6} is GUID to register this task with BackgroundTaskBuilder. Generate a random GUID before implementing.
    [ComVisible(true)]
    [ClassInterface(ClassInterfaceType.None)]
    [Guid("14C5882B-35D3-41BE-86B2-5106269B97E6")]
    [ComSourceInterfaces(typeof(IBackgroundTask))]
    public class SampleTask : IBackgroundTask
    {
        private volatile int cleanupTask; // flag used to indicate to Run method that it should exit
        private Queue<int> numbersQueue; // the data structure holding the set of primes in memory

        private const int maxPrimeNumber = 1000000000; // the number up to which task will attempt to calculate primes
        private const int queueDepthToWrite = 10; // how frequently this task should flush its queue of primes
        private const string numbersQueueFile = "numbersQueue.log"; // the file to write to relative to AppData

        public SampleTask()
        {
            cleanupTask = 0;
            numbersQueue = new Queue<int>(queueDepthToWrite);
        }

        /// <summary>
        /// This method writes all the numbers in the current queue to the specified file.
        /// </summary>
        private void FlushNumbersToFile(Queue<int> queueToWrite)
        {
            string logPath = Path.Combine(ApplicationData.Current.LocalFolder.Path,
                                        System.Diagnostics.Process.GetCurrentProcess().ProcessName);

            if (!Directory.Exists(logPath))
            {
                Directory.CreateDirectory(logPath);
            }

            logPath = Path.Combine(logPath, numbersQueueFile);

            const string delimiter = ", ";
            UnicodeEncoding unicodeEncoding = new UnicodeEncoding();
            // convert the queue to a list of comma separated values.
            string stringToWrite = String.Join(delimiter, queueToWrite);
            // Add the comma at the end.
            stringToWrite += delimiter;

            File.AppendAllText(logPath, stringToWrite);
        }

        /// <summary>
        /// This method determines if the specified number is a prime number.
        /// </summary>
        private bool IsPrimeNumber(int dividend)
        {
            bool isPrime = true;
            for (int divisor = dividend - 1; divisor > 1; divisor -= 1)
            {
                if ((dividend % divisor) == 0)
                {
                    isPrime = false;
                    break;
                }
            }

            return isPrime;
        }

        /// <summary>
        /// Given the current number, this method calculates the next prime number (excluding the specified number).
        /// </summary>
        private int GetNextPrime(int previousNumber)
        {
            int currentNumber = previousNumber + 1;
            while (!IsPrimeNumber(currentNumber))
            {
                currentNumber += 1;
            }

            return currentNumber;
        }

        /// <summary>
        /// This method is the main entry point for the background task. The system will believe this background task
        /// is complete when this method returns.
        /// </summary>
        [MTAThread]
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Start with the first applicable number.
            int currentNumber = 1;

            taskDeferral = taskInstance.GetDeferral();

            // Wire the cancellation handler.
            taskInstance.Canceled += this.OnCanceled;

            // Set the progress to indicate this task has started
            taskInstance.Progress = 10;

            // Calculate primes until a cancellation has been requested or until
            // the maximum number is reached.
            while ((cleanupTask == 0) && (currentNumber < maxPrimeNumber)) {
                // Compute the next prime number and add it to our queue.
                currentNumber = GetNextPrime(currentNumber);
                numbersQueue.Enqueue(currentNumber);
                // Once the queue is filled to its max size, flush the numbers to the file.
                if (numbersQueue.Count >= queueDepthToWrite)
                {
                    FlushNumbersToFile(numbersQueue);
                    numbersQueue.Clear();
                }
            }

            // Flush any remaining numbers to the file as part of cleanup.
            FlushNumbersToFile(numbersQueue);

            if (taskDeferral != null)
            {
                taskDeferral.Complete();
            }
        }

        /// <summary>
        /// This method is signaled when the system requests the background task be canceled. This method will signal
        /// to the Run method to clean up and return.
        /// </summary>
        [MTAThread]
        public void OnCanceled(IBackgroundTaskInstance taskInstance, BackgroundTaskCancellationReason cancellationReason)
        {
            cleanupTask = 1;
        }
    }


    // COM server startup code.
    class SampleTaskServer
    {
        SampleTaskServer()
        {
            comRegistrationToken = 0;
            waitHandle = new EventWaitHandle(false, EventResetMode.AutoReset);
        }

        ~SampleTaskServer()
        {
            Stop();
        }

        public void Start()
        {
            RegistrationServices registrationServices = new RegistrationServices();
            comRegistrationToken = registrationServices.RegisterTypeForComClients(typeof(SampleTask), RegistrationClassContext.LocalServer, RegistrationConnectionType.MultipleUse);

            // Either have the background task signal this handle when it completes, or never signal this handle to keep this
            // process as the COM server until the process is closed.
            waitHandle.WaitOne();
        }

        public void Stop()
        {
            if (comRegistrationToken != 0)
            {
                RegistrationServices registrationServices = new RegistrationServices();
                registrationServices.UnregisterTypeForComClients(registrationCookie);
            }

            waitHandle.Set();
        }

        private int comRegistrationToken;
        private EventWaitHandle waitHandle;
    }


    // Background task registration code.
    class SampleTaskRegistrar
    {
        public static void Register()
        {
            var taskRegistered = false;
            var sampleTaskName = "SampleTask";

            foreach (var task in BackgroundTaskRegistration.AllTasks)
            {
                if (task.Value.Name == sampleTaskName)
                {
                    taskRegistered = true;
                    break;
                }
            }

            if (!taskRegistered)
            {
                var builder = new BackgroundTaskBuilder();

                builder.Name = sampleTaskName;
                builder.SetTaskEntryPointClsid(typeof(SampleTask).GUID);
                builder.SetTrigger(new TimeTrigger(15, false));
            }

            try
            {
                var task = builder.Register();
            }
            catch (...)
            {
                // Indicate an error was encountered.
            }
        }
    }


    // Application entry point.
    static class Program
    {
        [MTAThread]
        static void Main()
        {
            string[] commandLineArgs = Environment.GetCommandLineArgs();
            if (commandLineArgs.Length < 2)
            {
                // Open the WPF UI when no arguments are specified.
            }
            else
            {
                if (commandLineArgs.Contains("-RegisterSampleTask", StringComparer.InvariantCultureIgnoreCase))
                {
                    SampleTaskRegistrar.Register();
                }

                if (commandLineArgs.Contains("-StartSampleTaskServer", StringComparer.InvariantCultureIgnoreCase))
                {
                    var sampleTaskServer = new SampleTaskServer();
                    sampleTaskServer.Start();
                }
            }

            return;
        }
    }
}

```

```cppwinrt

#include <unknwn.h>
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.ApplicationModel.Background.h>

using namespace winrt;
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::Foundation::Collections;
using namespace winrt::Windows::ApplicationModel::Background;

namespace PackagedWinMainBackgroundTaskSample
{
    // Background task implementation.
    // {14C5882B-35D3-41BE-86B2-5106269B97E6} is GUID to register this task with BackgroundTaskBuilder. Generate a random GUID before implementing.
    struct __declspec(uuid("14C5882B-35D3-41BE-86B2-5106269B97E6"))
    SampleTask : implements<SampleTask, IBackgroundTask>
    {
        const unsigned int maxPrimeNumber = 1000000000;
        volatile bool isCanceled = false;
        BackgroundTaskDeferral taskDeferral = nullptr;

        void __stdcall Run (_In_ IBackgroundTaskInstance taskInstance)
        {
            taskInstance.Canceled({ this, &SampleTask::OnCanceled });

            taskDeferral = taskInstance.GetDeferral();

            unsigned int currentPrimeNumber = 1;
            while (!isCanceled && (currentPrimeNumber < maxPrimeNumber))
            {
                currentPrimeNumber = GetNextPrime(currentPrimeNumber);
            }

            taskDeferral.Complete();
        }

        void __stdcall OnCanceled (_In_ IBackgroundTaskInstance, _In_ BackgroundTaskCancellationReason)
        {
            isCanceled = true;
        }
    };

    struct TaskFactory : implements<TaskFactory, IClassFactory>
    {
        HRESULT __stdcall CreateInstance (_In_opt_ IUnknown* aggregateInterface, _In_ REFIID interfaceId, _Outptr_ VOID** object) noexcept final
        {
            if (aggregateInterface != nullptr) {
                return CLASS_E_NOAGGREGATION;
            }

            return make<SampleTask>().as(interfaceId, object);
        }

        HRESULT __stdcall LockServer (BOOL) noexcept final
        {
            return S_OK;
        }
    };


    // COM server startup code.
    class SampleTaskServer
    {
    public:
        SampleTaskServer()
        {
            waitHandle = EventWaitHandle(false, EventResetMode::AutoResetEvent);
            comRegistrationToken = 0;
        }

        ~SampleTaskServer()
        {
            Stop();
        }

        void Start()
        {
            try
            {
                com_ptr<IClassFactory> taskFactory = make<TaskFactory>();

                winrt::check_hresult(CoRegisterClassObject(__uuidof(SampleTask),
                                                           taskFactory.get(),
                                                           CLSCTX_LOCAL_SERVER,
                                                           REGCLS_MULTIPLEUSE,
                                                           &comRegistrationToken));

                // Either have the background task signal this handle when it completes, or never signal this handle to
                // keep this process as the COM server until the process is closed.
                waitHandle.WaitOne();

            }
            catch (...)
            {
                // Indicate an error has been encountered.
            }
        }

        void Stop()
        {
            if (comRegistrationToken != 0)
            {
                CoRevokeClassObject(comRegistrationToken);
            }

            waitHandle.Set();
        }

    private:
        DWORD comRegistrationToken;
        EventWaitHandle waitHandle;
    };


    // Background task registration code.
    class SampleTaskRegistrar
    {
        public static void Register()
        {
            bool taskRegistered = false;
            std::wstring sampleTaskName = L"SampleTask";
            auto allTasks = BackgroundTaskRegistration::AllTasks();

            for (auto const& task : allTasks)
            {
                if (task.Value().Name() == sampleTaskName)
                {
                    taskRegistered = true;
                    break;
                }
            }

            if (!taskRegistered)
            {
                BackgroundTaskBuilder builder;

                builder.Name(sampleTaskName);
                builder.SetTaskEntryPointClsid(__uuidof(SampleTask));
                builder.SetTrigger(TimeTrigger(15, false));
            }

            try
            {
                auto task = builder.Register();
            }
            catch (...)
            {
                // Indicate an error was encountered.
            }
        }
    }

}

using namespace PackagedWinMainBackgroundTaskSample;

// Application entry point.
int wmain(_In_ int argc, _In_reads_(argc) const wchar** argv)
{
    unsigned int argumentIndex;

    winrt::init_apartment();

    if (argc <= 1)
    {
        return E_INVALIDARG;
    }

    for (argumentIndex = 0; argumentIndex < argc ; argumentIndex += 1)
    {
        if (_wcsnicmp(L"RegisterSampleTask",
                      argv[argumentIndex],
                      wcslen(L"RegisterSampleTask")) == 0)
        {
            SampleTaskRegistrar::Register();
        }

        if (_wcsnicmp(L"StartSampleTaskServer",
                      argv[argumentIndex],
                      wcslen(L"StartSampleTaskServer")) == 0)
        {
            SampleTaskServer sampleTaskServer;
            sampleTaskServer.Start();
        }
    }

    return S_OK;
}

```

</p>
</details>


## <a name="remarks"></a>备注

与可在新式备用模式下运行后台任务的 UWP 应用不同，WinMain apps 无法从新式备用的低能耗阶段运行代码。 有关详细信息，请参阅[新式备用](https://docs.microsoft.com/windows-hardware/design/device-experiences/modern-standby)。

有关 API 引用、后台任务概念指南以及编写使用后台任务的应用的更多详细说明，请参阅以下相关主题。

## <a name="related-topics"></a>相关主题

* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [注册后台任务](register-a-background-task.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
* [创建并注册进程内后台任务](create-and-register-an-inproc-background-task.md)。
* [将进程外后台任务转换为进程内后台任务](convert-out-of-process-background-task.md)

**后台任务指南**

* [后台任务指南](guidelines-for-background-tasks.md)
* [调试后台任务](debug-a-background-task.md)
* [如何在 UWP 应用中触发暂停、恢复和后台事件（在调试时）](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

**后台任务 API 引用**

* [**Windows.ApplicationModel.Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)
