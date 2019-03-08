---
Description: 了解如何在通知中创建多个步骤的交互。
title: 具有挂起更新激活的 Toast
label: Toast with pending update activation
template: detail.hbs
ms.date: 12/14/2017
ms.topic: article
keywords: windows 10, uwp, toast, 挂起的更新, 多步骤交互性, 多步骤交互
ms.localizationpriority: medium
ms.openlocfilehash: b1574ee2913bd2889af204aae1089dc170df95b8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57648552"
---
# <a name="toast-with-pending-update-activation"></a>具有挂起更新激活的 Toast

可以使用 **PendingUpdate** 在 Toast 通知内创建多步骤交互。 例如，如下所示，可以创建一系列的 Toast，其中后续的 Toast 依赖来自先前 Toast 的响应。

![有待更新的 Toast](images/toast-pendingupdate.gif)

> [!IMPORTANT]
> **需要桌面 Fall Creators Update 和通知库 2.0.0**:您必须运行桌面生成 16299 或更高版本，若要查看挂起的更新工作。 必须使用版本 2.0.0 或更高版本的 [UWP 社区工具包通知 NuGet 库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)来分配按钮上的 **PendingUpdate**。 **PendingUpdate** 仅支持桌面设备，在其他设备上会被忽略。


## <a name="prerequisites"></a>必备条件

本文假定读者具备后列应用知识：

- [构造 toast 通知内容](adaptive-interactive-toasts.md)
- [发送 toast 通知和处理应用程序激活背景](send-local-toast.md)


## <a name="overview"></a>概述

实现一个 Toast，该 Toast 使用挂起的更新作为其激活后行为…

1. 在 Toast 后台激活按钮上指定 **PendingUpdate** 的 **AfterActivationBehavior**

2. 在发送 Toast 时分配 **Tag**（或 **Group**）

3. 当用户单击按钮时，会激活后台任务，Toast 会保持在屏幕上并处于挂起更新状态

4. 在后台任务中，使用相同的 **Tag** 和 **Group**，发送含有新内容的新 Toast


## <a name="assign-pendingupdate"></a>分配 PendingUpdate

在后台激活按钮上，将 **AfterActivationBehavior**设置为 **PendingUpdate**。 请注意，这仅适用于具有 **Background** 的 **ActivationType** 的按钮。

```csharp
new ToastButton("Yes", "action=orderLunch")
{
    ActivationType = ToastActivationType.Background,

    ActivationOptions = new ToastActivationOptions()
    {
        AfterActivationBehavior = ToastAfterActivationBehavior.PendingUpdate
    }
}
```

```xml
<action
    content='Yes'
    arguments='action=orderLunch'
    activationType='background'
    afterActivationBehavior='pendingUpdate' />
```


## <a name="use-a-tag-on-the-notification"></a>在通知上使用“标记”

若要以后替换该通知，必须在通知上分配 **Tag**（或 **Group**）。

```csharp
// Create the notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And show it
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="replace-the-toast-with-new-content"></a>使用新内容替换 Toast

作为对用户单击按钮的响应，会触发后台任务，并需要使用新内容替换 Toast。 只需通过使用相同的 **Tag** 和 **Group** 发送新 Toast 来替换 Toast。

强烈建议在替换时**将音频设置为静音模式**以响应按钮单击，因为用户已经在与 Toast 交互。

```csharp
// Generate new content
ToastContent content = new ToastContent()
{
    ...

    // We disable audio on all subsequent toasts since they appear right after the user
    // clicked something, so the user's attention is already captured
    Audio = new ToastAudio() { Silent = true }
};

// Create the new notification
var notif = new ToastNotification(content.GetXml())
{
    Tag = "lunch"
};

// And replace the old one with this one
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="related-topics"></a>相关主题

- [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/quickstart-toast-pending-update)
- [发送本地 toast 和句柄激活](send-local-toast.md)
- [Toast 通知内容文档](adaptive-interactive-toasts.md)
- [Toast 通知进度栏](toast-progress-bar.md)