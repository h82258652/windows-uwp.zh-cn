---
author: mijacobs
Description: "应用图标资源（它以各种形式出现在整个 Windows 10 操作系统中）是通用 Windows 平台 (UWP) 应用的调用卡。"
title: "磁贴和图标资源"
ms.assetid: D6CE21E5-2CFA-404F-8679-36AA522206C7
label: Tile and icon assets
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 54ad78d5799a96ddcec7b060704ee198e0bf8db5
ms.sourcegitcommit: 9a1310468970c8d1ade0fb200126dff56ea8c9e1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/14/2017
---
# <a name="guidelines-for-tile-and-icon-assets"></a>磁贴和图标资源指南

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


应用图标资源（它以各种形式出现在整个 Windows 10 操作系统中）是通用 Windows 平台 (UWP) 应用的调用卡。 这些指南详细介绍应用图标资源在系统中的显示位置，并提供有关如何创建最完美图标的深入设计提示。

![Windows 10“开始”菜单和磁贴](images/assetguidance01.jpg)

## <a name="adaptive-scaling"></a>自适应缩放


首先，简要概述自适应缩放可以更好地了解缩放如何与资源一起使用。 Windows 10 引入了现有缩放模型的演变。 除了缩放矢量内容外，还有一系列统一的比例系数，用于在各种屏幕大小和显示分辨率中为 UI 元素提供一致的大小。 比例系数还与其他操作系统（如 iOS 和 Android）的比例系数兼容，这使在这些平台之间共享资源变得更容易。

应用商店选择要下载的资源在一定程序上取决于设备的 DPI。 仅下载最匹配设备的资源。

## <a name="tile-elements"></a>磁贴元素


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

 

## <a name="tile-assets"></a>磁贴资源


每个磁贴资源的大小与在其上放置的磁贴大小相同。 可以通过两种不同的资源表示形式标记你的应用磁贴的品牌：

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

对于大磁贴，将图标宽度限制为磁贴大小的 66%，将高度限制为 50%：

![大磁贴大小比率](images/assetguidance12.png)

某些图标专用于水平或垂直方向，而其他图标具有更复杂形状，使它们无法完全拟合目标尺寸。 居中显示的图标可向一侧加权。 在此情况下，如果图标占据的视觉权重与完全拟合的图标相同，则该图标的一部分可能悬挂在建议的占用之外：

![三个居中的图标](images/assetguidance13.png)

使用全出血资源，将在磁贴的边距和边缘内相交的元素考虑在内。 保留的边距至少占据 16% 的磁贴高度或宽度。 此百分比表示最小磁贴大小的边距宽度的两倍：

![带有边距的全出血磁贴](images/assetguidance14.png)

在此示例中，边距太紧：

![带有过紧边距的全出血磁贴](images/assetguidance15.png)

## <a name="tile-assets-in-list-views"></a>列表视图中的磁贴资源


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

## <a name="target-based-assets"></a>基于目标的资源


基于目标的资源是指显示在以下位置的图标和磁贴：Windows 任务栏、任务视图、ALT+TAB、贴靠助手和“开始”磁贴的右下角。 无需向这些资源添加填充；Windows 会根据需要添加填充。 这些资源应占据最少 16 pixel 的占用空间。 以下是这些资源在 Windows 任务栏上的图标中显示时的示例：

![Windows 任务栏中的资源](images/assetguidance21.png)

尽管默认情况下这些 UI 将使用位于彩色背板顶部的基于目标的资源，但你还可以使用基于目标的未着色资源。 应创建未着色资源，因为它们有可能在各种背景色上显示：

![未着色和着色资源](images/assetguidance22.png)

对于基于目标的资源（比例为 100%），有以下大小建议：

![基于目标的资源大小调整（比例为 100%）](images/assetguidance23.png)

## <a name="splash-screen-assets"></a>初始屏幕资源


初始屏幕图像可以作为指向图像文件的直接路径或作为资源提供。 通过使用资源引用，你可以提供不同比例的图像，以便 Windows 可以选择适合设备和屏幕分辨率的最佳大小。 你还可以提供辅助功能的高对比度图像和本地化图像，以匹配不同的 UI 语言。

