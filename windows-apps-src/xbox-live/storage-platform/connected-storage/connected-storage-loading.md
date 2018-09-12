---
title: 使用连接存储加载数据
author: aablackm
description: 了解如何使用连接存储加载数据。
ms.assetid: c660a456-fe7d-453a-ae7b-9ecaa2ba0a15
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储
ms.localizationpriority: medium
ms.openlocfilehash: 9fc67b19619f8b7cf7e873acd956c06f65491c06
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3935345"
---
# <a name="use-connected-storage-to-load-data"></a>使用连接存储加载数据

数据是使用 `ReadAsync` 或 `GetAsync` 连接存储方法异步读取的。

### <a name="to-load-data-from-connected-storage"></a>要从连接存储加载数据

1.  通过调用 `GetForUserAsync` 检索用户的 `ConnectedStorageSpace` 对象。

    在 XDK 示例中，返回的 `ConnectedStorageSpace` 会添加到映射中，以便轻松管理多个用户的 `ConnectedStorageSpace` 对象。

2.  通过对 `ConnectedStorageSpace` 对象调用 `CreateContainer` 创建 `ConnectedStorageContainer` 对象。
3.  通过对 `ConnectedStorageContainer` 调用 `ReadAsync` 或 `GetAsync` 检索数据。 `ReadAsync` 要求你在 `GetAsync` 分配新缓冲区以存储读取的数据时传递到缓冲区中。

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

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status);

// Load data from a fixed container/blob name into an IBuffer
void Load(Windows::Storage::Streams::IBuffer^ buffer)
{

    auto reads = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
    reads->Insert("data", buffer);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(gCurrentUser->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    //Save Data is Loading

    auto op = container->ReadAsync(reads->GetView());

    op->Completed = ref new AsyncActionCompletedHandler(OnLoadCompleted);
}

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status)
{
    switch (action->Status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        //Successful load logic here.
        break;

    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        //Fail logic here
        break;

    default:
        //all other possible values of action->status are also failures, alternate fail logic here. 
        break;
    }
}
```

可以在以下路径找到 XDK .chm 文件中介绍的 XDK 连接存储 API：**Xbox ONE XDK >> API Reference >> Platform API Reference >> System API Reference >> Windows.Xbox.Storage**。
在 [developer.microsoft.com 网站](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)也可查阅 XDK API。
XDK API 链接要求你具有启用了 Xbox 开发人员工具包 (XDK) 访问的 Microsoft 帐户 (MSA)。
Windows.Xbox.Storage 是 Xbox One 主机的连接存储命名空间的名称。

## <a name="c-uwp-sample"></a>C# UWP 示例

虽然 XDK 游戏和 UWP 应用可能使用不同的 API，但 UWP API 外观与 XDK API 非常相似。 要加载数据，你仍然需要按照相同的基本步骤进行操作，并注意某些命名空间和类名称更改。 不使用命名空间 `Windows::Xbox::Storage`，而将使用 `Windows.Gaming.XboxLive.Storage`。 类 `ConnectedStorageSpace` 相当于 `GameSaveProvider`。 类 `ConnectedStorageContainer` 相当于 `GameSaveContainer`。 在[将 Xbox Live 代码从 XDK 移植到 UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) 的连接存储部分进一步详细介绍了这些更改。

```csharp
//Namespace Required
Windows.Gaming.XboxLive.Storage

//Get The User
var users = await Windows.System.User.FindAllAsync();

int intData = 0;
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
    //throw new Exception("Game Save Provider Initialization failed");;
}

//Now you have a GameSaveProvider
//Next you need to call CreateContainer to get a GameSaveContainer

GameSaveContainer gameSaveContainer = gameSaveProvider.CreateContainer(c_saveContainerName);
//Parameter
//string name (name of the GameSaveContainer Created)

//form an array of strings containing the blob names you would like to read.
string[] blobsToRead = new string[] { c_saveBlobName };

// GetAsync allocates a new Dictionary to hold the retrieved data. You can also use ReadAsync
// to provide your own preallocated Dictionary.
GameSaveBlobGetResult result = await container.GetAsync(blobsToRead);

int loadedData = 0;

//Check status to make sure data was read from the container
if(result.Status == GameSaveErrorStatus.Ok)
{
    //prepare a buffer to receive blob
    IBuffer loadedBuffer;

    //retrieve the named blob from the GetAsync result, place it in loaded buffer.
    result.Value.TryGetValue(c_saveBlobName, out loadedBuffer);

    if(loadedBuffer == null)
    {

        throw new Exception(String.Format("Didn't find expected blob \"{0}\" in the loaded data.", c_saveBlobName));

    }
    DataReader reader = DataReader.FromBuffer(loadedBuffer);
    loadedData = reader.ReadInt32();
}
```

在 [Xbox Live API 参考](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)中可以查阅适用于 UWP 应用的连接存储 API。
有关使用连接存储的其他示例，请查看 [Xbox Live API 示例游戏保存项目](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave)。