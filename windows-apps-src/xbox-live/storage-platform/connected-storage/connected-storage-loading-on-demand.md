---
title: "按需加载连接存储"
author: KevinAsgari
description: "了解如何按需加载连接存储数据，而不是一次性全部加载。"
ms.assetid: a0797a14-c972-4017-864c-c6ba0d5a3363
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储"
ms.openlocfilehash: a0c1e15a163dce8e64eee177497ff8febad27e10
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="connected-storage-loading-on-demand"></a>按需加载连接存储

*GetSyncOnDemandForUserAsync* 允许你“按需”从连接存储空间加载基于云的数据，而不是一次性全部加载。 如果出现文件保存特别大的情况，这可以提高 *GetForUserAsync* 的性能。

### <a name="to-load-data-from-a-connected-storage-space-on-demand"></a>要按需从连接存储空间加载数据

1.  调用 *GetSyncOnDemandForUserAsync*。 这会触发一次部分同步，下载容器列表及其在云中的元数据，但不会触发其内容。 此操作速度很快，而且（在良好的网络条件下）用户不会看到任何加载的 UI。

2.  使用 *GetContainerInfo2Async* 执行容器查询。 这将返回 *ContainerInfo2* 的集合，此集合包含 3 个新的元数据字段。

    -   *DisplayName*：使用 *SubmitUpdatesAsync*（它会将显示名称字符串作为参数）的重载编写的任意显示名称。 我们建议你始终使用此字段存储用户友好名称。
    -   *LastModifiedTime*：上次修改此容器的时间。 注意，如果本地和远程时间戳发生冲突，请使用远程时间戳。
    -   *NeedsSync*：一个布尔值，指示此容器中是否有需要从云中下载的数据。

    你可以使用此附加元数据向用户展示有关其游戏保存的核心信息（包括名称、上次使用时间和选择一个游戏保存是否需要下载），而无需实际从云中执行完整下载。

3.  通过调用以下现有的连接存储 API 中的任意一个触发同步。

    -   *BlobInfoQueryResult::GetBlobInfoAsync*
    -   *BlobInfoQueryResult::GetItemCountAsync*
    -   *ConnectedStorageContainer::GetAsync*
    -   *ConnectedStorageContainer::ReadAsync*
    -   *ConnectedStorageSpace::DeleteContainerAsync*

    这会导致从用户选择的容器中下载数据时，用户可以看到常用的同步 UI 和进度条。 注意，只会同步选择的容器中的数据；不会下载其他容器中的数据。

    在按需同步的上下文中调用这些 API 时，这些操作全都可能生成以下新的错误代码：

    -   *ConnectedStorageErrorStatus::ContainerSyncFailed*：此错误指示操作失败，且容器无法与云同步。 最有可能的原因是，用户的网络条件导致同步失败。 在网络修复后，用户可能想要重试，或者可能会选择使用无需同步的容器。 你的 UI 应允许以下选项之一。 无需重试对话框，因为用户已经看到系统 UI 重试对话框。

    -   *ConnectedStorageErrorStatus::ContainerNotInSync*：此错误表示游戏未正确尝试写入未同步的容器。 不允许在将 NeedsSync 标志设置为 true 的容器上调用 *ConnectedStorageContainer::SubmitUpdates*。 你必须先读取容器，在能在写入之前触发同步。 如果未从容器中读取便写入容器，则游戏中很可能有 Bug，会丢失用户进度。

        此行为与用户脱机运行时的行为不同。 脱机时没有容器是否同步的指示，所以由用户决定是否稍后解决任何冲突。 但是，在这种情况下，系统知道用户需要进行同步，因此，系统不允许用户通过使用过期容器进入错误状态（不过用户仍可以在需要时重新启动游戏并脱机运行）。

4.  像平常一样使用连接存储 API 的其余部分。 按需同步时，其行为保持不变。
