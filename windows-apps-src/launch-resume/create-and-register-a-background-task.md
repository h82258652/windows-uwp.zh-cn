---
author: TylerMSFT
title: "创建和注册后台任务"
description: "创建一个后台任务类并注册它，以便在应用不在前台运行时运行。"
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: dd107f55e6dbeda6f48de27b3a84006954a46338

---

# 创建和注册后台任务


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
-   [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781)

创建一个后台任务类并注册它，以便在应用不在前台运行时运行。

## 创建后台任务类


你可以通过编写用于实现 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 接口的类来在后台运行代码。 在使用诸如 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839) 或 [**MaintenanceTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700517) 等触发器触发特定事件时，将运行该代码。

以下示例向你展示如何编写用于实现 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 接口的新类。 在开始之前，请在你的解决方案中创建用于后台任务的新项目。 添加用于后台任务的空白新类，并导入 [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847) 命名空间。

1.  为后台任务创建新项目并将其添加到你的解决方案。 若要执行此操作，请在“解决方案资源管理器”****中右键单击你的解决方案节点并依次选择“添加”-&gt;“新建项目”。 然后，选择“Windows 运行时组件(通用 Windows)”****项目类型、为该项目命名，并单击“确定”。
2.  从通用 Windows 平台 (UWP) 应用项目中引用后台任务项目。

    对于 C++ 应用，请右键单击你的应用项目并选择“属性”****。 然后，转到“通用属性”****并单击“添加新引用”****、选中后台任务项目旁的框，并单击这两个对话框上的“确定”****。

    对于 C# 应用，在你的应用项目中，右键单击“引用”****并选择“添加新引用”****。 在“解决方案”****下，选择“项目”****，然后选择你的后台任务项目名称并单击“确定”****。

3.  创建一个用于实现 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 接口的新类。 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 方法是一个需要的入口点，当触发指定事件时，将调用该入口点；每个后台任务都需要该方法。

    > **注意** 后台任务类本身和后台任务项目中的所有其他类都需要是处于 **sealed** 状态的 **public** 类。

    下面的示例代码显示用于后台任务类的非常基本的起点：

> [!div class="tabbedCodeSnippets"]
> ```cs
>     //
>     // ExampleBackgroundTask.cs
>     //
>
>     using Windows.ApplicationModel.Background;
>
>     namespace Tasks
>     {
>         public sealed class ExampleBackgroundTask : IBackgroundTask
>         {
>             public void Run(IBackgroundTaskInstance taskInstance)
>             {
>                 
>             }        
>         }
>     }
> ```
> ```cpp
>     //
>     // ExampleBackgroundTask.cpp
>     //
>
>     #include "ExampleBackgroundTask.h"
>
>     using namespace Tasks;
>
>     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
>     {
>
>     }
>  ```


> ```cpp
>     //
>     // ExampleBackgroundTask.h
>     //
>
>     #pragma once
>
>     using namespace Windows::ApplicationModel::Background;
>
>     namespace RuntimeComponent1
>     {
>         public ref class ExampleBackgroundTask sealed : public IBackgroundTask
>         {
>
>         public:
>             ExampleBackgroundTask();
>
>             virtual void Run(IBackgroundTaskInstance^ taskInstance);
>             void OnCompleted(
>                     BackgroundTaskRegistration^ task,
>                     BackgroundTaskCompletedEventArgs^ args
>                     );
>         };
>     }
> ```

4.  [!div class="tabbedCodeSnippets"] 如果你在后台任务中运行任何异步代码，则你的后台任务需要使用延迟。

    如果不使用延迟，则后台任务进程可能会意外终止（如果 Run 方法在完成异步方法调用之前完成）。 调用异步方法之前，在 Run 方法中请求延迟。 将延迟保存到某个全局变量以便可以从异步方法访问。

    完成异步代码之后声明延迟完成。

