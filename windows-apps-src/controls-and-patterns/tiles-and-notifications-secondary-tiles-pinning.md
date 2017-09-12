---
author: anbare
Description: "了解从 UWP 应用固定辅助磁贴的方法。"
title: "固定辅助磁贴"
label: Pin secondary tiles
template: detail.hbs
ms.author: wdg-dev-content
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, 辅助磁贴, 固定, 快速入门, 代码示例, 示例, secondarytile"
ms.openlocfilehash: 9525dc91ddd40265d7c7fa9ff8e73509ba3029ab
ms.sourcegitcommit: 6396a69aab081f5c7a9a59739c83538616d3b1c7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/30/2017
---
# <a name="pin-secondary-tiles"></a>固定辅助磁贴
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

本主题指导你完成为 UWP 应用创建辅助磁贴并将其固定到“开始”菜单的步骤。

![辅助磁贴的屏幕截图](images/secondarytiles.png)

若要了解有关辅助磁贴的详细信息，请参阅[辅助磁贴概述](tiles-and-notifications-secondary-tiles.md)。


## <a name="add-namespace"></a>添加命名空间

Windows.UI.StartScreen 命名空间包括 SecondaryTile 类。

```csharp
using Windows.UI.StartScreen;
```


## <a name="initialize-the-secondary-tile"></a>初始化辅助磁贴

辅助磁贴由一些主要组件构成...

* **TileId**：用于标识其他辅助磁贴中的磁贴的唯一标识符。
* **DisplayName**：想要显示在磁贴上的名称。
* **Arguments**：想要在用户单击磁贴时传递回应用的参数。
* **Square150x150Logo**：显示在中等大小磁贴上的所需徽标（如果未提供小徽标，则会按较小大小的磁贴来调整大小）。

你**必须**为所有上述属性提供初始化值，否则将引起异常。

你可以使用各种构造函数，但是如果使用包含 tileId、displayName、arguments、square150x150Logo 和 desiredSize 的构造函数，则有助于确保设置所有必需的属性。

```csharp
// Construct a unique tile ID, which you will need to use later for updating the tile
string tileId = "City" + zipCode;

// Use a display name you like
string displayName = cityName;

// Provide all the required info in arguments so that when user
// clicks your tile, you can navigate them to the correct content
string arguments = "action=viewCity&zipCode=" + zipCode;

// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    tileId,
    displayName,
    arguments,
    new Uri("ms-appx:///Assets/CityTiles/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="optional-add-support-for-larger-tile-sizes"></a>可选：添加对较大磁贴大小的支持

如果你打算在辅助磁贴上显示大量磁贴通知，则可能需要允许用户将其磁贴调宽或调大，以便他们能够查看更多内容。

若要启用宽磁贴大小和大磁贴大小，你需要提供 *Wide310x150Logo* 和 *Square310x310Logo*。 此外，如果可能，应为小磁贴大小提供 *Square71x71Logo*（否则我们将针对小磁贴减小所需的 Square150x150Logo）。

你还可以提供唯一的 *Square44x44Logo*，（可选）有通知时它会显示在右下角。 如果未提供，则将改用主要磁贴中的 *Square44x44Logo*。

```csharp
// Enable wide and large tile sizes
tile.VisualElements.Wide310x150Logo = new Uri("ms-appx:///Assets/CityTiles/Wide310x150Logo.png");
tile.VisualElements.Square310x310Logo = new Uri("ms-appx:///Assets/CityTiles/Square310x310Logo.png");

// Add a small size logo for better looking small tile
tile.VisualElements.Square71x71Logo = new Uri("ms-appx:///Assets/CityTiles/Square71x71Logo.png");

// Add a unique corner logo for the secondary tile
tile.VisualElements.Square44x44Logo = new Uri("ms-appx:///Assets/CityTiles/Square44x44Logo.png");
```


## <a name="optional-enable-showing-the-display-name"></a>可选：允许显示显示名称

默认情况下将不显示显示名称。 若要在中等/宽/大磁贴上显示显示名称，请添加以下代码。

```csharp
// Show the display name on all sizes
tile.VisualElements.ShowNameOnSquare150x150Logo = true;
tile.VisualElements.ShowNameOnWide310x150Logo = true;
tile.VisualElements.ShowNameOnSquare310x310Logo = true;
```


## <a name="pin-the-secondary-tile"></a>固定辅助磁贴

最后，请求固定磁贴。 请注意，这必须从 UI 线程中调用。 在桌面上，将显示一个对话框，并请求用户确认他们是否想要固定磁贴。

> [!IMPORTANT]
> 如果你的 Windows 桌面应用程序使用的是桌面桥，则必须先执行一个额外步骤，如[从桌面应用程序固定](tiles-and-notifications-secondary-tiles-desktop-pinning.md)中所述

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="check-if-a-secondary-tile-exists"></a>检查是否存在辅助磁贴

如果用户在你已固定到“开始”菜单的应用中访问页面，则将需要改为显示“取消固定”按钮。

因此，在选择要显示的按钮时，你需要首先检查当前是否已固定辅助磁贴。

```csharp
// Check if the secondary tile is pinned
bool isPinned = SecondaryTile.Exists(tileId);

// TODO: Update UI to reflect whether user can either unpin or pin
```


## <a name="unpinning-a-secondary-tile"></a>取消固定辅助磁贴

如果当前已固定磁贴，并且用户单击你的取消固定按钮，则将需要取消固定（删除）磁贴。

```csharp
// Initialize a secondary tile with the same tile ID you want removed
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then unpin the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="updating-a-secondary-tile"></a>更新辅助磁贴

如果你需要更新辅助磁贴上的徽标、显示名称或任何其他内容，可以使用 *RequestUpdateAsync*。

```csharp
// Initialize a secondary tile with the same tile ID you want to update
SecondaryTile tile = new SecondaryTile(tileId);

// Assign ALL properties, including ones you aren't changing

// And then update it
await tile.UpdateAsync();
```


## <a name="enumerating-all-pinned-secondary-tiles"></a>枚举所有固定的辅助磁贴

如果你需要发现用户已固定的所有磁贴，而不是使用 *SecondaryTile.Exists*，则也可以使用 *SecondaryTile.FindAllAsync()*。

```csharp
// Get all secondary tiles
var tiles = await SecondaryTile.FindAllAsync();
```


## <a name="send-a-tile-notification"></a>发送磁贴通知

若要了解如何通过磁贴通知在磁贴上显示丰富的内容，请参阅[发送本地磁贴通知](tiles-and-notifications-sending-a-local-tile-notification.md)。


## <a name="related"></a>相关

* [辅助磁贴概述](tiles-and-notifications-secondary-tiles.md)
* [辅助磁贴指南](tiles-and-notifications-secondary-tiles-guidance.md)
* [磁贴资源](tiles-and-notifications-app-assets.md)
* [磁贴内容文档](tiles-and-notifications-create-adaptive-tiles.md)
* [发送本地磁贴通知](tiles-and-notifications-sending-a-local-tile-notification.md)