如果你在文本编辑器中打开“Package.appxmanifest”，[**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br211467) 元素会显示为 [**VisualElements**](https://msdn.microsoft.com/library/windows/apps/br211471) 元素的子元素。 清单文件中的默认初始屏幕标记在文本编辑器中的显示如下：

```XML
<uap:SplashScreen Image="Assets\SplashScreen.png" /></code></pre></td>
</tr>
</tbody>
</table>
```

初始屏幕资源会在显示它的任何设备上居中放置：

![初始屏幕资源的大小调整](images/assetguidance27.png)

## <a name="high-contrast-assets"></a>高对比度资源


高对比度模式将单独的资源集用于高对比度白（白色背景搭配黑色文本）和高对比度黑（黑色背景搭配白色文本）。 如果未向你的应用提供高对比度资源，将使用标准资源。

如果你的应用的标准资源在黑白背景上呈现时提供可接受的查看体验，那么你的应用在高对比度模式下的外观至少是令人满意的。 如果你的标准资源在黑白背景上呈现时不提供可接受的查看体验，请明确包含高对比度资源。 这些示例介绍了两种类型的高对比度资源：

![高对比度比率资源示例](images/assetguidance28.png)

如果你决定提供高对比度资源，需要包含两组 - 黑底白字和白底黑字。 在将这些资源包含在你的程序包后，可以为上白下黑资源创建“contrast-black”文件夹，并为上黑下白资源创建“contrast-white”文件夹。

## <a name="asset-size-tables"></a>资源大小表


我们强烈建议你至少为 100、200 和 400 比例系数提供资源。 为所有比例系数提供资源将提供最佳的用户体验。

<br/>

<table>
<thead>
<tr><th colspan="3">小磁贴 (Square71x71Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 缩放比例</td>
    <td width="20%">71x71</td>
    <td>Square71x71Logo.scale-100.png</td>
</tr>
<tr>
    <td>125% 缩放比例</td>
    <td>89x89</td>
    <td>Square71x71Logo.scale-125.png</td>
</tr>
<tr>
    <td>150% 缩放比例</td>
    <td>107x107</td>
    <td>Square71x71Logo.scale-150.png</td>
</tr>
<tr>
    <td>200% 缩放比例</td>
    <td>142x142</td>
    <td>Square71x71Logo.scale-200.png</td>
</tr>
<tr>
    <td>400% 缩放比例</td>
    <td>284x284</td>
    <td>Square71x71Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">中等磁贴 (Square150x150Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 缩放比例</td>
    <td width="20%">150x150</td>
    <td>Square150x150Logo.scale-100.png</td>
</tr>
<tr>
    <td>125% 缩放比例</td>
    <td>188x188</td>
    <td>Square150x150Logo.scale-125.png</td>
</tr>
<tr>
    <td>150% 缩放比例</td>
    <td>225x225</td>
    <td>Square150x150Logo.scale-150.png</td>
</tr>
<tr>
    <td>200% 缩放比例</td>
    <td>300x300</td>
    <td>Square150x150Logo.scale-200.png</td>
</tr>
<tr>
    <td>400% 缩放比例</td>
    <td>600x600</td>
    <td>Square150x150Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">宽磁贴 (Wide310x150Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 缩放比例</td>
    <td width="20%">310x150</td>
    <td>Wide310x150Logo.scale-100.png</td>
</tr>
<tr>
    <td>125% 缩放比例</td>
    <td>388x188</td>
    <td>Wide310x150Logo.scale-125.png</td>
</tr>
<tr>
    <td>150% 缩放比例</td>
    <td>465x225</td>
    <td>Wide310x150Logo.scale-150.png</td>
</tr>
<tr>
    <td>200% 缩放比例</td>
    <td>620x300</td>
    <td>Wide310x150Logo.scale-200.png</td>
</tr>
<tr>
    <td>400% 缩放比例</td>
    <td>1240x600</td>
    <td>Wide310x150Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">大磁贴 (Square310x310Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 缩放比例</td>
    <td width="20%">310x310</td>
    <td>Square310x310Logo.scale-100.png</td>
</tr>
<tr>
    <td>125% 缩放比例</td>
    <td>388x388</td>
    <td>Square310x310Logo.scale-125.png</td>
</tr>
<tr>
    <td>150% 缩放比例</td>
    <td>465x465</td>
    <td>Square310x310Logo.scale-150.png</td>
</tr>
<tr>
    <td>200% 缩放比例</td>
    <td>620x620</td>
    <td>Square310x310Logo.scale-200.png</td>
</tr>
<tr>
    <td>400% 缩放比例</td>
    <td>1240x1240</td>
    <td>Square310x310Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">应用列表图标 (Square44x44Logo)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 缩放比例</td>
    <td width="20%">44x44</td>
    <td>Square44x44Logo.scale-100.png</td>
</tr>
<tr>
    <td>125% 缩放比例</td>
    <td>55x55</td>
    <td>Square44x44Logo.scale-125.png</td>
</tr>
<tr>
    <td>150% 缩放比例</td>
    <td>66x66</td>
    <td>Square44x44Logo.scale-150.png</td>
</tr>
<tr>
    <td>200% 缩放比例</td>
    <td>88x88</td>
    <td>Square44x44Logo.scale-200.png</td>
</tr>
<tr>
    <td>400% 缩放比例</td>
    <td>176x176</td>
    <td>Square44x44Logo.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>

<table>
<thead>
<tr><th colspan="3">初始屏幕 (SplashScreen)</th></tr>
</thead>
<tbody>
<tr>
    <td width="20%">100% 缩放比例</td>
    <td width="20%">620x300</td>
    <td>SplashScreen.scale-100.png</td>
</tr>
<tr>
    <td>125% 缩放比例</td>
    <td>775x375</td>
    <td>SplashScreen.scale-125.png</td>
</tr>
<tr>
    <td>150% 缩放比例</td>
    <td>930x450</td>
    <td>SplashScreen.scale-150.png</td>
</tr>
<tr>
    <td>200% 缩放比例</td>
    <td>1240x600</td>
    <td>SplashScreen.scale-200.png</td>
</tr>
<tr>
    <td>400% 缩放比例</td>
    <td>2480x1200</td>
    <td>SplashScreen.scale-400.png</td>
</tr>
</tbody>
</table>

<br/>
 

**基于目标的资源**

跨多个比例系数使用基于目标的资源。 基于目标的资源的元素名称是 **Square44x44Logo**。 我们强烈建议至少提交以下资源：

16x16、24x24、32x32、48x48、256x256

下表列出了所有基于目标的资源大小和相应的文件名示例：

| 资源大小 | 文件名示例                  |
|------------|------------------------------------|
| 16x16\*    | Square44x44Logo.targetsize-16.png  |
| 24x24\*    | Square44x44Logo.targetsize-24.png  |
| 32x32\*    | Square44x44Logo.targetsize-32.png  |
| 48x48\*    | Square44x44Logo.targetsize-48.png  |
| 256x256\*  | Square44x44Logo.targetsize-256.png |
| 20x20      | Square44x44Logo.targetsize-20.png  |
| 30x30      | Square44x44Logo.targetsize-30.png  |
| 36x36      | Square44x44Logo.targetsize-36.png  |
| 40x40      | Square44x44Logo.targetsize-40.png  |
| 60x60      | Square44x44Logo.targetsize-60.png  |
| 64x64      | Square44x44Logo.targetsize-64.png  |
| 72x72      | Square44x44Logo.targetsize-72.png  |
| 80x80      | Square44x44Logo.targetsize-80.png  |
| 96x96      | Square44x44Logo.targetsize-96.png  |

 

\* 提交这些资源大小作为基线

## <a name="asset-types"></a>资源类型


此处列出所有资源类型、用途和建议的文件名。

**磁贴资源**

-   通常在“开始”菜单上使用居中放置的资源来展示你的应用。
-   文件名格式：[Square\Wide]\*x\*Logo.scale-\*.png
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
-   文件名格式：Square44x44Logo.scale-\*.png
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
-   文件名格式：Square44x44Logo.targetsize-\*.png
-   受影响的应用：每个 UWP 应用
-   使用：
    -   “开始”菜单跳转列表（桌面）
    -   磁贴下角的“开始”菜单（桌面）
    -   快捷方式（桌面）
    -   控制面板（桌面）

**未着色的目标大小列表资源**

-   这些资源未通过系统进行着色或缩放。
-   文件名格式：Square44x44Logo.targetsize-\*\_altform-unplated.png
-   受影响的应用：每个 UWP 应用
-   使用：
    -   任务栏和任务栏缩略图（桌面）
    -   任务栏跳转列表
    -   任务视图
    -   ALT+TAB

**文件扩展名资源**

-   这些是特定于文件扩展名的资源。 它们显示在文件资源管理器中的 Win32 样式文件关联图标旁，并且必须独立于主题。 大小调整在桌面平台和移动平台上是不同的。
-   文件名格式：\*LogoExtensions.targetsize-\*.png
-   受影响的应用：音乐、视频、照片、Microsoft Edge、Microsoft Office
-   使用：
    -   文件资源管理器
    -   Cortana
    -   各种 UI 图面（桌面）

**初始屏幕**

-   显示在你的应用的初始屏幕上的资源。 在桌面平台和移动平台上自动缩放。
-   文件名格式：SplashScreen.scale-*.png
-   受影响的应用：每个 UWP 应用
-   使用：
    -   应用的初始屏幕