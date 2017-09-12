---
author: laurenhughes
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: "获取文件属性"
description: "获取由 StorageFile 对象表示的文件属性&\\#8212;顶级、基本和扩展&\\#8212;。"
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: e564c44a99419f1900ab6c322af4ea6dedb195cb
ms.sourcegitcommit: 8c4d50ef819ed1a2f8cac4eebefb5ccdaf3fa898
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/27/2017
---
# <a name="get-file-properties"></a>获取文件属性

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737)
-   [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh770652)

获取由 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象表示的文件属性：顶级、基本和扩展。

**注意**  另请参阅[文件访问示例](http://go.microsoft.com/fwlink/p/?linkid=619995)。

 


## <a name="prerequisites"></a>先决条件

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C# 或 Visual Basic 编写异步应用，请参阅[使用 C# 或 Visual Basic 调用异步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **对位置的访问权限**

    例如，这些示例中的代码需要 **picturesLibrary** 功能，但是你的位置可能需要其他功能或根本不需要任何功能。 若要了解详细信息，请参阅[文件访问权限](file-access-permissions.md)。

## <a name="getting-a-files-top-level-properties"></a>获取文件的顶级属性

很多顶级文件属性都可以作为 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 类的成员进行访问。 这些属性包括文件属性、内容类型、创建日期、显示名称和文件类型等。

**注意**  请记住，要声明 **picturesLibrary** 功能。

 

此示例枚举了图片库中的所有文件，从而访问每个文件中的一些顶层属性。

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get top-level file properties.
    fileProperties.AppendLine("File name: " + file.Name);
    fileProperties.AppendLine("File type: " + file.FileType);
}
```

## <a name="getting-a-files-basic-properties"></a>获取文件的基本属性

很多基本文件属性都通过先调用 [**StorageFile.GetBasicPropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/hh701737) 方法获得。 此方法会返回一个 [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113) 对象，该对象将定义项（文件或文件夹）的大小属性，以及上次修改项的时间。

此示例枚举了图片库中的所有文件，从而访问每个文件中的一些基础属性。

```csharp
// Enumerate all files in the Pictures library.
var folder = Windows.Storage.KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Get file's basic properties.
    Windows.Storage.FileProperties.BasicProperties basicProperties =
        await file.GetBasicPropertiesAsync();
    string fileSize = string.Format("{0:n0}", basicProperties.Size);
    fileProperties.AppendLine("File size: " + fileSize + " bytes");
    fileProperties.AppendLine("Date modified: " + basicProperties.DateModified);
}
 ```

## <a name="getting-a-files-extended-properties"></a>获取文件的扩展属性

除了顶级和基本文件属性之外，还有一些与文件内容有关的属性。 这些扩展属性可以通过调用 [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124) 方法来访问。 （通过调用 [**StorageFile.Properties**](https://msdn.microsoft.com/library/windows/apps/br227225) 属性可以获得 [**BasicProperties**](https://msdn.microsoft.com/library/windows/apps/br212113) 对象。）尽管顶级和基本文件属性可以分别作为类的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 和 **BasicProperties** 属性进行访问，但扩展属性只能通过以下方法获得：将代表将要检索的属性名称的 [String](http://go.microsoft.com/fwlink/p/?LinkID=325032) 对象的 [IEnumerable](http://go.microsoft.com/fwlink/p/?LinkID=313091) 集合传递到 **BasicProperties.RetrievePropertiesAsync** 方法。 此方法随后会返回一个 [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238) 集合。 然后，可以按名称或按索引从该集合中检索每个扩展属性。

以下示例枚举了图片库中的所有文件，并指定了一个 [List](http://go.microsoft.com/fwlink/p/?LinkID=325246) 对象中所需属性（**DataAccessed** 和 **FileOwner**）的名称，将该 [List](http://go.microsoft.com/fwlink/p/?LinkID=325246) 对象传递到 [**BasicProperties.RetrievePropertiesAsync**](https://msdn.microsoft.com/library/windows/apps/br212124) 以检索这些属性，然后按名称从返回的 [IDictionary](http://go.microsoft.com/fwlink/p/?LinkId=325238) 对象中检索这些属性。

有关文件扩展属性的完整列表，请参阅 [Windows 核心属性](https://msdn.microsoft.com/library/windows/desktop/mt805470)。

```csharp
const string dateAccessedProperty = "System.DateAccessed";
const string fileOwnerProperty = "System.FileOwner";

// Enumerate all files in the Pictures library.
var folder = KnownFolders.PicturesLibrary;
var query = folder.CreateFileQuery();
var files = await query.GetFilesAsync();

foreach (Windows.Storage.StorageFile file in files)
{
    StringBuilder fileProperties = new StringBuilder();

    // Define property names to be retrieved.
    var propertyNames = new List<string>();
    propertyNames.Add(dateAccessedProperty);
    propertyNames.Add(fileOwnerProperty);

    // Get extended properties.
    IDictionary<string, object> extraProperties =
        await file.Properties.RetrievePropertiesAsync(propertyNames);

    // Get date-accessed property.
    var propValue = extraProperties[dateAccessedProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("Date accessed: " + propValue);
    }

    // Get file-owner property.
    propValue = extraProperties[fileOwnerProperty];
    if (propValue != null)
    {
        fileProperties.AppendLine("File owner: " + propValue);
    }
}
```

 

 
