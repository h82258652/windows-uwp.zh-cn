---
Description: 如何创建应用程序图标/徽标表示你的应用在开始菜单中，应用程序磁贴、 任务栏、 Microsoft Store 中，和的详细信息。
title: 应用图标和徽标
template: detail.hbs
ms.date: 04/17/2018
ms.topic: article
keywords: windows 10, uwp
design-contact: Judysa
doc-status: Published
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: b755e8d165d58ce4303d9fefe6d051abce6c9765
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57622452"
---
# <a name="app-icons-and-logos"></a>应用图标和徽标 

每个应用程序有表示它，图标/徽标，并且该图标将出现在 Windows shell 中的多个位置： 

:::row:::
    :::column:::
        * 应用程序窗口的标题栏
        * 开始菜单中的应用列表
        * 任务栏和任务管理器
        * 你的应用的磁贴
        * 应用程序的初始屏幕
        * 在 Microsoft Store
    :::column-end:::
    :::column:::
        ![windows 10 start and tiles](images/assetguidance01.jpg)
    :::column-end:::
:::row-end:::

本文介绍如何创建应用图标的基础知识如何应需要使用 Visual Studio 来管理它们，以及如何手动管理它们。
 
(这篇文章是专门为图标，表示应用程序本身; 有关常规图标的指南，请参阅[图标](icons.md)文章。)

## <a name="icon-types-locations-and-scale-factors"></a>图标类型、 位置和缩放比例

默认情况下，Visual Studio 将图标资产存储在资产子目录中。 下面是不同类型的图标，显示位置和即所谓的列表。 

| 图标名称 | 将出现在 | 资产文件名称 |
| ---      | ---        | --- |
| 小磁贴 | “开始”菜单 |  SmallTile.png  |
| 中等磁贴 |开始菜单，Microsoft Store 列表\*  |  Square150x150Logo.png |
| 宽磁贴  | “开始”菜单   | Wide310x150Logo.png |
| 大磁贴   | 开始菜单，Microsoft Store 列表\* |  LargeTile.png  |
| 应用图标 | 在开始菜单、 任务栏、 任务管理器中的应用列表 | Square44x44Logo.png |
| 初始屏幕 | 应用程序的初始屏幕 | SplashScreen.png  |
| 徽章徽标 | 你的应用的磁贴 | BadgeLogo.png  |
| 包徽标/应用商店徽标 | 应用安装程序，合作伙伴中心存储区中存储的"写评论"选项中的"报告应用程序"选项 | StoreLogo.png  |

\* 使用，除非你选择[仅显示上传的映像存储区中的](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)。 

若要确保这些图标看起来清晰每个屏幕上，可以创建多个版本的不同的显示缩放比例为相同的图标。 

缩放比例确定 UI 元素，例如，文本的大小。 缩放因素范围从 100%到 400%。 较大的值创建较大的 UI 元素，使其更轻松地查看在高 DPI 显示器上。 

:::row:::
    :::column:::
        Windows automatically sets the scale factor for each display based on its DPI (dots-per-inch) and the viewing distance of the device. 

        (Users can override the default value by going to the **Settings &gt; Display &gt; Scale and layout** page.)
    :::column-end:::
    :::column:::
        ![](images/icons/display-settings-screen.png)
    :::column-end:::
:::row-end:::  


由于应用图标资产是位图，位图的扩展性不好，我们建议每个缩放比例为每个图标资产提供的版本：100%、 125%、 150%、 200%到 400%。 这是很多图标 ！ 幸运的是，Visual Studio 提供了一种工具，轻松地生成和更新这些图标。 

## <a name="microsoft-store-listing-image"></a>Microsoft Store 列出映像

"如何指定我的应用程序列表的映像在 Microsoft Store 中？"

