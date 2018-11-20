---
author: laurenhughes
ms.assetid: CAC6A7C7-3348-4EC4-8327-D47EB6E0C238
title: 访问 SD 卡
description: 你可以在可选 MicroSD 卡上存储和访问不重要的数据，尤其是内部存储具有限制的低成本移动设备。
ms.author: lahugh
ms.date: 03/08/2017
ms.topic: article
keywords: windows 10, uwp, sd 卡, 存储
ms.localizationpriority: medium
ms.openlocfilehash: 498b43dc82100102c90fc7a920bed1538a164afc
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/19/2018
ms.locfileid: "7287006"
---
# <a name="access-the-sd-card"></a>访问 SD 卡



你可以在可选 MicroSD 卡上存储和访问不重要的数据，尤其是内部存储具有限制并且具有 SD 卡插槽的低成本移动设备。

大多数情况下，你必须在应用清单文件中指定 **removableStorage** 功能，然后你的应用才能在 SD 卡上存储和访问文件。 通常，你还需要注册才能处理你的应用存储和访问的文件类型。

你可以通过使用以下方法在可选 SD 卡上存储和访问文件。
- 文件选取器。
- [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) API。

## <a name="what-you-can-and-cant-access-on-the-sd-card"></a>SD 卡上的可访问内容和不可访问内容

### <a name="what-you-can-access"></a>你可以访问的内容

- 你的应用仅可以读取和写入以下类型的文件，该文件类型是应用已注册可在应用部件清单文件中处理的类型。
- 你的应用还可以创建和管理文件夹。

### <a name="what-you-cant-access"></a>你无法访问的内容

