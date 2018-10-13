---
title: 按需加载连接存储
author: aablackm
description: 了解如何按需加载连接存储数据，而不是一次性全部加载。
ms.assetid: a0797a14-c972-4017-864c-c6ba0d5a3363
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储
ms.localizationpriority: medium
ms.openlocfilehash: 62aa90caa899a25fc25b728c0e60cb2db2013c97
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/12/2018
ms.locfileid: "4572670"
---
# <a name="connected-storage-loading-on-demand"></a>按需加载连接存储

`GetSyncOnDemandForUserAsync` 允许“按需”从连接存储空间加载基于云的数据，而不是一次性全部加载。 如果出现文件保存特别大的情况，这可以提高 `GetForUserAsync` 的性能。

## <a name="to-load-data-from-a-connected-storage-space-on-demand"></a>要按需从连接存储空间加载数据

### <a name="1--call-getsyncondemandforuserasync"></a>1：调用 `GetSyncOnDemandForUserAsync`

这会触发一次部分同步，下载容器列表及其在云中的元数据，但不会触发其内容。 此操作速度很快，而且（在良好的网络条件下）用户不会看到任何加载的 UI。

```cpp
auto op = ConnectedStorageSpace::GetSyncOnDemandForUserAsync(user);
op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
{
    switch (status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        auto storageSpace = operation->GetResults();
        break;
    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        // Present user option: ?Would you like to continue without saving progress??
        if (GetUserInputYesOrNo())
            SetGameState(LoadSaveState::NO_SAVE_MODE);
        else
            SetGameState(LoadSaveState::RETRY_LOAD);
        break;
    }
});
```

```csharp
var users = await Windows.System.User.FindAllAsync();

GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetSyncOnDemandForUserAsync(users[0], context.AppConfig.ServiceConfigurationId); 
//Paramaters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
```


### <a name="2--perform-a-container-query-using-getcontainerinfo2async"></a>2：使用 `GetContainerInfo2Async` 执行容器查询

这将返回 `ContainerInfo2` 的集合，此集合包含 3 个新的元数据字段：

    -   `DisplayName`：使用 `SubmitUpdatesAsync`（它会将显示名称字符串作为参数）的重载编写的任意显示名称。 我们建议你始终使用此字段存储用户友好名称。
    -   `LastModifiedTime`：此容器的上次修改时间。 注意，如果本地和远程时间戳发生冲突，请使用远程时间戳。
    -   `NeedsSync`：布尔值，指示此容器中是否有需要从云中下载的数据。

    你可以使用此附加元数据向用户展示有关其游戏保存的核心信息（包括名称、上次使用时间和选择一个游戏保存是否需要下载），而无需实际从云中执行完整下载。

```cpp
auto containerQuery = storageSpace->CreateContainerInfoQuery(nullptr); //return list of containers in ConnectedStorageSpace
auto queryOperation = containerQuery->GetContainerInfo2Async();

queryOperation->Completed = ref new AsyncOperationCompletedHandler<IVectorView<ContainerInfo2>^ >( 
    [=] (IAsyncOperation<IVectorView<ContainerInfo2>^ >^ operation, Windows::Foundation::AsyncStatus status)
    {
        switch (status)
        {
        case Windows::Foundation::AsyncStatus::Completed:
            // get the resulting vector of container info
            auto infoVector = operation->GetResults();
            break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
            // handle error cases
            break;
        }
    });
```

```csharp
GameSaveContainerInfoQuery infoQuery = gameSaveProvider.createContainerInfoQuery();
GameSaveContainerInfoGetResult containerInfoResult = await infoQuery.GetContainerInfoAsync();
var containerInfoList;

if(containerInfoResult.Status == GameSaveErrorStatus.Ok)
{
    containerInfoList = containerInfoResult.value;
}

// Use the containerInfoList to inform further actions or display container data to user. 
```

### <a name="3--trigger-a-sync"></a>3：触发同步

通过调用以下现有的连接存储 API 中的任意可触发连接存储同步：

**C++**

    -   BlobInfoQueryResult::GetBlobInfoAsync
    -   BlobInfoQueryResult::GetItemCountAsync
    -   ConnectedStorageContainer::GetAsync
    -   ConnectedStorageContainer::ReadAsync
    -   ConnectedStorageSpace::DeleteContainerAsync

**C#**

    -   GameSaveBlobInfoQuery.GetBlobInfoAsync
    -   GameSaveBlobInfoQuery.GetItemCountAsync
    -   GameSaveContainer.GetAsync
    -   GameSaveContainer.ReadAsync
    -   GameSaveProvider.DeleteContainerAsync

这会导致从用户选择的容器中下载数据时，用户可以看到常用的同步 UI 和进度条。 注意，只会同步选择的容器中的数据；不会下载其他容器中的数据。

在按需同步的上下文中调用这些 API 时，这些操作全都可能生成以下新的错误代码：

-   `ConnectedStorageErrorStatus::ContainerSyncFailed`（UWP C# API 中的 `GameSaveErrorStatus.ContainerSyncFailed`）：此错误指示操作失败，且容器无法与云同步。 最有可能的原因是，用户的网络条件导致同步失败。 在网络修复后，用户可能需要重试，或者选择使用无需同步的容器。UI 应允许以下选项之一。 无需重试对话框，因为用户已经看到系统 UI 重试对话框。

-   `ConnectedStorageErrorStatus::ContainerNotInSync`（UWP C# API 中的 `GameSaveErrorStatus.ContainerNotInSync`）：此错误指示游戏不正确地尝试写入未同步的容器。 不允许对 NeedsSync 标记设置为 true 的容器调用 `ConnectedStorageContainer::SubmitUpdatesAsync`（在 UWP C# API 中为 `GameSaveContainer.SubmitUpdatesAsync`）。 你必须先读取容器，在能在写入之前触发同步。 如果未从容器中读取便写入容器，则游戏中很可能有 Bug，会丢失用户进度。

此行为与用户脱机运行时的行为不同。 脱机时没有容器是否同步的指示，所以由用户决定是否稍后解决任何冲突。 但是，在这种情况下，系统知道用户需要进行同步，因此，系统不允许用户通过使用过期容器进入错误状态（不过用户仍可以在需要时重新启动游戏并脱机运行）。

### <a name="4--use-the-rest-of-the-connected-storage-api-as-normal"></a>4：像平常一样使用连接存储 API 的其余部分。

按需同步时，连接存储行为保持不变。