默认情况下，我们使用一些从您的包的映像存储区中，在此页顶部表中所述 (以及其他[提交过程中提供的映像](https://docs.microsoft.com/en-us/windows/uwp/publish/app-screenshots-and-images))。 但是，您可以选择要阻止应用商店向客户 （包括 Xbox） 的 Windows 10 上显示的列表时在应用程序的包中使用的徽标图像，而让使用你上传的映像的存储。 这可以使你更好地控制应用在整个应用商店的各种显示中的外观。 （请注意，是否您的产品支持早期的 OS 版本，这些客户仍可能会看到映像从您的包，即使使用此选项。）您可以执行此操作**存储徽标**一部分**应用商店列表**提交过程的步骤。

![在应用程序提交过程的过程中指定应用商店徽标](images/app-icons/storelogodisplay.png)

选中此框时，新区域称为**存储区显示图像**出现。 在这里，你可以上载 3 个存储区将用于替换你的应用包中的徽标图像的图像大小：300x300、 150 x 150 和 71x71 像素。 不过，我们建议提供所有 3 种大小，是必需的仅 300x300 大小。

有关详细信息，请参阅[显示仅上传徽标图像的存储区中](/windows/uwp/publish/app-screenshots-and-images#display-only-uploaded-logo-images-in-the-store)。

<!-- ### Fallback images for the Store

The simplest way to control the Store listing image is to specify it during the app submission process. If you don't provide these images during the app submission process, the Store will use a tile image:

1. Large tile
2. Medium tile

If these images aren't provided, the Store will search all matching images of the same image type with a square aspect ratio, preferable with a height greated than the scaled requested height (scaled height is the machine's resolution scale factor * display height). If none of the images meet this criteria, the Store will ignore the scale factor and select an image based on height.  -->

<!-- You can provide screenshots, logos, and other art assets (such as trailers and promotional images to include in your app's Microsoft Store listing. Some of these are required, and some are optional (although some of the optional images are important to include for the best Store display).

The Store may also use your app's tile and other images that you include in your app's package. 

For more information, see [App screenshots, images, and trailers in the Microsoft Store](/windows/uwp/publish/app-screenshots-and-images). -->


## <a name="managing-app-icons-with-the-visual-studio-manifest-designer"></a>管理与 Visual Studio 清单设计器的应用图标

Visual Studio 提供了一个非常有用的工具来管理您应用的图标名为**清单设计器**。 

> 如果还没有 Visual Studio 2017，有几个版本可用，包括免费版本，(Visual Studio 2017 Community Edition)，和其他版本提供免费试用版。 可以在此处下载它们： [https://developer.microsoft.com/windows/downloads](https://developer.microsoft.com/windows/downloads)


若要启动清单设计器：
<!-- 1. Use Visual Studio to open a UWP project.
2. In the **Solution Explorer**, double-click the package.appmanifest file. 

    ![The Visual Studio 2017 Solution Explorer](images/icons/vs-solution-explorer.png)

    Visual Studio displays the manifest designer.

    ![The Visual Studio 2017 manifest designer](images/icons/vs-manfiest-designer.png)
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
        2. 在中**解决方案资源管理器**，双击 Package.appmxanifest 文件。
    :::column-end:::
    :::column:::
        ![The Visual Studio 2017 Manifest Designer](images/icons/vs-solution-explorer.png)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
            Visual Studio displays the Manifest Designer.
    :::column-end:::
    :::column:::
            ![The Visual Assets tab](images/icons/vs-manfiest-designer.png)
    :::column-end:::
:::row-end:::    
:::row:::
    :::column:::
        3. 单击**视觉对象资产**选项卡。
    :::column-end:::
    :::column:::
        ![The Visual Assets tab](images/icons/vs-manfiest-designer-visual-assets.png)
    :::column-end:::
:::row-end:::        

## <a name="generating-all-assets-at-once"></a>立即生成所有资产

第一个菜单项中**视觉对象资产**选项卡上，**所有视觉对象资产**，没有完全什么其名称所示： 生成每个应用程序需要与按下某个按钮的视觉资产。

![在 Visual Studio 中生成所有视觉对象资产](images/app-icons/all-visual-assets.png)

需要执行的是提供的单一映像，Visual Studio 将生成小磁贴、 中等磁贴、 大磁贴、 宽磁贴、 大磁贴、 应用程序图标、 初始屏幕中，并打包徽标的每个缩放比例的资产。

若要立即生成所有资产：
1. 单击 **...** 旁边**源**字段中，选择你想要使用的映像。 如果使用的位图图像，请确保已至少 400 像素的 400，以便获得 sharp 结果。 基于矢量的图像效果最佳;Visual Studio，您可以使用 AI (Adobe Illustrator) 和 PDF 文件。 
2. （可选）。在中**显示设置**部分中，配置这些选项：

    a.  **短名称**:指定您的应用程序的短名称。

    b.  **显示名称**:指示是否要在中、 宽、 或大磁贴上显示的短名称。 

    c. **磁贴背景**:指定十六进制值或磁贴背景色的颜色名称。 例如， `#464646`。 默认值为 `transparent`。

    d. **初始屏幕背景**:指定初始屏幕背景的十六进制值或颜色名称。 

3. 单击**生成**。 

Visual Studio 生成图像文件，并将它们添加到项目。 如果你想要更改你的资产，只需重复此过程。 

缩放的图标资产遵循此文件命名约定：

*文件名*-缩放-*比例系数*.png

例如，

Square150x150Logo-缩放-100.png、 Square150x150Logo 规模 200.png、 Square150x150Logo 规模 400.png

请注意，Visual Studio 不会默认情况下生成徽章徽标。 这是因为徽章徽标是唯一的可能不应匹配其他应用程序图标。 有关详细信息，请参阅[锁屏提醒通知为 UWP 应用项目](/windows/uwp/design/shell/tiles-and-notifications/badges)。 


## <a name="more-about-app-icon-assets"></a>有关应用图标资产的详细信息
Visual Studio 将生成由你的项目，所需的所有应用图标资产，但如果你想要自定义它们，最好先了解它们之间的差异从其他应用程序资产。 

应用图标资产将出现在多个位置： 在 Windows 任务栏、 任务视图、 ALT + 选项卡上，和开始磁贴右下角。 由于应用图标资产将出现在这么多地方，它有一些其他的大小调整和 plating 没有其他资产的选项:"目标大小"资产和"未着色"资产。 

### <a name="target-size-app-icon-assets"></a>目标大小应用图标资产
除了标准的缩放因子大小 ("Square44x44Logo.scale-400.png")，我们还建议创建"目标大小"的资产。 我们称这些资产的目标大小，因为它们针对特定大小，例如 16 像素，而不是特定的缩放比例，例如 400。 对于不使用缩放平稳状态系统的图面是目标大小资产：

* “开始”菜单跳转列表（桌面）
* 磁贴下角的“开始”菜单（桌面）
* 快捷方式（桌面）
* 控制面板（桌面）

下面是目标大小资产的列表：


| 资源大小 | 文件名示例                  |
|------------|------------------------------------|
| 16 x 16\*    | Square44x44Logo.targetsize-16.png  |
| 24 x 24\*    | Square44x44Logo.targetsize-24.png  |
| 32 x 32\*    | Square44x44Logo.targetsize-32.png  |
| 48 x 48\*    | Square44x44Logo.targetsize-48.png  |
| 256 x 256\*  | Square44x44Logo.targetsize-256.png |
| 20x20      | Square44x44Logo.targetsize-20.png  |
| 30x30      | Square44x44Logo.targetsize-30.png  |
| 36x36      | Square44x44Logo.targetsize-36.png  |
| 40x40      | Square44x44Logo.targetsize-40.png  |
| 60x60      | Square44x44Logo.targetsize-60.png  |
| 64x64      | Square44x44Logo.targetsize-64.png  |
| 72x72      | Square44x44Logo.targetsize-72.png  |
| 80x80      | Square44x44Logo.targetsize-80.png  |
| 96x96      | Square44x44Logo.targetsize-96.png  |

\* 至少，我们建议提供这些大小。 

无需向这些资源添加填充；Windows 会根据需要添加填充。 这些资源应占据最少 16 pixel 的占用空间。 

以下是这些资源在 Windows 任务栏上的图标中显示时的示例：

![Windows 任务栏中的资源](images/assetguidance21.png)

### <a name="unplated-assets"></a>未着色的资产
默认情况下，Windows 默认情况下使用彩色背板顶部的基于目标的资产。 如果你想，您可以基于目标的未着色的资产。 "未着色"，则意味着该资产将透明的背景上显示。 请记住，这些资产会在不同的背景色。 

![未着色和着色资源](images/assetguidance22.png)

下面是使用未着色的应用图标资产的图面：
* 任务栏和任务栏缩略图（桌面）
* 任务栏跳转列表
* 任务视图
* Alt+Tab


### <a name="target-and-unplated-sizing"></a>目标和未着色的大小调整

下面是基于目标的资产，在 100%规模较大的大小建议：

![基于目标的资源大小调整（比例为 100%）](images/assetguidance23.png)


## <a name="more-about-splash-screen-assets"></a>有关初始屏幕资产的详细信息
有关初始屏幕的详细信息，请参阅[UWP 初始屏幕文章](/windows/uwp/launch-resume/splash-screens)。

## <a name="more-about-badge-logo-assets"></a>徽章徽标资产有关的详细信息

使用资产生成器以生成所需的所有资产，时为什么它不会默认情况下都生成徽章徽标的原因： 它们是非常不同于其他应用程序资产。 徽章徽标是出现在通知和应用的磁贴上的状态图像。 

有关详细信息，请参阅[锁屏提醒通知为 UWP 应用项目](/windows/uwp/design/shell/tiles-and-notifications/badges)。


## <a name="customizing-asset-padding"></a>自定义资产的填充

默认情况下，Visual Studio 资产生成器向任何映像应用建议的填充。 如果你的映像已包含填充或你希望扩展到末尾的磁贴的完整出血图像，您可以关闭此功能通过取消选中**应用推荐的填充**复选框。 

### <a name="tile-padding-recommendations"></a>磁贴填充建议
如果你想要提供您自己的填充，以下是我们建议对于磁贴。 

有 4 个磁贴大小： 小型 (71x71)、 中等 (150 x 150)、 宽 (310 x 150) 和大型 (310 x 310)。 

每个磁贴资源的大小与在其上放置的磁贴大小相同。

![磁贴显示完整出血](images/app-icons/tile-assets1.png)

如果不希望您的图标将扩展到磁贴的边缘，可用于透明像素在资产中创建空白。 

![磁贴和背板](images/assetguidance05.png)

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