- 你的应用无法查看或访问系统文件夹及其包含的文件。
- 你的应用无法查看使用“隐藏”属性标记的文件。 “隐藏”属性通常用于减少意外删除数据的风险。
- 你的应用无法通过使用 [**KnownFolders.DocumentsLibrary**](https://msdn.microsoft.com/library/windows/apps/br227152) 查看或访问文档库。 但是，你可以通过遍历文件系统在 SD 卡上访问文档库。

## <a name="security-and-privacy-considerations"></a>安全和隐私注意事项

当应用将文件保存在 SD 卡上的全局位置中时，这些文件不会加密，因此通常可供其他应用访问。

- 当 SD 卡位于该设备中时，已注册可处理相同文件类型的其他应用可以访问你的文件。
- 当从该设备删除并从电脑中打开 SD 卡时，你的文件将在“文件资源管理器”中可见，并可供其他应用访问。

但是，当安装在 SD 卡上的应用将文件保存在其 [**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) 中时，这些文件将会加密，且不可供其他应用访问。

## <a name="requirements-for-accessing-files-on-the-sd-card"></a>访问 SD 卡上的文件的要求

若要访问 SD 卡上的文件，通常你必须指定以下内容。

1.  必须在应用清单文件中指定 **removableStorage** 功能。
2.  还必须注册以处理与你希望访问的媒体类型相关联的文件扩展名。

也可以使用前述方法访问 SD 卡上的媒体文件，无需引用已知文件夹（如 **KnownFolders.MusicLibrary**），或者访问存储在媒体库文件夹外部的媒体文件。

若要通过使用已知文件夹访问存储在媒体库（音乐、照片或视频）中的媒体文件，只需在应用清单文件中（**musicLibrary**、**picturesLibrary** 或 **videoLibrary**）指定关联功能。 无需指定 **removableStorage** 功能。 有关详细信息，请参阅[音乐、图片和视频库中的文件和文件夹](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。

## <a name="accessing-files-on-the-sd-card"></a>访问 SD 卡上的文件

### <a name="getting-a-reference-to-the-sd-card"></a>为 SD 卡获取引用。

[**KnownFolders.RemovableDevices**](https://msdn.microsoft.com/library/windows/apps/br227158) 文件夹是适用于一组可移动设备（当前已连接到该设备）的逻辑根 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)。 如果存在 SD 卡，**KnownFolders.RemovableDevices** 文件夹下的第一个（且唯一一个）**StorageFolder** 代表 SD 卡。

使用如下所示的代码确定 SD 卡是否存在，并为其获取 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 引用。

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    // An SD card is present and the sdCard variable now contains a reference to it.
}
else
{
    // No SD card is present.
}
```

> [!NOTE]
> 如果你的 SD 卡读卡器是嵌入式读卡器（例如，电脑或笔记本电脑本身中的插槽），则可能无法通过 KnownFolders.RemovableDevices 访问它。

### <a name="querying-the-contents-of-the-sd-card"></a>查询 SD 卡的内容

SD 卡可以包含系统无法识别为已知文件夹且无法通过使用 [**KnownFolders**](https://msdn.microsoft.com/library/windows/apps/br227151) 的位置查询的许多文件夹和文件。 若要查找文件，你的应用必须通过递归遍历文件系统来枚举该卡的内容。 使用 [**GetFilesAsync (CommonFileQuery.DefaultQuery)**](https://msdn.microsoft.com/library/windows/apps/br227274) 和 [**GetFoldersAsync (CommonFolderQuery.DefaultQuery)**](https://msdn.microsoft.com/library/windows/apps/br227281) 有效获取 SD 卡的内容。

我们建议你使用后台线程遍历 SD 卡。 一个 SD 卡可以包含数 GB 的数据。

你的应用还可以要求用户通过使用文件夹选取器选择特定文件夹。

当你使用派生自 [**KnownFolders.RemovableDevices**](https://msdn.microsoft.com/library/windows/apps/br227158) 的路径访问 SD 卡上的文件系统时，下列方法的行为方式如下。

-   [**GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227273) 方法将返回你要通过注册来处理的文件扩展名和与你指定的任何媒体库功能相关联的文件扩展名的联合。
-   如果你尚未注册即处理正尝试访问的文件的文件扩展名，[**GetFileFromPathAsync**](https://msdn.microsoft.com/library/windows/apps/br227206) 方法会失败。

## <a name="identifying-the-individual-sd-card"></a>标识单个 SD 卡

首先装载 SD 卡时，操作系统将为该卡生成一个唯一标识符。 它将此 ID 存储在该卡的根 WPSystem 文件夹的某个文件中。 应用可以使用此 ID 确定它是否可以识别该卡。 如果应用可以识别该卡，该应用将能够推迟之前完成的某些操作。 但是，由于该卡最后由应用访问，因此卡上的内容可能会发生变化。

例如，考虑索引电子书的应用。 如果该应用之前扫描了整个 SD 卡以查找电子书文件并创建了电子书索引，它在重新插入该卡时会立即显示列表，并且应用可以识别该卡。 此外，它可以启动低优先级后台线程以搜索新电子书。 当用户尝试访问删除的电子书时，它还可以处理故障以查找之前存在的电子书。

包含此 ID 的属性名称为 **WindowsPhone.ExternalStorageId**。

```csharp
using Windows.Storage;

// Get the logical root folder for all external storage devices.
StorageFolder externalDevices = Windows.Storage.KnownFolders.RemovableDevices;

// Get the first child folder, which represents the SD card.
StorageFolder sdCard = (await externalDevices.GetFoldersAsync()).FirstOrDefault();

if (sdCard != null)
{
    var allProperties = sdCard.Properties;
    IEnumerable<string> propertiesToRetrieve = new List<string> { "WindowsPhone.ExternalStorageId" };

    var storageIdProperties = await allProperties.RetrievePropertiesAsync(propertiesToRetrieve);

    string cardId = (string)storageIdProperties["WindowsPhone.ExternalStorageId"];

    if (...) // If cardID matches the cached ID of a recognized card.
    {
        // Card is recognized. Index contents opportunistically.
    }
    else
    {
        // Card is not recognized. Index contents immediately.
    }
}
```

 

 
