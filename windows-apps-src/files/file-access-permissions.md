---
ms.assetid: 3A404CC0-A997-45C8-B2E8-44745539759D
title: 文件访问权限
description: 默认情况下，应用可以访问特定文件系统位置。 应用也可以通过文件选取器或通过声明功能访问其他位置。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- javascript
ms.openlocfilehash: 6a14a011971d8cea7b05758dc1a8a91ccab37edd
ms.sourcegitcommit: b0930dfeb45e696fe4fa14bdb547de13ba5ade89
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/11/2020
ms.locfileid: "77146367"
---
# <a name="file-access-permissions"></a>文件访问权限

默认情况下，通用 Windows 平台 (UWP) 应用可以访问特定文件系统位置。 应用也可以通过文件选取器或通过声明功能访问其他位置。

## <a name="the-locations-that-all-apps-can-access"></a>所有应用均可访问的位置

在创建新的应用时，默认情况下你可以访问以下文件系统位置：

### <a name="application-install-directory"></a>应用程序安装目录
用户系统上用于安装应用的文件夹。

主要可以使用两种方式来访问应用的安装目录中的文件和文件夹：

1. 可以检索代表应用的安装目录的 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)，如下所示：

    ```csharp
    Windows.Storage.StorageFolder installedLocation = Windows.ApplicationModel.Package.Current.InstalledLocation;
    ```
    
    ```javascript
    var installDirectory = Windows.ApplicationModel.Package.current.installedLocation;
    ```
    
    ```cppwinrt
    #include <winrt/Windows.Storage.h>
    ...
    Windows::Storage::StorageFolder installedLocation{ Windows::ApplicationModel::Package::Current().InstalledLocation() };
    ```
    
    ```cpp
    Windows::Storage::StorageFolder^ installedLocation = Windows::ApplicationModel::Package::Current->InstalledLocation;
    ```

    可以使用 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) 方法访问目录中的文件和文件夹。 在本例中，此 **StorageFolder** 存储在 `installDirectory` 变量中。 可以从 GitHub 上的[应用包信息示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Package)了解有关使用应用包和安装目录的详细信息。

2. 你可以通过使用应用 URI 直接从应用的安装目录检索文件，如下所示：

    ```csharp
    using Windows.Storage;            
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appx:///file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
    {
        Windows::Storage::StorageFile file{
            co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{L"ms-appx:///file.txt"})
        };
        // Process file
    }
    ```
    
    ```cpp
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appx:///file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```

    当 [**GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) 完成时，它将返回一个 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)，代表应用的安装目录中的 `file.txt` 文件（示例中的 `file`）。
    
    URI 中的“ms-appx:///”前缀是指应用的安装目录。 你可以在[如何使用 URI 来引用内容](https://docs.microsoft.com/previous-versions/windows/apps/hh781215(v=win.10))中了解有关使用应用 URI 的更多信息。

此外，与其他位置不同，你还可以使用一些[用于通用 Windows 平台 (UWP) 应用的 Win32 和 COM](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) 和一些 [Microsoft Visual Studio 中的 C/C++ 标准库函数](https://docs.microsoft.com/cpp/cpp/c-cpp-language-and-standard-libraries)来访问应用安装目录中的文件。

应用的安装目录是一个只读位置。 不能通过文件选取器获得对安装目录的访问。

### <a name="application-data-locations"></a>应用程序数据位置
你的应用可以存储数据的文件夹。 在安装你的应用时创建这些文件夹（本地、漫游和临时）。

可以通过两种主要方法来访问应用的数据位置中的文件和文件夹：

1.  使用 [**ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData) 属性检索应用数据文件夹。

    例如，可以使用 [**ApplicationData**](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData).[**LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) 检索代表应用的本地文件夹的 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)，如下所示：
    
    ```csharp
    using Windows.Storage;
    StorageFolder localFolder = ApplicationData.Current.LocalFolder;
    ```
    
    ```javascript
    var localFolder = Windows.Storage.ApplicationData.current.localFolder;
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFolder storageFolder{
        Windows::Storage::ApplicationData::Current().LocalFolder()
    };
    ```
    
    ```cpp
    using namespace Windows::Storage;
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    ```
    
    如果希望访问应用的漫游或临时文件夹，可以改用 [**RoamingFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingfolder) 或 [**TemporaryFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) 属性。
    
    在检索代表应用数据位置的 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) 之后，可以使用 **StorageFolder** 方法访问该位置中的文件和文件夹。 在本例中，这些 **StorageFolder** 对象存储在 `localFolder` 变量中。 可以从 [ApplicationData 类](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata)页面上的指南，以及通过从 GitHub 下载[应用程序数据示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData)，来了解有关使用应用数据位置的详细信息。

