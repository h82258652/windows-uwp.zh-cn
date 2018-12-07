---
title: Windows10 UWP 应用生命周期
description: 本主题介绍 Windows10 通用 Windows 平台 (UWP) 应用的生命周期，从其激活时直到其关闭。
keywords: 已暂停的应用生命周期恢复启动激活
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
ms.date: 01/23/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8555f9594ac3d2e7ea1b9f7006750c1084db3d9f
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/07/2018
ms.locfileid: "8781059"
---
# <a name="windows-10-universal-windows-platform-uwp-app-lifecycle"></a>Windows10 通用 Windows 平台 (UWP) 应用生命周期


本主题介绍通用 Windows 平台 (UWP) 应用的生命周期，从其激活时直到其关闭。

## <a name="a-little-history"></a>简短历史

在 Windows8 之前，应用的生命周期非常简单。 Win32 和 .NET 应用始终处于运行状态或未运行状态。 当用户最小化应用或离开应用时，它们将继续运行。 随着便携设备和电源管理变得日益重要，这种情况不再可行。

Windows 8 随 UWP 应用引入了新应用程序模型。 在高级别上，添加了新的已暂停状态。 当用户最小化 UWP 应用或切换到其他应用后，该应用会立刻处于暂停状态。 这意味着应用的线程已停止，并且应用保留在内存中（除非操作系统需要回收资源）。 当用户切换回该应用时，该应用可快速还原到正在运行状态。

向需要继续运行的后台应用提供多种方法，如[后台任务](support-your-app-with-background-tasks.md)、[扩展执行](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.extendedexecution.aspx)和活动赞助执行（例如，**BackgroundMediaEnabled** 功能，允许应用继续[在后台播放媒体](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)）。 此外，即使你的应用暂停甚至终止，后台传输操作仍会继续。 有关详细信息，请参阅[如何下载文件](https://msdn.microsoft.com/library/windows/apps/xaml/jj152726.aspx#downloading_a_file_using_background_transfer)。

默认情况下，暂停不在前台运行的应用可以节能，同时向当前处于前台的应用提供更多资源。

已暂停状态对作为开发人员的你提出了新要求，因为操作系统可能会选择终止已暂停的应用，以便释放资源。 已终止的应用仍会显示在任务栏中。 当用户单击该应用时，该应用必须恢复它终止之前所处的状态，因为用户不会意识到系统已关闭该应用。 用户会认为在他们执行其他操作时该应用一直在后台等待，并且希望该应用处于他们离开它之前所处的同一状态。 在本主题中，我们将演示如何实现该目的。

Windows10 版本 1607 又引入了两个应用模型状态：**在前台运行**和**在后台运行**。 我们还会在接下来的部分中演示这些新状态。

## <a name="app-execution-state"></a>应用执行状态

此图显示了从 Windows 10 版本 1607 开始的可能应用模型状态。 演练 UWP 应用的典型生命周期。

![显示应用执行状态之间转换的状态图](images/updated-lifecycle.png)

当启动或激活应用时，这些应用会进入在后台运行状态。 如果应用由于前台应用启动而需要移动到前台，则该应用将获取 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground) 事件。

虽然“启动”和“激活”看起来非常相似，但它们在操作系统启动应用时所引用的方法有所不同。 首先查看如何启动应用。

## <a name="app-launch"></a>应用启动

当启动应用时，会调用 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法。 向该方法传递 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) 参数，该参数提供的内容包括：已传递给应用的实际参数、启动应用的磁贴的标识符以及应用之前所处的状态。

从 [LaunchActivatedEventArgs.PreviousExecutionState](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.launchactivatedeventargs.previousexecutionstate) 获取应用之前的状态，这将返回 [ApplicationExecutionState](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.activation.applicationexecutionstate.aspx)。 其值以及对应状态要采取的操作如下所示：

| ApplicationExecutionState | 说明 | 采取的操作 |
|-------|-------------|----------------|
| **NotRunning** | 应用可能处于此状态，因为该应用自上一次用户重新启动或登录后一直未启动。 应用在以下情况下也会处于此状态：如果应用运行后出现了故障，或者因为用户之前就将它关闭。| 初始化应用，如同第一次在当前用户会话中运行它。 |
|**Suspended** | 用户已最小化或离开应用，并且在数秒内未返回该应用。 | 当应用暂停时，其状态保留在内存中。 只需重新获取任何文件句柄或应用暂停时释放的其他资源。 |
| **Terminated** | 应用之前处于暂停状态，但之后某些时候因系统需要回收内存而被终止。 | 恢复用户离开应用时应用所处的状态。|
|**ClosedByUser** | 用户使用平板电脑模式下的关闭手势或 Alt+F4 关闭了应用。 当用户关闭应用时，它将首先暂停，然后终止。 | 从本质上说，由于应用经历了导致处于 Terminated 状态的相同步骤，因此处理此状态的步骤与 Terminated 状态相同。|
|**Running** | 当用户尝试再次启动应用时，该应用已经打开。 | 无。 请注意，不会启动应用的另一个实例。 只需激活已在运行的实例。 |

