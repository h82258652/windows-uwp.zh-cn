---
title: 连接存储概述
author: aablackm
description: 了解如何使用连接存储保存和加载设备中的游戏数据。
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储
ms.localizationpriority: medium
ms.openlocfilehash: 754367c1a8d2daaf37d236e65d241b05c52e84d5
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2018
ms.locfileid: "4055730"
---
# <a name="connected-storage"></a>连接存储
连接存储旨在允许游戏保存游戏数据以及应在设备之间漫游的其他相关状态数据。 连接存储 API 允许 Xbox One 和通用 Windows 平台 (UWP) 上的游戏保存、加载和删除本地存储的游戏数据，并在 Xbox One 或 UWP 游戏连接到 Internet 时保存、加载和删除同步到云的数据。 在同步发生后运行游戏的任何其他设备都可以使用保存的数据。 建议开发人员尽可能准确地保存游戏状态以提供最佳外出游戏体验。 连接存储允许你在家里玩游戏，然后在支持相同游戏的任何其他设备上从离开的游戏进度处继续游戏。

## <a name="connected-storage-features"></a>连接存储功能

连接存储 API 提供以下功能：

- 应用一次可以将多达 16 MB 的数据快速保存到内存缓冲区中，这些数据稍后会由系统在 HDD 上本地缓存，然后上传到云中。
- 对于托管的合作伙伴和 ID@Xbox 开发人员：
    - 每个云存储的用户/应用 256 MB。
- 对于 Xbox Live 创意者计划开发人员：
    - 每个云存储的用户/应用 64 MB。
- 电源失败的可靠响应—应用无需处理正在保存的部分数据。
- 即使应用没在运行，数据也会自动上传到云中。
- 数据在连接到 Xbox Live 的 Xbox One 或 UWP 设备中可用。
- Xbox Live 可以处理跨设备的同步和冲突管理，无需应用参与。

连接存储系统确保所有保存或者全部保存，或者根本不保存。 这意味着，作为一名开发人员，你永远不必担心在电源故障或者用户通过手动或在主机上打开另一个应用/游戏突然关闭应用时部分保存的数据影响游戏状态。 这还可以确保为游戏用户提供更流畅的游戏体验，因为连接存储是允许用户在游戏和应用间快速自由切换但仍能回到原始游戏的离开状态的重要部分。 要在你自己的游戏中实现这些功能，需要了解连接存储 API。

## <a name="connected-storage-structure"></a>连接存储结构

连接存储系统允许应用将数据存储为容器中的一个或多个 blob。 在高级别上，连接存储系统中的所有数据都与用户或计算机（对于 Xbox 开发工具包开发的游戏）关联。 由特定用户或计算机的应用保存的所有数据都存储在连接存储空间中。 应用的每个用户都能获取总存储限制为 256 或 64 MB 的连接存储空间。 请注意，此存储仅专用于应用，不与其他应用共享。

### <a name="containers-and-blobs"></a>容器和 blob

连接存储容器（或简称容器）是基本的存储单位。 每个连接存储空间都可以包含大量容器，如下图中所示。

连接存储空间（每个游戏/计算机或每个游戏/用户）

![connected_storage_space_containers.png](../../images/connected_storage/connected_storage_space_containers.png)

 数据会作为一个或多个称为 blob 的缓冲区存储在容器中。 下图说明了磁盘上容器的内部系统表示形式。 每个容器中都有容器文件，此文件包含对容器中的每个 blob 的数据文件的引用。

容器图示

![container_storage_blobs.png](../../images/connected_storage/container_storage_blobs.png)

要在容器中存储数据，请调用相应的 API 容器方法 SubmitUpdatesAsync，提供名称和 blob（缓冲区对象）的映射。 SubmitUpdatesAsync 调用中描述的所有更改会自动应用，也就是说，所有 blob 都会根据要求更新，或者，整个操作都会终止，且容器会保持其在调用之前的状态。

使用 SubmitUpdatesAsync 的单独保存操作限制为一次 16 MB 的数据。

## <a name="connected-storage-api"></a>连接存储 API

连接存储具有适用于 XDK 和 UWP 应用的独立 API。 幸运的是，这些 API 彼此十分相似。 这两种 API 的主要不同在于其命名空间和类名称。 使用 API [保存](connected-storage-saving.md)、[加载](connected-storage-loading.md)和[删除](connected-storage-deleting.md)数据所需的函数具有相同的名称。

[将 Xbox Live 代码从 XDK 移植到 UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) 的连接存储部分详细介绍这两种连接存储 API 之间的更多差异。

