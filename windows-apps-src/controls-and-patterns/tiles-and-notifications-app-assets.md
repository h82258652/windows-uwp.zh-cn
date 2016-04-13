---
应用图标资源（它以各种形式出现在整个 Windows 10 操作系统中）是通用 Windows 平台 (UWP) 应用的调用卡。
磁贴和图标资源
ms.assetid: D6CE21E5-2CFA-404F-8679-36AA522206C7
磁贴和图标资源
template: detail.hbs
---

# 磁贴和图标资源指南


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


应用图标资源（它以各种形式出现在整个 Windows 10 操作系统中）是通用 Windows 平台 (UWP) 应用的调用卡。 这些指南详细介绍应用图标资源在系统中的显示位置，并提供有关如何创建最完美图标的深入设计提示。

![Windows 10“开始”菜单和磁贴](images/assetguidance01.jpg)

## <span id="Adaptive_scaling"> </span> <span id="adaptive_scaling"> </span> <span id="ADAPTIVE_SCALING"> </span>自适应缩放


首先，简要概述自适应缩放可以更好地了解缩放如何处理资源。 Windows 10 引入了现有缩放模型的演变。 除了缩放矢量内容外，还有一系列统一的比例系数，用于在各种屏幕大小和显示分辨率中为 UI 元素提供一致的大小。 比例系数还与其他操作系统（如 iOS 和 Android）的比例系数兼容，这使在这些平台之间共享资源变得更容易。

应用商店选择要下载的资源在一定程序上取决于设备的 DPI。 仅下载最匹配设备的资源。

## <span id="Tile_elements"> </span> <span id="tile_elements"> </span> <span id="TILE_ELEMENTS"> </span>磁贴元素


“开始”菜单磁贴的基本组件由背板、图标、品牌栏、边距和应用标题构成：

![磁贴元素细分](images/assetguidance02.png)

位于磁贴底部的品牌栏是显示应用名称、商标和计数器（如果使用）的位置：

![磁贴中的品牌栏](images/assetguidance03.png)

品牌栏的高度取决于显示它的设备的比例系数：

| 比例系数 | 像素 |
|--------------|--------|
| 100%         | 32     |
| 125%         | 40     |
| 150%         | 48     |
| 200%         | 64     |
| 400%         | 128    |

 

系统设置磁贴边距，并且无法修改。 大部分内容都显示在边距内，如此例中所示：

![磁贴边距](images/assetguidance04.png)

边距宽度取决于显示它的设备的比例系数：

| 比例系数 | 像素 |
|--------------|--------|
| 100%         | 8      |
| 125%         | 10     |
| 150%         | 12     |
| 200%         | 16     |
| 400%         | 32     |

 

## <span id="Tile_assets"> </span> <span id="tile_assets"> </span> <span id="TILE_ASSETS"></span>磁贴资源


每个磁贴资源的大小与放置它的磁贴大小相同。 可以通过两种不同的资源表示形式标记你的应用磁贴的品牌：

1. 通过填充居中的图标或徽标。 这使背板颜色可隐约显示：

![磁贴和背板](images/assetguidance05.png)

2. 没有填充的全出血品牌磁贴：

![显示全出血的磁贴](images/assetguidance06.png)

为实现跨设备的一致性，每个磁贴大小（小、中、宽和大）都具有其自己的大小调整关系。 为了在磁贴之间实现一致的图标放置，我们针对以下磁贴大小建议几个基本填充指南。 两个紫色叠加相交处的区域表示图标的理想占用。 尽管图标不会一直适合占用，图标的视觉体积也应该大致等于提供的示例。

小磁贴大小调整：

![小磁贴大小调整示例](images/assetguidance07a.png)

中磁贴大小调整：

![中磁贴大小调整示例](images/assetguidance07b.png)

宽磁贴大小调整：

![宽磁贴大小调整示例](images/assetguidance07c.png)

大磁贴大小调整：

![大磁贴大小调整示例](images/assetguidance07d.png)

在此示例中，图标对于磁贴而言太大：

![图标对于磁贴而言太大](images/assetguidance08a.png)

在此示例中，图标对于磁贴而言太小：

![图标对于磁贴而言太小](images/assetguidance08b.png)

以下填充率最适合用于水平或垂直方向的图标。

对于小磁贴，将图标宽度和高度限制为磁贴大小的 66%：

![小磁贴大小调整比率](images/assetguidance09.png)

对于中磁贴，将图标宽度限制为磁贴大小的 66%，将高度限制为 50%。 这可以防止品牌栏中的元素重叠：

![中磁贴大小调整比率](images/assetguidance10.png)

对于宽磁贴，将图标宽度限制为磁贴大小的 66%，将高度限制为 50%。 这可以防止品牌栏中的元素重叠：

