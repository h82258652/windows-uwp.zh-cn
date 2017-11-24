---
author: PatrickFarley
ms.assetid: 67a46812-881c-404b-9f3b-c6786f39e72b
title: "自定义打印工作流"
description: "创建自定义打印工作流体验以满足组织的需求。"
ms.author: pafarley
ms.date: 08/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
localizationpriority: medium
ms.openlocfilehash: d4fc2ea5186acff1b8b2e63ae19884ee1bc3d02d
ms.sourcegitcommit: 44a24b580feea0f188c7eae36e72e4a4f412802b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/31/2017
---
# <a name="customize-the-print-workflow"></a>自定义打印工作流

## <a name="overview"></a>概述
开发人员可以使用打印工作流应用自定义打印工作流。 打印工作流应用是对 [Microsoft Store 设备应用 (WSDA)](https://docs.microsoft.com/windows-hardware/drivers/devapps/) 功能进行扩展的 UWP 应用，因此在继续之前，熟悉一下 WSDA 将很有帮助。 

在使用 WSDA 的情况下，当源应用程序的用户选择打印某些内容，并通过打印对话框进行浏览时，系统会检查工作流应用是否与该打印机关联。 如果关联，则打印工作流应用将启动（主要以后台任务形式运行；下方提供了相关详细信息）。 工作流应用能够改变打印票证（配置当前打印任务的打印机设备设置的 XML 文档）和要打印的实际 XPS 内容。 它可以选择通过在过程进行到一半时启动 UI 向用户显示此功能。 完成后其工作之后，它将打印内容和打印票证传递给驱动程序。

因为它涉及了后台和前台组件，并且它在功能上要与其他应用配合使用，打印工作流应用比起其他类型的 UWP 应用实现起来可能更复杂。 建议在阅读本指南时查看[工作流应用示例](https://github.com/Microsoft/print-oem-samples)，更好地了解如何实现不同功能。 为简单起见，本指南中不介绍某些功能，如各种错误检查和 UI 管理。

## <a name="getting-started"></a>入门

工作流应用必须指明其到打印系统的入口点，以便在适当的时间启动它。 这是通过在 UWP 项目的 *package.appxmanifest* 文件的
 `Application/Extensions` 元素中插入以下声明来实现的。 

```xml
<uap:Extension Category="windows.printWorkflowBackgroundTask"  
    EntryPoint="WFBackgroundTasks.WfBackgroundTask" />
```

> [!IMPORTANT] 
> 在很多情况下，打印自定义不需要用户输入。 因此，打印工作流应用默认以后台任务形式运行。

如果工作流应用与启动打印作业的源应用程序关联（请参阅后面有关内容了解具体说明），打印系统会在其清单文件中查找后台任务入口点。

## <a name="do-background-work-on-the-print-ticket"></a>执行打印票证上的后台工作

打印系统对工作流应用首先要做的是激活其后台任务（在本例中为 `WFBackgroundTasks` 命名空间中的 `WfBackgroundTask` 类）。 在后台任务的 `Run` 方法中，应该将任务触发器的详细信息转换为 **[PrintWorkflowTriggerDetails](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails)** 实例。 这将为打印工作流后台任务提供特殊的功能。 它显示 **[PrintWorkflowSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtriggerdetails#Windows_Graphics_Printing_Workflow_PrintWorkflowTriggerDetails_PrintWorkflowSession)** 属性，这个属性是 **[PrintWorkFlowBackgroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession)** 的一个实例。 打印工作流会话类（后台和前台变体）将控制打印工作流应用中的后续步骤。 

然后为此会话类将触发的两个事件注册处理程序方法。 这些方法将在以后定义。

```csharp
public void Run(IBackgroundTaskInstance taskInstance) {
    // Take out a deferral here and complete once all the callbacks are done
    runDeferral = taskInstance.GetDeferral();

    // Associate a cancellation handler with the background task.
    taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

    // cast the task's trigger details as PrintWorkflowTriggerDetails
    PrintWorkflowTriggerDetails workflowTriggerDetails = taskInstance.TriggerDetails as PrintWorkflowTriggerDetails;

    // Get the session manager, which is unique to this print job
    PrintWorkflowBackgroundSession sessionManager = workflowTriggerDetails.PrintWorkflowSession;

    // add the event handler callback routines
    sessionManager.SetupRequested += OnSetupRequested;
    sessionManager.Submitted += OnXpsOMPrintSubmitted;

    // Allow the event source to start
    // This call blocks until all of the workflow callbacks complete
    sessionManager.Start();
}
```

调用 `Start` 方法时，会话管理器首先引发 **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession#Windows_Graphics_Printing_Workflow_PrintWorkflowBackgroundSession_SetupRequested)** 事件。 此事件公开有关打印任务的一般信息，以及打印票证。 在这个阶段，打印票证可以在后台进行编辑。 

```csharp
private void OnSetupRequested(PrintWorkflowBackgroundSession sessionManager, PrintWorkflowBackgroundSetupRequestedEventArgs printTaskSetupArgs) {
    // Take out a deferral here and complete once all the callbacks are done
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get general information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;

    // edit the print ticket
    WorkflowPrintTicket printTicket = printTaskSetupArgs.GetUserPrintTicketAsync();

    // ...
```

重要的是，它是在处理 **SetupRequested**，该应用将为其确定是否启动前台组件。 这可能取决于之前保存到本地存储的设置或者在编辑打印票证期间发生的事件，或者它也可能是特定应用的静态设置。

```csharp
    // ...
    
    if (UIrequested) {
        printTaskSetupArgs.SetRequiresUI();

        // Any data that is to be passed to the foreground task must be stored the app's local storage.
        // It should be prefixed with the sourceApplicationName string and the SessionId string, so that
        // it can be identified as pertaining to this workflow app session.
    }

    // Complete the deferral taken out at the start of OnSetupRequested
    setupRequestedDeferral.Complete();
}
```

## <a name="do-foreground-work-on-the-print-job-optional"></a>执行打印作业的前台工作（可选）

如果调用了 **[SetRequiresUI](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsetuprequestedeventargs#Windows_Graphics_Printing_Workflow_PrintWorkflowBackgroundSetupRequestedEventArgs_SetRequiresUI)** 方法，则打印系统将在清单文件中查找前台应用程序的入口点。 *package.appxmanifest* 文件的 `Application/Extensions` 元素必须包含以下几行。 将 `EntryPoint` 的值替换为前台应用的名称。

```xml
<uap:Extension Category="windows.printWorkflowForegroundTask"  
    EntryPoint="MyWorkFlowForegroundApp.App" /> 
```

接下来，打印系统将对给定应用入口点调用 **OnActivated** 方法。 在其 _App.xaml.cs_ 文件的 **OnActivated** 方法中，工作流应用应该检查此激活操作的类型，验证这是否为工作流激活。 如果是，则工作流应用可以将激活参数作为属性转换为 **[PrintWorkflowUIActivatedEventArgs](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowuiactivatedeventargs)** 对象，它将 **[PrintWorkflowForegroundSession](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession)** 对象作为属性公开。 此对象与在之前的部分中的后台对象一样，包含由打印系统触发的事件，你可以对这些事件分配处理程序。 在本例中，事件处理功能在另一个名为 `WorkflowPage` 的类中实现。

首先，在 _App.xaml.cs_ 文件中：

```csharp
protected override void OnActivated(IActivatedEventArgs args){

    if (args.Kind == ActivationKind.PrintWorkflowForegroundTask) {

        // the app should instantiate a new UI view so that it can properly handle the case when 
        // several print jobs are active at the same time.
        Frame rootFrame = new Frame();
        if (null == Window.Current.Content)
        {
            rootFrame.Navigate(typeof(WorkflowPage));
            Window.Current.Content = rootFrame;
        }

        // Get the main page
        WorkflowPage workflowPage = (WorkflowPage)rootFrame.Content;

        // Make sure the page knows it's handling a foreground task activation
        workflowPage.LaunchType = WorkflowPage.WorkflowPageLaunchType.ForegroundTask;

        // Get the activation arguments
        PrintWorkflowUIActivatedEventArgs printTaskUIEventArgs = args as PrintWorkflowUIActivatedEventArgs;

        // Get the session manager
        PrintWorkflowForegroundSession taskSessionManager = printTaskUIEventArgs.PrintWorkflowSession;

        // Add the callback handlers - these methods are in the workflowPage class
        taskSessionManager.SetupRequested += workflowPage.OnSetupRequested;
        taskSessionManager.XpsDataAvailable += workflowPage.OnXpsDataAvailable;

        // start raising the print workflow events
        taskSessionManager.Start();
    }
}
```

在 UI 已连接事件处理程序，并且 **OnActivated** 方法已退出后，打印系统将引发 **[SetupRequested](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession#Windows_Graphics_Printing_Workflow_PrintWorkflowForegroundSession_SetupRequested)** 事件以便 UI 处理。 此事件提供的数据与后台任务设置事件提供的数据相同，包括打印作业信息和打印票证文档，但不能请求启动其他 UI。 在 _WorkflowPage.xaml.cs_ 文件中：

```csharp
internal void OnSetupRequested(PrintWorkflowForegroundSession sessionManager, PrintWorkflowForegroundSetupRequestedEventArgs printTaskSetupArgs) {
    // If anything asynchronous is going to be done, you need to take out a deferral here,
    // since otherwise the next callback happens once this one exits, which may be premature
    Deferral setupRequestedDeferral = printTaskSetupArgs.GetDeferral();

    // Get information about the source application, print job title, and session ID
    string sourceApplicationName = printTaskSetupArgs.Configuration.SourceAppDisplayName;
    string jobTitle = printTaskSetupArgs.Configuration.JobTitle;
    string sessionId = printTaskSetupArgs.Configuration.SessionId;
    // the following string should be used when storing data that pertains to this workflow session
    // (such as user input data that is meant to change the print content later on)
    string localStorageVariablePrefix = string.Format("{0}::{1}::", sourceApplicationName, sessionID);
    
    try
    {
        // receive and store user input
        // ...
    }
    catch (Exception ex)
    {
        string errorMessage = ex.Message;
        Debug.WriteLine(errorMessage);
    }
    finally
    {
        // Complete the deferral taken out at the start of OnSetupRequested
        setupRequestedDeferral.Complete();
    }
}
```

接下来，打印系统将为 UI 引发 **[XpsDataAvailable](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowforegroundsession#Windows_Graphics_Printing_Workflow_PrintWorkflowForegroundSession_XpsDataAvailable)** 事件。 在此事件的处理程序中，工作流应用可以访问提供给设置事件的所有数据，并且还可以采取原始字节流或对象模型的形式直接读取 XPS 数据。 对 XPS 数据的访问权限让 UI 能够提供打印预览服务，并为用户提供有关工作流应用可对数据执行的操作的信息。 

作为此事件处理程序的一部分，如果工作流应用将继续与用户交付，则必须获取延迟对象。 如果没有延迟，当 **XpsDataAvailable** 事件处理程序退出或它调用异步方法时，打印系统将认为 UI 任务已完成。 当应用已从用户与 UI 的交互中收集了所需的所有信息时，它应该会结束延迟以便打印系统继续。

```csharp
internal async void OnXpsDataAvailable(PrintWorkflowForegroundSession sessionManager, PrintWorkflowXpsDataAvailableEventArgs printTaskXpsAvailableEventArgs)
{
    // Take out a deferral
    Deferral xpsDataAvailableDeferral = printTaskXpsAvailableEventArgs.GetDeferral();

    SpoolStreamContent xpsStream = printTaskXpsAvailableEventArgs.Operation.XpsContent.GetSourceSpoolDataAsStreamContent(); 
 
    IInputStream inputStream = xpsStream.GetInputSpoolStream(); 
 
    using (var inputReader = new Windows.Storage.Streams.DataReader(inputStream)) 
    { 
        // Read the XPS data from input stream 
        byte[] xpsData = new byte[inputReader.UnconsumedBufferLength]; 
        while (inputReader.UnconsumedBufferLength > 0) 
        { 
            inputReader.ReadBytes(xpsData); 
            // Do something with the XPS data, e.g. preview 
            // ...                  
        } 
    } 

    // Complete the deferral taken out at the start of this method
    xpsDataAvailableDeferral.Complete();
}
```

此外，事件参数公开的 **[PrintWorkflowSubmittedOperation](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation)** 实例提供了选项来取消打印作业，或指示打印作业成功完成但是无需输出打印作业。 这是通过使用 **[PrintWorkflowSubmittedStatus](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedstatus)** 值调用 **[Complete](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowsubmittedoperation#Windows_Graphics_Printing_Workflow_PrintWorkflowSubmittedOperation_Complete_Windows_Graphics_Printing_Workflow_PrintWorkflowSubmittedStatus_)** 方法完成的。

> [!NOTE]
> 如果工作流应用取消打印作业，强烈建议它提供指示取消打印作业原因的 toast 通知。 


## <a name="do-final-background-work-on-the-print-content"></a>对打印内容执行最终后台工作

UI 在 **PrintTaskXpsDataAvailable** 事件中完成延迟（或绕过 UI 步骤）之后，打印系统将为后台任务引发 **[Submitted](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowbackgroundsession#Windows_Graphics_Printing_Workflow_PrintWorkflowBackgroundSession_Submitted)** 事件。 在对此事件的处理程序中，工作流应用可以访问 **XpsDataAvailable** 事件提供的所有相同数据。 但是，与之前的所有事件都不同的是，**Submitted** 还通过 **[PrintWorkflowTarget](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow.printworkflowtarget)** 实例提供对最终打印作业的*写入*访问权限。 

用于为进行最终打印而在后台打印数据的对象取决于是以原始字节流还是以 XPS 对象模型的形式访问源数据。 当工作流应用通过字节流访问源数据时，将提供用来写入最终作业数据的输出字节流。 当工作流应用通过对象模型访问源数据时，将提供用来将对象写入输出作业的文档写入程序。 无论是哪一种情况，工作流应用都应该读取所有源数据、根据需要修改任何数据，并将修改后的数据写入输出目标。

当后台任务完成写入数据时，它应该对相应的 **PrintWorkflowSubmittedOperation** 对象调用 **Complete**。 工作流应用完成此步骤后，**Submitted** 事件处理程序退出，工作流会话关闭，用户可以通过标准打印对话框监视最终打印作业的状态。

## <a name="final-steps"></a>最后的步骤

### <a name="register-the-print-workflow-app-to-the-printer"></a>向打印机注册打印工作流应用

工作流应用使用与 WSDA 相同类型的元数据文件提交与打印机关联。 实际上，一个 UWP 应用程序既可以充当工作流应用，又可以充当提供打印任务设置功能的 WSDA。 按照相应[创建元数据关联的 WSDA 步骤](https://docs.microsoft.com/windows-hardware/drivers/devapps/step-2--create-device-metadata)操作。

区别是 WSDA 会自动为用户激活（该应用将始终在用户通过关联设备打印时启动），而工作流应用不会。 后者有一个不同的策略，必须对其进行设置。

### <a name="set-the-workflow-apps-policy"></a>设置工作流应用的策略
工作流应用策略通过在要运行工作流应用的设备上的 Powershell 命令设置。 将修改 Set-Printer、Add-Printer（现有端口）和 Add-Printer（新 WSD 端口）命令以允许设置工作流策略。 
* `Off`：不会激活工作流应用。
* `Optional`：如果工作流 DCA 已安装在系统中，则将激活工作流应用。 如果未安装该应用，仍将继续进行打印。 
* `On`：如果工作流 DCA 已安装在系统中，则将激活工作流合同。 如果未安装该应用，打印将失败。 

以下命令可使工作流应用在指定打印机上成为必需。
```Powershell
Set-Printer –Name "Microsoft XPS Document Writer" -WorkflowPolicy On
```

本地用户可以在本地打印机上运行此策略，或者，对于企业实施，打印机管理员可以在打印服务器上运行此策略。 然后，该策略将同步到所有客户端连接。 只要增加新打印机，打印机管理员就可以使用此策略。

## <a name="see-also"></a>另请参阅

[工作流应用示例](https://github.com/Microsoft/print-oem-samples)

[Windows.Graphics.Printing.Workflow 命名空间](https://docs.microsoft.com/uwp/api/windows.graphics.printing.workflow)