2. 可以通过使用应用 URI 直接从应用的本地文件夹检索文件，如下所示：
    
    ```csharp
    using Windows.Storage;
    StorageFile file = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appdata:///local/file.txt"));
    ```
    
    ```javascript
    Windows.Storage.StorageFile.getFileFromApplicationUriAsync("ms-appdata:///local/file.txt").done(
        function(file) {
            // Process file
        }
    );
    ```
    
    ```cppwinrt
    Windows::Storage::StorageFile file{
        co_await Windows::Storage::StorageFile::GetFileFromApplicationUriAsync(Windows::Foundation::Uri{ L"ms-appdata:///local/file.txt" })
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto getFileTask = create_task(StorageFile::GetFileFromApplicationUriAsync(ref new Uri("ms-appdata:///local/file.txt")));
    getFileTask.then([](StorageFile^ file) 
    {
        // Process file
    });
    ```
    
    当 [**GetFileFromApplicationUriAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefromapplicationuriasync) 完成时，它将返回一个 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)，代表应用的本地文件夹中的 `file.txt` 文件（示例中的 `file`）。
    
    URI 中的“ms-appdata:///local/”前缀是指应用的本地文件夹。 要访问应用的漫游文件夹或临时文件夹中的文件，请改用“ms-appdata:///roaming/”或“ms-appdata:///temporary/”。 你可以在[如何加载文件资源](https://docs.microsoft.com/previous-versions/windows/apps/hh781229(v=win.10))中了解关于使用应用 URI 的详细信息。

此外，与其他位置不同，你还可以使用一些[用于 UWP 应用的 Win32 和 COM](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps) 以及和一些 Visual Studio 中的 C/C++ 标准库函数来访问应用数据位置中的文件。

无法通过文件选取器访问本地、漫游或临时文件夹。

### <a name="removable-devices"></a>可移动设备