![宽磁贴大小调整比率](images/assetguidance11.png)

对于大磁贴，将图标宽度和高度限制为磁贴大小的 50%：

![大磁贴大小比率](images/assetguidance12.png)

某些图标专用于水平或垂直方向，而其他图标具有更复杂形状，使它们无法完全拟合目标尺寸。 居中显示的图标可向一侧加权。 在此情况下，如果图标占据的视觉权重与完全拟合的图标相同，则该图标的一部分可能悬挂在建议的占用之外：

![三个居中的图标](images/assetguidance13.png)

使用全出血资源，将在磁贴的边距和边缘内相交的元素考虑在内。 保留的边距至少占据 16% 的磁贴高度或宽度。 此百分比表示最小磁贴大小的边距宽度的两倍：

![带有边距的全出血磁贴](images/assetguidance14.png)

在此示例中，边距太紧：

![带有过紧边距的全出血磁贴](images/assetguidance15.png)

## <span id="Tile_assets_in_list_views"> </span> <span id="tile_assets_in_list_views"> </span> <span id="TILE_ASSETS_IN_LIST_VIEWS"> </span>列表视图中的磁贴资源


磁贴也可以显示在列表视图中。 显示在列表视图中的磁贴资源大小调整指南与之前所述的磁贴资源略有不同。 本节详细介绍这些大小调整细节。

![列表视图中的磁贴资源](images/assetguidance16.png)

将图标宽度和高度限制为磁贴大小的 75%：

![列表视图磁贴的图标大小调整](images/assetguidance17.png)

对于垂直和水平图标格式，将宽度和高度限制为磁贴大小的 75%：

![列表视图磁贴的图标大小调整](images/assetguidance18.png)

对于重要品牌元素的全出血插图，保留至少 12.5% 的边距：

![列表视图磁贴中的全出血插图](images/assetguidance19.png)

在此示例中，该图标在其磁贴内的大小太大：

![对于磁贴而言太大的图标](images/assetguidance20a.png)

在此示例中，该图标在其磁贴内的大小太小：

![对于磁贴而言太小的图标](images/assetguidance20b.png)

## <span id="Target-based_assets"> </span> <span id="target-based_assets"> </span> <span id="TARGET-BASED_ASSETS"> </span>基于目标的资源


基于目标的资源用于显示在 Windows 任务栏、任务视图、ALT+TAB、贴靠助手和“开始”菜单磁贴的右下角的图标和磁贴。 无需向这些资源添加填充；Windows 会根据需要添加填充。 这些资源应占据最少 16 pixel 的占用空间。 以下是这些资源在 Windows 任务栏上的图标中显示时的示例：

![Windows 任务栏中的资源](images/assetguidance21.png)

尽管默认情况下这些 UI 将使用位于彩色背板顶部的基于目标的资源，但你还可以使用基于目标的未着色资源。 应创建未着色资源，因为它们有可能在各种背景色上显示：

![未着色和着色资源](images/assetguidance22.png)

对于基于目标的资源（比例为 100%），有以下大小建议：

![基于目标的资源大小调整（比例为 100%）](images/assetguidance23.png)

**图标模板应用资源**

图标模板（也称为“IconWithBadge”模板）允许你在磁贴中心显示较小的图像。 Windows 10 的手机版和平板电脑/桌面版都支持该模板。 （在[特殊磁贴模板文章](tiles-and-notifications-special-tile-templates-catalog.md)中了解创建图标磁贴。）

使用图标模板的应用（如消息、手机和应用商店）具有的基于目标的资源可以展示锁屏提醒（带有动态计数器）。 与其他基于目标的资源一样，不需要进行填充。 图标资源不是应用清单的一部分，但是动态磁贴负载的一部分。 缩放资源以使其适合 3:2 比率的容器并居中放置：

![带有和不带有锁屏提醒的资源的大小调整](images/assetguidance24.png)

对于方形资源，在容器内自动居中放置：

![方形资源大小调整（带有和不带有锁屏提醒）](images/assetguidance25.png)

对于非方形资源，会自动进行水平/垂直居中放置并贴靠到容器的宽度/高度：

![非方形资源大小调整（带有和不带有锁屏提醒）](images/assetguidance26a.png)

![非方形资源大小调整（带有和不带有锁屏提醒）](images/assetguidance26b.png)

## <span id="Splash_screen_assets"> </span> <span id="splash_screen_assets"> </span> <span id="SPLASH_SCREEN_ASSETS"> </span>初始屏幕资源


初始屏幕图像可以作为指向图像文件的直接路径或作为资源提供。 通过使用资源引用，你可以提供不同比例的图像，以便 Windows 可以选择适合设备和屏幕分辨率的最佳大小。 你还可以提供辅助功能的高对比度图像和本地化图像，以匹配不同的 UI 语言。

