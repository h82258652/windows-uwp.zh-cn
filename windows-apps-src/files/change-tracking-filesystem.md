---
title: 在后台跟踪文件系统更改
description: 描述用户在系统中移动文件和文件夹时，如何在后台跟踪文件和文件夹中的更改。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b0ec7762fd64f0f0b8de65faa1aaf079bdaba3a3
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "63807021"
---
# <a name="track-file-system-changes-in-the-background"></a>在后台跟踪文件系统更改

**重要的 API**

-   [StorageLibraryChangeTracker](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) 
-   [StorageLibraryChangeReader](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader) 
-   [StorageLibraryChangedTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) 
-   [StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) 

通过 [StorageLibraryChangeTracker](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibraryChangeTracker) 类，应用可在用户在系统中移动文件和文件夹时跟踪文件和文件夹中的更改  。 使用 StorageLibraryChangeTracker 类，应用可以跟踪内容  ：

- 添加、删除、修改等文件操作。
- 重命名和删除等文件夹操作。
- 驱动器上移动的文件和文件夹。

使用本指南学习使用更改跟踪器的编程模型，查看部分示例代码，并了解由 StorageLibraryChangeTracker 跟踪的不同类型的文件操作  。

StorageLibraryChangeTracker 适用于用户库或本地计算机上的任何文件夹  。 这包括辅助驱动器或可移动驱动器，但不包括 NAS 驱动器或网络驱动器。

## <a name="using-the-change-tracker"></a>使用更改跟踪器

更改跟踪器是在系统上作为存储最后 N 个文件系统操作的循环缓冲区实现的  。 应用能够读取缓冲区的更改，然后将其处理到自己的体验。 应用完成更改后，它会将更改标记为已处理，并再不显示。

若要在文件夹上使用更改跟踪器，请遵循以下步骤：

1. 启用文件夹的更改跟踪。
2. 等待更改。
3. 读取更改。
4. 接受更改。

后续部分将使用一些代码示例介绍每个步骤。 文末提供完整代码示例。

### <a name="enable-the-change-tracker"></a>启用更改跟踪器

应用需进行的第一件事是告知系统它对跟踪给定库的更改感兴趣。 通过针对感兴趣的库在更改跟踪器上调用 [Enable](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) 方法来完成这一点  。

```csharp
StorageLibrary videosLib = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
StorageLibraryChangeTracker videoTracker = videosLib.ChangeTracker;
videoTracker.Enable();
```

部分重要说明：

