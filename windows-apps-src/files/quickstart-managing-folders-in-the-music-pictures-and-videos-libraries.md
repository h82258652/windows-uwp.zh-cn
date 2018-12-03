---
ms.assetid: 1AE29512-7A7D-4179-ADAC-F02819AC2C39
title: 音乐、图片和视频库中的文件和文件夹
description: 将现有的音乐、图片和视频文件夹添加到相应的库。 你还可以从库中删除文件夹、获取库中的文件夹列表，并发现存储的照片、音乐和视频。
ms.date: 06/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8e04170fb8952ecd5802b6190816d44012f56d8a
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8458950"
---
# <a name="files-and-folders-in-the-music-pictures-and-videos-libraries"></a>音乐、图片和视频库中的文件和文件夹

将现有的音乐、图片和视频文件夹添加到相应的库。 你还可以从库中删除文件夹、获取库中的文件夹列表，并发现存储的照片、音乐和视频。

库是虚拟的文件夹集合，其中包括一个默认的已知文件夹，以及用户通过使用你的应用或任一内置应用添加到库的任何其他文件夹。 例如，图片库默认包含“图片”已知文件夹。 用户可以通过使用你的应用或内置的“照片”应用，将文件夹添加到图片库或从中删除它们。

## <a name="prerequisites"></a>先决条件


-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C# 或 Visual Basic 编写异步应用，请参阅[使用 C# 或 Visual Basic 调用异步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **对位置的访问权限**

    在 Visual Studio 中，在“清单设计器”中打开应用清单文件。 在**功能**页上，选择你的应用管理的库。

    -   **音乐库**
    -   **图片库**
    -   **视频库**

    若要了解详细信息，请参阅[文件访问权限](file-access-permissions.md)。

## <a name="get-a-reference-to-a-library"></a>获取对库的引用

