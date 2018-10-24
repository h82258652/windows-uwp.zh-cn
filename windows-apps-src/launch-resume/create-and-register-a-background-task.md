---
author: TylerMSFT
title: 创建和注册进程外后台任务
description: 创建一个进程外后台任务类并注册它，以便在应用不在前台运行时运行。
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.author: twhitney
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，后台任务
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: a599fdef47bb681ef4909fe5bba2a01a1687ba66
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5439529"
---
# <a name="create-and-register-an-out-of-process-background-task"></a>创建和注册进程外后台任务

**重要的 API**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

创建一个后台任务类并注册它，以便在应用不在前台运行时运行。 本主题演示了如何创建和注册在单独进程（而不是应用的进程）中运行的后台任务。 若要直接在前台应用程序中执行后台任务，请参阅[创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)。

> [!NOTE]
> 如果你使用后台任务在后台播放媒体，请参阅[在后台播放媒体](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)，了解有关 Windows10 版本 1607 中使此操作更加简单的改进信息。

## <a name="create-the-background-task-class"></a>创建后台任务类

你可以通过编写用于实现 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 接口的类来在后台运行代码。 通过使用，例如， [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)或[**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517)触发特定事件时，在运行此代码。

以下示例向你展示如何编写用于实现 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 接口的新类。

1.  为后台任务创建新项目并将其添加到你的解决方案。 若要执行此操作，在**解决方案资源管理器**中你的解决方案节点上右键单击并选择**添加** \> **新建项目**。 然后选择**Windows 运行时组件**项目类型名称、 该项目，并单击确定。
2.  从通用 Windows 平台 (UWP) 应用项目中引用后台任务项目。 C# 或 c + + 应用，在应用项目中，右键单击**引用**，然后选择**添加新引用**。 在**解决方案**下，选择**项目**，然后选择你的后台任务项目名称并单击**确定**。
3.  到后台任务项目中，添加一个新类实现[**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)接口。 [**IBackgroundTask.Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run)方法是将触发指定的事件; 时调用的所需的入口点每个后台任务，则需要此方法。

> [!NOTE]
> 后台任务类本身&mdash;和后台任务项目中的所有其他类&mdash;需要是**密封**（或**最终**） 的**公共**类。

以下示例代码显示了一个后台任务类的非常基本的起始点。

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

4.  如果你在后台任务中运行任何异步代码，则你的后台任务需要使用延迟。 如果你不使用延迟，则后台任务进程可能会终止意外如果**Run**方法返回之前已完成运行任何异步工作。

之前，请求延迟**Run**方法中的调用异步方法。 将延迟保存到类数据成员，以便可以从异步方法访问它。 完成异步代码之后声明延迟完成。

以下示例代码获取延迟、 保存它，并异步代码完成后释放它。

