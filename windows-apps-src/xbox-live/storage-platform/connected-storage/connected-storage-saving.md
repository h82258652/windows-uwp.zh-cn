---
title: 使用连接存储保存数据
author: aablackm
description: 了解如何使用连接存储保存数据。
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.author: aablackm
ms.date: 02/27/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储
ms.localizationpriority: medium
ms.openlocfilehash: 1705af67d1bfe89e8b91ee60bc6c52cf9e50300b
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/22/2018
ms.locfileid: "5401878"
---
# <a name="use-connected-storage-to-save-data"></a>使用连接存储保存数据


数据是通过在用户的 `ConnectedStorageSpace` 中创建 `ConnectedStorageContainer` 并对容器调用 `SubmitUpdatesAsync` 方法异步保存的。

> [!IMPORTANT]
> 跨连接存储容器的数据依赖关系是不安全的。 例如，将一个容器上传到云可能会完成，但是其他容器可能就要保持排队等待上传。 如果用户移动到其他控制台中，同步操作会允许第一个容器在第二个控制台上进行同步和访问，而不需要显示第一个容器。

## <a name="to-save-data-to-connected-storage"></a>将数据保存到连接存储

1.  通过调用 `GetForUserAsync` 检索用户的 `ConnectedStorageSpace` 对象。

    在 XDK 示例中，返回的 `ConnectedStorageSpace` 对象会添加到映射中，以便轻松管理多个用户的 `ConnectedStorageSpace` 对象。

2.  通过对 `ConnectedStorageSpace` 对象调用 `CreateContainer` 创建 `ConnectedStorageContainer` 对象。
3.  使用你的游戏保存数据 blob 作为 `blobsToWrite` 参数，对 `ConnectedStorageContainer` 调用 `SubmitUpdatesAsync`。

## <a name="c-xdk-sample"></a>C++ XDK 示例

```cpp
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

bool GetUserInputYesOrNo() {return true;};

User^ gCurrentUser;
IBuffer^ WrapRawBuffer( void* ptr, size_t size );

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

enum Color { RED, BLUE };
enum EngineSize { BIG, SMALL };

struct CarData
{
    Color color;
    bool hasWheels;
    bool hasFancyRims;
    EngineSize engineSize;
};


const int MAX_CARS = 10;

struct SaveData
{
    CarData cars[MAX_CARS];
    int numCars;
    int currentCar;
    int cash;
};

SaveData gMySaveData;

void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user);

bool ItIsTimeToSaveACheckpoint() {return true;};

void Update()
{
    if (ItIsTimeToSaveACheckpoint())
        SaveCheckpoint(WrapRawBuffer(&gMySaveData,sizeof(SaveData)),gCurrentUser);
}


void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user)
{
     auto storageSpace = gConnectedStorageSpaceForUsers->Lookup( user->Id );

     auto container = storageSpace->CreateContainer("Saves/Checkpoint");

     auto updates = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
     updates->Insert("data", buffer);

     auto op = container->SubmitUpdatesAsync(updates->GetView(), nullptr);

     //Save is happening here asynchronously

     op->Completed = ref new AsyncActionCompletedHandler(
               [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
                   //Save function has completed
                   //This area can be filled with further post save logic.
     });
}
```

可以在以下路径找到 XDK .chm 文件中介绍的 XDK 连接存储 API：**Xbox ONE XDK >> API Reference >> Platform API Reference >> System API Reference >> Windows.Xbox.Storage**。
在 [developer.microsoft.com 网站](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n)也可查阅 XDK API。
XDK API 链接要求你具有启用了 Xbox 开发人员工具包 (XDK) 访问的 Microsoft 帐户 (MSA)。
Windows.Xbox.Storage 是 Xbox One 主机的连接存储命名空间的名称。

## <a name="c-uwp-sample"></a>C# UWP 示例

虽然 XDK 游戏和 UWP 应用可能使用不同的 API，但 UWP API 外观与 XDK API 非常相似。 要保存数据，你仍然需要按照相同的基本步骤进行操作，并注意某些命名空间和类名称更改。 不使用命名空间 `Windows::Xbox::Storage`，而将使用 `Windows.Gaming.XboxLive.Storage`。 类 `ConnectedStorageSpace` 相当于 `GameSaveProvider`。 类 `ConnectedStorageContainer` 相当于 `GameSaveContainer`。 在[将 Xbox Live 代码从 XDK 移植到 UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) 的连接存储部分进一步详细介绍了这些更改。

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
//string name

// To store a value in the container, it needs to be written into a buffer, then stored with
// a blob name in a Dictionary.

DataWriter writer = new DataWriter();

writer.WriteInt32(intData); //some number you want to save, in this case 23.

IBuffer dataBuffer = writer.DetachBuffer();

var blobsToWrite = new Dictionary<string, IBuffer>();

blobsToWrite.Add(c_saveBlobName, dataBuffer);

GameSaveOperationResult gameSaveOperationResult = await gameSaveContainer.SubmitUpdatesAsync(blobsToWrite, null, c_saveContainerDisplayName);
//IReadOnlyDictionary<String, IBuffer> blobsToWrite
//IEnumerable<string> blobsToDelete
//string displayName
```

在 [Xbox Live API 参考](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage)中可以查阅适用于 UWP 应用的连接存储 API。
有关使用连接存储的其他示例，请查看 [Xbox Live API 示例游戏保存项目](https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK/GameSave)。