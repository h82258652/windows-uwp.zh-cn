---
author: andrewleader
Description: Learn how to use headers to visually group your toast notifications in Action Center.
title: Toast 标题
label: Toast headers
template: detail.hbs
ms.author: mijacobs
ms.date: 12/7/2017
ms.topic: article
keywords: windows 10, uwp, toast, 标题, toast 标题, 通知, toast 分组, 操作中心
ms.localizationpriority: medium
ms.openlocfilehash: fcc515b811a11be045ce80ed816708d230720a29
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/12/2018
ms.locfileid: "6446187"
---
# <a name="toast-headers"></a>Toast 标题

通过在通知中使用 toast 标题，可在操作中心中直观地将相关通知进行归组。

> [!IMPORTANT]
> **需要桌面创意者更新和 1.4.0 的通知库**：必须运行桌面内部版本 15063 或更高版本才可查看 toast 标题。 必须使用版本 1.4.0 或更高版本的 [UWP 社区工具包通知 NuGet 库](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)来构造 toast 内容中的标题。 仅桌面设备支持标题。

如下所示，这组对话统一在标题“野营！！”下。 对话中的每条单独消息都是一个独立的 toast 通知，它们具有相同的 toast 标题。

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

也可以选择按类别对通知进行直观分组，例如航班提醒、包裹跟踪等。

## <a name="add-a-header-to-a-toast"></a>向 toast 添加标题

下面介绍如何向 toast 通知添加标题。

> [!NOTE]
> 仅桌面设备支持标题。 不支持标题的设备会直接忽略标题。

```csharp
ToastContent toastContent = new ToastContent()
{
    Header = new ToastHeader()
    {
        Id = "6289",
        Title = "Camping!!",
        Arguments = "action=openConversation&id=6289",
    },

    Visual = new ToastVisual() { ... }
};
```

```xml
<toast>

    <header
        id="6289"
        title="Camping!!"
        arguments="action=openConversation&amp;id=6289"/>

    <visual>
        ...
    </visual>

</toast>
```

总结...

1. 向 **ToastContent** 添加 **Header**
2. 指定所需的 **Id**、**Title** 和 **Arguments** 属性
3. 发送通知（[了解详细信息](send-local-toast.md)）
4. 在另一个通知上，使用相同的标题 **Id**，以将其统一到该标题下。 **Id** 是用于确定是否应对通知进行分组的唯一属性，也就是说 **Title** 和 **Arguments**可以不同。 会使用组内最新通知中的**Title** 和 **Arguments**。 如果该通知已删除，则使用其次最新通知中的 **Title** 和 **Arguments**。


## <a name="handle-activation-from-a-header"></a>从标题中处理激活

用户可以点击标题，以便用户可通过此操作获取应用中的更多信息。

因此，应用可在标题上提供**参数**，类似于 toast 本身的启动参数。

激活的处理方式与[常规 toast 激活](send-local-toast.md#handling-activation-1)相同，这意味着可以在 `App.xaml.cs` 的 **OnActivated** 方法中检索这些参数，就像用户单击 toast 主体或 toast 上的按钮时一样。

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        // Arguments specified from the header
        string arguments = (e as ToastNotificationActivatedEventArgs).Argument;
    }
}
```


## <a name="additional-info"></a>其他信息

标题可直观地分离通知并将其归组。 它不会更改有关应用可以具有的最大通知数 (20) 以及通知列表的先入先出行为的任何其他逻辑。

标题内的通知顺序如下... 对于给定的应用，会首先显示应用（如果是标题的一部分，则为整个标题组）中最新的通知。

**Id** 可以是所选择的任何字符串。 **ToastHeader** 中的任何属性都没有长度或字符限制。 唯一的限制是整个 XML toast 内容不能大于 5KB。

在出现“查看详细信息”按钮前，创建标题不会更改操作中心内显示的通知数量（此数量默认为 3，并且用户可以在系统的通知“设置”中为每个应用配置此数量）。

单击标题（就像单击应用程序标题一样）不会清除属于此标题的任何通知（应用应使用 toast API 来清除相关通知）。


## <a name="related-topics"></a>相关主题

- [发送本地 toast 和处理激活](send-local-toast.md)
- [toast 内容文档](adaptive-interactive-toasts.md)