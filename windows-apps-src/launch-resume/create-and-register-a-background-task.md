---
title: 创建和注册进程外后台任务
description: 创建一个进程外后台任务类并注册它，以便在应用不在前台运行时运行。
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.date: 02/27/2019
ms.topic: article
keywords: windows 10, uwp, background task
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 2124a7141740ef9c16273714864587feff4268f3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258691"
---
# <a name="create-and-register-an-out-of-process-background-task"></a>创建和注册进程外后台任务

**重要的 API**

-   [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

创建一个后台任务类并注册它，以便在应用不在前台运行时运行。 本主题演示了如何创建和注册在单独进程（而不是应用的进程）中运行的后台任务。 若要直接在前台应用程序中执行后台任务，请参阅[创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)。

> [!NOTE]
> 如果你使用后台任务在后台播放媒体，请参阅[在后台播放媒体](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio)，了解有关 Windows 10 版本 1607 中使此操作更加简单的改进信息。

## <a name="create-the-background-task-class"></a>创建后台任务类

你可以通过编写用于实现 [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 接口的类来在后台运行代码。 This code runs when a specific event is triggered by using, for example, [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) or [**MaintenanceTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger).

以下示例向你展示如何编写用于实现 [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 接口的新类。

1.  为后台任务创建新项目并将其添加到你的解决方案。 To do this, right-click on your solution node in the **Solution Explorer** and select **Add** \> **New Project**. Then select the **Windows Runtime Component** project type, name the project, and click OK.
2.  从通用 Windows 平台 (UWP) 应用项目中引用后台任务项目。 For a C# or C++ app, in your app project, right-click on **References** and select **Add New Reference**. 在**解决方案**下，选择**项目**，然后选择你的后台任务项目名称并单击**确定**。
3.  To the background tasks project, add a new class that implements the [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) interface. The [**IBackgroundTask.Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) method is a required entry point that will be called when the specified event is triggered; this method is required in every background task.

> [!NOTE]
> The background task class itself&mdash;and all other classes in the background task project&mdash;need to be **public** classes that are **sealed** (or **final**).

The following sample code shows a very basic starting point for a background task class.

```csharp
// ExampleBackgroundTask.cs
using Windows.ApplicationModel.Background;

namespace Tasks
{
    public sealed class ExampleBackgroundTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            
        }        
    }
}
```

```cppwinrt
// First, add ExampleBackgroundTask.idl, and then build.
// ExampleBackgroundTask.idl
namespace Tasks
{
    [default_interface]
    runtimeclass ExampleBackgroundTask : Windows.ApplicationModel.Background.IBackgroundTask
    {
        ExampleBackgroundTask();
    }
}

// ExampleBackgroundTask.h
#pragma once

#include "ExampleBackgroundTask.g.h"

namespace winrt::Tasks::implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask>
    {
        ExampleBackgroundTask() = default;

        void Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance);
    };
}

namespace winrt::Tasks::factory_implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask, implementation::ExampleBackgroundTask>
    {
    };
}

// ExampleBackgroundTask.cpp
#include "pch.h"
#include "ExampleBackgroundTask.h"

namespace winrt::Tasks::implementation
{
    void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
    {
        throw hresult_not_implemented();
    }
}
```

```cpp
// ExampleBackgroundTask.h
#pragma once

using namespace Windows::ApplicationModel::Background;

namespace Tasks
{
    public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    {

    public:
        ExampleBackgroundTask();

        virtual void Run(IBackgroundTaskInstance^ taskInstance);
        void OnCompleted(
            BackgroundTaskRegistration^ task,
            BackgroundTaskCompletedEventArgs^ args
        );
    };
}

// ExampleBackgroundTask.cpp
#include "ExampleBackgroundTask.h"

using namespace Tasks;

void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
}
```

4.  如果你在后台任务中运行任何异步代码，则你的后台任务需要使用延迟。 If you don't use a deferral, then the background task process can terminate unexpectedly if the **Run** method returns before any asynchronous work has run to completion.

Request the deferral in the **Run** method before calling the asynchronous method. Save the deferral to a class data member so that it can be accessed from the asynchronous method. 完成异步代码之后声明延迟完成。

The following sample code gets the deferral, saves it, and releases it when the asynchronous code is complete.

```csharp
BackgroundTaskDeferral _deferral; // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation
public async void Run(IBackgroundTaskInstance taskInstance)
{
    _deferral = taskInstance.GetDeferral();
    //
    // TODO: Insert code to start one or more asynchronous methods using the
    //       await keyword, for example:
    //
    // await ExampleMethodAsync();
    //

    _deferral.Complete();
}
```

```cppwinrt
// ExampleBackgroundTask.h
...
private:
    Windows::ApplicationModel::Background::BackgroundTaskDeferral m_deferral{ nullptr };

// ExampleBackgroundTask.cpp
...
Windows::Foundation::IAsyncAction ExampleBackgroundTask::Run(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    m_deferral = taskInstance.GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.
    // TODO: Modify the following line of code to call a real async function.
    co_await ExampleCoroutineAsync(); // Run returns at this point, and resumes when ExampleCoroutineAsync completes.
    m_deferral.Complete();
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    m_deferral = taskInstance->GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.

    //
    // TODO: Modify the following line of code to call a real async function.
    //       Note that the task<void> return type applies only to async
    //       actions. If you need to call an async operation instead, replace
    //       task<void> with the correct return type.
    //
    task<void> myTask(ExampleFunctionAsync());

    myTask.then([=]() {
        m_deferral->Complete();
    });
}
```

> [!NOTE]
> 在 C# 中，可以使用 **async/await** 关键字调用后台任务的异步方法。 In C++/CX, a similar result can be achieved by using a task chain.

有关异步模式的详细信息，请参阅[异步编程](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps)。 有关如何使用延迟阻止后台任务提前停止的其他示例，请参阅[后台任务示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)。

以下步骤在你的一个应用类（例如 MainPage.xaml.cs）中完成。

> [!NOTE]
> You can also create a function dedicated to registering background tasks&mdash;see [Register a background task](register-a-background-task.md). In that case, instead of using the next three steps, you can simply construct the trigger and provide it to the registration function along with the task name, task entry point, and (optionally) a condition.

## <a name="register-the-background-task-to-run"></a>注册要运行的后台任务

1.  Find out whether the background task is already registered by iterating through the [**BackgroundTaskRegistration.AllTasks**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) property. 此步骤非常重要；如果应用不检查现有后台任务注册，则它可能会轻松多次注册该任务，这会导致性能问题和工作结束前超出任务的最大可用 CPU 时间。

The following example iterates on the **AllTasks** property and sets a flag variable to true if the task is already registered.

```csharp
var taskRegistered = false;
var exampleTaskName = "ExampleBackgroundTask";

foreach (var task in BackgroundTaskRegistration.AllTasks)
{
    if (task.Value.Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}
```

```cppwinrt
std::wstring exampleTaskName{ L"ExampleBackgroundTask" };

auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

bool taskRegistered{ false };
for (auto const& task : allTasks)
{
    if (task.Value().Name() == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.
```

```cpp
boolean taskRegistered = false;
Platform::String^ exampleTaskName = "ExampleBackgroundTask";

auto iter = BackgroundTaskRegistration::AllTasks->First();
auto hascur = iter->HasCurrent;

while (hascur)
{
    auto cur = iter->Current->Value;

    if(cur->Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }

    hascur = iter->MoveNext();
}
```

2.  如果后台任务尚未注册，则使用 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 创建你的后台任务的一个实例。 任务入口点应为命名空间为前缀的后台任务的名称。

后台任务触发器控制后台任务何时运行。 有关可能的触发器的列表，请参阅 [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)。

For example, this code creates a new background task and sets it to run when the **TimeZoneChanged** trigger occurs:

```csharp
var builder = new BackgroundTaskBuilder();

builder.Name = exampleTaskName;
builder.TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
```

```cppwinrt
if (!taskRegistered)
{
    Windows::ApplicationModel::Background::BackgroundTaskBuilder builder;
    builder.Name(exampleTaskName);
    builder.TaskEntryPoint(L"Tasks.ExampleBackgroundTask");
    builder.SetTrigger(Windows::ApplicationModel::Background::SystemTrigger{
        Windows::ApplicationModel::Background::SystemTriggerType::TimeZoneChange, false });
    // The code in the next step goes here.
}
```

```cpp
auto builder = ref new BackgroundTaskBuilder();

builder->Name = exampleTaskName;
builder->TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
```

3.  （可选）在触发器事件发生后，你可以添加条件控制任务何时运行。 例如，如果你不希望在用户存在前运行任务，请使用条件 **UserPresent**。 有关可能条件的列表，请参阅 [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType)。

以下示例代码分配需要用户存在的条件：

```csharp
builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
```

```cppwinrt
builder.AddCondition(Windows::ApplicationModel::Background::SystemCondition{ Windows::ApplicationModel::Background::SystemConditionType::UserPresent });
// The code in the next step goes here.
```

```cpp
builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
```

4.  通过在 [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) 对象上调用 Register 方法来注册后台任务。 存储 [**BackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration) 结果，以便可以在下一步中使用该结果。

以下代码注册后台任务并存储结果：

```csharp
BackgroundTaskRegistration task = builder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ builder.Register() };
```

```cpp
BackgroundTaskRegistration^ task = builder->Register();
```

> [!NOTE]
> 通用 Windows 应用必须在注册任何后台触发器类型之前调用 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)。

若要确保通用 Windows 应用在你发布更新后继续正常运行，请使用 **ServicingComplete**（请参阅 [SystemTriggerType](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)）触发器执行任何更新后配置更改（如迁移应用的数据库和注册后台任务）。 最佳做法是注销与以前版本的应用关联的后台任务（请参阅 [**RemoveAccess**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)），同时为新版本的应用注册后台任务（请参阅 [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)）。

有关详细信息，请参阅[后台任务指南](guidelines-for-background-tasks.md)。

## <a name="handle-background-task-completion-using-event-handlers"></a>使用事件处理程序处理后台任务完成

你应该使用 [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler) 注册一个方法，以便应用可以从后台任务中获取结果。 When the app is launched or resumed, the marked method will be called if the background task has completed since the last time the app was in the foreground. （如果应用当前位于前台时后台任务完成，将立即调用 OnCompleted 方法。）

1.  编写一个 OnCompleted 方法，以处理后台任务的完成。 例如，后台任务结果可能导致 UI 更新。 此处所示的方法足迹对于 OnCompleted 事件处理程序方法来说是必需的，即使该示例不使用 *args* 参数也是如此。

以下示例代码识别后台任务完成并调用可获取消息字符串的一个示例 UI 更新方法。

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    var key = task.TaskId.ToString();
    var message = settings.Values[key].ToString();
    UpdateUI(message);
}
```

```cppwinrt
void UpdateUI(winrt::hstring const& message)
{
    MyTextBlock().Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [=]()
    {
        MyTextBlock().Text(message);
    });
}

