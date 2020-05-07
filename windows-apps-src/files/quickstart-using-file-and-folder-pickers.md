---
ms.assetid: F87DBE2F-77DB-4573-8172-29E11ABEFD34
title: 使用选取器打开文件和文件夹
description: 通过让用户选取器交互来访问文件和文件夹。 你可以使用 FileOpenPicker 和 FileSavePicker 类获取对文件的访问权限，并使用 FolderPicker 获取对文件夹的访问权限。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b3143247eab0bd8ed4266b78e34ec54dafa4dd7e
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "74259571"
---
# <a name="open-files-and-folders-with-a-picker"></a>使用选取器打开文件和文件夹

**重要的 API**

-   [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker)
-   [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker)
-   [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)

通过让用户选取器交互来访问文件和文件夹。 你可以使用 [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 和 [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) 类访问文件，并使用 [**FolderPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker) 访问文件夹。

> [!NOTE]
> 有关完整示例，请参阅[文件选取器示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FilePicker)。

## <a name="prerequisites"></a>必备条件


-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C# 或 Visual Basic 编写异步应用，请参阅[使用 C# 或 Visual Basic 调用异步 API](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。

-   **对位置的访问权限**

    请参阅[文件访问权限](file-access-permissions.md)。

## <a name="file-picker-ui"></a>文件选取器 UI


文件选取器显示信息以引导用户并在打开或保存文件时提供一致性体验。

该信息包括：

-   当前位置
-   用户选取的项
-   用户可以浏览到的位置的树。 这些位置包括文件系统位置（如音乐或下载文件夹）以及实现文件选取器合约的应用（如相机、照片和 Microsoft OneDrive）。

电子邮件应用可能会显示文件选取器，以供用户选取附件。

![选取了两个要打开的文件的文件选取器。](images/picker-multifile-600px.png)

## <a name="how-pickers-work"></a>选取器的工作原理


通过选取器，你的应用可以在用户的系统上访问、浏览以及保存文件和文件夹。 你的应用会接收这些选取项作为 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 和 [**StorageFolder**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFolder) 对象，然后你能在这些对象上进行操作。

选取器使用一个单一的统一界面，让用户从文件系统或从其他应用选取文件和文件夹。 从其他应用选取的文件与文件系统中的文件类似：它们是作为 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 对象返回的。 通常，你的应用可以按与其他对象相同的方式对它们进行操作。 其他应用通过参与文件选取器合约使文件可用。 如果你希望你的应用提供文件、保存位置或其他应用的文件更新，请参阅[与文件选取器合约集成](https://docs.microsoft.com/previous-versions/windows/apps/hh465192(v=win.10))。

例如，你可能会在你的应用中调用文件选取器，以便你的用户可以打开文件。 这会使你的应用成为调用应用。 文件选取器与系统和/或其他应用交互来让用户导航和选取文件。 当你的用户选择文件时，文件选取器会将该文件返回到你的应用。 这里是从提供的应用（如 OneDrive）中选择文件所遇到的情形的过程。

![显示将文件选取器用作两个应用之间的接口时一个应用获取文件以从另一个应用打开的过程的图表。](images/app-to-app-diagram-600px.png)

## <a name="pick-a-single-file-complete-code-listing"></a>选取单个文件：完成代码列表


```cs
var picker = new Windows.Storage.Pickers.FileOpenPicker();
picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
picker.FileTypeFilter.Add(".jpg");
picker.FileTypeFilter.Add(".jpeg");
picker.FileTypeFilter.Add(".png");

Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();
if (file != null)
{
    // Application now has read/write access to the picked file
    this.textBlock.Text = "Picked photo: " + file.Name;
}
else
{
    this.textBlock.Text = "Operation cancelled.";
}
```

## <a name="pick-a-single-file-step-by-step"></a>选取单个文件：分步


使用文件选取器需要创建和自定义文件选取器对象，然后显示文件选取器，以使用户能选取一个或多个项目。

1.  **创建和自定义 FileOpenPicker**

    ```cs
    var picker = new Windows.Storage.Pickers.FileOpenPicker();
    picker.ViewMode = Windows.Storage.Pickers.PickerViewMode.Thumbnail;
    picker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.PicturesLibrary;
    picker.FileTypeFilter.Add(".jpg");
    picker.FileTypeFilter.Add(".jpeg");
    picker.FileTypeFilter.Add(".png");
    ```
    在文件选取器对象上设置与你的用户和应用相关的属性。

    此示例在某个方便的位置创建一种丰富的图片视觉显示，用户可以通过设置以下三个属性从该位置选取：[**ViewMode**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.viewmode)、[**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) 和 [**FileTypeFilter**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter)。

    -   将 [**ViewMode**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.viewmode) 设置为 [**PickerViewMode**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerViewMode)。**Thumbnail** 枚举值可通过使用图片缩略图显示文件选取器中的文件，来创建丰富的视觉显示。 此操作用于选取可视文件（如图片或视频）。 否则，请使用 [**PickerViewMode.List**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerViewMode)。 假定的电子邮件应用可在显示文件选取器之前设置适用于功能的 **ViewMode**，该应用具有**附加图片或视频**和**附加文档**功能。

    -   使用 [**PickerLocationId.PicturesLibrary**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) 将 [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerLocationId) 设置为图片可让用户在他们有可能找到图片的某个位置开始。 将 **SuggestedStartLocation** 设置为适用于要选取的文件类型（例如音乐、图片、视频或文档）的位置。 用户可以从开始位置导航到其他位置。

    -   使用 [**FileTypeFilter**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter) 指定文件类型可保持用户专注于选取相关的文件。 若要将 **FileTypeFilter** 中以前的文件类型替换为新条目，请使用 [**ReplaceAll**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileextensionvector.replaceall) 而不是 [**Add**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileextensionvector.append)。

2.  **显示 FileOpenPicker**

    - **选取单个文件**

        ```cs
        Windows.Storage.StorageFile file = await picker.PickSingleFileAsync();
        if (file != null)
        {
            // Application now has read/write access to the picked file
            this.textBlock.Text = "Picked photo: " + file.Name;
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
        ```

    - **选取多个文件**  

        ```cs
        var files = await picker.PickMultipleFilesAsync();
        if (files.Count > 0)
        {
            StringBuilder output = new StringBuilder("Picked files:\n");
    
            // Application now has read/write access to the picked file(s)
            foreach (Windows.Storage.StorageFile file in files)
            {
                output.Append(file.Name + "\n");
            }
            this.textBlock.Text = output.ToString();
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
        ```

## <a name="pick-a-folder-complete-code-listing"></a>选取文件夹：完整代码列表


```cs
var folderPicker = new Windows.Storage.Pickers.FolderPicker();
folderPicker.SuggestedStartLocation = Windows.Storage.Pickers.PickerLocationId.Desktop;
folderPicker.FileTypeFilter.Add("*");

Windows.Storage.StorageFolder folder = await folderPicker.PickSingleFolderAsync();
if (folder != null)
{
    // Application now has read/write access to all contents in the picked folder
    // (including other sub-folder contents)
    Windows.Storage.AccessCache.StorageApplicationPermissions.
    FutureAccessList.AddOrReplace("PickedFolderToken", folder);
    this.textBlock.Text = "Picked folder: " + folder.Name;
}
else
{
    this.textBlock.Text = "Operation cancelled.";
}
```

> [!TIP]
> 不论何时，只要应用通过选取器访问文件或文件夹，就将它添加到应用的 [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) 或 [**MostRecentlyUsedList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.mostrecentlyusedlist) 以便对进行跟踪。 你可以在[如何跟踪最近使用的文件和文件夹](how-to-track-recently-used-files-and-folders.md)中了解有关使用这些列表的详细信息。