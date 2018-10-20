---
author: andrewleader
Description: Learn how to use a progress bar within your toast notification.
title: Toast 进度栏和数据绑定
label: Toast progress bar and data binding
template: detail.hbs
ms.author: mijacobs
ms.date: 12/7/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, toast, 进度栏, toast 进度栏, 通知, toast 数据绑定
ms.localizationpriority: medium
ms.openlocfilehash: b99c2479bef3c10ecc82707e475f49fd2b9014ec
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/20/2018
ms.locfileid: "5167874"
---
# <a name="toast-progress-bar-and-data-binding"></a>Toast 进度栏和数据绑定

使用 Toast 通知内的进度栏可向用户传送长时运行的操作的状态，如下载、视频呈现、练习目标等。

> [!IMPORTANT]
> **需要创意者更新和 1.4.0 的通知库**：目标必须为 SDK 15063 并且运行版本 15063 或更高版本以使用 Toast 上的进度栏。 必须使用版本 1.4.0 或更高版本的 [UWP 社区工具包通知 NuGet 库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)来构造 Toast 内容中的进度栏。

Toast 内的进度栏可为“不确定的”（没有特定值，动态点指示有操作正在发生）或“确定的”（在栏中填充了特定百分比，如 60%）。

> **重要 API**：[NotificationData 类](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationdata)，[ToastNotifier.Update 方法](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update)，[ToastNotification 类](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification)

> [!NOTE]
> 仅桌面设备支持 Toast 通知中的进度栏。 其他设备会从通知中删除进度栏。

以下图片显示一个确定的进度栏，其所有相应的属性已标记。

<img alt="Toast with progress bar properties labeled" src="images/toast-progressbar-annotated.png" width="626"/>