void OnCompleted(
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& sender,
    Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // You'll previously have inserted this key into local settings.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings().Values() };
    auto key{ winrt::to_hstring(sender.TaskId()) };
    auto message{ winrt::unbox_value<winrt::hstring>(settings.Lookup(key)) };

    UpdateUI(message);
}
```

```cpp
void MainPage::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    auto settings = ApplicationData::Current->LocalSettings->Values;
    auto key = task->TaskId.ToString();
    auto message = dynamic_cast<String^>(settings->Lookup(key));
    UpdateUI(message);
}
```

> [!NOTE]
> UI 更新应该异步执行，为的是避免占用 UI 线程。 有关示例，请参阅[后台任务示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)中的 UpdateUI 方法。

2.  回到已注册后台任务的位置。 在该代码行之后，添加一个新的 [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler) 对象。 提供 OnCompleted 方法作为 **BackgroundTaskCompletedEventHandler** 构造函数的参数。

以下示例代码将一个 [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler) 添加到 [**BackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration)：

```csharp
task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
```

```cppwinrt
task.Completed({ this, &MainPage::OnCompleted });
```

```cpp
task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &MainPage::OnCompleted);
```

## <a name="declare-in-the-app-manifest-that-your-app-uses-background-tasks"></a>Declare in the app manifest that your app uses background tasks

必须先在应用清单中声明各个后台任务，你的应用才能运行后台任务。 If your app attempts to register a background task with a trigger that isn't listed in the manifest, the registration of the background task will fail with a "runtime class not registered" error.

1.  通过打开名为 Package.appxmanifest 的文件打开程序包清单设计器。
2.  打开“声明”选项卡。
3.  在**可用声明**下拉菜单中，选择**后台任务**，然后单击**添加**。
4.  选中**系统事件**复选框。
5.  In the **Entry point:** textbox, enter the namespace and name of your background class which is for this example is Tasks.ExampleBackgroundTask.
6.  关闭清单设计器。

以下 Extensions 元素将添加到 Package.appxmanifest 文件以注册后台任务：

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTask">
    <BackgroundTasks>
      <Task Type="systemEvent" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="summary-and-next-steps"></a>摘要和后续步骤

现在，你应该已基本了解如何编写后台任务类、如何从应用中注册后台任务，以及如何让应用识别后台任务何时完成。 你还应该了解如何更新应用程序清单，以便你的应用可以成功注册后台任务。

> [!NOTE]
> 下载[后台任务示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask)以查看使用后台任务的完整且可靠的 UWP 应用上下文中的类似代码示例。

有关 API 引用、后台任务概念指南以及编写使用后台任务的应用的更多详细说明，请参阅以下相关主题。

## <a name="related-topics"></a>相关主题

**Detailed background task instructional topics**

* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [注册后台任务](register-a-background-task.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
* [创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)。
* [Convert an out-of-process background task to an in-process background task](convert-out-of-process-background-task.md)  

**Background task guidance**

* [后台任务指南](guidelines-for-background-tasks.md)
* [调试后台任务](debug-a-background-task.md)
* [How to trigger suspend, resume, and background events in UWP apps (when debugging)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

**Background Task API Reference**

* [**Windows.ApplicationModel.Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)
