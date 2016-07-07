---
author: TylerMSFT
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: "文件访问权限"
description: "默认情况下，应用可以访问特定文件系统位置。 应用也可以通过文件选取器或通过声明功能访问其他位置。"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 91f97f1ba245b0cf6cac1cff7971cace5ca3b5a0

---
# 文件访问权限

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


默认情况下，应用可以访问特定文件系统位置。 应用也可以通过文件选取器或通过声明功能访问其他位置。

## 所有应用均可访问的位置

在创建新的应用时，默认情况下你可以访问以下文件系统位置：

-   **应用程序安装目录**。 用户系统上用于安装应用的文件夹。

    主要可以使用两种方式来访问应用的安装目录中的文件和文件夹：

    1.  可以检索代表应用的安装目录的 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
        ```
        ```javascript
        var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
        ```

       可以使用 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 方法访问目录中的文件和文件夹。 在本例中，此 **StorageFolder** 存储在 `installDirectory` 变量中。 你可以通过下载适用于 Windows 8.1 的[应用程序包信息示例](http://go.microsoft.com/fwlink/p/?linkid=231526)并在 Windows 10 应用中重新使用其源代码，了解有关使用你的应用程序包和安装目录的详细信息。

    2.  你可以通过使用应用 URI 直接从应用的安装目录检索文件，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;            
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync("ms-appx:///file.txt");
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```
        
        当 [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 完成时，它将返回一个 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)，代表应用的安装目录中的 *file.txt* 文件（示例中的 `file`）。

        URI 中的“ms-appx:///”前缀是指应用的安装目录。 你可以在[如何使用 URI 来引用内容](https://msdn.microsoft.com/library/windows/apps/hh781215)中了解有关使用应用 URI 的更多信息。

    此外，与其他位置不同，你还可以使用一些[用于通用 Windows 平台 (UWP) 应用的 Win32 和 COM](https://msdn.microsoft.com/library/windows/apps/br205757) 和一些 [Microsoft Visual Studio 中的 C/C++ 标准库函数](http://msdn.microsoft.com/library/hh875057.aspx)来访问应用安装目录中的文件。

    应用的安装目录是一个只读位置。 不能通过文件选取器获得对安装目录的访问。

-   **应用程序数据位置。** 你的应用可以存储数据的文件夹。 在安装你的应用时创建这些文件夹（本地、漫游和临时）。

    可以通过两种主要方法来访问应用的数据位置中的文件和文件夹：

    1.  使用 [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587) 属性检索应用数据文件夹。

        例如，可以使用 [**ApplicationData**](https://msdn.microsoft.com/library/windows/apps/br241587).[**LocalFolder**](https://msdn.microsoft.com/library/windows/apps/br241621) 检索代表应用的本地文件夹的 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFolder localFolder = ApplicationData.Current.LocalFolder;
        ```
        ```javascript
        var localFolder = Windows.Storage.ApplicationData.current.localFolder;
        ```
 
        如果希望访问应用的漫游或临时文件夹，可以改用 [**RoamingFolder**](https://msdn.microsoft.com/library/windows/apps/br241623) 或 [**TemporaryFolder**](https://msdn.microsoft.com/library/windows/apps/br241629) 属性。

        在检索代表应用数据位置的 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 之后，可以使用 **StorageFolder** 方法访问该位置中的文件和文件夹。 在本例中，这些 **StorageFolder** 对象存储在 `localFolder` 变量中。 你可以在[管理应用程序数据](https://msdn.microsoft.com/library/windows/apps/hh465109)中，以及通过下载适用于 Windows 8.1 的[应用程序数据示例](http://go.microsoft.com/fwlink/p/?linkid=231478)并在 Windows 10 应用中重新使用其源代码，来了解有关使用应用数据位置的详细信息。

    2.  例如，你可以通过使用应用 URI 直接从应用的本地文件夹检索文件，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFile file = await StorageFile.GetFileFromApplicationUriAsync("ms-appdata:///local/file.txt");
        ```
        ```javascript
        Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
            function(file) {
                // Process file
            }
        );
        ```

        当 [**GetFileFromApplicationUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701741) 完成时，它将返回一个 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)，代表应用的本地文件夹中的 *file.txt* 文件（示例中的 `file`）。

        URI 中的“ms-appdata:///local/”前缀是指应用的本地文件夹。 要访问应用的漫游文件夹或临时文件夹中的文件，请改用“ms-appdata:///roaming/”或“ms-appdata:///temporary/”。 你可以在[如何加载文件资源](https://msdn.microsoft.com/library/windows/apps/hh781229)中了解关于使用应用 URI 的详细信息。

    此外，与其他位置不同，你还可以使用一些[用于 UWP 应用的 Win32 和 COM](https://msdn.microsoft.com/library/windows/apps/br205757) 以及和一些 Visual Studio 中的 C/C++ 标准库函数来访问应用数据位置中的文件。

    你不能通过文件选取器访问本地、漫游或临时文件夹。

-   **可移动设备。** 此外，默认情况下你的应用可以访问连接设备上的一些文件。 如果你的应用使用在用户将设备（如相机或 U 盘）连接到系统时自动启动的[自动播放扩展](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh464906.aspx#autoplay)，可以选择此选项。 你的应用可以访问的文件限于通过应用清单中的文件类型关联声明指定的特定文件类型。

    当然，还可以通过调用文件选取器（使用 [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 和 [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)）并让用户选取供你的应用访问的文件和文件夹，以获得对可移动设备上的文件和文件夹的访问权。 通过[使用选取器打开文件和文件夹](quickstart-using-file-and-folder-pickers.md)了解如何使用文件选取器。

    **注意** 有关从移动应用访问 SD 卡的详细信息，请参阅[访问 SD 卡](access-the-sd-card.md)。

     

## Windows 应用商店应用可以访问的位置

-   **用户的“下载”文件夹。** 默认情况下保存下载文件的文件夹。

    默认情况下，你的应用只能访问它创建的用户“下载”文件夹中的文件和文件夹。 不过，你可以通过调用文件选取器（[**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847) 或 [**FolderPicker**](https://msdn.microsoft.com/library/windows/apps/br207881)）让用户可以导航并选取供应用访问的文件或文件夹，以获取对用户下载文件夹中的文件和文件夹的访问权。

    -   你可以在用户的“下载”文件夹中创建文件，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFile newFile = await DownloadsFolder.CreateFileAsync("file.txt");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFileAsync("file.txt").done(
            function(newFile) {
                // Process file
            }
        );
        ```
 
        [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh996761) 重载，这样可以指定“下载”文件夹中已经存在同名文件时系统应执行的操作。 这些方法完成之后，它们将返回一个代表已创建文件的 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171)。 此文件在本例中称为 `newFile`。

    -   你可以在用户的“下载”文件夹中创建子文件夹，如下所示：
        > [!div class="tabbedCodeSnippets"]
        ```csharp
        using Windows.Storage;
        StorageFolder newFolder = await DownloadsFolder.CreateFolderAsync("New Folder");
        ```
        ```javascript
        Windows.Storage.DownloadsFolder.createFolderAsync("New Folder").done(
            function(newFolder) {
                // Process folder
            }
        );
        ```
 
        [**DownloadsFolder**](https://msdn.microsoft.com/library/windows/apps/br241632).[**CreateFolderAsync**](https://msdn.microsoft.com/library/windows/apps/hh996763) 重载，这样可以指定下载文件夹中已经存在同名子文件夹时系统应执行的操作。 这些方法完成之后，它们将返回一个代表已创建子文件夹的 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230)。 此文件在本例中称为 `newFolder`。

    如果在“下载”文件夹中创建文件或文件夹，我们建议将该项目添加到你的应用的 [**FutureAccessList**](https://msdn.microsoft.com/library/windows/apps/br207457)，这样你的应用以后可以随时访问该项目。

## 访问其他位置

除了默认位置以外，应用还可以通过在应用清单中声明功能（请参阅[应用功能声明](https://msdn.microsoft.com/library/windows/apps/mt270968)）或者通过调用文件选取器让用户选取供应用访问的文件和文件夹（请参阅[使用选取器打开文件和文件夹](quickstart-using-file-and-folder-pickers.md)）来访问其他文件和文件夹。

下表列出了其他可通过声明某个功能（或多个功能）和使用关联的 [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/br227346) API 来访问的位置：

| 位置 | 功能 | Windows.Storage API |
|----------|------------|---------------------|
| 文档 | DocumentsLibrary <br><br>注意：必须将文件类型关联（该关联声明你的应用可以在此位置中访问的特定文件类型）添加到你的应用清单。 <br><br>你的应用使用此功能的情况：<br>- 使用有效的 OneDrive URL 或资源 ID 促进跨平台离线访问特定 OneDrive 内容<br>- 在离线时将打开的文件自动保存到用户的 OneDrive | [KnownFolders.DocumentsLibrary](https://msdn.microsoft.com/library/windows/apps/br227152) |
| 音乐     | MusicLibrary <br>另请参阅[音乐、图片和视频库中的文件和文件夹](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.MusicLibrary](https://msdn.microsoft.com/library/windows/apps/br227155) |    
| 图片  | PicturesLibrary<br> 另请参阅[音乐、图片和视频库中的文件和文件夹](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.PicturesLibrary](https://msdn.microsoft.com/library/windows/apps/br227156) |  
| 视频    | VideosLibrary<br>另请参阅[音乐、图片和视频库中的文件和文件夹](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.VideosLibrary](https://msdn.microsoft.com/library/windows/apps/br227159) |   
| 可移动设备  | RemovableDevices <br><br>请注意，必须将文件类型关联（该关联声明你的应用可以在此位置中访问的特定文件类型）添加到你的应用清单。 <br><br>另请参阅[访问 SD 卡](access-the-sd-card.md)。 | [KnownFolders.RemovableDevices](https://msdn.microsoft.com/library/windows/apps/br227158) |  
| 家庭组库  | 至少需要下列功能之一。 <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.HomeGroup](https://msdn.microsoft.com/library/windows/apps/br227153) |      
| 媒体服务器设备 (DLNA) | 至少需要下列功能之一。 <br>- MusicLibrary <br>- PicturesLibrary <br>- VideosLibrary | [KnownFolders.MediaServerDevices](https://msdn.microsoft.com/library/windows/apps/br227154) | 
| 通用命名约定 (UNC) 文件夹 | 需要下列功能组合。 <br><br>家庭和工作网络功能： <br>- PrivateNetworkClientServer <br><br>至少一个 Internet 和公共网络功能： <br>- InternetClient <br>- InternetClientServer <br><br>域凭据功能（如果适用）：<br>- EnterpriseAuthentication <br><br>注意：必须将文件类型关联（该关联声明你的应用可以在此位置中访问的特定文件类型）添加到你的应用清单。 | 使用以下项检索文件夹： <br>[StorageFolder.GetFolderFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227278) <br><br>使用以下项检索文件： <br>[StorageFile.GetFileFromPathAsync](https://msdn.microsoft.com/library/windows/apps/br227206) |




<!--HONumber=Jun16_HO4-->