可以在以下路径找到 XDK .chm 文件中介绍的 XDK 连接存储 API：**Xbox ONE XDK >> API Reference >> Platform API Reference >> System API Reference >> Windows.Xbox.Storage**。
在 [developer.microsoft.com 网站](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)也可查阅 XDK API。
XDK API 链接要求你具有启用了 Xbox 开发人员工具包 (XDK) 访问的 Microsoft 帐户 (MSA)。
Windows.Xbox.Storage 是 Xbox One 主机的连接存储命名空间的名称。

可以在以下路径找到 Xbox Live SDK .chm 文件中介绍的 UWP 连接存储 API：**Xbox Live APIs >> Xbox Live Platform Extensions SDK API Reference >> Windows.Gaming.XboxLive.Storage**。
[Windows.Gaming.XboxLive.Storage 命名空间参考](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)也联机介绍 UWP 连接存储 API。
Windows.Gaming.XboxLive.Storage 是适用于 UWP 应用的连接存储命名空间的名称。

要开始使用连接存储，需要获取连接存储*空间*。 连接存储空间与用户或计算机关联，以*容器* 和 *blob* 的形式存储所有与相应用户或计算机关联的连接存储数据。 获取用于计算机或用户的连接存储空间将允许你进行访问以从相应实体读取或写入存储数据。 要获取连接存储空间，XDK 和 UWP 游戏都将调用 `GetForUserAsync` 方法，XDK 游戏可能还会调用 `GetForMachineAsync` 方法，UWP 游戏无法调用 `GetForMachineAsync`。 `GetForUserAsync` 和 `GetForMachineAsync` 包含在 XDK 的 `ConnectedStorageSpace` 类中。 `GetForUserAsync` 包含在 UWP API 的 `GameSaveProvider` 类中。 这些方法可能是长时间运行的操作，尤其是当用户在一台设备上保存数据并且第一次在其他设备上继续游戏时。 `GetForUserAsync` 检索某一用户的连接存储空间，然后就可以用来创建、访问和删除容器。

要创建容器或访问之前创建的容器，请调用 `ConnectedStorageSpace` 或 `GameSaveProvider` 类的 `CreateContainer` 函数，这将允许你访问与用于创建容器的 `ConnectedStorageSpace` 或 `GameSaveProvider` 关联的用户或计算机的指定容器。 `ConnectedStorageSpace` 和 `GameSaveProvider` 类还包含 `DeleteContainerAsync` 函数，该函数允许你在给出要删除容器的名称时删除容器。

要更新容器中的 blob，请调用 `SubmitUpdatesAsync`（在 XDK 中的 `ConnectedStorageContainer` 类中或 UWP API 的 `GameSaveContainer` 类中）。 `SubmitUpdatesAsync` 允许你提供名称和缓冲区列表作为要写入 blob 的数据、要删除的 blob 的名称列表和调用保存容器的显示名称。 这是更新连接存储空间中的数据最终需要调用的函数。

要查看连接存储 API 使用示例，请参阅以下连接存储文章：[保存数据](connected-storage-saving.md)
[加载数据](connected-storage-loading.md)
[删除数据](connected-storage-deleting.md)

> [!NOTE]
> 安全说明：
>
> 在电脑上运行的通用 Windows 平台 (UWP) 应用将本地数据保存在本地用户可访问的位置，本质上不能保护免受用户修改。
>你应考虑对游戏保存数据应用自己的加密和有效性检查，以帮助防止用户在游戏外部修改游戏保存。
>出于同样的原因，你应决定是允许游戏的电脑和 Xbox 版本共享保存还是保持独立。

## <a name="managing-local-storage"></a>管理本地存储

针对 Xbox 开发工具包或 UWP 应用测试连接存储时，可能需要在开发设备上操作本地存储的数据。

xbstorage 是 XDK 附带的命令行工具，可用于在开发控制台上操作本地存储。
对于 UWP 开发人员，有适用于电脑的相同工具 gamesaveutil.exe，该工具随适用于 Fall Creators Update 的 Windows 10 SDK (10.0.16299.91) 和更高版本提供。

这两种工具都允许使用以下命令操作设备上的本地存储：

|命令  |说明  |
|---------|---------|
|reset    | 对连接存储执行出厂重置。 |
|import   | 将数据从指定的 XML 文件导入连接存储空间。 |
|export   | 将数据从连接存储空间导出到指定的 XML 文件。 |
|delete   | 从连接存储空间删除数据。 |
|generate | 生成虚拟数据并保存到指定的 XML 文件中。 |
|模拟 | 模拟存储空间不足的情况。 |

要了解有关 xbstorage 工具中的可用函数和 gamesaveutils.exe 的更多信息，请参阅[管理本地连接存储](connected-storage-xb-storage.md)。

## <a name="technical-overview"></a>技术概述

要更深入地了解连接存储在 XDK 上的工作方式，请参阅[连接存储技术概述](connected-storage-technical-overview.md)。 该技术概述是专门针对 XDK 开发人员编写的，但其中包含的概念普遍适用于连接存储。