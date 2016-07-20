---
author: TylerMSFT
title: "应用生命周期"
description: "本主题介绍通用 Windows 平台 (UWP) 应用的生命周期，从其激活时直到其关闭。"
ms.assetid: 6C469E77-F1E3-4859-A27B-C326F9616D10
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: c35f3c4bbc33d7202769badd7a0bcdc91f39bc84

---

# 应用生命周期


\[ 已针对 Windows 10 上的 UWP 应用更新 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**Windows.UI.Xaml.Application 类**](https://msdn.microsoft.com/library/windows/apps/br242324)
-   [**Windows.ApplicationModel.Activation 命名空间**](https://msdn.microsoft.com/library/windows/apps/br224766)

本主题介绍通用 Windows 平台 (UWP) 应用的生命周期，从其激活时直到其关闭。 许多用户将他们的工作和娱乐分散在多个设备和应用上。 用户现在期望你的应用能够在他们在设备上执行多任务时记住其状态。 例如，他们期望页面滚动到与之前相同的位置，并且所有控件处于与之前相同的状态。 通过了解启动、暂停和恢复的应用程序生命周期，你可以提供这种无缝行为。

## 应用执行状态


此图表现了应用执行状态之间的转换。 我们在后面几部分中介绍了这些状态和事件。 有关每个状态转换以及你的应用应如何响应的详细信息，请参阅有关 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 枚举的参考文档。

![显示应用执行状态之间转换的状态图](images/state-diagram.png)

## 部署


为了使应用能够激活，必须先对其进行部署。 当用户安装你的应用时，或者当你使用 Visual Studio 在开发和测试期间生成并运行你的应用时，将部署你的应用。 有关此部署以及高级部署方案的详细信息，请参阅[应用包和部署](https://msdn.microsoft.com/library/windows/apps/hh464929)。

## 应用启动


当应用处于 **NotRunning** 状态并且用户点击“开始”屏幕或应用程序列表上的应用磁贴时，将启动应用。 也可以通过预启动常用应用来优化响应（请参阅[处理应用预启动](handle-app-prelaunch.md)）。 应用可能处于 **NotRunning** 状态，原因有：它从未启动、它运行后出现了故障，或者它在暂停后无法保留在内存中而被系统终止。 启动与激活不同。 激活是指通过合约或扩展（例如“搜索”合约）激活你的应用。

当启动应用时（包括当应用当前在内存中暂停时），调用 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法。 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) 参数包含你的应用之前的状态和激活参数。

当用户切换到终止的应用时，系统将发送 [**LaunchActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224731) 参数，其中 [**Kind**](https://msdn.microsoft.com/library/windows/apps/br224728) 设置为 **Launch**，[**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 设置为 **Terminated** 或 **ClosedByUser**。 应用应该加载其保存的应用程序数据并刷新其显示的内容。

如果 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 的值为 **NotRunning**，应用应重新启动，就好像初次启动它一样。

当应用启动时，Windows 显示应用的初始屏幕。 若要配置初始屏幕，请参阅[添加初始屏幕](https://msdn.microsoft.com/library/windows/apps/xaml/hh465331)。

当显示初始屏幕时，你的应用应使其用户界面准备就绪。 应用的主要任务是注册事件处理程序和设置它加载初始页面所需的任何自定义 UI。 这些任务仅应占用几秒的时间。 如果某个应用需要从网络请求数据或者需要从磁盘检索大量的数据，这些活动应在激活以外完成。 在应用等待这些长期运行的操作结束的同时，它可以使用自己的自定义加载 UI 或扩展的初始屏幕。 有关详细信息，请参阅[延长显示初始屏幕的时间](create-a-customized-splash-screen.md)和[初始屏幕示例](http://go.microsoft.com/fwlink/p/?linkid=234889)。 应用完成激活后，它将进入 **Running** 状态，初始屏幕也将消失（并将清除其所有资源和对象）。

## 应用激活


用户可通过各种扩展和合约（例如“共享”合约）激活应用。 有关可激活应用的方法的列表，请参阅 [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693)。

[**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 类定义了为处理各种激活类型而可以替代的一些方法。 多种激活类型具有可替代的特定方法，例如 [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)、[**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336) 等。对于其他激活类型，请替代 [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)方法。

你的应用的激活代码可以通过测试了解其激活原因以及是否已经处于 **Running** 状态。

当操作系统终止你的应用，随后用户重新启动它时，你的应用可以在激活期间还原之前保存的数据。 Windows 在应用暂停后可能出于一些原因而终止。 用户可以手动关闭你的应用或者注销，否则系统的资源可能不足。 如果用户在 Windows 终止你的应用之后启动它，该应用将收到一个 [**Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 回调，并且用户将看到应用的初始屏幕，直到该应用激活。 你可以通过此事件确定你的应用是否需要还原其在上次暂停时保存的数据，或者是否必须加载应用的默认数据。 由于初始屏幕已出现，因此你的应用代码可以在不明显拖延用户时间的情况下花费一些处理时间来完成此激活操作，尽管在重新启动或继续该操作时前面所提到的关于运行时间较长的操作的问题仍然存在。

[**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 事件数据包括一个 [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 属性，用于告诉你应用在激活之前处于哪种状态。 此属性是 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 枚举中的值之一：

| 终止原因                                                        | [**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 属性的值 | 采取的操作          |
|-------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------|
| 已由系统终止（例如，因为资源限制）       | **Terminated**                                                                                          | 还原会话数据    |
| 被用户关闭或被用户终止进程                             | **ClosedByUser**                                                                                        | 使用默认数据启动 |
| 意外终止，或者应用在*当前用户会话*期间未运行 | **NotRunning**                                                                                          | 使用默认数据启动 |

 

**注意** *当前用户会话*基于 Windows 登录。 只要当前用户未显式注销、关闭当前用户会话，或者 Windows 未出于其他原因而重新启动它，该会话便可以保留在诸如锁屏界面身份验证、切换用户的多个事件中。

 

[**PreviousExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224729) 还可能有 **Running** 或 **Suspended** 值，但在这些情况下，你的应用之前未终止，因此你无需还原任何数据，因为所有数据都已经在内存中。

**注意**  

如果你使用计算机的管理员帐户登录，则你将无法激活任何 UWP 应用。

有关详细信息，请参阅[应用扩展](https://msdn.microsoft.com/library/windows/apps/hh464906)。

### **OnActivated** 与特定激活

[**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) 方法旨在处理所有可能的激活类型。 但是，更常见的做法是使用不同的方法来处理最常见的激活类型，而对于不太常见的激活类型，则仅使用 **OnActivated** 作为回滚方法。 例如，[**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 具有 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法，用于在 [**ActivationKind**](https://msdn.microsoft.com/library/windows/apps/br224693) 是 **Launch** 时作为回调进行调用，这是适用于大多数应用的典型激活方法。 有超过 6 种 **On\*** 方法可用于特定的激活：[**OnCachedFileUpdaterActivated**](https://msdn.microsoft.com/library/windows/apps/hh701797)、[**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)、[**OnFileOpenPickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701799)、[**OnFileSavePickerActivated**](https://msdn.microsoft.com/library/windows/apps/hh701801)、[**OnSearchActivated**](https://msdn.microsoft.com/library/windows/apps/br242336)、[**OnShareTargetActivated**](https://msdn.microsoft.com/library/windows/apps/hh701806)。 XAML 应用的起始模板具有 **OnLaunched** 的实现和 [**Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 的处理程序。

## 应用暂停


每当用户切换到其他应用、桌面或“开始”屏幕时，系统都会暂停你的应用。 当设备进入低功耗状态时，可能也会暂停你的应用。 每当用户切回到你的应用时，系统就会恢复你的应用。 当系统恢复你的应用时，你的变量和数据结构的内容与系统将你的应用暂停之前的内容相同。 系统会将你的应用完全恢复到你离开时的状态，使用户感觉你的应用好像一直在后台运行一样。

当用户将一个应用移动到后台时，Windows 将等待几秒，以查看用户是否打算立即返回该应用，以便在用户有此打算时快速切换。 如果用户在此时间段内没有切换回，Windows 将暂停该应用。

如果在应用暂停期间，你需要执行一些异步工作，则需要将暂停完成时间延迟到工作完成之后。 你可以使用 [**SuspendingOperation**](https://msdn.microsoft.com/library/windows/apps/br224688) 对象（可通过事件参数获取）上的 [**GetDeferral**](https://msdn.microsoft.com/library/windows/apps/br224690) 方法将暂停完成时间延迟到你对所返回的 [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684) 对象调用 [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685) 方法之后。

当你的应用被暂停后，系统会尝试将你的应用及其数据保留在内存中。 但是，如果系统没有资源将你的应用保存在内存里，则将终止你的应用。 应用不会收到它们被终止的通知，所以你保存应用数据的唯一机会是在暂停期间。 当应用确定它在终止后被激活时，它应该加载它在暂停期间保存的应用程序数据，以使应用处于与其暂停之前相同的状态。 当用户切换回已终止的暂停应用时，该应用应该在其 [**OnLaunched**](https://msdn.microsoft.com/library/windows/apps/br242335) 方法中还原其应用程序数据。 当终止应用时系统不会通知应用，因此当暂停应用时，你的应用必须保存其应用程序数据并释放独占资源和文件句柄，并且当在终止后又激活应用时还原这些内容。

如果应用已经为 [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br242341) 事件注册一个事件处理程序，则即将暂停应用前调用此代码。 你可以使用事件处理程序保存应用和用户数据。 我们建议使用应用程序数据 API 完成此目的，因为它们可保证在应用进入 **Suspended** 状态之前完成工作。 有关详细信息，请参阅[存储和检索设置以及其他应用数据](https://msdn.microsoft.com/library/windows/apps/mt299098)。

你还应释放独占资源和文件句柄，以便在你的应用没有使用它们时其他应用可以访问它们。 独占资源的示例包括相机、I/O 设备、外部设备以及网络资源。 显式释放独占资源和文件句柄有助于确保你的应用未使用它们时其他应用可以访问它们。 当在终止后又激活应用时，它应该重新打开其独占资源和文件句柄。

通常，你的应用应该在处理暂停事件时立即保存其状态并释放其资源和文件句柄，并且此代码最多只需 1 秒便可完成工作。 如果应用未在数秒内从暂停事件中返回，则 Windows 假设应用已停止响应并终止该应用。

有些应用必须继续运行才能完成后台任务。 例如，你的应用可以在后台继续播放音频；有关详细信息，请参阅[后台音频](https://msdn.microsoft.com/library/windows/apps/mt282140)）。 此外，即使你的应用暂停甚至终止，后台传输操作仍将继续；有关详细信息，请参阅[如何下载文件](https://msdn.microsoft.com/library/windows/apps/xaml/jj152726.aspx#downloading_a_file_using_background_transfer)）。

有关指南，请参阅[应用暂停和恢复指南](https://msdn.microsoft.com/library/windows/apps/hh465088)。

**有关使用 Visual Studio 进行调试的注释：**Visual Studio 阻止 Windows 暂停连接到调试程序的应用。 这是为了允许用户在应用正在运行时查看 Visual Studio 调试 UI。 调试应用时，可以使用 Visual Studio 将一个暂停事件发送给该应用。 请确保“调试位置”****工具栏正在显示，然后单击“暂停”****图标。

## 应用可见性


当用户从你的应用切换到其他应用时，你的应用将不再可见，但仍保持 **Running** 状态，直到 Windows 暂停它。 如果用户离开你的应用，但在暂停它之前又激活或切换回该应用，它会保持 **Running** 状态。

当应用可见性更改时，你的应用不会收到激活事件，因为该应用仍处于运行状态。 Windows 只需根据需要来回切换应用即可。 如果应用在用户切换离开和返回时需要执行某个操作，请处理 [**Window.VisibilityChanged**](https://msdn.microsoft.com/library/windows/apps/hh702458) 事件。

不要依靠这些事件的特定排序。

## 应用恢复


当用户切换到应用或当设备从低功耗状态恢复时该应用是活动的应用时，将恢复暂停的应用。

有关应用在恢复时所处状态的枚举，请参阅 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694)。 应用从 **Suspended** 状态恢复时，它会进入 **Running** 状态并从暂停的位置和时间处继续运行。 不会丢失存储在内存中的任何应用数据。 因此，大多数应用在恢复时不需要执行任何操作。 但是，应用可能暂停数小时甚至数天。 如果应用具有可能已过时的内容或网络连接，这些内容或网络连接应该在应用恢复时刷新。 如果应用已经为 [**Application.Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件注册了一个事件处理程序，则在应用从 **Suspended** 状态恢复时调用它。 你可以使用该事件处理程序刷新应用内容和数据。

如果激活暂停的应用以加入一个应用合约或扩展，它会首先收到 **Resuming** 事件，然后收到 **Activated** 事件。

当应用暂停时，它不会收到它注册接收的任何网络事件。 这些网络事件没有排队，它们只是丢失了。 因此，你的应用应该在恢复时测试网络状态。

**注意** 由于 [**Resuming**](https://msdn.microsoft.com/library/windows/apps/br242339) 事件未从 UI 线程中引发，因此如果恢复处理程序中的代码与 UI 通信，则必须使用调度程序。

 

有关指南，请参阅[应用暂停和恢复指南](https://msdn.microsoft.com/library/windows/apps/hh465088)。

## 应用关闭


通常，用户不需要关闭应用，他们可以让 Windows 管理它们。 但是，用户可以选择以下方法来关闭应用：使用关闭手势，在 Windows 上按 Alt+F4，或在 Windows Phone 上使用任务切换器。

没有任何特殊事件指示用户关闭了应用。

在用户关闭应用之后，它将首先暂停然后终止，之后进入 **NotRunning** 状态。

在 Windows 8.1 和更高版本中，在用户关闭应用之后，该应用将从屏幕和切换列表中删除，但不显式终止。

如果应用已经为 **Suspending** 事件注册了一个事件处理程序，当应用暂停时将调用该处理程序。 你可以使用此事件处理程序将相关应用程序和用户数据保存到持久性存储中。

**用户关闭行为：**如果应用在被用户关闭时需要执行不同于被 Windows 关闭时的操作，你可以使用激活事件处理程序确定应用是被用户还是被 Windows 终止的。 请参阅 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 枚举的参考中 **ClosedByUser** 和 **Terminated** 状态的说明。

我们建议，应用不要以编程方式自行关闭，除非绝对必要。 例如，如果应用检测到内存泄漏，它可以关闭自己来确保用户个人数据的安全。 当你以编程方式关闭应用时，系统会将此视为应用崩溃。

## 应用故障


系统故障体验旨在使用户尽快返回他们当时正在执行的操作。 不应提供警告对话框或其他通知，因为这会给用户带来延迟。

如果应用出现故障、停止响应或者发生意外，系统将通过用户的[反馈和诊断设置](http://go.microsoft.com/fwlink/p/?LinkID=614828)向 Microsoft 发送问题报告。 Microsoft 在问题报告中向你提供错误数据的一个子集，这样你可以使用这些数据改进你的应用。 你可以在“仪表板”中应用的“质量”页面中看到此数据。

当用户在应用发生崩溃之后激活该应用时，其激活事件处理程序将收到 **NotRunning** 的 [**ApplicationExecutionState**](https://msdn.microsoft.com/library/windows/apps/br224694) 值，并且应显示其初始 UI 和数据。 崩溃后，不要经常使用原本将用于 **Resuming** 和 **Suspended** 的应用数据，因为该数据可能已损坏；请参阅[应用暂停和恢复指南](https://msdn.microsoft.com/library/windows/apps/hh465088)。

## 应用删除


当用户删除你的应用时，会一同删除应用及其所有本地数据。 删除应用不会影响存储在公用位置的用户数据，例如“文档”或“图片库”。

## 应用生命周期和 Visual Studio 项目模板


在起始 Visual Studio 项目模板中提供了与应用生命周期相关的基本代码。 基本应用可处理启动激活、为你提供还原应用数据的位置，甚至可以在你添加任何你自己的代码之前显示主要 UI。 有关详细信息，请参阅[适用于应用的 C#、VB 和 C++ 项目模板](https://msdn.microsoft.com/library/windows/apps/hh768232)。

## 应用程序生命周期中的关键 API


-   [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) 命名空间
-   [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766) 命名空间
-   [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865) 命名空间
-   [**Windows.UI.Xaml.Application**](https://msdn.microsoft.com/library/windows/apps/br242324) 类 (XAML)
-   [**Windows.UI.Xaml.Window**](https://msdn.microsoft.com/library/windows/apps/br209041) 类 (XAML)

**注意**  
本文适用于编写通用 Windows 平台 (UWP) 应用的 Windows 10 开发人员。 如果你要针对 Windows 8.x 或 Windows Phone 8.x 进行开发，请参阅[存档文档](http://go.microsoft.com/fwlink/p/?linkid=619132)。

 

## 相关主题


* [应用暂停和恢复指南](https://msdn.microsoft.com/library/windows/apps/hh465088)
* [处理应用预启动](handle-app-prelaunch.md)
* [处理应用激活](activate-an-app.md)
* [处理应用暂停](suspend-an-app.md)
* [处理应用恢复](resume-an-app.md)

 

 



<!--HONumber=Jul16_HO2-->