- 确保创建 [StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) 对象之前，应用拥有对清单中的正确库的权限  。 若要了解详细信息，请参阅[文件访问权限](https://docs.microsoft.com/en-us/windows/uwp/files/file-access-permissions)。
- [Enable](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable) 是线程安全的，不会重置指针，并可根据需要多次调用（稍后详述）  。

![启用空的更改跟踪器](images/changetracker-enable.png)

### <a name="wait-for-changes"></a>等待更改

初始化更改跟踪器后，它将开始记录库中发生的所有操作，即使应用并未运行。 在由于注册 [StorageLibraryChangedTrigger](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.StorageLibraryContentChangedTrigger) 事件导致发生更改的任何时间点，都可以进行注册以激活应用  。

![添加到更改跟踪器的更改无需应用进行读取](images/changetracker-waiting.png)

### <a name="read-the-changes"></a>读取更改

然后，应用可以从更改跟踪器轮询更改，并接收自上次检查以来的更改列表。 以下代码显示如何从更改跟踪器中获取更改列表。

```csharp
StorageLibrary videosLibrary = await StorageLibrary.GetLibraryAsync(KnownLibraryId.Videos);
videosLibrary.ChangeTracker.Enable();
StorageLibraryChangeReader videoChangeReader = videosLibrary.ChangeTracker.GetChangeReader();
IReadOnlyList changeSet = await changeReader.ReadBatchAsync();
```

然后，应用负责根据需要将更改处理到自己的体验或数据库。

![将更改从更改跟踪器读入应用数据库](images/changetracker-reading.png)

> [!TIP]
> 要启用的第二个调用用于保护竞争条件（如果用户在应用读取更改时向库添加了另一个文件夹）。 无需额外调用 Enable，如果用户在其库中更改文件夹，代码将失败，并显示 ecSearchFolderScopeViolation (0x80070490) 

### <a name="accept-the-changes"></a>接受更改

应用处理完更改后，应通过调用 [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) 方法告知系统不再显示这些更改  。

```csharp
await changeReader.AcceptChangesAsync();
```

![将更改标记为已读，以便其不再显示](images/changetracker-accepting.png)

应用现只能在未来读取更改跟踪器时接收新的更改。

- 如果调用 [ReadBatchAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.readbatchasync) 和 [AcceptChangesAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangereader.acceptchangesasync) 期间发生了更改，则指针只会指向应用已显示的最近更改  。 下次指针调用 ReadBatchAsync 时，这些其他更改将仍可用  。
- 不接受更改将导致系统在应用再次调用 ReadBatchAsync 时返回相同的更改集  。

## <a name="important-things-to-remember"></a>要记住的重要事项

使用更改跟踪器时，应该记住一些事项，以确保一切正常工作。

### <a name="buffer-overruns"></a>缓冲区溢出

尽管我们试图在更改跟踪器中预留足够的空间来保存系统上进行的所有操作，直到应用能够读取，但是很容易想象这样一种场景：循环缓冲区覆盖自己之前，应用不会读取更改。 特别是用户正在从备份中恢复数据或同步拍照手机中的大量照片时。

在此情况下，ReadBatchAsync 将返回错误代码 [StorageLibraryChangeType.ChangeTrackingLost](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetype)   。 如果应用收到这个错误代码，它意味着以下几项内容：

* 自上次查看缓冲区以来，缓冲区已覆盖了自己。 最佳做法是重新抓取库，因为跟踪器中的任何信息都是不完整的。
* 更改跟踪器将不会返回任何其他更改，除非调用[重置](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.reset)  。 应用调用重置后，指针将移动到最近更改，跟踪将正常恢复。

这种情况应该很少发生，但用户在磁盘上移动大量文件时，我们不希望更改跟踪器膨胀并占用太多的存储空间。 应用借此应可对大量文件系统操作做出反应，同时不破坏 Windows 中的客户体验。

### <a name="changes-to-a-storagelibrary"></a>对 StorageLibrary 的更改

[StorageLibrary](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary) 类作为包含其他文件夹的根文件夹的虚拟组存在  。 为使其与文件系统更改跟踪器协调，我们做出了以下选择：

- 对根库文件夹的派生文件夹的任何更改都将在更改跟踪器中显示。 可使用[文件夹](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders)属性查找到根库文件夹  。
- 从 StorageLibrary 中（通过 [RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestaddfolderasync) 和 [RequestRemoveFolderAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.requestremovefolderasync)）添加或删除根文件夹将不会在更改跟踪器中创建条目    。 可通过 [DefinitionChanged](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.definitionchanged) 事件或通过使用[文件夹](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrary.folders)属性枚举库中的根文件夹来跟踪这些更改   。
- 如果将已包含内容的文件夹添加到库，则不会生成更改通知或更改跟踪器条目。 对该文件夹的派生文件夹的任何后续更改都将生成通知并更改跟踪器条目。

### <a name="calling-the-enable-method"></a>调用“启用”方法

应用应在开始跟踪文件系统时和枚举更改前调用 [Enable](https://docs.microsoft.com/uwp/api/windows.storage.storagelibrarychangetracker.enable)  。 这将确保更改跟踪器将捕获所有更改。  

## <a name="putting-it-together"></a>综合运用

以下是用于记录视频库中的更改和开始从更改跟踪器中提取更改的所有代码。

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
