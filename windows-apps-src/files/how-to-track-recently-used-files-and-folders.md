---
ms.assetid: BF929A68-9C82-4866-BC13-A32B3A550005
title: 跟踪最近使用的文件和文件夹
description: 通过将用户经常访问的文件添加到你的应用的最近使用列表 (MRU) 中来跟踪这些文件。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c0eb2b6e668baec9f5ad1ef859b7213f20748beb
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2019
ms.locfileid: "9049574"
---
# <a name="track-recently-used-files-and-folders"></a>跟踪最近使用的文件和文件夹

**重要的 API**

- [**MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458)
- [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/hh738369)

通过将用户经常访问的文件添加到你的应用的最近使用列表 (MRU) 中来跟踪这些文件。 该平台会为你管理 MRU，它会根据各个项的上次访问时间对它们进行排序，并在列表中的项超过 25 个的限制时删除最旧的项。 所有应用都有其各自的 MRU。

你的应用的 MRU 由 [**StorageItemMostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207475) 类表示，你可从静态 [**StorageApplicationPermissions.MostRecentlyUsedList**](https://msdn.microsoft.com/library/windows/apps/br207458) 属性获取该类。 MRU 项存储为 [**IStorageItem**](https://msdn.microsoft.com/library/windows/apps/br227129) 对象，这意味着 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象（代表文件）和 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 对象（代表文件夹）都可以添加到 MRU 中。

> [!NOTE]
> 有关完整示例，请参阅[文件选取器示例](https://go.microsoft.com/fwlink/p/?linkid=619994)和[文件访问示例](https://go.microsoft.com/fwlink/p/?linkid=619995)。

## <a name="prerequisites"></a>先决条件

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C# 或 Visual Basic 编写异步应用，请参阅[使用 C# 或 Visual Basic 调用异步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **对位置的访问权限**

    请参阅[文件访问权限](file-access-permissions.md)。

-   [使用选取器打开文件和文件夹](quickstart-using-file-and-folder-pickers.md)

    选取的文件通常是用户再三返回查看的文件。

 ## <a name="add-a-picked-file-to-the-mru"></a>向 MRU 中添加选取的文件

-   用户选取的文件通常是他们重复返回的文件。 因此，当选取文件时，请考虑将选取的文件添加到你的应用的 MRU。 操作方法如下：

    ```cs
    Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();

    var mru = Windows.Storage.AccessCache.StorageApplicationPermissions.MostRecentlyUsedList;
    string mruToken = mru.Add(file, "profile pic");
    ```

    将重载 [**StorageItemMostRecentlyUsedList.Add**](https://msdn.microsoft.com/library/windows/apps/br207476)。 此示例中使用了 [**Add(IStorageItem, String)**](https://msdn.microsoft.com/library/windows/apps/br207481)，这样便可将元数据与文件关联。 设置元数据可以记录项目的用途，例如“用户头像”。 你还可以通过调用 [**Add(IStorageItem)**](https://msdn.microsoft.com/library/windows/apps/br207480) 将文件添加到不包含元数据的 MRU 中。 当你向 MRU 中添加项时，该方法会返回一个唯一标识的字符串（称为令牌），用于检索该项。

> [!TIP]
> 你将需要该令牌从 MRU 中检索项，因此请将它保留在某处。 有关应用数据的详细信息，请参阅[管理应用程序数据](https://msdn.microsoft.com/library/windows/apps/hh465109)。

## <a name="use-a-token-to-retrieve-an-item-from-the-mru"></a>使用令牌从 MRU 检索项

使用最适合希望检索的项的检索方法。

-   使用 [**GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br207486) 检索作为 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 的文件。
-   使用 [**GetFolderAsync**](https://msdn.microsoft.com/library/windows/apps/br207489) 检索作为 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 的文件夹。
-   使用 [**GetItemAsync**](https://msdn.microsoft.com/library/windows/apps/br207492) 检索既可代表文件也可代表文件夹的通用 [**IStorageItem**](https://msdn.microsoft.com/library/windows/apps/br227129)。

这里介绍了如何取回我们刚刚添加的文件。

```cs
StorageFile retrievedFile = await mru.GetFileAsync(mruToken);
```

这里介绍了如何进行迭代以获取令牌，然后获取项。

```cs
foreach (Windows.Storage.AccessCache.AccessListEntry entry in mru.Entries)
{
    string mruToken = entry.Token;
    string mruMetadata = entry.Metadata;
    Windows.Storage.IStorageItem item = await mru.GetItemAsync(mruToken);
    // The type of item will tell you whether it's a file or a folder.
}
```

使用 [**AccessListEntryView**](https://msdn.microsoft.com/library/windows/apps/br227349) 可以迭代 MRU 中的条目。 这些条目是包含项的令牌和元数据的 [**AccessListEntry**](https://msdn.microsoft.com/library/windows/apps/br227348) 结构。

## <a name="removing-items-from-the-mru-when-its-full"></a>当 MRU 已满时从中删除项

当达到 MRU 的 25 项这一限制并尝试添加新项时，会自动删除最早访问的项。 因此，添加新项之前无需删除任何项。

## <a name="future-access-list"></a>未来访问列表

除了 MRU，你的应用还具有一个未来访问列表。 通过选择文件和文件夹，你的用户向你的应用授予访问项的权限（若未授权可能无法访问）。 如果将这些项添加到你的未来访问列表，然后当你的应用需要以后重新访问这些项时将保留该权限。 你的应用的未来访问列表由 [**StorageItemAccessList**](https://msdn.microsoft.com/library/windows/apps/br207459) 类表示，你能从静态 [**StorageApplicationPermissions.FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) 属性获取该类。

当用户选取一个项时，请考虑将它添加到你的未来访问列表以及 MRU 中。

-   [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) 最多可保留 1000 个项。 请记住：它可以保留文件夹以及文件，所以有很多文件夹。
-   平台永远不会为你从 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457) 删除项。 当你达到 1000 个项的限制时，你就无法添加另一个项，直到你采用 [**Remove**](https://msdn.microsoft.com/library/windows/apps/br207497) 方法腾出空间。
