---
title: 处理文件
description: 了解如何在通用 Windows 平台中处理文件。
ms.date: 05/01/2018
ms.topic: article
keywords: 入门, uwp, windows 10, 学习轨迹, 文件, 文件 io, 读取文件, 写入文件, 创建文件, 写入文本, 读取文本
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: e0abc146820ca27ee83662ba5f8b79a1daf90bab
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2019
ms.locfileid: "9045526"
---
# <a name="work-with-files"></a>处理文件

本主题介绍开始在通用 Windows 平台 (UWP) 应用中读取或写入文件需要了解的内容。 介绍了主要 API 和类型，并提供了可帮助你了解详细信息的链接。

这不是教程。 如果你需要教程，请参阅[创建、写入和读取文件](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files)，其中除了演示如何创建、读取和写入文件，还展示了如何使用缓冲区和流。 你还可能对[文件访问示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)感兴趣，其中展示了如何创建、读取、写入、复制和删除文件，以及如何检索文件属性、记住文件或文件夹，以便你的应用可以轻松地再次访问。

我们会了解一下用于在文件中写入和读取文本的代码，并了解如何访问应用的本地、漫游和临时文件夹。

## <a name="what-do-you-need-to-know"></a>需要了解哪些内容

下面是你需要了解的在文件读取或写入文本的主要类型：

- [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) 表示文件。 此类具有提供文件相关信息的属性，以及创建、打开、复制、删除和重命名文件的方法。
你可以用来处理字符串路径。 某些 UWP API 获取字符串路径，但更多情况下，你将使用 **StorageFile** 表示文件，因为你在 UWP 中处理的某些文件可能没有路径，或者可能具有不实用的路径。 使用 [StorageFile.GetFileFromPathAsync()](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) 将字符串路径转换为 **StorageFile**。 

