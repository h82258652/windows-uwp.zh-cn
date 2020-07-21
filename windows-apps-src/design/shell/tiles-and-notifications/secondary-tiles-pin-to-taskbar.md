---
Description: 了解如何将辅助磁贴固定到任务栏。
title: 将辅助磁贴固定到任务栏
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: windows 10，uwp，固定到任务栏，辅助磁贴，将辅助磁贴固定到任务栏，快捷方式
ms.localizationpriority: medium
ms.openlocfilehash: 5d4041faf2fbc729291da902e66be1e0979f9d97
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/08/2020
ms.locfileid: "82971012"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>将辅助磁贴固定到任务栏

就像固定辅助磁贴来开始，可以将辅助磁贴固定到任务栏，使用户能够快速访问应用中的内容。

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **受限访问 api**：此 API 是一种有限的访问功能。 若要使用此 API，请[taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar)联系。

> **需要10月2018更新**：必须以 SDK 17763 为目标，运行版本17763或更高版本以固定到任务栏。


## <a name="guidance"></a>指南

辅助磁贴为用户提供了一个一致且高效的方法，可让用户直接访问应用中的特定区域。 尽管用户选择是否将辅助磁贴 "固定" 到任务栏，但应用中的 pinnable 区域由开发人员决定。 有关更多指导，请参阅[辅助磁贴指南](secondary-tiles-guidance.md)。


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. 确定 API 是否存在和解锁受限访问

较旧的设备没有任务栏固定 Api （如果面向的是较旧版本的 Windows 10）。 因此，不应在无法固定的设备上显示固定按钮。

