---
title: "使用连接存储加载数据"
author: KevinAsgari
description: "了解如何使用连接存储加载数据。"
ms.assetid: c660a456-fe7d-453a-ae7b-9ecaa2ba0a15
ms.author: kevinasg
ms.date: 04-04-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "xbox live, xbox, 游戏, uwp, windows 10, xbox one, 连接存储"
ms.openlocfilehash: c683e7219c5c3dfd3a7e4d5cbd91aba270df75c7
ms.sourcegitcommit: 2b436dc5e5681b8884e0531ee303f851a3e3ccf2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/18/2017
---
# <a name="use-connected-storage-to-load-data"></a>使用连接存储加载数据

使用 *ReadAsync* 或 *GetAsync* 连接存储方法异步读取数据。

### <a name="to-load-data-from-connected-storage"></a>要从连接存储加载数据

1.  通过调用 *GetForUserAsync* 检索用户的 *ConnectedStorageSpace*。

    在此示例中，返回的 *ConnectedStorageSpace* 会添加到映射，以便于管理多个用户的 *ConnectedStorageSpace* 对象。

2.  通过在 *ConnectedStorageSpace* 上调用 *CreateContainer*，创建 *ConnectedStorageContainer*。
3.  通过在 *ConnectedStorageContainer* 上调用 *ReadAsync* 或 *GetAsync* 检索数据。 *ReadAsync* 要求你在 *GetAsync* 分配新缓冲区以存储读取的数据时传递到缓冲区中。

## <a name="sample"></a>示例

```cpp
Platform::Guid gPrimarySCID;
Platform::Guid gSecondarySCID;
Windows::Xbox::Storage::ConnectedStorageSpace^ gConnectedStorageSpaceForMachine;
enum LoadSaveState { LOADING, LOAD_COMPLETED, LOAD_FAILED, NO_SAVE_MODE, RETRY_LOAD, GETTING_STORAGE_SPACE, DELETE_SAVE_UI, SAVING, SAVE_COMPLETED, NONE };
LoadSaveState loadSaveState = LoadSaveState::NONE;
auto gConnectedStorageSpaceForUsers = ref new Platform::Collections::Map<unsigned int, Windows::Xbox::Storage::ConnectedStorageSpace^>();

void SetGameState(LoadSaveState state) { loadSaveState = state; }
bool GetUserInputYesOrNo() { return true; };

User^ gCurrentUser;
byte* GetBufferData(Windows::Storage::Streams::IBuffer^ buffer);
IBuffer^ WrapRawBuffer(void* ptr, size_t size);

void PrepareConnectedStorage(User^ user)
{
    auto op = ConnectedStorageSpace::GetForUserAsync(user);
    op->Completed = ref new AsyncOperationCompletedHandler<ConnectedStorageSpace^>(
        [=](IAsyncOperation<ConnectedStorageSpace^>^ operation, Windows::Foundation::AsyncStatus status)
    {
        switch (status)
        {
        case Windows::Foundation::AsyncStatus::Completed:
            gConnectedStorageSpaceForUsers->Insert(user->Id, operation->GetResults());
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
}

extern void SetGameState(LoadSaveState state);

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status);

// Load data from a fixed container/blob name into an IBuffer
void Load(Windows::Storage::Streams::IBuffer^ buffer)
{

    auto reads = ref new Platform::Collections::Map<Platform::String^, Windows::Storage::Streams::IBuffer^>();
    reads->Insert("data", buffer);

    auto storageSpace = gConnectedStorageSpaceForUsers->Lookup(gCurrentUser->Id);
    auto container = storageSpace->CreateContainer("Saves/Checkpoint");

    SetGameState(LoadSaveState::LOADING);

    auto op = container->ReadAsync(reads->GetView());

    op->Completed = ref new AsyncActionCompletedHandler(OnLoadCompleted);
}

void OnLoadCompleted(IAsyncAction^ action, Windows::Foundation::AsyncStatus status)
{
    switch (action->Status)
    {
    case Windows::Foundation::AsyncStatus::Completed:
        SetGameState(LoadSaveState::LOAD_COMPLETED);
        break;

    case Windows::Foundation::AsyncStatus::Error:
    case Windows::Foundation::AsyncStatus::Canceled:
        SetGameState(LoadSaveState::LOAD_FAILED);
        break;

    default:
        SetGameState(LoadSaveState::LOAD_FAILED);
        break;
    }
}
```