- [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) 类提供读取和写入文本的简单方式。 此类还可以读取/写入字节数组或缓冲区内容。 此类与 [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 类非常相似。 主要区别在于它不像 **PathIO** 那样获取字符串路径，而是获取 **StorageFile**。
- [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) 表示文件夹（目录）。 此类具有创建文件、查询文件夹内容以及创建、重命名和删除文件夹的方法，并具有提供文件夹相关信息的属性。 

获取 **StorageFolder** 的常见方法包括：

- [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) 允许用户导航到想要使用的文件夹。
- [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) 提供特定于应用本地文件夹之一的 **StorageFolder**，如本地、漫游和临时文件夹。
- [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders) 提供已知库（如音乐或图片库）的 **StorageFolder**。

## <a name="write-text-to-a-file"></a>将文本写入文件

 在此介绍中，我们主要介绍简单场景：读取和写入文本。 我们先来看看一些使用 **FileIO** 类向文件写入文本的代码。

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.CreateFileAsync("test.txt",
        Windows.Storage.CreationCollisionOption.OpenIfExists);

await Windows.Storage.FileIO.WriteTextAsync(file, "Example of writing a string\r\n");

// Append a list of strings, one per line, to the file
var listOfStrings = new List<string> { "line1", "line2", "line3" };
await Windows.Storage.FileIO.AppendLinesAsync(file, listOfStrings); // each entry in the list is written to the file on its own line.
```

我们首先确定文件应位于的位置。 `Windows.Storage.ApplicationData.Current.LocalFolder` 提供对本地数据文件夹的访问，此文件夹在安装应用时为应用创建。 请参阅[访问文件系统](#access-the-file-system)了解应用可以访问的文件夹的详细信息。

然后，我们使用 **StorageFolder** 创建文件（或者如果文件已存在，打开它）。

**FileIO** 类提供向文件写入文本的方便方式。 `FileIO.WriteTextAsync()` 将文件的所有内容替换为提供的文本。 `FileIO.AppendLinesAsync()` 将字符串集合附加到文件--每行写入一个字符串。

## <a name="read-text-from-a-file"></a>从文件读取文本

与写入文件相同，读取文件也是从指定文件的位置开始。 我们将使用与上述示例使用的相同位置。 然后我们将使用**FileIO**类读取其内容。

```csharp
Windows.Storage.StorageFolder storageFolder = Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile file = await storageFolder.GetFileAsync("test.txt");

string text = await Windows.Storage.FileIO.ReadTextAsync(file);
```

你还可以将文件的每一行读入集合中的各个字符串 `IList<string> contents = await Windows.Storage.FileIO.ReadLinesAsync(sampleFile);`

## <a name="access-the-file-system"></a>访问文件系统

在 UWP 平台中，文件夹访问仅限用于确保用户数据的完整性和隐私。 

### <a name="app-folders"></a>应用文件夹

安装 UWP 应用后，将在 c:\users\<user name>\AppData\Local\Packages\<app package identifier>\ 下创建若干文件夹，用来存储除其他内容以外的应用的本地、漫游和临时文件。 应用不需要声明任何访问这些文件夹的功能，其他应用无法访问这些文件夹。 当卸载应用时，这些文件夹也会被删除。

以下是一些你通常会使用的应用文件夹：

- **LocalState**：存储当前设备的本地数据。 在备份设备时，此目录中的数据将保存到 OneDrive 中的备份映像中。 如果用户重置或更换设备，数据将还原。 使用 `Windows.Storage.ApplicationData.Current.LocalFolder.` 访问此文件夹 将不希望备份的本地数据保存到 **LocalCacheFolder**（可以使用 `Windows.Storage.ApplicationData.Current.LocalCacheFolder` 访问）中的 OneDrive。

- **RoamingState**：存储在安装应用时应在所有设备上复制的数据。 Windows 限制漫游的数据量，所以仅在此处保存用户设置和小文件。 使用 `Windows.Storage.ApplicationData.Current.RoamingFolder` 访问漫游文件夹。

- **TempState**：存储应用未运行时可能被删除的数据。 使用 `Windows.Storage.ApplicationData.Current.TemporaryFolder` 访问此文件夹。

### <a name="access-the-rest-of-the-file-system"></a>访问文件系统的其余部分

UWP 应用必须通过在其清单中添加相应的功能来声明访问特定用户库的意图。 当安装应用时，用户随后会收到提示，以确认他们授权对指定库的访问。 否则，将不安装应用。 提供访问图片、视频和音乐库的功能。 请参阅[应用功能声明](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)获取完整列表。 若要获取这些库的 **StorageFolder**，请使用 [Windows.Storage.KnownFolders](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders) 类。

#### <a name="documents-library"></a>文档库

虽然存在访问用户文档库的功能，但该功能是受限功能，这意味着声明该功能的应用将被 Microsoft Store 拒绝，除非你按照流程获得特殊批准。 它不供常规使用。 请改为使用文件或文件夹选取器（请参阅[使用选取器打开文件和文件夹](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers)和[使用选取器保存文件](https://docs.microsoft.com/windows/uwp/files/quickstart-save-a-file-with-a-picker)），这允许用户导航到文件夹或文件。 当用户导航到文件夹或文件时，他们已隐式授予应用访问权限，系统将允许访问。

#### <a name="general-access"></a>常规访问

或者，你的应用可以在其清单中声明受限的 [broadFileSystem](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations) 功能，这也需要 Microsoft Store 批准。 随后应用可以访问用户有限访问的任何文件，而无需使用文件或文件夹选取器。

要获取应用可以访问的位置的完整列表，请参阅[文件访问权限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions)。

## <a name="useful-apis-and-docs"></a>有用的 API 和文档

以下是 API 和其他有用文档的核心摘要，可帮助你开始处理文件和文件夹。

### <a name="useful-apis"></a>有用的 API

| API | 描述 |
|------|---------------|
|  [Windows.Storage.StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) | 提供文件相关信息，以及创建、打开、复制、删除和重命名文件的方法。 |
| [Windows.Storage.StorageFolder](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder) | 提供文件夹相关信息、创建文件的方法以及创建、重命名、删除文件夹的方法。 |
| [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) |  提供读取和写入文本的简单方式。 此类还可以读取/写入字节数组或缓冲区内容。 |
| [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio) | 提供在给定了字符串路径的文件中读取/写入文本的简单方式。 此类还可以读取/写入字节数组或缓冲区内容。 |
| [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) & [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) |  在流中读取和写入缓冲区、字节、整数、GUID、TimeSpan 等内容。 |
| [Windows.Storage.ApplicationData.Current](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.current) | 提供对为应用创建的文件夹的访问，如本地文件夹、漫游文件夹和临时文件文件夹。 |
| [Windows.Storage.Pickers.FolderPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.folderpicker) |  让用户可以选择文件夹，并为其返回 **StorageFolder**。 这是你获得对应用默认无法访问的位置的访问权限的方式。 |
| [Windows.Storage.Pickers.FileOpenPicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker) | 让用户可以选择要打开的文件，并为其返回 **StorageFile**。 这是你获得对应用默认无法访问的文件的访问权限的方式。 |
| [Windows.Storage.Pickers.FileSavePicker](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker) | 让用户可以选择文件的文件名、扩展名和存储位置。 返回 **StorageFile**。 这是你将文件保存到应用默认无法访问的位置的方式。 |
|  [Windows.Storage.Streams namespace](https://docs.microsoft.com/uwp/api/windows.storage.streams) | 涵盖读取和写入流。 请特别了解一下 [DataReader](https://docs.microsoft.com/uwp/api/windows.storage.streams.datareader) 和 [DataWriter](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) 类，它们读取和写入缓冲区、字节、整数、GUID、TimeSpan 等。 |

### <a name="useful-docs"></a>有用的文档

| 主题 | 描述 |
|-------|----------------|
| [Windows.Storage 命名空间](https://docs.microsoft.com/uwp/api/windows.storage) | API 参考文档。 |
| [文件、文件夹和库](https://docs.microsoft.com/windows/uwp/files/) | 概念文档。 |
| [创建、写入和读取文件](https://docs.microsoft.com/windows/uwp/files/quickstart-reading-and-writing-files) | 涵盖创建、读取和写入文本、二进制数据和流。 |
| [本地存储应用数据入门](https://blogs.windows.com/buildingapps/2016/05/10/getting-started-storing-app-data-locally/#pCbJKGjcShh5DTV5.97) | 除了涵盖保存本地数据的最佳实践外，还涵盖了 LocalSettings 和 LocalCache 文件夹的用途。 |
| [开始使用漫游应用数据](https://blogs.windows.com/buildingapps/2016/05/03/getting-started-with-roaming-app-data/#RgjgLt5OkU9DbVV8.97) | 有关如何使用漫游应用数据的两部分系列文章。 |
| [漫游应用程序数据指南](https://msdn.microsoft.com/library/windows/apps/hh465094) | 请在设计应用时按照这些数据漫游指南操作。 |
| [存储和检索设置以及其他应用数据](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data) | 提供各种应用数据存储（如本地、漫游和临时文件夹）的概述。 请参阅[漫游数据](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#roaming-data)部分，了解有关写入在设备之间漫游的数据的指南和其他信息。 |
| [文件访问权限](https://docs.microsoft.com/windows/uwp/files/file-access-permissions) | 有关你的应用可以访问哪些文件系统位置的信息。 |
| [使用选取器打开文件和文件夹](https://docs.microsoft.com/windows/uwp/files/quickstart-using-file-and-folder-pickers) | 展示如何通过让用户通过选取器 UI 决定来访问文件和文件夹。 |
| [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/windows.storage.streams) | 用于读取和写入流的类型。 |
| [音乐、图片和视频库中的文件和文件夹](https://docs.microsoft.com/windows/uwp/files/quickstart-managing-folders-in-the-music-pictures-and-videos-libraries) | 涵盖如何从库中删除文件夹、获取库中的文件夹列表，并发现存储的照片、音乐和视频。 |

## <a name="useful-code-samples"></a>有用的代码示例

| 代码示例 | 描述 |
|-----------------|---------------|
| [应用程序数据示例](https://code.msdn.microsoft.com/windowsapps/ApplicationData-sample-fb043eb2) | 展示如何通过使用应用程序数据 API 来存储和检索特定于每个用户的数据。 |
| [文件访问示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess) | 展示如何创建、读取、写入、复制和删除文件。 |
| [文件选取器示例](https://code.msdn.microsoft.com/windowsapps/File-picker-sample-9f294cba) | 展示如何通过让用户使用 UI 选择文件和文件夹来访问它们，以及如何保存文件，以便用户可以指定要保存文件的名称、文件类型和位置。 |
| [JSON 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Json) | 展示如何使用 [Windows.Data.Json 命名空间](https://docs.microsoft.com/uwp/api/Windows.Data.Json)编码和解码 JavaScript 对象表示法 (JSON) 的对象、数组、字符串、数字和布尔值。 |
| [其他代码示例](https://developer.microsoft.com//windows/samples) | 在类别下拉列表中选择**文件、文件夹和库**。 |
