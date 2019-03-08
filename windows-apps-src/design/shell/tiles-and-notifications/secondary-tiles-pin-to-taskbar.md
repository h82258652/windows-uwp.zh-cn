---
Description: 了解如何将固定到任务栏的辅助磁贴。
title: 将辅助的磁贴固定到任务栏
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: windows 10，uwp，固定到任务栏，辅助磁贴固定到任务栏，辅助磁贴快捷方式
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591972"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>将辅助的磁贴固定到任务栏

就像固定到开始辅助磁贴，可以固定到任务栏，辅助磁贴使用户在应用中快速访问内容。

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **有限的访问权限 API**:此 API 是有限的访问权限功能。 若要使用此 API，请联系[ taskbarsecondarytile@microsoft.com ](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar)。

> **需要 2018 年 10 月更新**:您必须为目标 SDK 17763 和运行生成 17763 或更高版本将其固定到任务栏。


## <a name="guidance"></a>指南

辅助磁贴提供一致、 高效的方式，用户可以直接访问应用内的特定区域。 尽管用户选择""辅助磁贴固定到任务栏，由开发人员确定在应用中可固定的区域。 有关更多指导，请参阅[辅助磁贴指南](secondary-tiles-guidance.md)。


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1.确定是否存在 API 和解锁有限访问权限

较旧的设备没有任务栏固定 Api （如果你面向的较旧版本的 Windows 10）。 因此，您不应在不支持固定的这些设备上显示图钉按钮。

此外，此功能已被锁定在有限访问权限。 若要获取访问权限，请联系 Microsoft。 API 调用 **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**，  **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)**，和**[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** 将失败并出现拒绝访问异常。 应用程序不能使用此 API 而无需权限，并随时可能更改的 API 定义。

使用[ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_)方法，以确定是否存在 Api。 然后使用**[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** 尝试解锁 API 的 API。

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

UWP 应用可以在各种设备上运行；并非所有设备都支持任务栏。 现在，仅桌面设备支持任务栏。 此外，在任务栏的状态可能会出现并转。 若要检查是否存在当前任务栏，请调用**[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** 方法，并检查返回的实例不为 null。 如果任务栏不存在，则不显示图钉按钮。

我们建议保存到实例上的单个操作中，等固定，然后获取的下次需要执行其他操作的新实例的持续时间。

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3.检查是否当前将磁贴固定到任务栏

如果你的磁贴已固定，应改为显示一个取消固定按钮。 可以使用**[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** 方法来检查是否当前固定磁贴 （用户可取消固定它在任何时候）。 在这种方法，传递**TileId**你想要知道的磁贴已固定。

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

可以通过组策略禁用固定到任务栏。 [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed)属性，可以检查是否允许固定。

当用户单击图钉按钮时，应检查此属性，而如果为 false，应显示一个消息对话框，通知用户在此计算机上不允许将固定的。

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


## <a name="5-construct-and-pin-your-tile"></a>5.构造并将固定磁贴

用户单击固定按钮的位置，并且您已确定的 Api 是存在、 任务栏存在，且允许固定...pin 的时间 ！

首先，构建了辅助磁贴固定到开始时一样。 可以通过阅读了解有关辅助磁贴属性的详细信息[将辅助的磁贴固定到开始](secondary-tiles-pinning.md)。 但是，在固定到任务栏，除了以前所需属性时，还需要 Square44x44Logo （这是由任务栏的徽标）。 否则，将引发异常。

然后，将传递到磁贴**[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** 方法。 由于这是在访问受限的这将不会显示一个确认对话框，并不需要 UI 线程。 但在将来时这会打开，超出有限访问权限，调用方不利用有限访问权限，将收到一个对话框，并需要使用 UI 线程。

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

此方法返回一个布尔值，该值指示是否在磁贴现在锁定至任务栏。 如果已固定磁贴，该方法将更新现有磁贴，并返回 true。 如果固定应用程序不允许或任务栏不受支持，该方法返回 false。


## <a name="enumerate-tiles"></a>枚举磁贴

若要查看所有创建的磁贴和是否仍固定某处 （开始、 任务栏，或两者），请使用 **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**。 随后可以检查这些磁贴已固定到任务栏和/或开始。 如果不支持在图面，这些方法返回 false。

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

若要更新已固定的磁贴，可以使用[ **SecondaryTile.UpdateAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync)方法如中所述[更新辅助磁贴](secondary-tiles-pinning.md#updating-a-secondary-tile)。


## <a name="unpin-a-tile"></a>取消固定磁贴

如果当前固定该磁贴，您的应用程序应提供一个取消固定按钮。 若要取消固定磁贴，只需调用 **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**，并传入**TileId**辅助磁贴想取消固定。

此方法返回一个布尔值，该值指示是否在磁贴不再固定到任务栏。 如果不第一个位置中固定磁贴，这也返回 true。 如果取消固定应用程序不允许，则返回 false。

如果你的磁贴仅已固定到任务栏，这将删除磁贴，因为它不再固定任何位置。

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>删除磁贴

如果你想要取消固定磁贴从任意位置 （从开始，任务栏），使用**[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** 方法。

这是适用于其中的内容固定用户不再是适用的情况。 例如，如果您的应用程序，可以将固定启动和任务栏中，notebook，然后用户删除笔记本，只需应删除与笔记本配合关联的磁贴。

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>只能从开始取消固定

如果只想要保留在任务栏上的同时取消固定辅助磁贴从开始，可以调用**[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** 方法。 类似地，如果它不再被固定到任何其他表面，这将删除该磁贴。

此方法返回一个布尔值，该值指示是否在磁贴不再固定到开始。 如果不第一个位置中固定磁贴，这也返回 true。 如果取消固定应用程序不允许或开始不受支持，这将返回 false。

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>资源

* [TaskbarManager 类](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [将辅助的磁贴固定到开始](secondary-tiles-pinning.md)