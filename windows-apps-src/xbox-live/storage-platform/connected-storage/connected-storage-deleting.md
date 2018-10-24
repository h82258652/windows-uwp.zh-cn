---
title: 使用连接存储删除数据
author: aablackm
description: 了解如何使用连接存储删除 blob 和容器数据。
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储
ms.localizationpriority: medium
ms.openlocfilehash: a90a880d1f18ff692c388993e0df6da81fa7966e
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2018
ms.locfileid: "5432907"
---
# <a name="use-connected-storage-to-delete-data"></a>使用连接存储删除数据

数据 blob 是通过在用户的 `ConnectedStorageSpace` 中创建 `ConnectedStorageContainer` 并对容器调用 `SubmitUpdatesAsync` 方法，为 blobsToDelete 参数提供表示要删除的指定 blob 的字符串来异步删除的。

数据容器是通过删除 `ConnectedStorageContainer` 并调用其 `DeleteContainerAsync` 方法异步删除的。

## <a name="to-delete-blob-data-from-connected-storage"></a>从连接存储删除 blob 数据

1.  通过调用 `GetForUserAsync` 检索用户的 `ConnectedStorageSpace` 对象。

    在 XDK 示例中，返回的 `ConnectedStorageSpace` 对象会添加到映射中，以便轻松管理多个用户的 `ConnectedStorageSpace` 对象。

2.  通过对 `ConnectedStorageSpace` 对象调用 `CreateContainer` 创建 `ConnectedStorageContainer` 对象。
3.  `ConnectedStorageContainer` 对象调用 `SubmitUpdatesAsync`。

## <a name="to-delete-a-container-from-connected-storage"></a>从连接存储删除容器

1.  通过调用 `GetForUserAsync` 检索用户的 `ConnectedStorageSpace` 对象。

    在 XDK 示例中，返回的 `ConnectedStorageSpace` 对象会添加到映射中，以便轻松管理多个用户的 `ConnectedStorageSpace` 对象。

2.  调用 ConnectedStorageSpace 方法的 `DeleteContainerAsync` 方法。

## <a name="c-xdk-sample"></a>C++ XDK 示例
```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() { return true; };

User^ gCurrentUser;
IBuffer^ WrapRawBuffer(void* ptr, size_t size);

// Acquire a Connected Storage space for a user. A Connected Storage space is required to manipulate Connected Storage Data.
void PrepareConnectedStorage(User^ user)
{
  auto op = ConnectedStorageSpace::GetForUserAsync(user);
  op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
    [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
    {
      switch(status)
      {
        case Windows::Foundation::AsyncStatus::Completed:
          gConnectedStorageSpaceForUsers->Insert(user->Id, operation->GetResults());
          break;
        case Windows::Foundation::AsyncStatus::Error:
        case Windows::Foundation::AsyncStatus::Canceled:
          // Present user option: ?Would you like to continue without saving progress??
          if( GetUserInputYesOrNo() )
            //If the users opts yes, continue in offline mode
          else
            //If the users opts no, retry.
          break;
      }
    });
}

// Delete data from a fixed container/blob name into an IBuffer
void DeleteBlob(User^ user)
{

    //Create a list of blob names to delete
    std::vector<Platform::String^> blobsToDelete;

    //Add the blob name to your Generic Iterable
    blobsToDelete.push_back(L"someBlobName");

    auto blobNames = ref new Platform::Collections::VectorView<Platform::String^>(blobsToDelete);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Delete is occurring asynchronously at this point.

    auto op = container->SubmitUpdatesAsync(nullptr, blobNames);

    op->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });
}

void DeleteContainer(User^ user)
{
    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(user->Id);

    auto deleteOperation = storageSpace->DeleteContainerAsync("Saves/Checkpoint");

    deleteOperation->Completed = ref new AsyncActionCompletedHandler(
        [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
            switch(status)
            {
                case Windows::Foundation::AsyncStatus::Completed:
                //Delete was successful no need for action
                break;
                case Windows::Foundation::AsyncStatus::Error:
                case Windows::Foundation::AsyncStatus::Canceled:
                // Handle failed deletion
            }
     });

}
```

可以在以下路径找到 XDK .chm 文件中介绍的 XDK 连接存储 API：**Xbox ONE XDK >> API Reference >> Platform API Reference >> System API Reference >> Windows.Xbox.Storage**。
在 [developer.microsoft.com 网站](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)也可查阅 XDK API。
XDK API 链接要求你具有启用了 Xbox 开发人员工具包 (XDK) 访问的 Microsoft 帐户 (MSA)。
Windows.Xbox.Storage 是 Xbox One 主机的连接存储命名空间的名称。


## <a name="c-uwp-sample"></a>C# UWP 示例

虽然 XDK 游戏和 UWP 应用可能使用不同的 API，但 UWP API 外观与 XDK API 非常相似。 要删除数据，你仍然需要按照相同的基本步骤进行操作，并注意某些命名空间和类名称更改。 不使用命名空间 `Windows::Xbox::Storage`，而将使用 `Windows.Gaming.XboxLive.Storage`。 类 `ConnectedStorageSpace` 相当于 `GameSaveProvider`。 类 `ConnectedStorageContainer` 相当于 `GameSaveContainer`。 在[将 Xbox Live 代码从 XDK 移植到 UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) 的连接存储部分进一步详细介绍了这些更改。

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 23;
const string c_saveBlobName = "Jersey";
const string c_saveContainerDisplayName = "GameSave";
const string c_saveContainerName = "GameSaveContainer";
GameSaveProvider gameSaveProvider;

GameSaveProviderGetResult gameSaveTask = await GameSaveProvider.GetForUserAsync(users[0], context.AppConfig.ServiceConfigurationId);
//Parameters
//Windows.System.User user
//string SCID

if(gameSaveTask.Status == GameSaveErrorStatus.Ok)
{
    gameSaveProvider = gameSaveTask.Value;
}
else
{
    return;
    //throw new Exception("Game Save Provider Initialization failed");
}

//Now you have a GameSaveProvider (formerly ConnectedStorageSpace)
//Next you need to call CreateContainer to get a GameSaveContainer (formerly ConnectedStorageContainer)

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName); // this will create a new named game save container with the name = to the input name
//Parameter
//string name (name of container to be created) 

//DELETE A BLOB
//form an array of strings containing the blob names you would like to delete.
string[] blobsToDelete = new string[] { c_saveBlobName };

//Call SubmitUpdatesAsync with the dictionary of the names of blobs to delete as the second parameter.
GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(null, blobsToDelete, c_saveContainerDisplayName);
//Parameters
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName

//Check status of the operation to ensure successful delete.
if(gameSaveOperationResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}

//DELETE A SAVE CONTAINER
GameSaveOperationResult deleteContainerResult = await gameSaveProvider.DeleteContainerAsync(c_saveContainerName);
//Parameter
//string name (name of container to be deleted)

//Check status of the operation to ensure successful delete.
if(deleteContainerResult.Status == GameSaveErrorStatus.Ok)
{
    //Successful Deletion Logic
}
else
{
    //handle failed deletion logic. 
}
```

在 [Xbox Live API 参考](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)中可以查阅适用于 UWP 应用的连接存储 API。
有关使用连接存储的其他示例，请查看 [Xbox Live API 示例游戏保存项目](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave)。
