---
title: 跟踪在后台中的文件系统更改
description: 介绍了如何跟踪文件中的更改，并在后台中的文件夹，如用户移动这些系统。
ms.date: 11/20/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e90692753924572a932767b9c188ed6d24f94593
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2018
ms.locfileid: "8757937"
---
# <a name="track-file-system-changes-in-the-background"></a>跟踪在后台中的文件系统更改

[StorageLibraryChangeTracker](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)类允许应用跟踪文件和文件夹中的更改，如用户移动这些系统。 使用**StorageLibraryChangeTracker**类，可以跟踪应用：

- 文件操作包括添加、 删除、 修改。
- 文件夹重命名和删除等操作。
- 文件和文件夹在驱动器上移动。

使用本指南以了解使用更改跟踪器的编程模型，查看一些示例代码，并了解不同类型的由**StorageLibraryChangeTracker**跟踪的文件操作。

**StorageLibraryChangeTracker**适用于本地计算机的用户的库，或任何文件夹。 这包括辅助驱动器或可移动驱动器，但不是包括 NAS 驱动器或网络驱动器。

## <a name="using-the-change-tracker"></a>使用更改跟踪器

更改跟踪器将在系统上实现为循环缓冲区存储的最后一个*N*文件系统操作。 应用将能够读取到缓冲区的其他地方所做的更改，然后为他们自己的体验中处理它们。 一旦应用完成所做的更改将更改为已处理，并将永远不会再次看到它们。

若要使用的文件夹上更改跟踪器，请按照下列步骤：

1. 启用更改跟踪的文件夹。
2. 等待更改。
3. 读取更改。
4. 接受更改。

接下来的部分介绍了每个步骤的一些代码示例。 在本文末尾提供完整代码示例。

### <a name="enable-the-change-tracker"></a>启用更改跟踪器

应用需要进行第一件事是告诉系统它希望更改跟踪给定的库中。 通过调用[启用](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)上更改跟踪器感兴趣的库执行此操作。

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

几个重要说明：

- 请确保你的应用有权限访问清单中正确的库之前创建[StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)对象。 有关详细信息，请参阅[文件访问权限](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions)。
- [启用](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)是线程安全，不会重置你的指针，并可根据你喜欢 （更对此进行更高版本） 多次调用。

![启用空更改跟踪器](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>等待更改

更改跟踪器初始化后，它将开始录制的所有操作时发生的在库中，即使应用未运行。 应用可以注册要激活的随时发生更改时通过[StorageLibraryChangedTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)事件注册。

![无需应用读取它们添加到更改跟踪器的更改](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>阅读所做的更改

应用可以则轮询从更改跟踪器的更改并对其检查上次后接收的更改列表。 下面的代码显示了如何从更改跟踪器获取的更改列表。

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

然后，应用负责处理到它自己的体验或数据库，根据需要更改。

![更改跟踪器到应用数据库读取所做的更改](images/changetracker-reading.png)

> [!TIP]
> 若要启用在第二个调用旨在帮助防范争用情况，如果用户在你的应用读取更改时向库添加另一个文件夹。 无需额外调用**启用**代码将失败，并 ecSearchFolderScopeViolation (0x80070490) 如果用户更改其库中的文件夹

### <a name="accept-the-changes"></a>接受所做的更改

应用完成后处理所做的更改，它应该告诉系统永远不会通过调用[AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync)方法再次显示这些更改。

```csharp
await changeReader.AcceptChangesAsync();
```

![因此永远不会再显示标记更改作为读取](images/changetracker-accepting.png)

在未来读取更改跟踪器时，应用将现在仅收到新的更改。

- 如果调用[ReadBatchAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync)和[AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync)之间发生了更改，该指针将只能看到该应用具有最新更改高级。 这些其他更改仍将的下次调用**ReadBatchAsync**。
- 不会接受所做的更改将导致系统返回的下次应用调用**ReadBatchAsync**的同一组更改。

## <a name="important-things-to-remember"></a>要记住的重要事项

在使用更改跟踪器时，有应请记住，以确保一切正常工作的一些事项。

### <a name="buffer-overruns"></a>缓冲区溢出

尽管我们尝试保留足够的空间中更改跟踪器以保留在系统上发生，直到你的应用可以读取它们的所有操作，但它是很容易想象循环缓冲区覆盖本身之前应用不会读取所做的更改的其中一个方案。 尤其是当用户从备份还原数据或同步大量从相机手机的图片的集合。

在此情况下， **ReadBatchAsync**将返回的错误代码[StorageLibraryChangeType.ChangeTrackingLost](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype)。 如果应用收到此错误代码，这意味着的几个事项：

* 缓冲区已覆盖本身自上次看它。 操作的最佳做法是重新爬网库中，因为从跟踪器的任何信息都将不完整。
* 更改跟踪器不会返回任何更改，直到你调用[重置](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset)。 应用调用重置后，指针将移至最近的更改，并跟踪以正常方式将继续。

应该很少见获取这些情况下，但在用户正在大量周围其磁盘上的文件的情况下，我们不希望更改跟踪器提示框并占用太多存储。 这应该允许应用响应不在 Windows 中的客户体验的破坏性的同时极大的文件系统操作。

### <a name="changes-to-a-storagelibrary"></a>对 StorageLibrary 更改

作为虚拟的一组包含其他文件夹的根文件夹存在[StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)类。 这对帐文件系统更改跟踪器，我们需要进行以下几种选择：

- 更改跟踪器中将表示对下级根库文件夹的任何更改。 可以使用[文件夹](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders)属性找到根库文件夹。
- 添加或删除从**StorageLibrary** （通过[RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync)和[RequestRemoveFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)） 的根文件夹不会创建一项更改跟踪器中。 通过[DefinitionChanged](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged)事件或枚举中使用[文件夹](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders)属性的库的根文件夹，则可以跟踪这些更改。
- 如果已经包含在其中的内容文件夹添加到库中，将不会出现更改通知或更改生成的跟踪条目。 对该文件夹的后代任何后续更改将生成通知，并将更改跟踪条目。

### <a name="calling-the-enable-method"></a>调用启用方法

应用应立即启动跟踪文件系统和更改的每个枚举之前调用[启用](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)。 这将确保所有更改将在都捕获更改跟踪器。  

## <a name="putting-it-together"></a>将它放在一起

下面是用于注册的视频库中更改和开始菜单中更改跟踪器拉取所做的更改的所有代码。

```csharp
private async void EnableChangeTracker()
{
    StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
    videoTracker.Enable();
}

private async void GetChanges()
{
    StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
    videosLibrary.ChangeTracker.Enable();
    StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
    IReadOnlyList changeSet = await changeReader.ReadBatchAsync();


    //Below this line is for the blog post. Above the line is for the magazine
    foreach (StorageLibraryChange change in changeSet)
    {
        if (change.ChangeType == StorageLibraryChangeType.ChangeTrackingLost)
        {
            //We are in trouble. Nothing else is going to be valid.
            log("Resetting the change tracker");
            videosLibrary.ChangeTracker.Reset();
            return;
        }
        if (change.IsOfType(StorageItemTypes.Folder))
        {
            await HandleFileChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.File))
        {
            await HandleFolderChange(change);
        }
        else if (change.IsOfType(StorageItemTypes.None))
        {
            if (change.ChangeType == StorageLibraryChangeType.Deleted)
            {
                RemoveItemFromDB(change.Path);
            }
        }
    }
    await changeReader.AcceptChangesAsync();
}
```
