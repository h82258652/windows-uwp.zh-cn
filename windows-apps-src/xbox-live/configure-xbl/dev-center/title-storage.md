---
title: 开发人员中心上的标题存储配置
author: aablackm
description: 了解如何在 Windows 开发人员中心配置标题存储
ms.author: aablackm
ms.date: 04/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, 游戏, uwp, windows 10, Xbox one, 标题存储, Windows 开发人员中心
ms.openlocfilehash: 7a258db9c4615de81ba32be4b636e007d3b9471c
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5402873"
---
# <a name="configure-storage-for-you-title-on-windows-dev-center"></a>在 Windows 开发人员中心为标题配置存储

借助 Xbox Live，可通过标题存储服务将与游戏相关的数据保存到云中。 通过标题存储配置页，可确定游戏允许使用哪些类型的云存储服务，并上传用于全局存储的文件。

你可以找到 Xbox Live 标题存储配置页面通过转到[Windows 开发人员中心仪表板](https://developer.microsoft.com/en-us/dashboard/windows/overview)，选择你的应用**概述**或**产品**，打开**服务**从下拉列表，并选择**Xbox Live**。 创意者计划中的开发人员需点击其配置页面**云保存和存储**部分中的**显示选项**方可查看标题存储配置选项。 可使用全套 Xbox Live 功能的用户需找到**标题存储**链接，以导航到标题存储配置页面。

标题存储配置有两个主要部分。 标题存储设置部分和全局存储文件管理部分。

## <a name="section-1-title-storage-settings"></a>第 1 部分：标题存储设置

![标题存储设置屏幕截图](../../images/dev-center/title-storage/title-storage-settings.JPG)

在本部分，勾选**活动**列中的复选框，即可启用四种存储类型中的任何一种存储。 选择标题的存储类型后，可点击存储类型行的**仅所有者**单选按钮，选择是否只有拥有该数据的玩家可读取数据，或点击**所有人**单选按钮，公开共享数据。 如果针对给定**存储类型**选择了**仅所有者**，则只有生成该数据的玩家可读取该类型的标题存储数据。 如果针对给定**存储类型**选择了**所有人**，则所有玩家均可读取该类型的标题存储数据。 在任何情况下，只有生成数据的用户可写入或修改保存的该数据。

## <a name="storage-types"></a>存储类型

在标题存储配置页面上可激活四种类型的存储。 将鼠标悬停在每个存储类型名称旁的信息图标上，即可查看每种存储类型的说明，也可从下表中查看。

|存储类型 |说明 |示例用法  |
|---------|---------|---------|
|全局             |对于上传到 Windows 开发人员中心的数据，可通过任何设备读取，且每个用户均可访问。 只有将数据上传到 Windows 开发人员中心的开发人员可写入该数据。 | 通过游戏内新闻源向所有用户发布更新通知。     |
|连接存储  |允许后台同步 Xbox One 和 Windows 10 游戏上的游戏数据。 可靠的容错游戏保存服务。 可通过任何设备进行读取，可通过 Xbox One 和 Windows 10 设备写入    | 可保存单个用户的文件，让用户能在单独的控制台上进行游戏。         |
|通用          |可通过网络访问的 blob 存储，通过除 Xbox 360 或 Windows Phone 外的其他设备均可进行读取/写入。 可通过 Android 和 iOS 设备进行读取。      | 可保存玩游戏的时间或其他统计数据，可从多个 Windows 设备访问这些数据。        |
|受信任            |可通过网络访问的 blob 存储，只能通过 Xbox One、Xbox 360 和 Windows Phone 进行写入。 可通过任何设备进行读取。 可通过 Android 和 iOS 进行读取。     | 可存储玩家在多人游戏中的排名。        |

## <a name="section-2-global-storage-file-management"></a>第 2 部分：全局存储文件管理

![全局存储文件管理屏幕截图](../../images/dev-center/title-storage/global-storage-file-management.JPG)

若要查看完整的全局存储文件管理选项，必须单击**显示选项**下拉菜单。 在本部分，你可添加文件和文件夹，在标题存储设置中将**全局**存储类型设置为活动后，即可访问这些文件和文件夹。 为了在本节中添加文件，需发布游戏进行测试。 如果未正确发布游戏进行测试，那么标题存储配置页面的顶部可能会出现一条警告。

## <a name="manage-global-storage-files"></a>管理全局存储文件

通过全局存储文件管理可上传和下载要用于全局存储的文件。 只要是拥有你的标题的人就有可能访问这些文件，且这些文件可能会在所有玩你的游戏的玩家中共享。 若要查看全局存储文件选项，必须单击该部分标题旁的**显示选项**下拉菜单。 若要添加第文件，请点击 **+新建...** 链接。 随后可选择将新文件或文件夹添加到全局存储文件。

### <a name="new-folders"></a>新文件夹

将新文件夹添加到全局存储文件时，只需命名该文件夹并点击**创建**按钮即可。 新文件夹会出现在文件资源管理器表中。

![添加文件夹对话框](../../images/dev-center/title-storage/add-folder-global-storage-filled.JPG)

若要将文件添加到文件夹，必须直接将文件上传到文件夹，上传方法为：按文件夹**操作**按钮 **...**，然后选择**上传文件**。 不能将文件拖放到文件资源管理器表中的文件夹中。 使用文件夹**操作**菜单中的**创建文件夹**操作即可创建子文件夹。 选择文件夹**操作**菜单中的**删除**操作即可删除该文件夹及其所有内容。

### <a name="new-files"></a>新文件

将新文件添加到全局存储时，系统会提示你从计算机的文件资源管理器上传文件，然后要求你从三种文件类型（二进制文件，配置文件和 JSON）中选择一种类型。 除使用 **+新建...** 按钮上传文件外，还可将计算机上的文件拖放到文件资源管理器表中。

> [!WARNING]
> 不能将文件夹拖放到文件资源管理器表中，若尝试执行此操作，则会导致文件夹被视为文件，无法按预期生效。

文件管理操作：![文件管理 gif](../../images/dev-center/title-storage/global-storage-management.gif)

#### <a name="file-types"></a>文件类型

* 二进制文件 - 二进制类型应用于图像、声音和自定义数据。 此数据必须是 HTTP 友好数据。
* 配置文件 - 配置文件可保存有关游戏的信息，且可基于某些输入返回动态查询值。
* JSON - .json 文件。 这些文件可保存关于游戏某方面的一些信息，与配置文件类似。

## <a name="further-reading"></a>进一步阅读

若要详细了解 Xbox Live 存储平台，请参阅[标题存储文档](../../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md)，其中详细介绍了通用、受信任的、全局存储和存储文件类型，以及[连接存储文档](../../storage-platform/connected-storage/connected-storage-overview.md)，这些文档会详细介绍如何在云中保存游戏进度，便于玩家在各设备间继续游戏。