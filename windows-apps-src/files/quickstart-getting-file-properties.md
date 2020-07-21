---
ms.assetid: AC96F645-1BDE-4316-85E0-2FBDE0A0A62A
title: 获取文件属性
description: 获取由 StorageFile\#对象表示的文件的属性 - \#顶级、基本和扩展属性。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 81fcb4b62f9a10e8ff1fcb233c95317746cdb3b0
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "74259589"
---
# <a name="get-file-properties"></a>获取文件属性

**重要的 API**

-   [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync)
-   [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties)
-   [**StorageItemContentProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.retrievepropertiesasync)

获取由 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 对象表示的文件属性：顶级、基本和扩展。

> [!NOTE]
> 有关完整示例，请参阅[文件访问示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)。

## <a name="prerequisites"></a>必备条件

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C# 或 Visual Basic 编写异步应用，请参阅[使用 C# 或 Visual Basic 调用异步 API](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。

-   **对位置的访问权限**

    例如，这些示例中的代码需要 **picturesLibrary** 功能，但是你的位置可能需要其他功能或根本不需要任何功能。 若要了解详细信息，请参阅[文件访问权限](file-access-permissions.md)。

## <a name="getting-a-files-top-level-properties"></a>获取文件的顶级属性

很多顶级文件属性都可以作为 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 类的成员进行访问。 这些属性包括文件属性、内容类型、创建日期、显示名称和文件类型等。

> [!NOTE]
> 请记住声明 **picturesLibrary** 功能。

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

很多基本文件属性都通过先调用 [**StorageFile.GetBasicPropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getbasicpropertiesasync) 方法获得。 此方法会返回一个 [**BasicProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties) 对象，该对象将定义项（文件或文件夹）的大小属性，以及上次修改项的时间。

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

除了顶级和基本文件属性之外，还有一些与文件内容有关的属性。 这些扩展属性可以通过调用 [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) 方法来访问。 （通过调用 [**StorageFile.Properties**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.properties) 属性可以获得 [**BasicProperties**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.BasicProperties) 对象。）尽管顶级和基本文件属性可以分别作为类的 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 和 **BasicProperties** 属性进行访问，但扩展属性只能通过以下方法获得：将代表将要检索的属性名称的 [String](https://msdn.microsoft.com/library/system.string.aspx) 对象的 [IEnumerable](https://msdn.microsoft.com/library/system.collections.ienumerable.aspx) 集合传递到 **BasicProperties.RetrievePropertiesAsync** 方法。 此方法随后会返回一个 [IDictionary](https://msdn.microsoft.com/library/system.collections.idictionary.aspx) 集合。 然后，可以按名称或按索引从该集合中检索每个扩展属性。

以下示例枚举了图片库中的所有文件，并指定了一个 [List](https://msdn.microsoft.com/library/6sh2ey19.aspx) 对象中所需属性（**DataAccessed** 和 **FileOwner**）的名称，将该 [List](https://msdn.microsoft.com/library/6sh2ey19.aspx) 对象传递到 [**BasicProperties.RetrievePropertiesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.basicproperties.retrievepropertiesasync) 以检索这些属性，然后按名称从返回的 [IDictionary](https://msdn.microsoft.com/library/system.collections.idictionary.aspx) 对象中检索这些属性。

有关文件扩展属性的完整列表，请参阅 [Windows 核心属性](https://docs.microsoft.com/windows/desktop/properties/core-bumper)。

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

 

 
