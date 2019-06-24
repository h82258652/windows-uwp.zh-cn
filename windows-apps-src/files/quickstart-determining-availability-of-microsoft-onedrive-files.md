---
ms.assetid: 3604524F-112A-474F-B0CA-0726DC8DB885
title: 确定 Microsoft OneDrive 文件的可用性
description: 使用 StorageFile.IsAvailable 属性确定 Microsoft OneDrive 文件是否可用。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: effb28fa453ec884152dbc404245f00f4893ef5a
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369425"
---
# <a name="determining-availability-of-microsoft-onedrive-files"></a>确定 Microsoft OneDrive 文件的可用性


**重要的 API**

-   [**FileIO 类**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO)
-   [**StorageFile 类**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)
-   [**StorageFile.IsAvailable 属性**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable)

使用 [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) 属性确定 Microsoft OneDrive 文件是否可用。

## <a name="prerequisites"></a>必备条件

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C# 或 Visual Basic 编写异步应用，请参阅[使用 C# 或 Visual Basic 调用异步 API](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。

-   **应用功能声明**

    请参阅[文件访问权限](file-access-permissions.md)。

## <a name="using-the-storagefileisavailable-property"></a>使用 StorageFile.IsAvailable 属性

用户能够将 OneDrive 文件标记为脱机可用（默认）或仅联机可用。 此功能使用户能够将大文件（例如图片和视频）移动到其 OneDrive、将其标记为仅联机可用，并节省磁盘空间（在本地保留的唯一内容是元数据文件）。

[**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) 用于确定某个文件当前是否可用。 下表显示了在各种方案中 **StorageFile.IsAvailable** 属性的值。

| 文件类型                              | 联机 | 按流量计费的网络        | 脱机 |
|-------------------------------------------|--------|------------------------|---------|
| 本地文件                                | True   | True                   | True    |
| 标记为脱机可用的 OneDrive 文件 | True   | True                   | True    |
| 标记为仅联机可用的 OneDrive 文件       | True   | 基于用户设置 | False   |
| 网络文件                              | True   | 基于用户设置 | False   |

 

以下步骤演示了如何确定文件是否当前可用。

1.  声明适用于要访问的库的功能。
2.  包括 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) 命名空间。 此命名空间包含用于管理文件、文件夹和应用程序设置的类型。 它还包含所需的 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 类型。
3.  获取所需文件的 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 对象。 如果你枚举一个库，则通常通过以下方法来完成此步骤：调用 [**StorageFolder.CreateFileQuery**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.createfilequery) 方法，然后调用生成的 [**StorageFileQueryResult**](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.StorageFileQueryResult) 对象的 [**GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync) 方法。 **GetFilesAsync** 方法返回 **StorageFile** 对象的 [IReadOnlyList](https://go.microsoft.com/fwlink/p/?LinkId=324970) 集合。
4.  有权访问表示所需文件的 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 对象后，[**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) 属性的值会反映该文件是否可用。

以下通用方法说明了如何枚举任一文件夹，并返回此文件夹的 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 对象集合。 调用方法随后会迭代返回的集合，该集合引用了每个文件的 [**StorageFile.IsAvailable**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable) 属性。

```cs
/// <summary>
/// Generic function that retrieves all files from the specified folder.
/// </summary>
/// <param name="folder">The folder to be searched.</param>
/// <returns>An IReadOnlyList collection containing the file objects.</returns>
async Task<System.Collections.Generic.IReadOnlyList<StorageFile>> GetLibraryFilesAsync(StorageFolder folder)
{
    var query = folder.CreateFileQuery();
    return await query.GetFilesAsync();
}

private async void CheckAvailabilityOfFilesInPicturesLibrary()
{
    // Determine availability of all files within Pictures library.
    var files = await GetLibraryFilesAsync(KnownFolders.PicturesLibrary);
    for (int i = 0; i < files.Count; i++)
    {
        StorageFile file = files[i];

        StringBuilder fileInfo = new StringBuilder();
        fileInfo.AppendFormat("{0} (on {1}) is {2}",
                    file.Name,
                    file.Provider.DisplayName,
                    file.IsAvailable ? "available" : "not available");
    }
}
```
