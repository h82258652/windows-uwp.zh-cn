---
title: "使用连接存储保存数据"
author: KevinAsgari
description: "了解如何使用连接存储保存数据。"
ms.assetid: ccf7488c-5d55-480e-b3aa-412220d03104
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Xbox live, xbox, 游戏, uwp, windows 10, xbox one"
ms.openlocfilehash: 0da3e79c0a89575504a1cd3e772c10158b34eb6c
ms.sourcegitcommit: 2b436dc5e5681b8884e0531ee303f851a3e3ccf2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="use-connected-storage-to-save-data"></a>使用连接存储保存数据


通过在用户的 *ConnectedStorageSpace* 中创建 *ConnectedStorageContainer*，并在容器中调用 *SubmitUpdatesAsync* 方法，异步保存数据。

| 重要提示                                                                                                                                                                                                                                                                                                                                                                        |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 连接存储容器中的数据依赖项是不安全的。 例如，将一个容器上传到云可能会完成，但是其他容器可能就要保持排队等待上传。 如果用户移动到其他控制台中，同步操作会允许第一个容器在第二个控制台上进行同步和访问，而不需要显示第一个容器。 |

### <a name="to-save-data-to-connected-storage"></a>要将数据保存到连接存储

1.  通过调用 *GetForUserAsync* 为用户检索 *ConnectedStorageSpace* 对象。

    在此示例中，返回的 *ConnectedStorageSpace* 对象会添加到映射，以便于管理多个用户的 *ConnectedStorageSpace* 对象。

2.  通过在 *ConnectedStorageSpace* 对象上调用 *CreateContainer*，创建 *ConnectedStorageContainer* 对象。
3.  在 *ConnectedStorageContainer* 对象上调用 *SubmitUpdatesAsync*。

## <a name="sample"></a>示例

```cpp
Platform::Guid gPrimarySCID;
Platform::Guid gSecondarySCID;
Windows::Xbox::Storage::ConnectedStorageSpace^ gConnectedStorageSpaceForMachine;
enum LoadSaveState { LOADING, LOAD_COMPLETED, LOAD_FAILED, NO_SAVE_MODE, RETRY_LOAD, GETTING_STORAGE_SPACE, DELETE_SAVE_UI, SAVING, SAVE_COMPLETED, NONE };
LoadSaveState loadSaveState = LoadSaveState::NONE;
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

void SetGameState(LoadSaveState state) {loadSaveState = state;}
bool GetUserInputYesOrNo() {return true;};

User^ gCurrentUser;
byte* GetBufferData(Windows::Storage::Streams::IBuffer^ buffer);
IBuffer^ WrapRawBuffer( void* ptr, size_t size );

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
            SetGameState(LoadSaveState::NO_SAVE_MODE);
          else
            SetGameState(LoadSaveState::RETRY_LOAD);
          break;
      }
    });
}

uint8* GetBufferPointer(IBuffer^ buffer);




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

bool gSaveInProgress;
void SaveCheckpoint(Windows::Storage::Streams::IBuffer^ buffer, User^ user);

void RenderSpinner() {};
bool ItIsTimeToSaveACheckpoint() {return true;};

void RenderOneFrame()
{
    // ...

    if (gSaveInProgress)
        RenderSpinner();

    // ...
}

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

     SetGameState(LoadSaveState::SAVING);
     //gSaveInProgress = true;

     op->Completed = ref new AsyncActionCompletedHandler(
               [=](IAsyncAction^ a, Windows::Foundation::AsyncStatus status){
                   SetGameState(LoadSaveState::SAVE_COMPLETED);
     });
}
```
