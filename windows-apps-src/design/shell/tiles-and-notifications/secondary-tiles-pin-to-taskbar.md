---
Description: Learn how to pin secondary tiles to taskbar.
title: 固定到任务栏的辅助磁贴
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: windows 10，uwp，固定到任务栏，辅助磁贴，固定到任务栏，辅助磁贴快捷方式
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933171"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>固定到任务栏的辅助磁贴

就像固定到开始菜单的辅助磁贴，你可以辅助磁贴固定到任务栏，使你的用户在你的应用内快速访问内容。

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **有限访问 API**： 此 API 是有限的访问权限功能。 若要使用此 API，请联系[taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar)。

> **需要 2018 年 10 月更新**： 必须面向 SDK 17763 且必须运行版本 17763 或更高版本到固定到任务栏。


## <a name="guidance"></a>指导

辅助磁贴提供了一种一致、 高效的方式，让用户直接访问应用中的特定区域。 虽然用户可以选择在""辅助磁贴固定到任务栏，但在应用中的可固定区域由开发人员决定。 有关更多指南，请参阅[辅助磁贴的指南](secondary-tiles-guidance.md)。


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1.确定 API 是否存在和解锁受限访问

较旧的设备没有任务栏固定 Api （如果你面向较早版本的 Windows 10）。 因此，你不应不支持的固定这些设备上显示的固定按钮。

此外，此功能处于锁定状态下有限访问。 若要获取访问权限，请联系 Microsoft。 **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**、 **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)**，和**[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** API 调用将失败并拒绝访问异常。 不允许应用使用未经许可情况下，此 API，API 定义可能随时更改。

使用[ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_)方法来确定 Api 是否存在。 然后使用**[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** API 尝试解锁该 API。

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


## <a name="2-get-the-taskbarmanager-instance"></a>2.获取 TaskbarManager 实例

UWP 应用可以在各种设备上运行；并非所有设备都支持任务栏。 现在，仅桌面设备支持任务栏。 此外，任务栏存在可能来自并转。 若要检查是否任务栏当前是否存在、 调用**[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** 方法并检查返回的实例不是 null。 如果任务栏不存在，则不显示 pin 的按钮。

我们建议按住到该实例上的单个操作，如固定，然后抓取的新实例的下次你需要执行其他操作的持续时间。

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3.检查你的磁贴当前是否已固定到任务栏

如果你的磁贴已经固定，你应改为显示取消固定按钮。 你可以使用**[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 方法来检查当前是否已固定磁贴 （用户可以取消固定它在任何时间）。 在此方法中，传递你希望了解该磁贴**TileId**是否已固定。

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


## <a name="4-check-whether-pinning-is-allowed"></a>4.检查是否允许固定

可以通过组策略禁用固定到任务栏。 [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed)属性允许你检查是否允许固定。

当用户单击你的 pin 的按钮时，你应检查此属性，并且如果它为 false，你应该会显示消息对话框通知允许固定，不在此计算机的用户。

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


## <a name="5-construct-and-pin-your-tile"></a>5.构造和固定磁贴

在用户单击你的 pin 的按钮，并已确定 Api 是否存在、 任务栏是否存在，并且允许固定，...固定时间 ！

首先，就像固定到开始菜单时构造的辅助磁贴。 你可以通过阅读[到开始菜单固定辅助磁贴](secondary-tiles-pinning.md)来了解有关辅助磁贴属性的详细信息。 但是，当固定到任务栏，除了之前所需的属性，Square44x44Logo （这是由任务栏的徽标） 还需要。 否则，将引发异常。

然后，将在磁贴传递给**[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** 方法。 由于这是下访问受到限制，这不会显示确认对话框，并不需要的 UI 线程。 但将来这打开时之外访问受到限制，未使用受限访问将收到一个对话框，并需要使用 UI 线程的调用方。

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

此方法返回一个布尔值，指示你的磁贴现在是否已固定到任务栏。 如果你的磁贴已固定，此方法更新现有的磁贴，并将返回 true。 如果未允许固定或任务栏不受支持，该方法将返回 false。


## <a name="enumerate-tiles"></a>枚举磁贴

若要查看你创建的某处仍固定的所有磁贴 （开始菜单、 任务栏，或两者），请使用**[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**。 随后，你可以检查这些磁贴是否已固定到任务栏和/或开始菜单。 如果 surface 不受支持，这些方法将返回 false。

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

若要更新已固定的磁贴，你可以使用[**SecondaryTile.UpdateAsync**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync)方法[更新辅助磁贴](secondary-tiles-pinning.md#updating-a-secondary-tile)中所述。


## <a name="unpin-a-tile"></a>取消固定磁贴

如果磁贴当前已固定，你的应用应提供取消固定按钮。 若要取消固定磁贴，只需调用**[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**，在辅助磁贴你想要取消固定**TileId**中传递。

此方法返回一个布尔值，指示是否已不再将你的磁贴固定到任务栏。 如果未在第一时间固定磁贴，这也会返回 true。 如果取消固定不允许，则返回 false。

如果你的磁贴仅已固定到任务栏，这将删除磁贴，因为它不会再固定任意位置。

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>删除磁贴

如果你想要取消固定磁贴从任意位置 （开始菜单、 任务栏），使用**[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** 方法。

这是合适的情况下，用户固定的内容将不再适用。 例如，如果你的应用可以固定到开始菜单和任务栏，笔记本，然后用户删除笔记本，你应只需删除笔记本与相关联的磁贴。

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>取消固定仅从开始菜单

如果你仅想要取消时将其保留在任务栏上固定辅助磁贴与开始菜单，你可以调用**[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** 方法。 这同样将删除磁贴，如果它不会再固定到任何其他表面。

此方法返回一个布尔值，指示你的磁贴是否不再固定到开始菜单。 如果未在第一时间固定磁贴，这也会返回 true。 如果取消固定不允许或开始菜单不受支持，这将返回 false。

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>资源

* [TaskbarManager 类](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [固定到开始菜单的辅助磁贴](secondary-tiles-pinning.md)