此外，默认情况下你的应用可以访问连接设备上的一些文件。 如果你的应用使用在用户将设备（如相机或 U 盘）连接到系统时自动启动的[自动播放扩展](https://docs.microsoft.com/previous-versions/windows/apps/hh464906(v=win.10))，可以选择此选项。 你的应用可以访问的文件限于通过应用清单中的文件类型关联声明指定的特定文件类型。

当然，还可以通过调用文件选取器（使用 [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 和 [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker)）并让用户选取供你的应用访问的文件和文件夹，以获得对可移动设备上的文件和文件夹的访问权。 通过[使用选取器打开文件和文件夹](quickstart-using-file-and-folder-pickers.md)了解如何使用文件选取器。

> [!NOTE]
> 有关访问 SD 卡或其他可移动设备的详细信息，请参阅[访问 SD 卡](access-the-sd-card.md)。

## <a name="locations-that-uwp-apps-can-access"></a>UWP 应用可访问的位置

### <a name="users-downloads-folder"></a>用户的“下载”文件夹

默认情况下保存下载文件的文件夹。

默认情况下，你的应用只能访问它创建的用户“下载”文件夹中的文件和文件夹。 不过，你可以通过调用文件选取器（[**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 或 [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker)）让用户可以导航并选取供应用访问的文件或文件夹，以获取对用户下载文件夹中的文件和文件夹的访问权。

- 你可以在用户的下载文件夹中创建文件，如下所示：

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
    
    ```cppwinrt
    Windows::Storage::StorageFile newFile{
        co_await Windows::Storage::DownloadsFolder::CreateFileAsync(L"file.txt")
    };
    // Process file
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFileTask = create_task(DownloadsFolder::CreateFileAsync(L"file.txt"));
    createFileTask.then([](StorageFile^ newFile)
    {
        // Process file
    });
    ```

    [**DownloadsFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfileasync) 重载，这样可以指定在“下载”文件夹中已经存在同名文件时系统应执行的操作。 这些方法完成之后，它们将返回一个代表已创建文件的 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)。 此文件在本例中称为 `newFile`。

- 你可以在用户的“下载”文件夹中创建子文件夹，如下所示：

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
    
    ```cppwinrt
    Windows::Storage::StorageFolder newFolder{
        co_await Windows::Storage::DownloadsFolder::CreateFolderAsync(L"New Folder")
    };
    // Process folder
    ```
    
    ```cpp
    using Windows::Storage;
    auto createFolderTask = create_task(DownloadsFolder::CreateFolderAsync(L"New Folder"));
    createFolderTask.then([](StorageFolder^ newFolder)
    {
        // Process folder
    });
    ```

    [**DownloadsFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.DownloadsFolder).[**CreateFolderAsync**](https://docs.microsoft.com/uwp/api/windows.storage.downloadsfolder.createfolderasync) 重载，这样可以指定在“下载”文件夹中已经存在同名子文件夹时系统应执行的操作。 这些方法完成之后，它们将返回一个代表已创建子文件夹的 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder)。 此文件在本例中称为 `newFolder`。

如果在“下载”文件夹中创建文件或文件夹，我们建议将该项目添加到你的应用的 [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist)，这样你的应用以后可以随时访问该项目。

## <a name="accessing-additional-locations"></a>访问其他位置

除了默认位置以外，应用还可以通过[在应用清单中声明功能](../packaging/app-capability-declarations.md)或者通过[调用文件选取器](quickstart-using-file-and-folder-pickers.md)让用户选取供应用访问的文件和文件夹来访问其他文件和文件夹。

声明 [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) 扩展的应用具有来自控制台窗口中启动的目录及下级目录的文件系统权限。

下表列出了其他可通过声明一项或多项功能和使用关联的 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) API 来访问的位置。

| 位置 | 功能 | Windows.Storage API |
|----------|------------|---------------------|
| 用户有权访问的所有文件。 例如：文档、图片、照片、下载、桌面、OneDrive 等。 | **broadFileSystemAccess**<br><br>此功能受限。 可以通过“设置”   > “隐私”   > “文件系统”  配置访问。 由于用户随时可以在“设置”中授予或拒绝权限，因此，你应确保应用能够弹性应对这些更改  。 如果你发现应用没有访问权限，可以选择通过提供 [Windows 10 文件系统访问权限和隐私](https://support.microsoft.com/help/4468237/windows-10-file-system-access-and-privacy-microsoft-privacy)一文的链接，来提示用户更改该设置。 请注意，用户必须关闭应用、切换设置，然后重启应用。 如果他们在应用正在运行时切换设置，则平台将挂起该应用，以便你可以保存状态，然后强制终止该应用以应用新的设置。 在 2018 年 4 月更新中，该权限默认设置为“打开”。 在 2018 年 10 月更新中，默认设置为“关闭”。<br /><br />如果向 Microsoft Store 提交声明此功能的应用，将需要额外说明应用需要此功能的原因以及打算使用它的方式。<br/><br/>此功能适用于 [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage) 命名空间中的 API。 请参阅本文末尾的**示例**部分，其中的某个示例演示了如何在应用中启用此功能。<br/><br/>**注意：** 此功能在 Xbox 上不受支持。 | 不适用 |
| 文档 | **documentsLibrary**<br><br>注意：必须将文件类型关联（该关联声明应用可以在此位置中访问的特定文件类型）添加到应用清单。 <br><br>你的应用使用此功能的情况：<br>- 使用有效的 OneDrive URL 或资源 ID 促进跨平台离线访问特定 OneDrive 内容<br>- 在离线时将打开的文件自动保存到用户的 OneDrive | [KnownFolders.DocumentsLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.documentslibrary) |
| 音乐     | **musicLibrary** <br>另请参阅[音乐、图片和视频库中的文件和文件夹](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.MusicLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.musiclibrary) |    
| 图片  | **picturesLibrary**<br> 另请参阅[音乐、图片和视频库中的文件和文件夹](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.PicturesLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.pictureslibrary) |  
| 视频    | **videosLibrary**<br>另请参阅[音乐、图片和视频库中的文件和文件夹](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md)。 | [KnownFolders.VideosLibrary](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.videoslibrary) |   
| 可移动设备  | **removableStorage**  <br><br>必须将文件类型关联（该关联声明应用可以在此位置中访问的特定文件类型）添加到应用清单。 <br><br>另请参阅[访问 SD 卡](access-the-sd-card.md)。 | [KnownFolders.RemovableDevices](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.removabledevices) |  
| 家庭组库  | 至少需要下列功能之一。 <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.HomeGroup](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.homegroup) |      
| 媒体服务器设备 (DLNA) | 至少需要下列功能之一。 <br>- **musicLibrary** <br>- **picturesLibrary** <br>- **videosLibrary** | [KnownFolders.MediaServerDevices](https://docs.microsoft.com/uwp/api/windows.storage.knownfolders.mediaserverdevices) |
| 通用命名约定 (UNC) 文件夹 | 需要下列功能组合。 <br><br>家庭和工作网络功能： <br>- **privateNetworkClientServer** <br><br>至少一个 Internet 和公共网络功能： <br>- **internetClient** <br>- **internetClientServer** <br><br>域凭据功能（如果适用）：<br>- **enterpriseAuthentication** <br><br>**注意：** 必须将文件类型关联（该关联声明应用可以在此位置中访问的特定文件类型）添加到应用清单。 | 使用以下项检索文件夹： <br>[StorageFolder.GetFolderFromPathAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfolderfrompathasync) <br><br>使用以下项检索文件： <br>[StorageFile.GetFileFromPathAsync](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getfilefrompathasync) |

### <a name="example"></a>示例

此示例添加受限的 **broadFileSystemAccess** 功能。 除了指定功能，还必须添加 `rescap` 命名空间并将其添加到 `IgnorableNamespaces`。

```xaml
<Package
  ...
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp uap5 rescap">
...
<Capabilities>
    <rescap:Capability Name="broadFileSystemAccess" />
</Capabilities>
```

> [!NOTE]
> 有关应用功能的完整列表，请参阅[应用功能声明](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations)。
