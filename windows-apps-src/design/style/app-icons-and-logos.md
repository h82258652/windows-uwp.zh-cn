---
Description: 如何创建在“开始”菜单、应用磁贴、任务栏、Microsoft Store 以及其他位置表示应用的应用图标/徽标。
title: 应用图标和徽标
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 25d9df392d6ed2725b171fe6513334a39458410b
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684588"
---
# <a name="app-icons-and-logos"></a>应用图标和徽标 

每个应用都有表示自己的图标/徽标，该图标在 Windows shell 中的多个位置显示： 

:::row:::
    :::column:::
        * “开始”菜单中的应用列表
        * 任务栏和任务管理器
        * 应用磁贴
        * 应用的初始屏幕
        * Microsoft Store
    :::column-end:::
    :::column:::
        ![Windows 10“开始”菜单和磁贴](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

本文介绍创建应用图标的基本知识，如何使用 Visual Studio 管理这些图标，以及如何手动管理图标（如果你需要这样做）。
 
（本文专门针对表示应用本身的图标；有关常规图标指南，请参阅[图标](icons.md)一文。）

## <a name="icon-types-locations-and-scale-factors"></a>图标类型、位置和缩放比例

默认情况下，Visual Studio 将图标资源存储在资源子目录中。 下面是不同类型的图标、显示位置和名称的列表。 

| 图标名称 | 位置 | 资源文件名称 |
| ---      | ---        | --- |
| 小磁贴 | “开始”菜单 |  SmallTile.png  |
| 中等磁贴 |“开始”菜单，Microsoft Store 列表\*  |  Square150x150Logo.png |
| 宽磁贴  | “开始”菜单   | Wide310x150Logo.png |
| 大磁贴   | “开始”菜单，Microsoft Store 列表\* |  LargeTile.png  |
| 应用图标 | “开始”菜单、任务栏、任务管理器中的应用列表 | Square44x44Logo.png |
| 初始屏幕 | 应用的初始屏幕 | SplashScreen.png  |
| 锁屏提醒徽标 | 应用磁贴 | BadgeLogo.png  |
| 包徽标/Store 徽标 | 应用安装程序、合作伙伴中心、Store 中的“报告应用”选项和 Store 中的“写评论”选项 | StoreLogo.png  |

\*已使用，除非你选择[仅显示 Store 中已上传的图像](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)。 

若要确保这些图标在每个屏幕上看起来清晰，可以为不同显示比例系数创建同一图标的多个版本。 

比例系数决定 UI 元素（例如文本）的大小。 比例系数范围是：100%到 400%。 较大的值会创建较大的 UI 元素，可以轻松地在高 DPI 显示器上查看。 

:::row:::
   :::column:::
      Windows 会根据每个显示器的 DPI（每英寸点数）和设备的观看距离自动为显示器设置缩放比例。 
      （用户可以转到“设置”&gt;“显示器”&gt;“缩放和布局”  页来重写默认值。）
   :::column-end:::
   :::column:::
      ![](images/icons/display-settings-screen.png)
   :::column-end:::
:::row-end:::  


由于应用图标资源是位图，位图的扩展性不好，我们建议为每个比例系数提供一个图标资源版本：100%、125%、150%、200% 和 400%。 有很多图标！ 幸运的是，Visual Studio 提供了一种工具，可以轻松地生成和更新这些图标。 

## <a name="microsoft-store-listing-image"></a>Microsoft Store 列表图像

“如何在 Microsoft Store 中为我的应用列表指定图像？”

默认情况下，我们使用 Store 的包中的一些图像，正如此页顶部表中所述（以及其他[提交过程中提供的图像](https://docs.microsoft.com/windows/uwp/publish/app-screenshots-and-images)。 但是，当在 Windows 10（包括 Xbox）上向客户显示你的列表时，可以选择阻止应用商店使用应用包中的徽标图像，让其仅使用你上传的图像。 这可以使你能在整个应用商店的各种显示中更好地控制应用外观。 （请注意你的产品是否支持早期的 OS 版本，即使使用此选项，客户仍可能会看到来自你的包的图像。）可以在提交过程的“Store 列表”步骤的“Store 徽标”部分执行此操作   。

![在应用提交过程中指定 Store 徽标](images/app-icons/storelogodisplay.png)

选中此框后，一个名为“Store 显示图像”的新部分会出现  。 在这里，可以上传 3 个图像大小，Store 将用它们来替换你的应用包中的徽标图像：300 x 300、150 x 150 和 71 x 71 像素。 尽管我们建议提供所有 3 种大小，但仅 300 x 300 是必需的。

有关详细信息，请参阅[仅显示 Store 中上传的徽标图像](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)。

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>使用 Visual Studio 清单设计器管理应用图标

Visual Studio 提供了一个非常有用的工具来管理应用图标，名为“清单设计器”  。 

> 如果未安装 Visual Studio 2019，则有几个版本可供使用，其中包括免费版本 (Visual Studio 2019 Community Edition)，其他版本也提供免费试用版。 可以在此处下载：[https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


若要启动清单设计器：
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2019 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2019 manifest designer](images/icons/vs-manfiest-designer.png)
3. Click the **Visual Assets** tab.

    ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png) -->


:::row:::
    :::column:::
        1. 使用 Visual Studio 打开 UWP 项目。
    :::column-end:::
    :::column:::
        
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        2. 在解决方案资源管理器中，双击 Package.appxmanifest 文件  。
    :::column-end:::
    :::column:::
        ![Visual Studio 2019 清单设计器](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio 将显示清单设计器。
    :::column-end:::
    :::column:::
            ![“可视化资源”选项卡](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. 单击“可视化资源”选项卡  。
    :::column-end:::
    :::column:::
        ![“可视化资源”选项卡](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>立即生成所有资源

“可视化资源”选项卡中的第一个菜单项“所有可视化资源”，顾名思义，只需按下按钮即可生成应用所需的所有可视化资源   。

![在 Visual Studio 中生成所有可视化资源](images/app-icons/all-visual-assets.png)

你只需要提供一个图像，Visual Studio 将生成不同比例系数的小磁贴、中等磁贴、大磁贴、宽磁贴、应用图标、初始屏幕和包徽标资源。

若要立即生成所有资源：
1. 单击“源”字段旁边的“...”，然后选择要使用的图像   。 如果要使用位图图像，请确保至少是 400 x 400 像素，以便获得清晰效果。 基于矢量的图像效果最佳，Visual Studio 支持使用 AI (Adobe Illustrator) 和 PDF 文件。 
2. （可选。）在“显示设置”部分中，配置这些选项  ：

    a.  **短名称**：为应用指定一个短名称。

    b.  **显示名称**：指示是否要在中等磁贴、宽磁贴或大磁贴上显示短名称。 

    c. **磁贴背景**：指定磁贴背景色的十六进制值或颜色名称。 例如，`#464646`。 默认值为 `transparent`。

    d. **初始屏幕背景**：指定初始屏幕背景的十六进制值或颜色名称。 

3. 单击“生成”  。 

Visual Studio 生成图像文件，并将它们添加到项目。 如果要更改资源，只需重复此过程。 

缩放的图标资源遵循此文件命名约定：

filename-scale-scale factor.png  

例如，

Square150x150Logo-scale-100.png、Square150x150Logo scale 200.png、Square150x150Logo scale 400.png

请注意，在默认情况下，Visual Studio 不会生成锁屏提醒徽标。 因为锁屏提醒徽标是唯一的且可能不应该与其他应用图标匹配。 有关详细信息，请参阅[适用于UWP 应用的锁屏提醒通知](/windows/uwp/design/shell/tiles-and-notifications/badges)一文。 


## <a name="more-about-app-icon-assets"></a>有关应用图标资源的详细信息
Visual Studio 将生成你的项目所需的所有应用图标资源，但如果你想要自定义它们，最好先了解它们与其他应用资源的差异。 

应用图标资源在多个位置出现：Windows 任务栏、任务视图、Alt+Tab 和“开始”磁贴右下角。 由于应用图标资源在这么多位置出现，它有一些其他资源没有的其他大小和着色选项：“目标大小”资源和“未着色”资源。 

### <a name="target-size-app-icon-assets"></a>目标大小应用图标资源
除了标准的比例系数大小（“Square44x44Logo.scale-400.png”），我们还建议创建“目标大小”资源。 我们称这些资源为目标大小资源，是因为它们针对特定大小（例如 16 像素），而不是特定的比例系数，例如 400。 目标大小资源适用于未使用缩放倍数系统的图面：

* “开始”菜单跳转列表（桌面）
* 磁贴下角的“开始”菜单（桌面）
* 快捷方式（桌面）
* 控制面板（桌面）

下面是目标大小资源的列表：


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

\*我们建议至少提供这些大小。 

无需向这些资源添加填充；Windows 会根据需要添加填充。 这些资源应占据最少 16 像素的占用空间。 

以下是这些资源在 Windows 任务栏上的图标中显示时的示例：

![Windows 任务栏中的资源](images/assetguidance21.png)

### <a name="unplated-assets"></a>未着色资源
Windows 默认在彩色背板顶部上使用基于目标的资源。 如果需要，可以提供基于目标的未着色资源。 “未着色”意味着该资源将在透明的背景上显示。 请记住这些资源将会出现在不同的背景色上。 

![未着色和着色资源](images/assetguidance22.png)

下面是使用未着色的应用图标资源的图面：
* 任务栏和任务栏缩略图（桌面）
* 任务栏跳转列表
* 任务视图
* Alt+Tab

### <a name="unplated-assets-and-themes"></a>未着色的资源和主题

用户所选的主题决定任务栏的颜色。 如果未着色资源不符合当前主题的条件，系统会检查该资源的对比度。 如果该资源与任务栏有足够的对比度，系统将使用它。 否则，系统将查找该资源的高对比度版本。 如果找不到，系统会绘制该资源的着色形式。 


### <a name="target-and-unplated-sizing"></a>目标和未着色大小

对于基于目标的资源（缩放比例为 100%），以下是大小建议：

![基于目标的资源大小调整（比例为 100%）](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>初始屏幕资源的详细信息
有关初始屏幕的详细信息，请参阅 [UWP 初始屏幕文章](/windows/uwp/launch-resume/splash-screens)。

## <a name="more-about-badge-logo-assets"></a>锁屏提醒徽标资源的详细信息

使用资源生成器以生成所需的所有资源时，默认情况下不会生成锁屏提醒徽标的原因是：它们与其他应用资源有很大的差别。 锁屏提醒徽标是出现在通知和应用磁贴上的状态图像。 

有关详细信息，请参阅 [UWP 应用的锁屏提醒通知](/windows/uwp/design/shell/tiles-and-notifications/badges)一文。


## <a name="customizing-asset-padding"></a>自定义资源填充

默认情况下，Visual Studio 资源生成器将建议的填充应用于任何图像。 如果图像已包含填充或你希望使用扩展到磁贴末端的全幅图像，可以通过取消选中“应用建议的填充”复选框关闭此功能  。 

### <a name="tile-padding-recommendations"></a>磁贴填充建议
如果你要提供自己的填充，以下是我们对于磁贴的建议。 

有 4 个磁贴大小：小型 (71 x 71)、中等 (150 x 150)、宽型 (310 x 150) 和大型 (310 x 310)。 

每个磁贴资源的大小与在其上放置的磁贴大小相同。

![显示全幅的磁贴](images/app-icons/tile-assets1.png)

如果不希望图标扩展到磁贴的边缘，可使用资源中的透明像素来创建填充。 

![磁贴和背板](images/assetguidance05.png)

对于小磁贴，将图标宽度和高度限制为磁贴大小的 66%：

![小磁贴大小调整比率](images/assetguidance09.png)

对于中磁贴，将图标宽度限制为磁贴大小的 66%，将高度限制为 50%。 这可以防止品牌栏中的元素重叠：

![中磁贴大小调整比率](images/assetguidance10.png)

对于宽磁贴，将图标宽度限制为磁贴大小的 66%，将高度限制为 50%。 这可以防止品牌栏中的元素重叠：

![宽磁贴大小调整比率](images/assetguidance11.png)

对于大磁贴，将图标宽度限制为磁贴大小的 66%，将高度限制为 50%：

![大磁贴大小调整比率](images/assetguidance12.png)

某些图标专用于水平或垂直方向，而其他图标具有更复杂形状，使它们无法完全拟合目标尺寸。 居中显示的图标可向一侧加权。 在此情况下，如果图标占据的视觉权重与完全拟合的图标相同，则该图标的一部分可能悬挂在建议的占用之外：

![三个居中的图标](images/assetguidance13.png)

使用全出血资源，将在磁贴的边距和边缘内相交的元素考虑在内。 保留的边距至少占据 16% 的磁贴高度或宽度。 此百分比表示最小磁贴大小的边距宽度的两倍：

![带有边距的全出血磁贴](images/assetguidance14.png)

在此示例中，边距太紧：

![带有过紧边距的全出血磁贴](images/assetguidance15.png)


## <a name="optimizing-for-specific-themes-languages-and-other-conditions"></a>针对特定主题、语言和其他条件进行优化 

本文介绍了如何创建资源以实现特定的缩放比例，但你也可以针对多种条件和条件组合创建资源。 例如，可以为高对比度显示或浅色主题和深色主题创建图标。 甚至可以创建针对特定语言的资源。

有关说明，请参阅[针对语言、缩放、高对比度和其他限定符定制资源](../../app-resources/tailor-resources-lang-scale-contrast.md)。