| 属性 | 类型 | 必需 | 描述 |
|---|---|---|---|
| **Title** | 字符串或 [BindableString](toast-schema.md#bindablestring) | false | 获取或设置可选标题字符串。 支持数据绑定。 |
| **Value** | Double 或 [AdaptiveProgressBarValue](toast-schema.md#adaptiveprogressbarvalue) 或 [BindableProgressBarValue](toast-schema.md#bindableprogressbarvalue) | false | 获取或设置进度栏的值。 支持数据绑定。 默认为 0。 可以为 0.0 和 1.0 之间的双精度浮点数、`AdaptiveProgressBarValue.Indeterminate` 或 `new BindableProgressBarValue("myProgressValue")`。 |
| **ValueStringOverride** | 字符串或 [BindableString](toast-schema.md#bindablestring) | false | 获取或设置要显示的可选字符串，而不是默认百分比字符串。 如果未提供，会显示诸如“70%”的内容。 |
| **Status** | 字符串或 [BindableString](toast-schema.md#bindablestring) | true | 获取或设置状态字符串（必需），它显示在左侧进度栏下方。 此字符串应反映操作的状态，如“正在下载...”或“正在安装...” |


下面介绍如何生成上面看到的通知...

```csharp
ToastContent content = new ToastContent()
{
    Visual = new ToastVisual()
    {
        BindingGeneric = new ToastBindingGeneric()
        {
            Children =
            {
                new AdaptiveText()
                {
                    Text = "Downloading your weekly playlist..."
                },
 
                new AdaptiveProgressBar()
                {
                    Title = "Weekly playlist",
                    Value = 0.6,
                    ValueStringOverride = "15/26 songs",
                    Status = "Downloading..."
                }
            }
        }
    }
};
```

```xml
<toast>
    <visual>
        <binding template="ToastGeneric">
            <text>Downloading your weekly playlist...</text>
            <progress
                title="Weekly playlist"
                value="0.6"
                valueStringOverride="15/26 songs"
                status="Downloading..."/>
        </binding>
    </visual>
</toast>
```

但是，需要动态更新进度栏的值，才能让进度栏真正处于“实时”状态。 实现此操作的方法是使用数据绑定来更新 Toast。


## <a name="using-data-binding-to-update-a-toast"></a>使用数据绑定更新 Toast

使用数据绑定包括以下步骤…

1. 构造利用数据绑定字段的 Toast 内容
2. 将 **Tag**（或 **Group**）分配到 **ToastNotification**
3. 在 **ToastNotification** 上定义初始 **Data** 值
4. 发送 Toast
5. 利用 **Tag** 和 **Group**，使用新值来更新 **Data** 值

以下代码片段演示步骤 1-4。 下一个片段演示如何更新 Toast 的 **Data** 值。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications;
 
public void SendUpdatableToastWithProgress()
{
    // Define a tag (and optionally a group) to uniquely identify the notification, in order update the notification data later;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Construct the toast content with data bound fields
    var content = new ToastContent()
    {
        Visual = new ToastVisual()
        {
            BindingGeneric = new ToastBindingGeneric()
            {
                Children =
                {
                    new AdaptiveText()
                    {
                        Text = "Downloading your weekly playlist..."
                    },
    
                    new AdaptiveProgressBar()
                    {
                        Title = "Weekly playlist",
                        Value = new BindableProgressBarValue("progressValue"),
                        ValueStringOverride = new BindableString("progressValueString"),
                        Status = new BindableString("progressStatus")
                    }
                }
            }
        }
    };
 
    // Generate the toast notification
    var toast = new ToastNotification(content.GetXml());
 
    // Assign the tag and group
    toast.Tag = tag;
    toast.Group = group;
 
    // Assign initial NotificationData values
    // Values must be of type string
    toast.Data = new NotificationData();
    toast.Data.Values["progressValue"] = "0.6";
    toast.Data.Values["progressValueString"] = "15/26 songs";
    toast.Data.Values["progressStatus"] = "Downloading...";
 
    // Provide sequence number to prevent out-of-order updates, or assign 0 to indicate "always update"
    toast.Data.SequenceNumber = 1;
 
    // Show the toast notification to the user
    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

然后，在想要更改 **Data** 值时，可使用 [**Update**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotifier.Update) 方法提供新数据，而无需重新构造整个 Toast 负载。

```csharp
using Windows.UI.Notifications;
 
public void UpdateProgress()
{
    // Construct a NotificationData object;
    string tag = "weekly-playlist";
    string group = "downloads";
 
    // Create NotificationData and make sure the sequence number is incremented
    // since last update, or assign 0 for updating regardless of order
    var data = new NotificationData
    {
        SequenceNumber = 2
    };

    // Assign new values
    // Note that you only need to assign values that changed. In this example
    // we don't assign progressStatus since we don't need to change it
    data.Values["progressValue"] = "0.7";
    data.Values["progressValueString"] = "18/26 songs";

    // Update the existing notification's data by using tag/group
    ToastNotificationManager.CreateToastNotifier().Update(data, tag, group);
}
```

使用 **Update** 方法而不是替换整个 Toast，还可确保 Toast 通知保留在操作中心中的同一位置而不会向上或向下移动。 如果在进度条被填充时 Toast 每隔几秒钟就跳转到操作中心顶部，会让用户感到十分困惑！

**Update** 方法返回枚举 [**NotificationUpdateResult**](https://docs.microsoft.com/uwp/api/windows.ui.notifications.notificationupdateresult)，用于告知是更新成功还是找不到通知（这意味着用户可能已消除通知，应停止向其发送更新）。 在进度操作完成之前（如在下载完成前），不建议弹出另一个 Toast。


## <a name="elements-that-support-data-binding"></a>支持数据绑定的元素
Toast 通知中的以下元素支持数据绑定

- **AdaptiveProgress** 上的所有属性
- 顶级 **AdaptiveText** 元素上的 **Text** 属性


## <a name="update-or-replace-a-notification"></a>更新或替换通知

从 Windows 10 开始，始终可以通过使用相同的 **Tag** 和 **Group** 发送新 Toast 来**替换**通知。 那么**替换** Toast 和**更新** Toast 的数据之间有何区别？

| | 替换 | 更新 |
| -- | -- | --
| **在操作中心中的位置** | 将通知移动到操作中心的顶部。 | 将通知就地保留在操作中心内。 |
| **修改内容** | 可彻底更改 Toast 的全部内容/布局 | 仅可更改支持数据绑定的属性（进度栏和顶级文本） |
| **作为弹出窗口重新出现** | 如果保持 **SuppressPopup** 的设置为 `false`（或设置为 true 以自动将其发送到操作中心），则可以作为 Toast 弹出窗口重新出现 | 不会作为弹出窗口重新出现；在操作中心内自动更新 Toast 的数据 |
| **用户已消除** | 无论用户是否消除上一个通知，将始终发送用于替换的 Toast | 如果用户已消除 Toast，将无法更新 Toast |

一般情况下，**更新可用于…**

- 在短时间内经常更改且不需要引起用户注意的信息
- Toast 内容的细微更改，如将 50% 更改为 65%

通常情况下，更新序列完成（如文件已下载）之后，建议替换最后一步，因为…

- 最终的通知很可能会有较大的布局更改，如删除进度栏、添加新按钮等
- 用户可能会消除待处理的进度通知，因为用户不一定要监视下载进程，而是希望在操作完成时通过弹出 Toast 获取通知


## <a name="related-topics"></a>相关主题

- [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/quickstart-toast-progress-bar)
- [Toast 内容文档](adaptive-interactive-toasts.md)