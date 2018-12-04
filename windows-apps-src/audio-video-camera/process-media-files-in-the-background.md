---
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: 本文向你显示了如何使用 MediaProcessingTrigger 和后台任务在后台处理媒体文件。
title: 在后台处理媒体文件
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0194ccba43e2ba5270b9ff8eacf045ca140af6cb
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8473137"
---
# <a name="process-media-files-in-the-background"></a>在后台处理媒体文件



本文向你显示了如何使用 [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) 和后台任务在后台处理媒体文件。

本文中介绍的示例应用允许用户选择要转换代码的输入媒体文件，并指定用于转换结果代码的输出文件。 然后，启动后台任务以执行转换代码操作。 
            [
              **MediaProcessingTrigger**
            ](https://msdn.microsoft.com/library/windows/apps/dn806005) 旨在支持转换代码之外的许多不同媒体处理方案，包括将媒体组合呈现到磁盘，以及在完成处理后上载已处理的媒体文件。

有关此示例中利用的不同通用 Windows 应用功能的更多详细信息，请参阅：

-   [转换媒体文件代码](transcode-media-files.md)
-   [启动和恢复后台任务](https://msdn.microsoft.com/library/windows/apps/mt227652)
-   [磁贴、锁屏提醒和通知](https://msdn.microsoft.com/library/windows/apps/mt185606)

## <a name="create-a-media-processing-background-task"></a>创建用于处理后台任务的媒体

若要在 Microsoft Visual Studio 中将后台任务添加到现有解决方案，请输入你的组件的名称

1.  在**文件**菜单上，依次选择**添加**和**新建项目...**。
2.  选择 **Windows 运行时组件(通用 Windows)** 项目类型。
3.  输入新的组件项目的名称。 此示例使用 **MediaProcessingBackgroundTask** 项目名称。
4.  单击“确定”。

在**解决方案资源管理器**，右键单击默认情况下创建的“Class1.cs”文件的图标，然后选择**重命名**。 将该文件重命名为“MediaProcessingTask.cs”。 当 Visual Studio 询问是否想要重命名对此类的所有引用时，请单击**是**。

在重命名的类文件中，添加以下 **using** 指令以在你的项目中包含这些命名空间。
                                  
[!code-cs[BackgroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundUsing)]

更新类声明以使你的类继承自 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)。

[!code-cs[BackgroundClass](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundClass)]

将下列成员变量添加到你的类：

-   
            [
              **IBackgroundTaskInstance**
            ](https://msdn.microsoft.com/library/windows/apps/br224797)，将用于通过后台任务的进度更新前台应用。
-   
            [
              **BackgroundTaskDeferral**
            ](https://msdn.microsoft.com/library/windows/apps/hh700499)，用于在以异步方式执行媒体转换代码时，防止系统关闭你的后台任务。
-   
            **CancellationTokenSource** 对象，可用于取消异步转换代码操作。
-   
            [
              **MediaTranscoder**
            ](https://msdn.microsoft.com/library/windows/apps/br207080) 对象，将用于转换媒体文件代码。

[!code-cs[BackgroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundMembers)]

启动任务时，系统将调用后台任务的 [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) 方法。 将传入该方法的 [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) 对象设置为相应的成员变量。 注册 [**Canceled**](https://msdn.microsoft.com/library/windows/apps/br224798) 事件的处理程序，在系统需要关闭后台任务时将引发该事件。 然后，将 [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224800) 属性设置为零。

接下来，调用后台任务对象的 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/hh700507) 方法来获取延迟。 这将告知系统不要关闭你的任务，因为你正在执行异步操作。

接下来，调用帮助程序方法 **TranscodeFileAsync**，将在下一节中定义此方法。 如果此操作成功完成，将调用帮助程序方法以启动 Toast 通知，从而提醒用户转换代码已完成。

在 **Run** 方法的末尾，将调用延迟对象上的 [**Complete**](https://msdn.microsoft.com/library/windows/apps/hh700504)，以让系统获知你的后台任务已完成并且可以将其终止。

[!code-cs[Run](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetRun)]

在 **TranscodeFileAsync** 帮助程序方法中，将从你的应用的 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 检索用于转换代码操作的输入和输出文件的文件名。 这些值将由你的前台应用进行设置。 创建用于输入和输出文件的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象，然后创建一个用于转换代码的编码配置文件。

调用 [**PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936)，从而传入输入文件、输出文件和编码配置文件。 从此调用返回的 [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941) 对象使你获知是否可以执行转换代码。 如果 [**CanTranscode**](https://msdn.microsoft.com/library/windows/apps/hh700942) 属性为 true，将调用 [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946) 以执行转换代码操作。


            **AsTask** 方法允许你跟踪异步操作进度，或将其取消。 创建新的 **Progress** 对象，从而指定所需的进度单元以及方法的名称，可调用该方法来通知你任务的当前进度。 向 **AsTask** 方法传入 **Progress** 对象以及允许你取消任务的取消标记。

[!code-cs[TranscodeFileAsync](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetTranscodeFileAsync)]

在上一步中用于创建 Progress 对象 **Progress** 的方法中，设置后台任务实例的进度。 这会将该进度传入前台应用中（如果它正在运行）。

[!code-cs[Progress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetProgress)]


            **SendToastNotification** 帮助程序方法通过获取模板 XML 文档为只含有文本内容的 Toast 创建新的 Toast 通知。 将设置 Toast XML 的文本元素，然后从 XML 文档创建新的 [**ToastNotification**](https://msdn.microsoft.com/library/windows/apps/br208641) 对象。 最后，通过调用 [**ToastNotifier.Show**](https://msdn.microsoft.com/library/windows/apps/br208659) 向用户显示 Toast。

[!code-cs[SendToastNotification](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetSendToastNotification)]

在 [**Canceled**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Background.IBackgroundTaskInstance.Canceled) 事件（在系统取消后台任务时调用）的处理程序中，可出于遥测目的记录错误。

[!code-cs[OnCanceled](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetOnCanceled)]

## <a name="register-and-launch-the-background-task"></a>注册和启动后台任务

可以从前台应用启动后台任务之前，你必须更新前台应用的 Package.appmanifest 文件，才能让系统知道你的应用可以使用后台任务。

1.  在**解决方案资源管理器**中，双击 Package.appmanifest 文件图标以打开清单编辑器。
2.  选择**声明**选项卡。
3.  在**可用声明**中，选择**后台任务**，然后单击**添加**。
4.  在**支持的声明**下，确保已选择**后台任务**项。 在**属性**下，选中**媒体处理**的复选框。
5.  在**入口点**文本框中，为你的后台测试指定命名空间和类名称，以句点分隔。 对于此示例，该项是：
   ```csharp
   MediaProcessingBackgroundTask.MediaProcessingTask
   ```
接下来，你需要将对后台任务的引用添加到前台应用。
1.  在**解决方案资源管理器**的前台应用项目下，右键单击**引用**文件夹，然后选择**添加引用...**。
2.  展开**项目**节点，然后选择**解决方案**。
3.  选中后台任务项目旁边的框，然后单击**确定**。

应将此示例中代码的其余部分添加到前台应用。 首先，需要将以下命名空间添加到你的项目。

[!code-cs[ForegroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundUsing)]

接下来，添加注册后台任务所需的以下成员变量。

[!code-cs[ForegroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundMembers)]


            **PickFilesToTranscode** 帮助程序方法使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 和 [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871) 打开输入和输出文件以进行转换代码。 用户可能选择你的应用无权访问的位置中的文件。 若要确保你的后台任务可以打开这些文件，请将它们添加到你的应用的 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) 中。

最后，设置你的应用的 [**LocalSettings**](https://msdn.microsoft.com/library/windows/apps/br241622) 中的输入和输出文件名的项目。 后台任务从该位置检索文件名。

[!code-cs[PickFilesToTranscode](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetPickFilesToTranscode)]

若要注册后台任务，请创建新的 [**MediaProcessingTrigger**](https://msdn.microsoft.com/library/windows/apps/dn806005) 和 [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)。 设置后台任务生成器的名称，以便可以稍后标识它。 将 [**TaskEntryPoint**](https://msdn.microsoft.com/library/windows/apps/br224774) 设置为清单文件中使用的同一命名空间和类名称字符串。 将 [**Trigger**](https://msdn.microsoft.com/library/windows/apps/dn641725) 属性设置为 **MediaProcessingTrigger** 实例。

注册该任务之前，确保通过循环浏览 [**AllTasks**](https://msdn.microsoft.com/library/windows/apps/br224787) 集合，并在具有在 [**BackgroundTaskBuilder.Name**](https://msdn.microsoft.com/library/windows/apps/br224771) 属性中指定的名称的任何任务上调用 [**Unregister**](https://msdn.microsoft.com/library/windows/apps/br229870)，取消注册任何先前注册的任务。

通过调用 [**Register**](https://msdn.microsoft.com/library/windows/apps/br224772) 注册后台任务。 为 [**Completed**](https://msdn.microsoft.com/library/windows/apps/br224788) 和 [**Progress**](https://msdn.microsoft.com/library/windows/apps/br224808) 事件注册处理程序。

[!code-cs[RegisterBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetRegisterBackgroundTask)]

当应用最初启动目的，如**OnNavigatedTo**事件时，典型的应用将注册其后台任务。

通过调用 **MediaProcessingTrigger** 对象的 [**RequestAsync**](https://msdn.microsoft.com/library/windows/apps/dn765071) 方法来启动后台任务。 此方法返回的 [**MediaProcessingTriggerResult**](https://msdn.microsoft.com/library/windows/apps/dn806007) 对象让你知道后台任务是否已成功启动，如果未成功启动，则让你知道后台任务未启动的原因。 

[!code-cs[LaunchBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetLaunchBackgroundTask)]

典型的应用将启动后台任务以响应用户交互，如在 UI 控件的**单击**事件。

当后台任务更新该操作的进度时，将调用 **OnProgress** 事件处理程序。 你可以利用此机会使用进度信息更新你的 UI。

[!code-cs[OnProgress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnProgress)]

当后台任务完成运行时，将调用 **OnCompleted** 事件处理程序。 这是更新你的 UI 以向你的用户提供状态信息的另一个机会。

[!code-cs[OnCompleted](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnCompleted)]


 

 