**注意***当前用户会话*基于 Windows 登录。 只要当前用户未注销、关机或者重新启动 Windows，当前用户会话便可以保留在诸如锁屏界面身份验证、切换用户等的多个事件中。 

需要注意的一种重要情形是：如果设备具有足够资源，操作系统会预启动针对该行为选择的常用应用，以优化响应性。 在后台启动要预启动的应用，然后快速暂停，以便当用户切换到这些应用时，可以恢复它们，这比启动应用的速度要快得多。

由于预启动，可能会由系统（而不是用户）启动应用的 **OnLaunched()** 方法。 由于应用在后台预启动，因此可能需要在 **OnLaunched()** 中采取不同操作。 例如，当已启动的应用开始播放音乐时，用户不会知道该应用来自何处，因为该应用已在后台预启动。 在后台预启动应用后，就会调用 **Application.Suspending**。 然后，当用户启动该应用时，将调用 resuming 事件以及 **OnLaunched()** 方法。 请参阅[处理应用预启动](handle-app-prelaunch.md)以获取有关如何处理预启动方案的其他信息。 仅预启动选择加入的应用。

当应用启动时，Windows 显示应用的初始屏幕。 若要配置初始屏幕，请参阅[添加初始屏幕](https://msdn.microsoft.com/library/windows/apps/xaml/hh465331)。

显示初始屏幕时，应用应该注册事件处理程序以及为初始页面设置它所需的任何自定义 UI。 务必使在应用程序的构造函数和 **OnLaunched()** 中运行的这些任务在数秒内完成，否则系统可能会认为应用未响应并终止它。 如果某个应用需要从网络请求数据或者需要从磁盘检索大量数据，应完成这些活动，但不启动。 在应用等待这些长时间运行的操作结束的同时，它可以使用自己的自定义加载 UI 或扩展的初始屏幕。 有关详细信息，请参阅[延长显示初始屏幕的时间](create-a-customized-splash-screen.md)和[初始屏幕示例](http://go.microsoft.com/fwlink/p/?linkid=234889)。

应用完成启动后，它将进入 **Running** 状态，初始屏幕也将消失，将清除所有初始屏幕资源和对象。

## <a name="app-activation"></a>应用激活

除了由用户启动之外，应用还可以由系统激活。 应用可以由合约（如“共享”合约）激活。 或者应用可能会激活以处理自定义 URI 协议或文件（附带注册应用以处理的扩展名）。 有关可激活应用的方法的列表，请参阅 [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693)。

[
              **Windows.UI.Xaml.Application**
            ](https://msdn.microsoft.com/library/windows/apps/br242324) 类定义可替代以处理各种应用激活方法的方法。
[**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 可以处理所有可能的激活类型。 但是，更常见的做法是使用特定方法来处理最常见的激活类型，而对于不太常见的激活类型，则使用 **OnActivated** 作为回滚方法。 有其他方法可用于特定激活：

[**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/hh701797)  
[**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)  
[**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701799)  [**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701801)  
[**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336)  
[**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/hh701806)

这些方法的事件数据包含我们之前所见的相同 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 属性，这会告诉你应用在激活之前处于哪种状态。 解释状态以及应该对它采取的操作，方法与上面[应用启动](#app-launch)部分中所述的方法相同。

**注意**如果你使用计算机的管理员帐户登录，你将无法激活 UWP 应用。

## <a name="running-in-the-background"></a>在后台运行 ##

从 Windows10 版本 1607 开始，应用可以在应用本身所在的同一进程内运行后台任务。 有关详细信息，请参阅[单个进程的后台活动模型](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)。 在本文中，我们不会介绍进程内后台处理，但会介绍这如何影响应用生命周期：已添加与应用何时处于后台有关的两个新的事件。 事件如下：[**EnteredBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.EnteredBackground) 和 [**LeavingBackground**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.Core.CoreApplication.LeavingBackground)。

这些事件还反映用户是否可以看到你的应用的 UI。

Running in the background 是应用程序启动、激活或恢复后所处的默认状态。 在此状态中，应用程序 UI 尚不可见。

## <a name="running-in-the-foreground"></a>Running in the foreground ##

Running in the foreground 意味着应用 UI 可见。

只在应用程序 UI 可见之前和进入 Running in foreground 状态之前，才会引发 **LeavingBackground** 事件。 还会在用户切换回应用时引发该事件。

以前，用于加载 UI 资源的最佳位置是在 **Activated** 或 **Resuming** 事件处理程序中。 现在，**LeavingBackground** 是用于验证 UI 是否准备就绪的最佳位置。

请务必在此时检查可视化资源是否已准备就绪，因为这是应用程序对用户可见之前执行此操作的最后机会。 适用于此事件处理程序的所有 UI 都应快速完成，因为会影响用户体验的启动和恢复时间。 **LeavingBackground** 用于确保 UI 的第一帧已准备就绪时。 然后，应该异步处理长时间运行的存储或网络调用，以便事件处理程序可能会返回。

当用户离开应用程序时，应用会重新进入 Running in background 状态。

## <a name="reentering-the-background-state"></a>重新进入后台状态

**EnteredBackground** 事件表明应用在前台不再可见。 **EnteredBackground** 在最小化应用时（在台式计算机上）引发，而在切换到主屏幕或另一个应用时（在手机上）引发。

### <a name="reduce-your-apps-memory-usage"></a>减少应用的内存使用量

由于应用对用户不再可见，因此这是用于停止 UI 呈现工作和动画的最佳位置。 可以使用 **LeavingBackground** 重新开始这项工作。

如果要在后台工作，可在此位置做好相关准备。 最好是检查 [MemoryManager.AppMemoryUsageLevel](https://msdn.microsoft.com/library/windows/apps/windows.system.memorymanager.appmemoryusagelevel.aspx)（如果需要）并减少应用正在使用的内存使用量（当应用在后台运行时），以便应用不会冒遭系统释放资源而被终止的风险。

有关更多详细信息，请参阅[在应用移动到后台状态时减少内存使用量](reduce-memory-usage.md)。

### <a name="save-your-state"></a>保存状态

suspending 事件处理程序是保存应用状态的最佳位置。 但是，如果你要在后台工作（例如，音频播放、使用扩展的执行会话或进程内后台任务），在 **EnteredBackground** 事件处理程序中以异步方式保存你的数据也是好的做法。 这是因为，当应用以较低优先级处于后台时可能会被终止。 而且由于应用在此情况下并不会经历 suspended 状态，因此会丢失数据。

在后台活动开始之前，在 **EnteredBackground** 事件处理程序中保存你的数据，以确保用户将应用重新返回前台时可获得良好的用户体验。 可以使用应用程序数据 API 保存数据和设置。 有关详细信息，请参阅[存储和检索设置以及其他应用数据](https://msdn.microsoft.com/library/windows/apps/mt299098)。

保存你的数据后，如果超出内存使用量限制，可以从内存中释放你的数据（因为可以稍后重新加载它）。 这将释放内存，释放的内存可供后台活动所需的资源使用。

请注意，如果你的应用在进程中具有后台活动，则应用可能会从 Running in the background 状态转变为 Running in the foreground 状态，而不曾经历 suspended 状态。

### <a name="asynchronous-work-and-deferrals"></a>异步工作和延迟

如果在处理程序中执行异步调用，控件将立即从该异步调用中返回。 这意味着，执行之后会从事件处理程序中返回，并且应用会转变为下一个状态，即使异步调用尚未完成。 使用传递给事件处理程序的 [**EnteredBackgroundEventArgs**](http://aka.ms/Ag2yh4) 对象上的 [**GetDeferral**](http://aka.ms/Kt66iv) 方法以延迟暂停，直到调用返回的 [**Windows.Foundation.Deferral**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.aspx) 对象上的 [**Complete**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.deferral.complete.aspx) 方法。

延迟并不会增加应用终止之前需要运行的代码量。 它仅延迟终止，直到调用延迟的 *Complete* 方法，或者达到延迟期限 - *以先发生者为准*。

如果需要更多时间来保存状态，请调查用于在应用进入后台状态之前的阶段保存状态的方法，以便减少在 **EnteredBackground** 事件处理程序中用于保存的时间。 或者可以请求 [ExtendedExecutionSession](https://msdn.microsoft.com/magazine/mt590969.aspx) 获取更多时间。 由于并不能保证该请求得到允许，因此最好是找到方法来最大程度地减少保存状态所需的时间量。

## <a name="app-suspend"></a>应用暂停

当用户最小化某个应用时，Windows 会等待数秒，以查看用户是否会切换回该应用。 如果用户在此时间范围内未切换回，并且任何扩展执行、后台任务或活动赞助执行都未处于活动状态，则 Windows 将暂停该应用。 只要应用中不存在任何处于活动状态的扩展执行会话等，该应用也将在出现锁屏界面时暂停。

当暂停应用时，它将调用 [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件。 Visual Studio 的 UWP 项目模板为在 **App.xaml.cs** 中称为 **OnSuspending** 的此事件提供处理程序。 在 Windows10 版本 1607 之前，要在此处添加用于保存状态的代码。 现在建议在进入后台状态时保存状态，如上所述。

你应释放独占资源和文件句柄，以便其他应用可以在你的应用暂停时访问它们。 独占资源的示例包括相机、I/O 设备、外部设备以及网络资源。 显式释放独占资源和文件句柄有助于确保其他应用可以在你的应用暂停时访问它们。 当恢复应用时，它应该重新获取其独占资源和文件句柄。

### <a name="be-aware-of-the-deadline"></a>注意截止时间

为了确保设备的快速响应，对你用于在 suspending 事件处理程序中运行代码的时间有限制。 此限制因设备而异，你可以使用称为截止时间的 [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) 对象的属性来查明其数值。

就 **EnteredBackground** 事件处理程序来说，如果在处理程序中执行异步调用，控件将立即从该异步调用中返回。 这意味着，执行之后会从事件处理程序中返回，并且应用将转变为暂停状态，即使异步调用尚未完成。 使用 [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) 对象（可通过事件参数获取）上的 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) 方法延迟进入 suspended 状态，直到调用返回的 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 对象上的 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 方法。

如果需要更多时间，可以请求 [ExtendedExecutionSession](https://msdn.microsoft.com/magazine/mt590969.aspx)。 由于并不能保证该请求得到允许，因此最好是找到方法最大程度地减少在 **Suspended** 事件处理程序中需要的时间量。

### <a name="app-terminate"></a>应用终止

当你的应用暂停时，系统会尝试将你的应用及其数据保留在内存中。 但是，如果系统没有资源将你的应用保留在内存中，它将终止你的应用。 应用不会收到它们被终止的通知，所以你只能将应用数据保存在 **OnSuspension** 事件处理程序中，或者在 **EnteredBackground** 处理程序中以异步方式保存。

当应用确定它在终止后被激活时，它应该加载它保存的应用程序数据，以使应用处于与其终止之前相同的状态。 当用户切换回已终止的暂停应用时，该应用应该在其 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法中还原其应用程序数据。 当终止应用时系统不会通知应用，因此在暂停应用之前，你的应用必须保存其应用程序数据并释放独占资源和文件句柄，并且当在终止后又激活应用时还原这些内容。

**有关使用 Visual Studio 进行调试的注释：** Visual Studio 阻止 Windows 暂停连接到调试程序的应用。 这是为了允许用户在应用正在运行时查看 Visual Studio 调试 UI。 调试应用时，可以使用 Visual Studio 将一个暂停事件发送给该应用。 请确保**调试位置**工具栏显示出来，然后单击**暂停**图标。

## <a name="app-resume"></a>应用恢复

如果用户切换到应用或当设备从低功耗状态恢复时该应用是活动的应用，将恢复暂停的应用。

当应用从 **Suspended** 状态恢复时，它会进入 **Running in background** 状态，系统将应用恢复到它离开时的状态，以使用户感觉该应用好像一直在运行。 不会丢失存储在内存中的任何应用数据。 因此，大多数应用在恢复时并不需要恢复状态（尽管它们应该重新获取它们在被暂停时释放的任何文件或设备句柄），也不需要恢复该应用被暂停时显式释放的任何状态。

应用可能会暂停数小时或数天。 如果应用具有可能已过时的内容或网络连接，这些内容或网络连接应该在应用恢复时刷新。 如果应用已为 [**Application.Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件注册一个事件处理程序，则在应用从 **Suspended** 状态恢复时调用它。 可以在该事件处理程序中刷新应用内容和数据。

如果激活暂停的应用以加入一个应用合约或扩展，它会首先收到 **Resuming** 事件，然后收到 **Activated** 事件。

如果暂停的应用已终止，则不存在任何 **Resuming** 事件，将改为使用 **Terminated** 的 **ApplicationExecutionState** 调用 **OnLaunched()**。 由于在暂停应用时保存了状态，因此可以在 **OnLaunched()** 期间恢复该状态，以使应用对用户来说好像它处于用户离开它时的状态。

当应用暂停时，它不会收到它注册用于接收的任何网络事件。 这些网络事件没有排队，它们只是丢失了。 因此，你的应用应该在恢复时测试网络状态。

**注意**如果恢复处理程序中的代码与 UI 通信，因为[**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339)事件未从 UI 线程中引发，必须使用调度程序。 请参阅[从后台线程更新 UI 线程](https://github.com/Microsoft/Windows-task-snippets/blob/master/tasks/UI-thread-access-from-background-thread.md)以获取有关如何执行此操作的代码示例。

有关一般准则，请参阅[应用暂停和恢复指南](https://msdn.microsoft.com/library/windows/apps/hh465088)。

## <a name="app-close"></a>应用关闭

通常，用户不需要关闭应用，他们可以让 Windows 管理它们。 但是，用户可以选择以下方法来关闭应用：使用关闭手势、按 Alt+F4，或在 Windows Phone 上使用任务切换程序。

没有事件指示用户关闭了应用。 当用户关闭应用时，应用首先处于暂停状态，以使你有机会保存其状态。 在 Windows8.1 及更高版本，应用已被用户关闭后，该应用删除从屏幕和切换列表，但不是会显式终止。

**由用户关闭行为：** 如果你的应用需要执行不同于被 Windows 关闭时用户关闭时，你可以使用激活事件处理程序以确定由用户还是被 Windows 终止应用。 请参阅 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 枚举的参考中 **ClosedByUser** 和 **Terminated** 状态的说明。

我们建议，应用不要以编程方式自行关闭，除非绝对必要。 例如，如果应用检测到内存泄漏，它可以关闭自身来确保用户个人数据的安全性。

## <a name="app-crash"></a>应用崩溃

系统故障体验旨在使用户尽快返回他们当时正在执行的操作。 不应提供警告对话框或其他通知，因为这会给用户带来延迟。

如果应用出现故障、停止响应或者发生意外，系统将通过用户的[反馈和诊断设置](http://go.microsoft.com/fwlink/p/?LinkID=614828)向 Microsoft 发送问题报告。 Microsoft 在问题报告中向你提供错误数据的一个子集，这样你可以使用这些数据改进你的应用。 你可以在“仪表板”中应用的“质量”页面中看到此数据。

当用户在应用发生崩溃之后激活该应用时，其激活事件处理程序将收到 **NotRunning** 的 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 值，并且应显示其初始 UI 和数据。 崩溃后，不要经常使用原本将用于 **Resuming** 和 **Suspended** 的应用数据，因为该数据可能已损坏；请参阅[应用暂停和恢复指南](https://msdn.microsoft.com/library/windows/apps/hh465088)。

## <a name="app-removal"></a>应用删除

当用户删除你的应用时，会一同删除应用及其所有本地数据。 删除应用不会影响存储在公用位置的用户数据，例如“文档”或“图片库”。

## <a name="app-lifecycle-and-the-visual-studio-project-templates"></a>应用生命周期和 Visual Studio 项目模板

在 Visual Studio 项目模板中提供与应用生命周期相关的基本代码。 基本应用可处理启动激活、为你提供还原应用数据的位置，甚至可以在你添加任何自己的代码之前显示主要 UI。 有关详细信息，请参阅[适用于应用的 C#、VB 和 C++ 项目模板](https://msdn.microsoft.com/library/windows/apps/hh768232)。

## <a name="key-application-lifecycle-apis"></a>关键应用程序生命周期 API

-   [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) 命名空间
-   [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766) 命名空间
-   [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865) 命名空间
-   [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 类 (XAML)
-   [**Windows.UI.Xaml.Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 类 (XAML)

## <a name="related-topics"></a>相关主题

* [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)
* [应用暂停和恢复指南](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [处理应用预启动](handle-app-prelaunch.md)
* [处理应用激活](activate-an-app.md)
* [处理应用暂停](suspend-an-app.md)
* [处理应用恢复](resume-an-app.md)
* [单个进程的后台活动模型](https://blogs.windows.com/buildingapps/2016/06/07/background-activity-with-the-single-process-model/#tMmI7wUuYu5CEeRm.99)
* [在后台播放媒体](https://msdn.microsoft.com/windows/uwp/audio-video-camera/background-audio)

 

 
