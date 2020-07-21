---
Description: 了解如何计划稍后显示本地 toast 通知。
title: 计划 toast 通知
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: windows 10，uwp，计划 toast 通知，scheduledtoastnotification，如何，快速入门，入门，代码示例，演练
ms.localizationpriority: medium
ms.openlocfilehash: 07339cf793bdada51f79d70d9e9e6b6d4a41851b
ms.sourcegitcommit: 017f2f1492f3220da0fae8b4c99de7206a185dff
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2020
ms.locfileid: "81386426"
---
# <a name="schedule-a-toast-notification"></a>计划 toast 通知

计划 toast 通知允许您将通知计划为稍后显示，而不管应用程序是否正在运行。 这适用于显示提醒或用户的其他后续任务的方案，其中通知的时间和内容是提前知道的。

请注意，计划 toast 通知的传递时段为5分钟。 如果计算机在计划的传递时间内处于关闭状态，并且保持不变超过5分钟，则通知将被 "删除"，因此不再与用户相关。 如果需要有保证的通知送达，而不考虑计算机的持续时间，我们建议使用带有时间触发器的后台任务，如[此代码示例](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off)中所示。

> [!IMPORTANT]
> 桌面应用程序（.MSIX/稀疏包和经典 Win32）的发送通知和处理激活步骤略有不同。 按照下面的说明进行操作，但将 `ToastNotificationManager` 替换为[桌面应用](toast-desktop-apps.md)文档中的 `DesktopNotificationManagerCompat` 类。

> **重要 api**： [ScheduledToastNotification 类](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>先决条件

若要完全理解此主题，事先掌握以下内容会很有用...

* Toast 通知术语和概念的应用知识。 有关详细信息，请参阅 [Toast 和操作中心概述](https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/07/08/toast-notification-and-action-center-overview-for-windows-10/)。
* 熟悉 Windows 10 toast 通知内容。 有关详细信息，请参阅 [toast 内容文档](adaptive-interactive-toasts.md)。
* Windows 10 UWP 应用项目


## <a name="install-nuget-packages"></a>安装 NuGet 程序包

我们建议你对你的项目安装以下两个 NuGet 程序包。 代码示例将使用这些程序包。

* [Microsoft.Toolkit.Uwp.Notifications](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)：通过对象而不是原始 XML 生成 toast 有效负载。
* [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/)：使用 C# 生成和分析查询字符串


## <a name="add-namespace-declarations"></a>添加命名空间声明

`Windows.UI.Notifications` 包括 toast Api。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
using Microsoft.QueryStringDotNET; // QueryString.NET
```


## <a name="construct-the-toast-content"></a>构造 toast 内容

在 Windows 10 中，你的 toast 通知内容是使用对于你的通知外观给予了最大程度灵活性的自适应语言描述的。 有关详细信息，请参阅 [toast 内容文档](adaptive-interactive-toasts.md)。

感谢通知库，生成 XML 内容非常简单。 如果不从 NuGet 安装通知库，则需要手动构造 XML，这样就可能出错。

应始终设置 **Launch** 属性，以便用户点击 toast 正文且应用启动时，应用能够知道应显示的内容。

```csharp
// In a real app, these would be initialized with actual data
string title = "ASTR 170B1";
string content = "You have 3 items due today!";

// Now we can construct the final toast content
ToastContent toastContent = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = title
                },
     
                new AdaptiveText()
                {
                    Text = content
                }
            }
        }
    },
 
    // Arguments when the user taps body of toast
    Launch = new QueryString()
    {
        { "action", "viewClass" },
        { "classId", "3910938180" }
 
    }.ToString()
};
```

## <a name="create-the-scheduled-toast"></a>创建计划 toast

初始化 toast 内容后，创建新的[ScheduledToastNotification](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)并传入内容的 XML，并传入要传递通知的时间。

```csharp
// Create the scheduled notification
var toast = new ScheduledToastNotification(toastContent.GetXml(), DateTime.Now.AddSeconds(5));
```


## <a name="provide-a-primary-key-for-your-toast"></a>为 toast 提供主键

如果要以编程方式取消、删除或替换计划的通知，则需要使用 Tag 属性（也可以是组属性）来提供通知的主密钥。 然后，你可以在将来使用此主密钥来取消、删除或替换通知。

要查看有关替换/删除已发送的 toast 通知的更多详细信息，请参阅[快速入门：在操作中心 (XAML) 中管理 toast 通知](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10))。

Tag 和 Group 组合充当复合主键。 Group 是两者中较为通用的标识符，你可以用它来分配如“wallPosts”、“messages”、“friendRequests”等组。而 Tag 应该唯一标识组中的通知本身。 使用通用组时，可以使用 [RemoveGroup API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 删除该组中的所有通知。

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="schedule-the-notification"></a>计划通知

最后，创建[ToastNotifier](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier)并调用 AddToSchedule （），并传入你计划的 toast 通知。

```csharp
// And your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="cancel-scheduled-notifications"></a>取消计划的通知

若要取消计划的通知，必须首先检索所有计划的通知的列表。

然后，找到与之前指定的标记（和选择性组）匹配的计划 toast，并调用 RemoveFromSchedule （）。

```csharp
// Create the toast notifier
ToastNotifier notifier = ToastNotificationManager.CreateToastNotifier();

// Get the list of scheduled toasts that haven't appeared yet
IReadOnlyList<ScheduledToastNotification> scheduledToasts = notifier.GetScheduledToastNotifications();

// Find our scheduled toast we want to cancel
var toRemove = scheduledToasts.FirstOrDefault(i => i.Tag == "18365" && i.Group == "ASTR 170B1");
if (toRemove != null)
{
    // And remove it from the schedule
    notifier.RemoveFromSchedule(toRemove);
}
```


## <a name="activation-handling"></a>激活处理

若要详细了解如何处理激活，请参阅[发送本地 toast](send-local-toast.md)文档。 激活计划 toast 通知的方式与激活本地 toast 通知相同。


## <a name="adding-actions-inputs-and-more"></a>添加操作、输入和其他内容

若要详细了解操作和输入等高级主题，请参阅[发送本地 toast](send-local-toast.md)文档。 操作和输入在本地 toast 中的工作方式与在计划 toast 中的工作方式相同。


## <a name="resources"></a>资源

* [Toast 内容文档](adaptive-interactive-toasts.md)
* [ScheduledToastNotification 类](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)