此外，此功能在受限访问下被锁定。 若要获取访问权限，请与 Microsoft 联系。 对 TaskbarManager、 **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**、 **[TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 和**[TaskbarManager](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** 的 API 调用将失败，并出现 "拒绝访问" 异常。 不允许应用在没有权限的情况下使用此 API，API 定义随时可能会更改。

使用[ApiInformation. IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_)方法确定是否存在 api。 然后使用**[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** api 尝试对 API 进行解锁。

```csharp
if (ApiInformation.IsMethodPresent("Windows.UI.Shell.TaskbarManager", "RequestPinSecondaryTileAsync"))
{
    // API present!
    // Unlock the pin to taskbar feature
    var result = LimitedAccessFeatures.TryUnlockFeature(
        "com.microsoft.windows.secondarytilemanagement",
        "<tokenFromMicrosoft>",
        "<publisher> has registered their use of com.microsoft.windows.secondarytilemanagement with Microsoft and agrees to the terms of use.");

    // If unlock succeeded
    if ((result.Status == LimitedAccessFeatureStatus.Available) ||
        (result.Status == LimitedAccessFeatureStatus.AvailableWithoutToken))
    {
        // Continue
    }
    else
    {
        // Don't show pin to taskbar button or call any of the below APIs
    }
}

else
{
    // Don't show pin to taskbar button or call any of the below APIs
}
```


## <a name="2-get-the-taskbarmanager-instance"></a>2. 获取 TaskbarManager 实例

Windows 应用可在各种设备上运行;并不是所有的都支持任务栏。 现在，仅桌面设备支持任务栏。 此外，还可能会出现任务栏。 若要检查任务栏当前是否存在，请调用**[TaskbarManager. adaptivesourcemanager.getdefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** 方法，并检查返回的实例是否不为 null。 如果任务栏不存在，请勿显示 "固定" 按钮。

建议在单个操作的持续时间内（例如，固定，然后在下一次需要执行其他操作时获取一个新实例）进行保存。

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();

if (taskbarManager != null)
{
    // Continue
}
else
{
    // Taskbar not present, don't display a pin button
}
```


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. 检查磁贴当前是否固定到任务栏

如果磁贴已固定，则应改为显示 "取消固定" 按钮。 您可以使用**[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 方法来检查您的磁贴当前是否处于固定状态（用户随时都可以对其进行解锁）。 在此方法中，传递要知道的磁贴的**TileId** 。

```csharp
if (await taskbarManager.IsSecondaryTilePinnedAsync("myTileId"))
{
    // The tile is already pinned. Display the unpin button.
}

else 
{
    // The tile is not pinned. Display the pin button.
}
```


## <a name="4-check-whether-pinning-is-allowed"></a>4. 检查是否允许锁定

可以通过组策略禁用锁定任务栏。 [TaskbarManager. IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed)属性允许你检查是否允许锁定。

当用户单击你的 pin 按钮时，你应该选中此属性，如果该属性为 false，则应显示消息对话框，通知用户此计算机上不允许锁定。

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager == null)
{
    // Display message dialog informing user that taskbar is no longer present, and then hide the button
}

else if (taskbarManager.IsPinningAllowed == false)
{
    // Display message dialog informing user pinning is not allowed on this machine
}

else
{
    // Continue pinning
}
```


## <a name="5-construct-and-pin-your-tile"></a>5. 构造和固定磁贴

用户已单击您的 "pin" 按钮，您已确定这些 Api 存在、存在任务栏且允许锁定 .。。固定时间！

首先，构造辅助磁贴，就像固定到开始时所做的一样。 可以通过读取[Pin 辅助磁贴来启动来](secondary-tiles-pinning.md)了解有关辅助磁贴属性的详细信息。 但是，在固定到任务栏时，除了之前所需的属性之外，还需要 Square44x44Logo （这是任务栏使用的徽标）。 否则，将引发异常。

然后，将磁贴传递到**[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** 方法。 由于这是受限访问权限，因此不会显示确认对话框，也不需要 UI 线程。 但在未来，如果超出限制访问权限，调用方不会使用有限访问权限，将收到一个对话框，并要求使用 UI 线程。

```csharp
// Initialize the tile (all properties below are required)
SecondaryTile tile = new SecondaryTile("myTileId");
tile.DisplayName = "PowerPoint 2016 (Remote)";
tile.Arguments = "app=powerpoint";
tile.VisualElements.Square44x44Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square44x44Logo.png");
tile.VisualElements.Square150x150Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square150x150Logo.png");

// Pin it to the taskbar
bool isPinned = await taskbarManager.RequestPinSecondaryTileAsync(tile);
```

此方法返回一个布尔值，该值指示磁贴是否现在固定到任务栏。 如果磁贴已固定，则该方法将更新现有磁贴并返回 true。 如果不允许固定或不支持任务栏，则方法返回 false。


## <a name="enumerate-tiles"></a>枚举磁贴

若要查看创建的所有磁贴，并将其固定到某个位置（启动、任务栏或两者），请使用**[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**。 随后可以检查这些磁贴是否固定到任务栏和/或 "开始"。 如果不支持该图面，则这些方法返回 false。

```csharp
var taskbarManager = TaskbarManager.GetDefault();
var startScreenManager = StartScreenManager.GetDefault();

// Look through all tiles
foreach (SecondaryTile tile in await SecondaryTile.FindAllAsync())
{
    if (taskbarManager != null && await taskbarManager.IsSecondaryTilePinnedAsync(tile.TileId))
    {
        // Tile is pinned to the taskbar
    }

    if (startScreenManager != null && await startScreenManager.ContainsSecondaryTileAsync(tile.TileId))
    {
        // Tile is pinned to Start
    }
}
```


## <a name="update-a-tile"></a>更新磁贴

若要更新已固定的磁贴，可以使用[**SecondaryTile. UpdateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync)方法，如[更新辅助磁贴](secondary-tiles-pinning.md#updating-a-secondary-tile)中所述。


## <a name="unpin-a-tile"></a>取消固定磁贴

如果磁贴当前已固定，则应用应提供取消固定按钮。 若要取消固定磁贴，只需调用**[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**，传入辅助磁贴的**TileId**即可取消固定。

此方法返回一个布尔值，该值指示磁贴是否不再固定到任务栏。 如果第一次未固定磁贴，还会返回 true。 如果不允许取消固定，则返回 false。

如果磁贴仅固定到任务栏，这将删除该磁贴，因为它不再固定在任何位置。

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>删除磁贴

若要从任何位置取消固定磁贴（启动、任务栏），请使用**[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** 方法。

这适用于用户固定的内容不再适用的情况。 例如，如果你的应用程序允许你将笔记本固定到 "开始" 和 "任务栏"，然后用户删除笔记本，则只需删除与笔记本关联的磁贴。

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>仅从开始处取消固定

如果只是想要在将辅助磁贴置于任务栏上时从该磁贴取消固定该磁贴，则可以调用**[StartScreenManager. TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** 方法。 如果磁贴不再固定到任何其他表面，这同样会删除该磁贴。

此方法返回一个布尔值，该值指示磁贴是否不再固定到开始处。 如果第一次未固定磁贴，还会返回 true。 如果不允许取消固定或不支持启动，则返回 false。

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>资源

* [TaskbarManager 类](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [固定辅助磁贴以开始](secondary-tiles-pinning.md)