> [!NOTE]
> 请记住声明相应的功能。 有关详细信息，请参阅[应用功能声明](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。
 

若要获取对用户的音乐、图片或视频库的引用，请调用 [**StorageLibrary.GetLibraryAsync**](https://msdn.microsoft.com/library/windows/apps/dn251725) 方法。 提供 [**KnownLibraryId**](https://msdn.microsoft.com/library/windows/apps/dn298399) 枚举中的相应值。

-   [**KnownLibraryId.Music**](https://msdn.microsoft.com/library/windows/apps/br227155)
-   [**KnownLibraryId.Pictures**](https://msdn.microsoft.com/library/windows/apps/br227156)
-   [**KnownLibraryId.Videos**](https://msdn.microsoft.com/library/windows/apps/br227159)

```cs
var myPictures = await Windows.Storage.StorageLibrary.GetLibraryAsync(Windows.Storage.KnownLibraryId.Pictures);
```

## <a name="get-the-list-of-folders-in-a-library"></a>获取库中的文件夹的列表


若要获取库中的文件夹的列表，请获取 [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 属性的值。

```cs
using Windows.Foundation.Collections;
IObservableVector<Windows.Storage.StorageFolder> myPictureFolders = myPictures.Folders;
```

## <a name="get-the-folder-in-a-library-where-new-files-are-saved-by-default"></a>获取默认保存新文件的库中的文件夹


若要获取默认保存新文件的库中的文件夹，请获取 [**StorageLibrary.SaveFolder**](https://msdn.microsoft.com/library/windows/apps/dn251728) 属性的值。

```cs
Windows.Storage.StorageFolder savePicturesFolder = myPictures.SaveFolder;
```

## <a name="add-an-existing-folder-to-a-library"></a>将现有文件夹添加到库

若要向库中添加文件夹，请调用 [**StorageLibrary.RequestAddFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251726)。 以图片库为例，调用此方法会导致向用户显示一个带有**将此文件夹添加到“图片”** 按钮的文件夹选取器。 如果用户选取了一个文件夹，则该文件夹仍将保留在其在磁盘上的原始位置并成为 [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 属性中（和内置“照片”应用中）的项，但该文件夹不会作为“图片”文件夹的子文件夹而出现在文件资源管理器中。


```cs
Windows.Storage.StorageFolder newFolder = await myPictures.RequestAddFolderAsync();
```

## <a name="remove-a-folder-from-a-library"></a>从库中删除文件夹

若要从库中删除文件夹，请调用 [**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727) 方法并指定要删除的文件夹。 你可以使用 [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 和 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 控件（或类似控件）帮助用户选择要删除的文件夹。

当你调用 [**StorageLibrary.RequestRemoveFolderAsync**](https://msdn.microsoft.com/library/windows/apps/dn251727) 时，用户将看到确认对话框，指出该文件夹“再也不会在‘图片’中显示，但不会被删除。” 这意味着该文件夹仍保留在其在磁盘上的原始位置上、会从 [**StorageLibrary.Folders**](https://msdn.microsoft.com/library/windows/apps/dn251724) 属性中删除，并且将不再包含在内置的“照片”应用中。

以下示例假设用户已从名为 **lvPictureFolders** 的 [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) 控件中选择要删除的文件夹。


```cs
bool result = await myPictures.RequestRemoveFolderAsync(folder);
```

## <a name="get-notified-of-changes-to-the-list-of-folders-in-a-library"></a>获取对库中的文件夹列表的更改的通知


若要获取对库中的文件夹列表的更改的通知，请为库的 [**StorageLibrary.DefinitionChanged**](https://msdn.microsoft.com/library/windows/apps/dn251723) 事件注册一个处理程序。


```cs
myPictures.DefinitionChanged += MyPictures_DefinitionChanged;

void HandleDefinitionChanged(Windows.Storage.StorageLibrary sender, object args)
{
    // ...
}
```

## <a name="media-library-folders"></a>媒体库文件夹


设备为用户和应用提供了五个预定义位置，以便存储媒体文件。 内置应用可在这些位置中存储用户创建的媒体和下载的媒体。

位置如下：

-   **图片**文件夹。 包含图片。

    -   **本机照片**文件夹。 包含内置相机中的照片和视频。

    -   **保存的图片**文件夹。 包含用户从其他应用中保存的图片。

-   **音乐**文件夹。 包含歌曲、播客和音频书籍。

-   **视频**文件夹。 包含视频。

用户和应用还可以将媒体文件存储在 SD 卡上媒体库文件夹的外部。 若要在 SD 卡上可靠查找媒体文件，请扫描 SD 卡的内容，或者要求用户通过使用文件选取器查找该文件。 有关详细信息，请参阅[访问 SD 卡](access-the-sd-card.md)。

## <a name="querying-the-media-libraries"></a>查询媒体库

若要获取一组文件，请指定所需库和文件类型。

```cs
using Windows.Storage;
using Windows.Storage.Search;

private async void getSongs()
{
    QueryOptions queryOption = new QueryOptions
        (CommonFileQuery.OrderByTitle, new string[] { ".mp3", ".mp4", ".wma" });

    queryOption.FolderDepth = FolderDepth.Deep;

    Queue<IStorageFolder> folders = new Queue<IStorageFolder>();

    var files = await KnownFolders.MusicLibrary.CreateFileQueryWithOptions
      (queryOption).GetFilesAsync();

    foreach (var file in files)
    {
        // do something with the music files
    }
}
```

### <a name="query-results-include-both-internal-and-removable-storage"></a>查询结果包括内部存储和可移动存储

默认情况下，用户可以选择将文件存储在可选的 SD 卡上。 但是，应用可以停止允许将文件存储在 SD 卡上。 因此，媒体库可以在设备的内部存储和 SD 卡上进行拆分。

你无需写入其他代码即可处理这种情况。 [
              **Windows.Storage**
            ](https://msdn.microsoft.com/library/windows/apps/br227346) 命名空间中用于查询已知文件夹的方法以透明方式合并这两个位置中的查询结果。 你也无需在应用清单文件中指定 **removableStorage** 功能来获取这些合并的结果。

请考虑下图中显示的设备的存储状态：

![电话和 SD 卡中的图像](images/phone-media-locations.png)

如果通过调用 `await KnownFolders.PicturesLibrary.GetFilesAsync()` 查询图片库的内容，结果将同时包括 internalPic.jpg 和 SDPic.jpg。


## <a name="working-with-photos"></a>使用照片

在相机保存每个图片的低分辨率图像和高分辨率图像所在的设备上，深入查询仅返回低分辨率图像。

“本机照片”和“已保存图片”文件夹不支持深入查询。

**在捕获照片的应用中打开该照片**

如果你希望让用户以后在捕获照片的应用中再次打开该照片，则可以通过使用类似于以下示例的代码，与照片的元数据一起保存 **CreatorAppId**。 在此示例中，**testPhoto** 是 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)。

```cs
IDictionary<string, object> propertiesToSave = new Dictionary<string, object>();

propertiesToSave.Add("System.CreatorOpenWithUIOptions", 1);
propertiesToSave.Add("System.CreatorAppId", appId);

testPhoto.Properties.SavePropertiesAsync(propertiesToSave).AsyncWait();   
```

## <a name="using-stream-methods-to-add-a-file-to-a-media-library"></a>使用流方法向媒体库添加文件

当你通过使用已知文件夹（如 **KnownFolders.PictureLibrary**）访问媒体库，并使用流方法向媒体库添加文件时，你必须确保关闭你的代码打开的所有流。 否则，这些用于按预期方式向媒体库添加文件的方法将失败，因为至少一个流仍具有该文件的句柄。

例如，当你运行以下代码时，不会将该文件添加到媒体库。 在代码行 `using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))` 中，**OpenAsync** 方法和 **GetOutputStreamAt** 方法均可以打开流。 但是，仅通过 **GetOutputStreamAt** 方法打开的流可以释放为 **using** 语句的结果。 其他流将保持打开并阻止保存该文件。

```cs
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");
using (var sourceStream = (await sourceFile.OpenReadAsync()).GetInputStreamAt(0))
{
    using (var destinationStream = (await destinationFile.OpenAsync(FileAccessMode.ReadWrite)).GetOutputStreamAt(0))
    {
        await RandomAccessStream.CopyAndCloseAsync(sourceStream, destinationStream);
    }
}
```

若要成功使用流方法以向媒体库添加文件，请确保关闭你的代码打开的所有流，如以下示例所示。

```cs
StorageFolder testFolder = await StorageFolder.GetFolderFromPathAsync(@"C:\test");
StorageFile sourceFile = await testFolder.GetFileAsync("TestImage.jpg");
StorageFile destinationFile = await KnownFolders.CameraRoll.CreateFileAsync("MyTestImage.jpg");

using (var sourceStream = await sourceFile.OpenReadAsync())
{
    using (var sourceInputStream = sourceStream.GetInputStreamAt(0))
    {
        using (var destinationStream = await destinationFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            using (var destinationOutputStream = destinationStream.GetOutputStreamAt(0))
            {
                await RandomAccessStream.CopyAndCloseAsync(sourceInputStream, destinationStream);
            }
        }
    }
}
```