```csharp
BackgroundTaskDeferral _deferral; // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation
public async void Run(IBackgroundTaskInstance taskInstance)
{
    _deferral = taskInstance.GetDeferral()
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
> 在 C# 中，可以使用 **async/await** 关键字调用后台任务的异步方法。 在 C + + /CX 可以通过使用任务链实现类似的结果。

有关异步模式的详细信息，请参阅[异步编程](https://msdn.microsoft.com/library/windows/apps/mt187335)。 有关如何使用延迟阻止后台任务提前停止的其他示例，请参阅[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)。

以下步骤在你的一个应用类（例如 MainPage.xaml.cs）中完成。

> [!NOTE]
> 你还可以创建专用于注册后台任务的函数&mdash;请参阅[注册后台任务](register-a-background-task.md)。 在此情况下，而不是使用接下来三个步骤，你可以只需构造触发器并将其提供给注册函数以及任务名称、 任务入口点，和 （可选） 条件。

## <a name="register-the-background-task-to-run"></a>注册要运行的后台任务

1.  找出是否通过循环[**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787)属性已注册后台任务。 此步骤非常重要；如果应用不检查现有后台任务注册，则它可能会轻松多次注册该任务，这会导致性能问题和工作结束前超出任务的最大可用 CPU 时间。

下面的示例**AllTasks**属性上进行迭代，并设置为 true，如果已注册该任务的标志变量。

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

2.  如果后台任务尚未注册，则使用 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 创建你的后台任务的一个实例。 任务入口点应为命名空间为前缀的后台任务的名称。

后台任务触发器控制后台任务何时运行。 有关可能的触发器的列表，请参阅 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)。

例如，此代码创建一个新后台任务，并将其设置为**TimeZoneChanged**触发器发生时运行：

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

3.  （可选）在触发器事件发生后，你可以添加条件控制任务何时运行。 例如，如果你不希望在用户存在前运行任务，请使用条件 **UserPresent**。 有关可能条件的列表，请参阅 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

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

4.  通过在 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 对象上调用 Register 方法来注册后台任务。 存储 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) 结果，以便可以在下一步中使用该结果。

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
> 通用 Windows 应用必须在注册任何后台触发器类型之前调用 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)。

若要确保通用 Windows 应用在你发布更新后继续正常运行，请使用 **ServicingComplete**（请参阅 [SystemTriggerType](https://msdn.microsoft.com/library/windows/apps/br224839)）触发器执行任何更新后配置更改（如迁移应用的数据库和注册后台任务）。 最佳做法是注销与以前版本的应用关联的后台任务（请参阅 [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471)），同时为新版本的应用注册后台任务（请参阅 [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485)）。

有关详细信息，请参阅[后台任务指南](guidelines-for-background-tasks.md)。

## <a name="handle-background-task-completion-using-event-handlers"></a>使用事件处理程序处理后台任务完成

你应该使用 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 注册一个方法，以便应用可以从后台任务中获取结果。 当应用启动或恢复时，如果后台任务已完成自上次在前台运行时应用将调用标记的方法。 （如果应用当前位于前台时后台任务完成，将立即调用 OnCompleted 方法。）

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
> UI 更新应该异步执行，为的是避免占用 UI 线程。 有关示例，请参阅[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)中的 UpdateUI 方法。

2.  回到已注册后台任务的位置。 在该代码行之后，添加一个新的 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 对象。 提供 OnCompleted 方法作为 **BackgroundTaskCompletedEventHandler** 构造函数的参数。

以下示例代码将一个 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 添加到 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)：

```csharp
task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
```

```cppwinrt
task.Completed({ this, &MainPage::OnCompleted });
```

```cpp
task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &MainPage::OnCompleted);
```

## <a name="declare-in-the-app-manifest-that-your-app-uses-background-tasks"></a>在应用清单中声明你的应用使用后台任务

必须先在应用清单中声明各个后台任务，你的应用才能运行后台任务。 如果你的应用尝试与未在清单中列出的触发器注册后台任务，注册后台任务将失败"未注册的运行时类"错误。

1.  通过打开名为 Package.appxmanifest 的文件打开程序包清单设计器。
2.  打开**声明**选项卡。
3.  在**可用声明**下拉菜单中，选择**后台任务**，然后单击**添加**。
4.  选中**系统事件**复选框。
5.  在**入口点：** 文本框中，输入的命名空间和此示例中为 Tasks.ExampleBackgroundTask 后台类的名称。
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
> 下载[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)以查看使用后台任务的完整且可靠的 UWP 应用上下文中的类似代码示例。

有关 API 引用、后台任务概念指南以及编写使用后台任务的应用的更多详细说明，请参阅以下相关主题。

## <a name="related-topics"></a>相关主题

**详细说明后台任务主题**

* [使用后台任务响应系统事件](respond-to-system-events-with-background-tasks.md)
* [注册后台任务](register-a-background-task.md)
* [设置后台任务的运行条件](set-conditions-for-running-a-background-task.md)
* [使用维护触发器](use-a-maintenance-trigger.md)
* [处理取消的后台任务](handle-a-cancelled-background-task.md)
* [监视后台任务进度和完成](monitor-background-task-progress-and-completion.md)
* [在计时器上运行后台任务](run-a-background-task-on-a-timer-.md)
* [创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)。
* [将进程外后台任务转换为进程内后台任务](convert-out-of-process-background-task.md)  

**后台任务指南**

* [后台任务指南](guidelines-for-background-tasks.md)
* [调试后台任务](debug-a-background-task.md)
* [如何在 UWP 应用中触发暂停、恢复和后台事件（在调试时）](http://go.microsoft.com/fwlink/p/?linkid=254345)

**后台任务 API 引用**

* [**Windows.ApplicationModel.Background**](https://msdn.microsoft.com/library/windows/apps/br224847)
