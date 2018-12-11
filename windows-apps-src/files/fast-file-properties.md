---
title: 快速访问 UWP 中的文件属性
description: 高效地从库中收集文件及其属性的列表以用于 UWP 应用。
ms.date: 03/22/2018
ms.topic: article
keywords: windows 10, uwp, 文件, 属性
ms.localizationpriority: medium
ms.openlocfilehash: 772abd3696850be202593c582e6338a04de38537
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8926532"
---
# <a name="fast-access-to-file-properties-in-uwp"></a>快速访问 UWP 中的文件属性 

了解如何从库中收集文件及其属性的列表，然后在应用中使用这些属性。  

先决条件 
- **异步编程的通用 Windows 平台 (UWP) 应用**    可了解如何在 C# 或 Visual Basic 编写异步应用，请参阅[调用异步 Api 采用 C# 或 Visual Basic](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。 
- **对库的访问权限**这些示例中的代码需要**picturesLibrary**功能，但你的文件位置可能根本需要其他功能或任何功能。 若要了解详细信息，请参阅[文件访问权限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)。 
- **简单文件枚举**此示例使用[QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions)来设置几个高级的枚举属性。 要详细了解如何只获取一个较小目录的简单文件列表，请参阅[枚举和查询文件和文件夹](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)。 

## <a name="usage"></a>用法  
许多应用都需要列出一组文件的属性，但不必始终直接与这些文件交互。 例如，一个音乐应用一次播放（打开）一个文件，但它需要一个文件夹中所有文件的属性，以便该应用显示歌曲队列，从而让用户能够选择有效的文件来播放。 

本页上的示例不应该用于会修改每个文件元数据的应用，或会与产生的所有 StorageFile 交互而不只是读取其属性的应用。 请参阅[枚举和查询文件和文件夹](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)了解详细信息。 

## <a name="enumerate-all-the-pictures-in-a-location"></a>枚举某个位置的所有图片 
在本示例中，我们将
-  构造一个 [QueryOptions](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) 对象来指示应用需要尽快枚举文件。
-  通过将 StorageFile 对象分页到应用中来获取文件属性。 将文件分页可减少应用使用的内存，并实现用户能感知的响应速度提升。

### <a name="creating-the-query"></a>创建查询 
为了构建查询，我们使用 QueryOptions 对象指定应用仅枚举特定类型的图像文件而筛选掉 Windows 信息保护 (System.Security.EncryptionOwners) 所保护的文件。 

请务必设置应用将使用 [QueryOptions.SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch) 访问的属性。 如果应用访问未预取的属性，会明显影响性能。

设置 [IndexerOption.OnlyUseIndexerAndOptimzeForIndexedProperties](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.IndexerOption) 即可通知系统尽快返回结果，但返回的结果仅包括在 [SetPropertyPrefetch](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch) 中指定的属性。 

### <a name="paging-in-the-results"></a>结果中的分页 
用户的图片库中可能有数千或数以百万计的文件，因此调用 [GetFilesAsync](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync) 会大量占用其计算机资源，因为它会为每个图像创建 StorageFile。 这个问题可以这样解决：一次只创建固定数量的 StorageFile、处理它们并显示到 UI，然后释放内存。 

本例中的方法是：使用 [StorageFileQueryResult.GetFilesAsync (UInt32 maxNumberOfItems UInt32 StartIndex)](https://docs.microsoft.com/uwp/api/windows.storage.search.storagefilequeryresult.getfilesasync)，每次只获取 100 个文件。 然后，应用将处理这些文件，并允许操作系统在此之后释放内存。 这种方法限制了应用能使用的最大内存，从而确保系统保持一定的响应能力。 当然，你需要根据应用场景调整返回的文件数量，但要确保所有用户的响应体验，建议每次获取的文件数不超过 500 个。


**示例**  
```csharp
StorageFolder folderToEnumerate = KnownFolders.PicturesLibrary; 
// Check if the folder is indexed before doing anything. 
IndexedState folderIndexedState = await folderToEnumerate.GetIndexedStateAsync(); 
if (folderIndexedState == IndexedState.NotIndexed || folderIndexedState == IndexedState.Unknown) 
{ 
    // Only possible in indexed directories.  
    return; 
} 
 
QueryOptions picturesQuery = new QueryOptions() 
{ 
    FolderDepth = FolderDepth.Deep, 
    // Filter out all files that have WIP enabled
    ApplicationSearchFilter = "System.Security.EncryptionOwners:[]", 
    IndexerOption = IndexerOption.OnlyUseIndexerAndOptimizeForIndexedProperties 
}; 

picturesQuery.FileTypeFilter.Add(".jpg"); 
string[] otherProperties = new string[] 
{ 
    SystemProperties.GPS.LatitudeDecimal, 
    SystemProperties.GPS.LongitudeDecimal 
}; 
 
picturesQuery.SetPropertyPrefetch(PropertyPrefetchOptions.BasicProperties | PropertyPrefetchOptions.ImageProperties, 
                                    otherProperties); 
SortEntry sortOrder = new SortEntry() 
{ 
    AscendingOrder = true, 
    PropertyName = "System.FileName" // FileName property is used as an example. Any property can be used here.  
}; 
picturesQuery.SortOrder.Add(sortOrder); 
 
// Create the query and get the results 
uint index = 0; 
const uint stepSize = 100; 
if (!folderToEnumerate.AreQueryOptionsSupported(picturesQuery)) 
{ 
    log("Querying for a sort order is not supported in this location"); 
    picturesQuery.SortOrder.Clear(); 
} 
StorageFileQueryResult queryResult = folderToEnumerate.CreateFileQueryWithOptions(picturesQuery); 
IReadOnlyList<StorageFile> images = await queryResult.GetFilesAsync(index, stepSize); 
while (images.Count != 0 || index < 10000) 
{ 
    foreach (StorageFile file in images) 
    { 
        // With the OnlyUseIndexerAndOptimizeForIndexedProperties set, this won't  
        // be async. It will run synchronously. 
        var imageProps = await file.Properties.GetImagePropertiesAsync(); 
 
        // Build the UI 
        log(String.Format("{0} at {1}, {2}", 
                    file.Path, 
                    imageProps.Latitude, 
                    imageProps.Longitude)); 
    } 
    index += stepSize; 
    images = await queryResult.GetFilesAsync(index, stepSize); 
} 
```

### <a name="results"></a>结果 
产生的 StorageFile 文件仅包含请求的属性，但与其他 IndexerOption 相比，返回速度快 10 倍。该应用仍然可以请求对还未包含在查询中的属性进行访问，但是打开文件和检索这些属性不会造成性能下降。  

## <a name="adding-folders-to-libraries"></a>将文件夹添加到库 
应用可以请求用户使用 [StorageLibrary.RequestAddFolderAsync](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageLibrary.RequestAddFolderAsync) 将位置添加到索引。 包含该位置后，它将自动编入索引，应用可以使用此技术来枚举文件。
 
## <a name="see-also"></a>另请参阅
[QueryOptions API 参考](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions)  
[枚举和查询文件和文件夹](https://docs.microsoft.com/windows/uwp/files/quickstart-listing-files-and-folders)  
[文件访问权限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)  
[快速属性访问演练](https://blogs.msdn.microsoft.com/adamdwilson/2017/12/20/fast-file-enumeration-with-partially-initialized-storagefiles/)
 
 
