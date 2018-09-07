---
author: TylerMSFT
title: 调试后台任务
description: 了解如何调试后台任务，其中包括后台任务激活和调试 Windows 事件日志中的跟踪。
ms.assetid: 24E5AC88-1FD3-46ED-9811-C7E102E01E9C
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，后台任务
ms.localizationpriority: medium
ms.openlocfilehash: f68c20a545e09d81912b8ef9a97a0ab0237ed0e0
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2018
ms.locfileid: "3422442"
---
# <a name="debug-a-background-task"></a>调试后台任务


**重要的 API**
-   [Windows.ApplicationModel.Background](https://msdn.microsoft.com/library/windows/apps/br224847)

了解如何调试后台任务，其中包括后台任务激活和调试 Windows 事件日志中的跟踪。

## <a name="debugging-out-of-process-vs-in-process-background-tasks"></a>调试进程外后台任务与进程内后台任务
本主题主要介绍在单独进程（而非主机应用）中运行的后台任务。 如果你正在调试进程内后台任务，则不会获得单独的后台任务项目，并且可以在 **OnBackgroundActivated()**（进程内后台代码运行时所在的位置）上设置断点，并请查看下面[手动触发后台任务以调试后台任务代码](#trigger-background-tasks-manually-to-debug-background-task-code)中的步骤 2，了解有关如何触发要执行的后台代码的说明。

## <a name="make-sure-the-background-task-project-is-set-up-correctly"></a>请确保正确设置了后台任务项目

本主题假设你已拥有一个带有要调试的后台任务的现有应用。 以下内容特定于在进程外运行的后台任务，并且不适用于进程内后台任务。

-   在 C# 和 C++ 中，确保主项目引用后台任务项目。 如果此引用没有就位，则后台任务将不包括在应用包中。
-   在 C# 和 C++ 中，确保后台任务项目的 **Output type** 是“Windows 运行时组件”。
-   必须在程序包清单中的入口点属性中声明后台类。

## <a name="trigger-background-tasks-manually-to-debug-background-task-code"></a>手动触发后台任务以调试后台任务代码

通过 Microsoft Visual Studio，可以手动触发后台任务。 然后可以单步执行代码并对其进行调试。

1.  在 C# 中，在后台类的 Run 方法中放置一个断点（对于进程内后台任务，请将断点放在 App.OnBackgroundActivated() 中），和/或通过使用 [**System.Diagnostics**](https://msdn.microsoft.com/library/windows/apps/xaml/hh441592.aspx) 编写调试输出。

    在 C++ 中，在后台类的 Run 函数中放置一个断点（对于进程内后台任务，请将断点放在 App.OnBackgroundActivated() 中），和/或通过使用 [**OutputDebugString**](https://msdn.microsoft.com/library/windows/desktop/aa363362) 编写调试输出。

2.  在调试程序中运行你的应用程序，然后使用**生命周期事件**工具栏触发该后台任务。 此下拉菜单显示可以由 Visual Studio 激活的后台任务的名称。

    若要此功能正常运行，则后台任务必须已注册并且必须仍等待触发。 例如，如果后台任务已使用一次性 TimeTrigger 注册，并且该触发器已引发，则通过 Visual Studio 启动该任务将无效。

> [!Note]
> 使用以下触发器的后台任务以及使用触发器类型为 [**SmsReceived**](https://msdn.microsoft.com/library/windows/apps/br224839) 的 [**SystemTrigger**](https://msdn.microsoft.com/library/windows/apps/br224838) 的后台任务均无法采用此方式激活：[**Application trigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.applicationtrigger.aspx)、 [**MediaProcessing trigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.mediaprocessingtrigger.aspx)、[**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032)、[**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543)。  
> **应用程序触发器**和 **MediaProcessingTrigger** 可以在带有 `trigger.RequestAsync()` 的代码中以信号形式手动发出。

![调试后台任务](images/debugging-activation.png)

3.  激活后台任务时，调试程序将附加到该后台任务并在 VS 中显示调试输出。

## <a name="debug-background-task-activation"></a>调试后台任务激活

> [!NOTE]
> 本部分特定于在进程外运行的后台任务，并且不适用于进程内后台任务。

后台任务能否激活取决于以下三项内容：

-   后台任务类的名称和命名空间
-   程序包清单中指定的入口点属性
-   在注册后台任务时由应用指定的入口点

1.  使用 Visual Studio 标注后台任务的入口点：

    -   在 C# 和 C++ 中，标注在后台任务项目中指定的后台任务类的名称和命名空间。

2.  使用清单设计器检查是否在包清单中正确声明了后台任务：

    -   在 C# 和 C++ 中，入口点属性必须与后台任务的命名空间相匹配而且后面必须跟着类名。 例如：RuntimeComponent1.MyBackgroundTask。
    -   还必须指定与后台任务一起使用的所有触发器类型。
    -   除非你使用 [**ControlChannelTrigger**](https://msdn.microsoft.com/library/windows/apps/hh701032) 或 [**PushNotificationTrigger**](https://msdn.microsoft.com/library/windows/apps/hh700543)，否则不得指定可执行文件。

3.  仅适用于 Windows。 若要查看 Windows 用来激活后台任务的入口点，请启用调试跟踪并使用 Windows 事件日志。

    如果你遵循此过程，但事件日志显示后台任务的入口点或触发器有误，则你的应用无法正确注册后台任务。 有关此任务的帮助，请参阅[注册后台任务](register-a-background-task.md)。

    1.  通过转到“开始”屏幕并搜索 eventvwr.exe 来打开事件查看器。
    2.  转到**应用程序和服务日志** - &gt; **Microsoft**  - &gt; **Windows**  - &gt; **BackgroundTaskInfrastructure**事件查看器中。
    3.  在操作窗格中，选择**视图** - &gt; **显示分析和调试日志**以启用诊断日志记录。
    4.  选择**诊断日志**并单击“**启用日志**”。
    5.  现在尝试使用你的应用再次注册并激活后台任务。
    6.  查看诊断日志以了解更详细的错误信息。 这将包括为后台任务注册的入口点。

![用于提供后台任务调试信息的事件查看器](images/event-viewer.png)

## <a name="background-tasks-and-visual-studio-package-deployment"></a>后台任务和 Visual Studio 程序包部署

如果使用后台任务的应用通过 Visual Studio 部署，并且在清单设计器中指定的版本（主要和/或次要）随后更新，则使用 Visual Studio 对该应用进行后续重新部署将导致应用的后台任务停止。 可按以下方式修复此问题：

-   通过运行和包一起生成的脚本，使用 Windows PowerShell 部署更新的应用（而不是 Visual Studio）。
-   如果你已经使用 Visual Studio 部署了应用，而且它的后台任务现在已暂停，请重新引导或注销/登录以再次运行应用的后台任务。
-   你可以选择“始终重新安装我的包”调试选项以在 C# 项目中避免此问题。
-   等待应用为最终部署准备就绪后递增程序包版本（不要在调试时更改）。

## <a name="remarks"></a>备注

-   请确保应用在再次注册后台任务前检查现有后台任务注册情况。 通过每次触发后台任务时多次运行该任务而多次注册同一后台任务可能导致异常结果。
-   如果后台任务需要锁屏界面访问权限，请确保在尝试调试后台任务之前将应用放在锁屏界面上。 有关为支持锁屏界面的应用指定清单选项的信息，请参阅[在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)。
-   后台任务注册参数在注册时验证。 如果有任何注册参数无效，则会返回一个错误。 确保你的应用能够流畅地处理后台任务注册失败的情况，否则，如果你的应用依赖于在尝试注册任务后具备有效注册对象，它可能会崩溃。

有关使用 VS 调试后台任务的详细信息请参阅[如何触发暂停、 恢复和后台事件在 UWP 应用中的](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx)。

## <a name="related-topics"></a>相关主题

* [创建和注册进程外后台任务](create-and-register-a-background-task.md)
* [创建和注册进程内后台任务](create-and-register-an-inproc-background-task.md)
* [注册后台任务](register-a-background-task.md)
* [在应用程序清单中声明后台任务](declare-background-tasks-in-the-application-manifest.md)
* [后台任务指南](guidelines-for-background-tasks.md)
* [如何触发暂停、 恢复和后台 UWP 应用中的事件](https://msdn.microsoft.com/library/windows/apps/xaml/hh974425.aspx)
* [分析与 Visual Studio 代码分析的 UWP 应用的代码质量](https://msdn.microsoft.com/library/windows/apps/xaml/hh441471.aspx)

 

 