> [!div class="tabbedCodeSnippets"]
> ```cs
>     BackgroundTaskDeferral _deferral = taskInstance.GetDeferral(); // Note: define at class scope
>     public async void Run(IBackgroundTaskInstance taskInstance)
>     {
>         //
>         // TODO: Insert code to start one or more asynchronous methods using the
>         //       await keyword, for example:
>         //
>         // await ExampleMethodAsync();
>         //
>         
>         _deferral.Complete();
>     }
> ```
> ```cpp
>     BackgroundTaskDeferral^ deferral = taskInstance->GetDeferral(); // Note: define at class scope
>     void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
>     {
>         //
>         // TODO: Modify the following line of code to call a real async function.
>         //       Note that the task<void> return type applies only to async
>         //       actions. If you need to call an async operation instead, replace
>         //       task<void> with the correct return type.
>         //
>         task<void> myTask(ExampleFunctionAsync());
>         
>         myTask.then([=] () {
>             deferral->Complete();
>         });
>     }
> ```

    **Note**  In C#, your background task's asynchronous methods can be called using the **async/await** keywords. In C++, a similar result can be achieved by using a task chain.

    For more information on asynchronous patterns, see [Asynchronous programming](https://msdn.microsoft.com/library/windows/apps/mt187335). For additional examples of how to use deferrals to keep a background task from stopping early, see the [background task sample](http://go.microsoft.com/fwlink/p/?LinkId=618666).

以下示例代码获取和保存延迟，并在异步代码完成后将其释放：

> [!div class="tabbedCodeSnippets"] 以下步骤在你的一个应用类（例如 MainPage.xaml.cs）中完成。

 
****注意** 你还可以创建专用于注册后台任务的函数，请参阅[注册后台任务](register-a-background-task.md)。**

1.  在这种情况下，不使用接下来的 3 个步骤，你可以简单构造触发器并将其随任务名称、任务入口点以及（可选）条件一起提供给注册函数。 注册要运行的后台任务

    通过在 [**BackgroundTaskRegistration.AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 属性中迭代，查明后台任务是否已注册。

> [!div class="tabbedCodeSnippets"]
> ```cs
>     var taskRegistered = false;
>     var exampleTaskName = "ExampleBackgroundTask";
>
>     foreach (var task in BackgroundTaskRegistration.AllTasks)
>     {
>         if (task.Value.Name == exampleTaskName)
>         {
>             taskRegistered = true;
>             break;
>         }
>     }
> ```
> ```cpp
>     boolean taskRegistered = false;
>     Platform::String^ exampleTaskName = "ExampleBackgroundTask";
>
>     auto iter = BackgroundTaskRegistration::AllTasks->First();
>     auto hascur = iter->HasCurrent;
>
>     while (hascur)
>     {
>         auto cur = iter->Current->Value;
>
>         if(cur->Name == exampleTaskName)
>         {
>             taskRegistered = true;
>             break;
>         }
>
>         hascur = iter->MoveNext();
>     }
> ```

2.  此步骤非常重要；如果应用不检查现有后台任务注册，则它可能会轻松多次注册该任务，这会导致性能问题和工作结束前超出任务的最大可用 CPU 时间。 下例将在 AllTasks 属性上进行迭代，并且如果任务已经注册，则将标志参数设置为 true：

    [!div class="tabbedCodeSnippets"] 如果后台任务尚未注册，则使用 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 创建你的后台任务的一个实例。

    任务入口点应为命名空间为前缀的后台任务的名称。

> [!div class="tabbedCodeSnippets"]
> ```cs
>     var builder = new BackgroundTaskBuilder();
>
>     builder.Name = exampleTaskName;
>     builder.TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
>     builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
> ```
> ```cpp
>     auto builder = ref new BackgroundTaskBuilder();
>
>     builder->Name = exampleTaskName;
>     builder->TaskEntryPoint = "RuntimeComponent1.ExampleBackgroundTask";
>     builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
> ```

3.  后台任务触发器控制后台任务何时运行。 有关可能的触发器的列表，请参阅 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224839)。 例如，此代码创建一个新后台任务并将其设置为在 **TimeZoneChanged** 触发器引发时运行：

    [!div class="tabbedCodeSnippets"]

> [!div class="tabbedCodeSnippets"]
> ```cs
>     builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
> ```
> ```cpp
>     builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
> ```

4.  （可选）在触发器事件发生后，你可以添加条件控制任务何时运行。 例如，如果你不希望在用户存在前运行任务，请使用条件 **UserPresent**。

    有关可能条件的列表，请参阅 [**SystemConditionType**](https://msdn.microsoft.com/library/windows/apps/br224835)。

> [!div class="tabbedCodeSnippets"]
>     ```cs
>     BackgroundTaskRegistration task = builder.Register();
>     ```
>     ```cpp
>     BackgroundTaskRegistration^ task = builder->Register();
>     ```

    > **Note**  Universal Windows apps must call [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) before registering any of the background trigger types.

    To ensure that your Universal Windows app continues to run properly after you release an update, you must call [**RemoveAccess**](https://msdn.microsoft.com/library/windows/apps/hh700471) and then call [**RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/hh700485) when your app launches after being updated. For more information, see [Guidelines for background tasks](guidelines-for-background-tasks.md).

## 以下示例代码分配需要用户存在的条件：


[!div class="tabbedCodeSnippets"] 通过在 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) 对象上调用 Register 方法来注册后台任务。 存储 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786) 结果，以便可以在下一步中使用该结果。

1.  以下代码注册后台任务并存储结果： [!div class="tabbedCodeSnippets"]     ```cs
    BackgroundTaskRegistration task = builder.Register();
    ```
    ```cpp
    BackgroundTaskRegistration^ task = builder->Register();
    ``` 使用事件处理程序处理后台任务完成

    你应该使用 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 注册一个方法，以便应用可以从后台任务中获取结果。

> [!div class="tabbedCodeSnippets"]
>     ```cs
>     private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
>     {
>         var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
>         var key = task.TaskId.ToString();
>         var message = settings.Values[key].ToString();
>         UpdateUI(message);
>     }
>     ```
>     ```cpp
>     void ExampleBackgroundTask::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
>     {
>         auto settings = ApplicationData::Current->LocalSettings->Values;
>         auto key = task->TaskId.ToString();
>         auto message = dynamic_cast<String^>(settings->Lookup(key));
>         UpdateUI(message);
>     }
>     ```

    > **Note**  UI updates should be performed asynchronously, to avoid holding up the UI thread. For an example, see the UpdateUI method in the [background task sample](http://go.microsoft.com/fwlink/p/?LinkId=618666).

     

2.  当启动或恢复应用时，如果自从上次在应用前台运行后后台任务就已完成，将调用标记方法。 （如果应用当前位于前台时后台任务完成，将立即调用 OnCompleted 方法。） 编写一个 OnCompleted 方法，以处理后台任务的完成。

    例如，后台任务结果可能导致 UI 更新。

> [!div class="tabbedCodeSnippets"]
>     ```cs
>     task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
>     ```
>     ```cpp
>     task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &ExampleBackgroundTask::OnCompleted);
>     ```

## 此处所示的方法足迹对于 OnCompleted 事件处理程序方法来说是必需的，即使该示例不使用 *args* 参数也是如此。


以下示例代码识别后台任务完成并调用可获取消息字符串的一个示例 UI 更新方法。 [!div class="tabbedCodeSnippets"]     ```cs
    private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
    {
        var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
        var key = task.TaskId.ToString();
        var message = settings.Values[key].ToString();
        UpdateUI(message);
    }
    ```
    ```cpp
    void ExampleBackgroundTask::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
    {
        auto settings = ApplicationData::Current->LocalSettings->Values;
        auto key = task->TaskId.ToString();
        auto message = dynamic_cast<String^>(settings->Lookup(key));
        UpdateUI(message);
    }
    ```

1.  回退到已注册后台任务的位置。
2.  在该代码行之后，添加一个新的 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 对象。
3.  提供 OnCompleted 方法作为 **BackgroundTaskCompletedEventHandler** 构造函数的参数。
4.  以下示例代码将一个 [**BackgroundTaskCompletedEventHandler**](https://msdn.microsoft.com/library/windows/apps/br224781) 添加到 [**BackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/br224786)：
5.  [!div class="tabbedCodeSnippets"]     ```cs
    task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
    ```
    ```cpp
    task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &ExampleBackgroundTask::OnCompleted);
    ```
6.  在应用清单中声明你的应用使用后台任务

    必须先在应用清单中声明各个后台任务，你的应用才能运行后台任务。

    ```xml
    <Extensions>
      <Extension Category="windows.backgroundTasks" EntryPoint="RuntimeComponent1.ExampleBackgroundTask">
        <BackgroundTasks>
          <Task Type="systemEvent" />
        </BackgroundTasks>
      </Extension>
    </Extensions>
    ```

## 如果你的应用尝试注册一个后台任务，其中有一个触发器未在清单中列出，那么注册将会失败。


通过打开名为 Package.appxmanifest 的文件打开程序包清单设计器。 打开“声明”****选项卡。

> 在“可用声明”****下拉菜单中，选择“后台任务”****，然后单击“添加”****。

 

选中“系统事件”****复选框。

> 在“入口点:”****文本框中，输入后台类的命名空间和名称，在此示例中为 RuntimeComponent1.ExampleBackgroundTask。 关闭清单设计器。

 

## 以下 Extensions 元素将添加到 Package.appxmanifest 文件以注册后台任务：


**摘要和后续步骤**

* [现在，你应该已基本了解如何编写后台任务类、如何从应用中注册后台任务，以及如何让应用识别后台任务何时完成。](respond-to-system-events-with-background-tasks.md)
* [你还应该了解如何更新应用程序清单，以便你的应用可以成功注册后台任务。](register-a-background-task.md)
* [**注意** 下载[后台任务示例](http://go.microsoft.com/fwlink/p/?LinkId=618666)以查看使用后台任务的完整且可靠的 UWP 应用上下文中的类似代码示例。](set-conditions-for-running-a-background-task.md)
* [有关 API 引用、后台任务概念指南以及编写使用后台任务的应用的更多详细说明，请参阅以下相关主题。](use-a-maintenance-trigger.md)
* [**注意** 本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。](handle-a-cancelled-background-task.md)
* [如果你面向 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。](monitor-background-task-progress-and-completion.md)
* [相关主题](run-a-background-task-on-a-timer-.md)

**详细说明后台任务主题**

* [使用后台任务响应系统事件](guidelines-for-background-tasks.md)
* [注册后台任务](debug-a-background-task.md)
* [设置后台任务的运行条件](http://go.microsoft.com/fwlink/p/?linkid=254345)

**使用维护触发器**

* [**处理取消的后台任务**](https://msdn.microsoft.com/library/windows/apps/br224847)

 

 



<!--HONumber=Jun16_HO5-->


