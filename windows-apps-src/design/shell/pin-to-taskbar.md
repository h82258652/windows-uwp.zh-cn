---
Description: 你可以通过编程方式将应用固定到任务栏，并且可以检查它当前是否已固定。
title: 将应用固定到任务栏
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 任务栏, 任务栏管理器, 固定到任务栏, 主要磁贴
ms.localizationpriority: medium
ms.openlocfilehash: 640dc637a1c50718210d87af87cb8b8e706a5ab7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57604092"
---
# <a name="pin-your-app-to-the-taskbar"></a>将应用固定到任务栏

你可以通过编程方式将自己的应用固定到任务栏上，就像[将应用固定到“开始”菜单](tiles-and-notifications/primary-tile-apis.md)一样。 你可以检查应用当前是否已固定，以及任务栏是否允许固定。 

![任务栏](images/taskbar/taskbar.png)

> [!IMPORTANT]
> **需要 Fall Creators Update**:必须为目标 SDK 版本 16299 和运行生成 16299 或更高版本使用任务栏 Api。

> **重要的 API**：[TaskbarManager 类](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) 


## <a name="when-should-you-ask-the-user-to-pin-your-app-to-the-taskbar"></a>应该何时请求用户将你的应用固定到任务栏？ 

利用 [TaskbarManager 类](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)，你可以请求用户将你的应用固定到任务栏；用户必须批准该请求。 你尽了很大努力来构建一流的应用，现在你有机会请求用户将其固定到任务栏。 但在我们深入探讨代码之前，你在设计体验时需要牢记以下几点：

* **务必**使用明确的“固定到任务栏”行动号召在应用中制作无干扰并且可轻松消除的 UX。 为此，请避免使用对话框和浮出控件。 
* **务必**在要求用户固定应用前明确解释你的应用值。
* 如果磁贴已经固定或设备不支持磁贴，**不要**请求用户固定应用。 （本文说明如何确定是否支持固定。）
* **不要**反复要求用户固定你的应用（用户可能会生气）。
* 没有显式用户交互或当你的应用最小化/未打开时**不要**调用固定 API。


## <a name="1-check-whether-the-required-apis-exist"></a>1.检查是否存在所需的 Api

如果你的应用支持较旧的 Windows 10 版本，则需要检查 TaskbarManager 类是否可用。 你可以使用 [ApiInformation.IsTypePresent 方法](https://docs.microsoft.com/en-us/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsTypePresent_System_String_)执行此检查。 如果 TaskbarManager 类不可用，请避免执行任何 API 调用。

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Shell.TaskbarManager"))
{
    // Taskbar APIs exist!
}

else
{
    // Older version of Windows, no taskbar APIs
}
```


## <a name="2-check-whether-taskbar-is-present-and-allows-pinning"></a>2.检查是否任务栏已存在，并允许固定

UWP 应用可以在各种设备上运行；并非所有设备都支持任务栏。 现在，仅桌面设备支持任务栏。 

即使任务栏可用，用户计算机上的组策略也可能会禁用任务栏固定。 因此，尝试固定应用之前，你需要检查是否支持固定到任务栏。 如果任务栏存在并且允许固定，则 [TaskbarManager.IsPinningAllowed 属性](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsPinningAllowed) 会返回 True。 

```csharp
// Check if taskbar allows pinning (Group Policy can disable it, or some device families don't have taskbar)
bool isPinningAllowed = TaskbarManager.GetDefault().IsPinningAllowed;
```

> [!NOTE]
> 如果你不希望将应用固定到任务栏，只是想了解任务栏是否可用，请使用 [TaskbarManager.IsSupported 属性](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsSupported)。


## <a name="3-check-whether-your-app-is-currently-pinned-to-the-taskbar"></a>3.检查是否应用当前锁定到任务栏

显然，如果应用已固定到任务栏，则请求用户允许你将应用固定到任务栏毫无意义。 你可以在请求用户之前使用 [TaskbarManager.IsCurrentAppPinnedAsync 方法](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsCurrentAppPinnedAsync)检查应用是否已固定。

```csharp
// Check whether your app is currently pinned
bool isPinned = await TaskbarManager.GetDefault().IsCurrentAppPinnedAsync();

if (isPinned)
{
    // The app is already pinned--no point in asking to pin it again!
}
else 
{
    //The app is not pinned. 
}
```


##  <a name="4-pin-your-app"></a>4.将应用固定

如果任务栏存在且允许固定，并且当前未固定你的应用，则你可能需要显示一条巧妙的提示，让用户知道他们可以固定你的应用。 例如，你可以在用户能够单击的 UI 中的某个地方显示固定图标。 

如果用户单击你的固定建议 UI，则将调用 [TaskbarManager.RequestPinCurrentAppAsync 方法](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.RequestPinCurrentAppAsync)。 此方法会显示一个对话框，并请求用户确认他们想要将你的应用固定到任务栏。

> [!IMPORTANT]
> 这必须从前台 UI 线程中进行调用，否则将出现异常。

```csharp
// Request to be pinned to the taskbar
bool isPinned = await TaskbarManager.GetDefault().RequestPinCurrentAppAsync();
```

![固定对话框](images/taskbar/pin-dialog.png)

此方法会返回一个布尔值，指明你的应用现在是否已固定到任务栏。 如果你的应用已固定，则此方法会立即返回 True，而不向用户显示对话框。 如果用户在对话框中单击“否”或者不允许将你的应用固定到任务栏，则此方法会返回 False。 相反，如果用户单击“是”并且固定了应用，则 API 将返回 True。


## <a name="resources"></a>资源

* [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/quickstart-pin-to-taskbar)
* [TaskbarManager 类](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [将应用固定到开始菜单](tiles-and-notifications/primary-tile-apis.md)