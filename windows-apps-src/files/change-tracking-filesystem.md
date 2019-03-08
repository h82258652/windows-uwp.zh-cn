---
title: 后台跟踪文件系统更改
description: 介绍如何跟踪文件中的更改并在后台中的文件夹，如用户移动这些系统。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0ec7762fd64f0f0b8de65faa1aaf079bdaba3a3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621572"
---
# <a name="track-file-system-changes-in-the-background"></a>后台跟踪文件系统更改

**重要的 Api**

-   [**StorageLibraryChangeTracker**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)
-   [**StorageLibraryChangeReader**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader)
-   [**StorageLibraryChangedTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)
-   [**StorageLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)

[ **StorageLibraryChangeTracker** ](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker)类允许应用程序用户移动它们系统文件和文件夹中的更改跟踪。 使用**StorageLibraryChangeTracker**类，应用可以跟踪：

- 文件操作，包括添加、 删除、 修改。
- 文件夹重命名和删除等操作。
- 文件和文件夹移动的驱动器上。

使用本指南来了解使用更改跟踪器的编程模型，请查看一些示例代码，并了解不同类型的跟踪的文件操作**StorageLibraryChangeTracker**。

**StorageLibraryChangeTracker**适用于本地计算机的用户库，或任何文件夹。 这包括辅助驱动器或可移动驱动器，但不包括 NAS 驱动器或网络驱动器。

## <a name="using-the-change-tracker"></a>使用更改跟踪器

更改跟踪器作为系统上存储最后一个循环缓冲区*N*文件系统操作。 应用程序均可读取从缓冲区所做的更改，然后处理这些到其自己的体验。 应用程序完成的更改，它将所做的更改，为已处理并将永远不会再次看到它们。

若要在文件夹上使用更改跟踪器，请按照下列步骤：

1. 启用更改跟踪的文件夹。
2. 等待的更改。
3. 读取更改。
4. 接受的更改。

接下来的部分介绍每个包含一些代码示例的步骤。 在本文末尾提供完整的代码示例。

### <a name="enable-the-change-tracker"></a>启用更改跟踪器

应用需要执行此第一件事是告诉系统它关注的更改跟踪给定的库中。 这是通过调用[**启用**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)感兴趣的库上的更改跟踪器方法。

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

几个重要说明：

- 请确保您的应用程序之前创建到清单中正确的库拥有权限[ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)对象。 请参阅[文件的访问权限](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions)的更多详细信息。
- [**启用**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)是线程安全的将不会重置指针，并可作为很多时候根据需要 （对此进行详细更高版本） 调用。

![启用空的更改跟踪器](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>等待的更改

更改跟踪器初始化后，它将开始记录的所有操作发生在库中，即使应用程序未运行。 应用可以注册以通过注册进行了更改任意时间激活[ **StorageLibraryChangedTrigger** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger)事件。

![要添加到更改跟踪器，而无需读取这些应用的更改](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>读取所做的更改

该应用然后轮询更改跟踪器中的更改并对其检查上次接收的更改的列表。 下面的代码显示了如何以获取更改跟踪器中的更改的列表。

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

然后，应用程序负责处理到其自己的体验或数据库中根据需要更改。

![到应用程序数据库读取更改跟踪器所做的更改](images/changetracker-reading.png)

> [!TIP]
> 若要启用的第二个调用是更有效地防止争用条件时，如果用户在您的应用程序正在读取的更改时向库添加另一个文件夹。 而未额外调用**启用**代码将因 ecSearchFolderScopeViolation (0x80070490)，如果用户正在更改其库中的文件夹

### <a name="accept-the-changes"></a>接受所做的更改

应用程序完成后处理所做的更改，它应该告诉系统永远不会通过调用再次显示这些更改[ **AcceptChangesAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync)方法。

```csharp
await changeReader.AcceptChangesAsync();
```

![因此，它们将永远不会再次显示将更改作为标记读取](images/changetracker-accepting.png)

在将来读取更改跟踪器时，应用将现在仅接收新的更改。

- 如果调用之间已发生更改，则[ **ReadBatchAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync)并[AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync)，该指针将仅前进到应用程序已发现的最新更改。 这些其他的更改仍将可用它将调用下一次**ReadBatchAsync**。
- 不接受所做的更改将导致返回的所有变更集的下一步的系统时间的应用程序调用**ReadBatchAsync**。

## <a name="important-things-to-remember"></a>要记住的重要事项

在使用更改跟踪器，有几件事，您需要记住，若要确保一切正常运行。

### <a name="buffer-overruns"></a>缓冲区溢出

虽然我们保留足够的空间来保存您的应用程序可以读取它们之前在系统上发生的所有操作的更改跟踪器中，是很容易想象循环缓冲区覆盖本身之前应用程序不会读取所做的更改的位置的方案。 尤其是当用户是从备份中还原数据，或同步大量图片从照相机电话的集合。

在这种情况下， **ReadBatchAsync**将返回错误代码[ **StorageLibraryChangeType.ChangeTrackingLost**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype)。 如果您的应用程序收到此错误代码，则表示两点：

* 缓冲区已覆盖本身自上次您深入了解了它。 最佳是操作的重新爬网库，因为来自跟踪器的任何信息将不完整。
* 更改跟踪器将不会返回任何其他更改，直到您调用[**重置**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset)。 该应用程序调用重置后，指针将被移至最新更改和跟踪时恢复正常。

它应该很少见，若要获取这些情况下，但在用户在其中移动大量的围绕其磁盘上的文件的情况下，我们不希望更改跟踪器急剧增加，占用了过多存储。 这应允许应用不在 Windows 客户体验的破坏性的同时对大量文件系统操作做出响应。

### <a name="changes-to-a-storagelibrary"></a>对 StorageLibrary 的更改

[ **StorageLibrary** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary)类存在包含其他文件夹的根文件夹的虚拟组的方式。 若要解决此问题与文件系统更改跟踪器，我们进行以下选择：

- 更改跟踪器也将表示对子代的根库文件夹的任何更改。 可以使用查找根库文件夹[**文件夹**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders)属性。
- 添加或删除从根文件夹**StorageLibrary** (通过[ **RequestAddFolderAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync)并[ **RequestRemoveFolderAsync** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)) 不会更改跟踪器中创建一个条目。 可以通过跟踪这些更改[ **DefinitionChanged** ](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged)事件或通过枚举在库中使用的根文件夹[**文件夹**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders)属性。
- 如果内容中已包含的文件夹添加到库中，存在不会更改生成的通知或更改跟踪器条目。 该文件夹的后代的任何后续更改将生成通知，并更改跟踪器条目。

### <a name="calling-the-enable-method"></a>调用 Enable 方法

应用程序应调用[**启用**](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)一旦它们开始跟踪文件系统和之前所做的更改的每个枚举。 这将确保所有更改，将都捕获的更改跟踪器。  

## <a name="putting-it-together"></a>将它放在一起

下面是用于注册以视频库中的更改并开始从更改跟踪器中拉取所做的更改的所有代码。

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
