---
ms.assetid: 8BDDE64A-77D2-4F9D-A1A0-E4C634BCD890
title: 使用选取器保存文件
description: 使用 FileSavePicker 让用户指定名称和他们想让应用保存文件的位置。
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c1480c17d97cb8b5e227cc44b384f13095bfd469
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/13/2019
ms.locfileid: "66369256"
---
# <a name="save-a-file-with-a-picker"></a>使用选取器保存文件

**重要的 API**

-   [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker)
-   [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile)

使用 [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) 让用户指定名称和他们想让应用保存文件的位置。

> [!NOTE]
> 有关完整示例，请参阅[文件选取器示例](https://go.microsoft.com/fwlink/p/?linkid=619994)。

 

## <a name="prerequisites"></a>必备条件


-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C# 或 Visual Basic 编写异步应用，请参阅[使用 C# 或 Visual Basic 调用异步 API](https://docs.microsoft.com/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。

-   **对位置的访问权限**

    请参阅[文件访问权限](file-access-permissions.md)。

## <a name="filesavepicker-step-by-step"></a>FileSavePicker：分步

使用 [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker)，以便你的用户可以指定要保存的文件的名称、类型以及位置。 创建、自定义并显示文件选取器对象，然后通过返回的 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 对象保存数据，该对象表示选取的文件。

1.  **创建并自定义 FileSavePicker**

    ```cs
    var savePicker = new Windows.Storage.Pickers.FileSavePicker();
    savePicker.SuggestedStartLocation =
        Windows.Storage.Pickers.PickerLocationId.DocumentsLibrary;
    // Dropdown of file types the user can save the file as
    savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
    // Default file name if the user does not type one in or select a file to replace
    savePicker.SuggestedFileName = "New Document";
    ```

在文件选取器对象上设置与你的用户和你的应用相关的属性。 此示例设置三个属性：[**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedstartlocation)、[**FileTypeChoices**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.filetypechoices) 和 [**SuggestedFileName**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedfilename)。
     
- 由于我们的用户正在保存文档或文本文件，因此该示例通过使用 [**LocalFolder**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localfolder) 将 [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedstartlocation) 设置为应用的本地文件夹。 将 [**SuggestedStartLocation**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.suggestedstartlocation) 设置为适用于被保存的文件类型（例如音乐、图片、视频或文档）的位置。 用户可以从开始位置导航到其他位置。

- 由于我们希望确保我们的应用可以在其保存后能打开该文件，我们将使用 [**FileTypeChoices**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.filetypechoices).指定支持该示例的文件类型（Microsoft Word 文档和文本文件）。 确保你的应用支持你指定的所有文件类型。 用户将能够以你指定的任意文件类型保存他们的文件。 他们还可以通过选择另一个你指定的文件类型更改文件类型。 默认情况下，将选择列表中的第一个文件类型选项：若要控制该选项，请设置 [**DefaultFileExtension**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.defaultfileextension) 属性。

    > [!NOTE]
    > 文件选取器还使用当前选中的文件类型筛选显示的文件，以便仅向用户显示与选中的文件类型匹配的文件类型。

- 为了给用户省去一些键入，示例设置了 [**SuggestedFileName**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.suggestedfilename)。 请确保你建议的文件名与正在保存的文件相关。 例如，和 Word 一样，你可以建议现有的文件名（如果有），或者文档的第一行（如果用户正在保存还没有名称的文件）。

> [!NOTE]
> [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) 对象使用 [**PickerViewMode.List**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.PickerViewMode) 视图模式显示文件选取器。

2.  **显示 FileSavePicker 并将其保存到已选取的文件**

    通过调用 [**PickSaveFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.filesavepicker.picksavefileasync) 显示文件选取器。 在用户指定名称、文件类型和位置并确认保存文件之后，**PickSaveFileAsync** 将返回一个表示已保存文件的 [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) 对象。 由于你对此文件具有读取和写入权限，因此你可以捕获和处理此文件。

    ```cs
    Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();
        if (file != null)
        {
            // Prevent updates to the remote version of the file until
            // we finish making changes and call CompleteUpdatesAsync.
            Windows.Storage.CachedFileManager.DeferUpdates(file);
            // write to file
            await Windows.Storage.FileIO.WriteTextAsync(file, file.Name);
            // Let Windows know that we're finished changing the file so
            // the other app can update the remote version of the file.
            // Completing updates may require Windows to ask for user input.
            Windows.Storage.Provider.FileUpdateStatus status =
                await Windows.Storage.CachedFileManager.CompleteUpdatesAsync(file);
            if (status == Windows.Storage.Provider.FileUpdateStatus.Complete)
            {
                this.textBlock.Text = "File " + file.Name + " was saved.";
            }
            else
            {
                this.textBlock.Text = "File " + file.Name + " couldn't be saved.";
            }
        }
        else
        {
            this.textBlock.Text = "Operation cancelled.";
        }
    ```

该示例检查了文件是否有效并将其自己的文件名写入其中。 另请参阅[创建、写入和读取文件](quickstart-reading-and-writing-files.md)。

> [!TIP]
> 应始终检查已保存的文件，以确保在执行任何其他处理之前该文件有效。 然后，你可以将内容保存到适合你的应用的文件，并在已选取的文件无效时提供相应的行为。