如果你在文本编辑器中打开“Package.appxmanifest”，[**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br211467) 元素会显示为 [**VisualElements**](https://msdn.microsoft.com/library/windows/apps/br211471) 元素的子元素。 清单文件中的默认初始屏幕标记在文本编辑器中的显示如下：

```XML
<uap:SplashScreen Image="Assets\SplashScreen.png" /></code></pre></td>
</tr>
</tbody>
</table>
```

初始屏幕资源会在显示它的任何设备上居中放置：

![调整初始屏幕资源的大小调整](images/assetguidance27.png)

## <span id="High-contrast_assets"> </span> <span id="high-contrast_assets"> </span> <span id="HIGH-CONTRAST_ASSETS"> </span>高对比度资源


高对比度模式将单独的资源集用于高对比度白（白色背景搭配黑色文本）和高对比度黑（黑色背景搭配白色文本）。 如果未向你的应用提供高对比度资源，将使用标准资源。

如果你的应用的标准资源在黑白背景上呈现时提供可接受的查看体验，那么你的应用在高对比度模式下的外观至少是令人满意的。 如果你的标准资源在黑白背景上呈现时不提供可接受的查看体验，请明确包含高对比度资源。 这些示例介绍了两种类型的高对比度资源：

![高对比度比率资源示例](images/assetguidance28.png)

如果你决定提供高对比度资源，需要包含两组 - 黑底白字和白底黑字。 在将这些资源包含在你的程序包后，可以为黑底白字资源创建“contrast-black”文件夹，为白底黑字资源创建“contrast-white”文件夹。

## <span id="Asset_size_tables"> </span> <span id="asset_size_tables"> </span> <span id="ASSET_SIZE_TABLES"> </span>资源大小表


我们强烈建议你至少为 100、200 和 400 比例系数提供资源。 为所有比例系数提供资源将提供最佳的用户体验。

**基于比例的资源**

| 类别             | 元素名称      | 100% 缩放比例 | 125% 缩放比例 | 150% 缩放比例 | 200% 缩放比例 | 400% 缩放比例 |
|----------------------|-------------------|---------------|---------------|---------------|---------------|---------------|
| 小                | Square71x71Logo   | 71x71         | 89x89         | 107x107       | 142x142       | 284x284       |
| 中               | Square150x150Logo | 150x150       | 188x188       | 225x225       | 300x300       | 600x600       |
| 宽                 | Square310x150Logo | 310x150       | 388x188       | 465x225       | 620x300       | 1240x600      |
| 大（仅限桌面） | Square310x310Logo | 310x310       | 388x388       | 465x465       | 620x620       | 1240x1240     |
| 应用列表（图标）      | Square44x44Logo   | 44x44         | 55x55         | 66x66         | 88x88         | 176x176       |

 

**基于缩放的资源的文件名示例**

| 类别             | 元素名称      | 100% 缩放比例                  | 125% 缩放比例                  | 150% 缩放比例                  |
|----------------------|-------------------|--------------------------------|--------------------------------|--------------------------------|
| 小                | Square71x71Logo   | AppNameSmallTile.scale-100.png | AppNameSmallTile.scale-125.png | AppNameSmallTile.scale-150.png |
| 中               | Square150x150Logo | AppNameMedTile.scale-100.png   | AppNameMedTile.scale-125.png   | AppNameMedTile.scale-150.png   |
| 宽                 | Square310x150Logo | AppNameWideTile.scale-100.png  | AppNameWideTile.scale-125.png  | AppNameWideTile.scale-150.png  |
| 大（仅限桌面） | Square310x310Logo | AppNameLargeTile.scale-100.png | AppNameLargeTile.scale-125.png | AppNameLargeTile.scale-150.png |
| 应用列表（图标）      | Square44x44Logo   | AppNameLargeTile.scale-100.png | AppNameLargeTile.scale-125.png | AppNameLargeTile.scale-150.png |

 

| 类别             | 元素名称      | 200% 缩放比例                  | 400% 缩放比例                  |
|----------------------|-------------------|--------------------------------|--------------------------------|
| 小                | Square71x71Logo   | AppNameSmallTile.scale-200.png | AppNameSmallTile.scale-400.png |
| 中               | Square150x150Logo | AppNameMedTile.scale-200.png   | AppNameMedTile.scale-400.png   |
| 宽                 | Square310x150Logo | AppNameWideTile.scale-200.png  | AppNameWideTile.scale-400.png  |
| 大（仅限桌面） | Square310x310Logo | AppNameLargeTile.scale-200.png | AppNameLargeTile.scale-400.png |
| 应用列表（图标）      | Square44x44Logo   | AppNameLargeTile.scale-200.png | AppNameLargeTile.scale-400.png |

 

**基于目标的资源**

跨多个比例系数使用基于目标的资源。 基于目标的资源的元素名称是 **Square44x44Logo**。 我们强烈建议至少提交以下资源：

16x16、24x24、32x32、48x48、256x256

下表列出了所有基于目标的资源大小和相应的文件名示例：

| 资源大小 | 文件名示例                 |
|------------|-----------------------------------|
| 16x16\*    | AppNameAppList.targetsize-16.png  |
| 24x24\*    | AppNameAppList.targetsize-24.png  |
| 32x32\*    | AppNameAppList.targetsize-32.png  |
| 48x48\*    | AppNameAppList.targetsize-48.png  |
| 256x256\*  | AppNameAppList.targetsize-256.png |
| 20x20      | AppNameAppList.targetsize-20.png  |
| 30x30      | AppNameAppList.targetsize-30.png  |
| 36x36      | AppNameAppList.targetsize-36.png  |
| 40x40      | AppNameAppList.targetsize-40.png  |
| 60x60      | AppNameAppList.targetsize-60.png  |
| 64x64      | AppNameAppList.targetsize-64.png  |
| 72x72      | AppNameAppList.targetsize-72.png  |
| 80x80      | AppNameAppList.targetsize-80.png  |
| 96x96      | AppNameAppList.targetsize-96.png  |

 

\* 提交这些资源大小作为基线

## <span id="Asset_types"> </span> <span id="asset_types"> </span> <span id="ASSET_TYPES"> </span>资源类型


此处列出所有资源类型、用途和建议的文件名。

**磁贴资源**

-   通常在“开始”菜单上使用居中放置的资源来展示你的应用。
-   文件名格式：\*Tile.scale-\*.PNG
-   受影响的应用：每个 UWP 应用
-   使用：
    -   默认“开始”磁贴（桌面和移动）
    -   操作中心（桌面和移动）
    -   任务切换程序（移动）
    -   共享选取器（移动）
    -   选取器（移动）
    -   应用商店

**着色的可扩展列表资源**

-   这些资源用于请求比例系数的图面。 资源通过系统进行着色，或附带其自己的背景色（如果应用包含该背景色）。
-   文件名格式：\*AppList.scale-\*.PNG
-   受影响的应用：每个 UWP 应用
-   使用：
    -   “开始”菜单所有应用列表（桌面）
    -   “开始”菜单常用列表（桌面）
    -   任务管理器（桌面）
    -   Cortana 搜索结果
    -   “开始”菜单所有应用列表（移动）
    -   设置

**着色的目标大小列表资源**

-   这些是固定的资源大小，不会随比例进行缩放。 主要用于传统体验。 系统会检查资源。
-   文件名格式：\*AppList.targetsize-\*.PNG
-   受影响的应用：每个 UWP 应用
-   使用：
    -   “开始”菜单跳转列表（桌面）
    -   磁贴下角的“开始”菜单（桌面）
    -   快捷方式（桌面）
    -   控制面板（桌面）

**未着色的目标大小列表资源**

-   这些资源未通过系统进行着色或缩放。
-   文件名格式：\*AppList.targetsize-\*\_altform-unplated.PNG
-   受影响的应用：每个 UWP 应用
-   使用：
    -   任务栏和任务栏缩略图（桌面）
    -   任务栏跳转列表
    -   任务视图
    -   ALT+TAB

**文件扩展名资源**

-   这些是特定于文件扩展名的资源。 它们显示在文件资源管理器中的 Win32 样式文件关联图标旁，并且必须独立于主题。 大小调整在桌面平台和移动平台上是不同的。
-   文件名格式：\*LogoExtensions.targetsize-\*.PNG
-   受影响的应用：音乐、视频、照片、Microsoft Edge、Microsoft Office
-   使用：
    -   文件资源管理器
    -   Cortana
    -   各种 UI 图面（桌面）

**初始屏幕**

-   显示在你的应用的初始屏幕上的资源。 在桌面平台和移动平台上自动缩放。
-   文件名格式：\*SplashScreen.screen-100.PNG
-   受影响的应用：每个 UWP 应用
-   使用：
    -   应用的初始屏幕

**图标磁贴资源**

-   这些是使用图标模板的应用的资源。
-   文件名格式：不适用
-   受影响的应用：Messaging、手机、应用商店等
-   使用：
    -   图标磁贴

\[本文包含特定于通用 Windows 平台 (UWP) 应用和 Windows 10 的信息。 有关 Windows 8.1 指南，请下载 [Windows 8.1 指南 PDF](https://go.microsoft.com/fwlink/p/?linkid=258743)。\]

## <span id="related_topics"> </span>相关主题



* [特殊磁贴模板](tiles-and-notifications-special-tile-templates-catalog.md)
 

 






<!--HONumber=Mar16_HO1-->


