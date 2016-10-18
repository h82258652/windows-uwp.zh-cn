---
author: normesta
ms.assetid: 4C59D5AC-58F7-4863-A884-E9E54228A5AD
title: "枚举和查询文件和文件夹"
description: "访问位于文件夹、库、设备或网络位置的文件和文件夹。 还可以通过构造文件和文件夹查询来查询某个位置的文件和文件夹。"
translationtype: Human Translation
ms.sourcegitcommit: de0b23cfd8f6323d3618c3424a27a7d0ce5e1374
ms.openlocfilehash: a7a8ba7166cf8c6778003396b13b7098578097ca

---
# 枚举和查询文件和文件夹


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


访问位于文件夹、库、设备或网络位置的文件和文件夹。 还可以通过构造文件和文件夹查询来查询某个位置的文件和文件夹。

**注意** 另请参阅[文件夹枚举示例](http://go.microsoft.com/fwlink/p/?linkid=619993)。

 
## 先决条件

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C# 或 Visual Basic 编写异步应用，请参阅[使用 C# 或 Visual Basic 调用异步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **对位置的访问权限**

    例如，这些示例中的代码需要 **picturesLibrary** 功能，但是你的位置可能需要其他功能或根本不需要任何功能。 若要了解详细信息，请参阅[文件访问权限](file-access-permissions.md)。

## 枚举某个位置中的文件和文件夹

> **注意** 记得声明 **picturesLibrary** 功能。

在此示例中，我们首先使用 [**StorageFolder.GetFilesAsync**](https://msdn.microsoft.com/library/windows/apps/br227276) 方法获取 [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) 的根文件夹（而不是在子文件夹）中的所有文件，并列出每个文件的名称。 接下来，我们使用 [**GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br227280) 方法获取 **PicturesLibrary** 中的所有子文件夹并列出每个子文件夹的名称。

<!--BUGBUG: IAsyncOperation<IVectorView<StorageFolder^>^>^  causes build to flake out-->
> [!div class="tabbedCodeSnippets"]
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Platform::Collections;
> using namespace concurrency;
> using namespace std;
>
> // Be sure to specify the Pictures Folder capability in the appxmanifext file.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
>
> // Use a shared_ptr so that the string stays in memory
> // until the last task is complete.
> auto outputString = make_shared<wstring>();
> *outputString += L"Files:\n";
>
> // Get a read-only vector of the file objects
> // and pass it to the continuation.
> create_task(picturesFolder->GetFilesAsync())        
>    // outputString is captured by value, which creates a copy
>    // of the shared_ptr and increments its reference count.
>    .then ([outputString] (IVectorView\<StorageFile^>^ files)
>    {        
>        for ( unsigned int i = 0 ; i < files->Size; i++)
>        {
>            *outputString += files->GetAt(i)->Name->Data();
>            *outputString += L"\n";
>       }
>    })
>    // We need to explicitly state the return type
>    // here: -> IAsyncOperation<...>
>    .then([picturesFolder]() -> IAsyncOperation\<IVectorView\<StorageFolder^>^>^
>    {
>        return picturesFolder->GetFoldersAsync();
>    })
>    // Capture "this" to access m_OutputTextBlock from within the lambda.
>    .then([this, outputString](IVectorView\<StorageFolder^>^ folders)
>    {        
>        *outputString += L"Folders:\n";
>
>        for ( unsigned int i = 0; i < folders->Size; i++)
>        {
>           *outputString += folders->GetAt(i)->Name->Data();
>           *outputString += L"\n";
>        }
>
>        // Assume m_OutputTextBlock is a TextBlock defined in the XAML.
>        m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>     });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
>
> IReadOnlyList<StorageFile> fileList =
>     await picturesFolder.GetFilesAsync();
>
> outputText.AppendLine("Files:");
> foreach (StorageFile file in fileList)
> {
>     outputText.Append(file.Name + "\n");
> }
>
> IReadOnlyList<StorageFolder> folderList =
>     await picturesFolder.GetFoldersAsync();
>            
> outputText.AppendLine("Folders:");
> foreach (StorageFolder folder in folderList)
> {
>     outputText.Append(folder.DisplayName + "\n");
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
>
> Dim fileList As IReadOnlyList(Of StorageFile) =
>     Await picturesFolder.GetFilesAsync()
>
> outputText.AppendLine("Files:")
> For Each file As StorageFile In fileList
>
>     outputText.Append(file.Name & vbLf)
>
> Next file
>
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await picturesFolder.GetFoldersAsync()
>
> outputText.AppendLine("Folders:")
> For Each folder As StorageFolder In folderList
>
>     outputText.Append(folder.DisplayName & vbLf)
>
> Next folder
> ```


> **注意** 在 C# 或 Visual Basic 中，请记得在使用 **await** 运算符的任何方法的方法声明中放入 **async** 关键字。
 

或者，你可以使用 [**GetItemsAsync**](https://msdn.microsoft.com/library/windows/apps/br227286) 方法获取某个特定位置中的所有项（文件和子文件夹）。 以下示例使用 **GetItemsAsync** 方法获取 [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) 的根文件夹（而不是在子文件夹）中的所有文件和子文件夹。 然后，该示例会列出每个文件和子文件夹的名称。 如果该项是子文件夹，则该示例会向该名称追加 `"folder"`。

> [!div class="tabbedCodeSnippets"]
> ```cpp
> // See previous example for comments, namespace and #include info.
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
> auto outputString = make_shared<wstring>();
>
> create_task(picturesFolder->GetItemsAsync())        
>     .then ([this, outputString] (IVectorView<IStorageItem^>^ items)
> {        
>     for ( unsigned int i = 0 ; i < items->Size; i++)
>     {
>         *outputString += items->GetAt(i)->Name->Data();
>         if(items->GetAt(i)->IsOfType(StorageItemTypes::Folder))
>         {
>             *outputString += L"  folder\n";
>         }
>         else
>         {
>             *outputString += L"\n";
>         }
>         m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>     }
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
> StringBuilder outputText = new StringBuilder();
>
> IReadOnlyList<IStorageItem> itemsList =
>     await picturesFolder.GetItemsAsync();
>
> foreach (var item in itemsList)
> {
>     if (item is StorageFolder)
>     {
>         outputText.Append(item.Name + " folder\n");
>
>     }
>     else
>     {
>         outputText.Append(item.Name + "\n");
>
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
>
> Dim itemsList As IReadOnlyList(Of IStorageItem) =
>     Await picturesFolder.GetItemsAsync()
>
> For Each item In itemsList
>
>     If TypeOf item Is StorageFolder Then
>
>         outputText.Append(item.Name & " folder" & vbLf)
>
>     Else
>
>         outputText.Append(item.Name & vbLf)
>
>     End If
>
> Next item
> ```

## 查询某个位置中的文件并枚举匹配的文件

在此示例中，我们查询按月分组的 [**PicturesLibrary**](https://msdn.microsoft.com/library/windows/apps/br227156) 中的所有文件，此时该示例会递归到子文件夹。 首先，我们调用 [**StorageFolder.CreateFolderQuery**](https://msdn.microsoft.com/library/windows/apps/br227262) 并将 [**CommonFolderQuery.GroupByMonth**](https://msdn.microsoft.com/library/windows/apps/br207957) 值传递给该方法。 这向我们提供了一个 [**StorageFolderQueryResult**](https://msdn.microsoft.com/library/windows/apps/br208066) 对象。

接下来，我们调用 [**StorageFolderQueryResult.GetFoldersAsync**](https://msdn.microsoft.com/library/windows/apps/br208074)，它将返回表示虚拟文件夹的 [**StorageFolder**](https://msdn.microsoft.com/library/windows/apps/br227230) 对象。 在此示例中，我们按月分组，因此每个虚拟文件夹都表示一组具有相同月份的文件。

> [!div class="tabbedCodeSnippets"]
> ```cpp
> //#include <ppltasks.h>
> //#include <string>
> //#include <memory>
> using namespace Windows::Storage;
> using namespace Windows::Storage::Search;
> using namespace concurrency;
> using namespace Platform::Collections;
> using namespace Windows::Foundation::Collections;
> using namespace std;
>
> StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
>
> StorageFolderQueryResult^ queryResult =
>     picturesFolder->CreateFolderQuery(CommonFolderQuery::GroupByMonth);
>
> // Use shared_ptr so that outputString remains in memory
> // until the task completes, which is after the function goes out of scope.
> auto outputString = std::make_shared<wstring>();
>
> create_task( queryResult->GetFoldersAsync()).then([this, outputString] (IVectorView<StorageFolder^>^ view)
> {        
>     for ( unsigned int i = 0; i < view->Size; i++)
>     {
>         create_task(view->GetAt(i)->GetFilesAsync()).then([this, i, view, outputString](IVectorView<StorageFile^>^ files)
>         {
>             *outputString += view->GetAt(i)->Name->Data();
>             *outputString += L"(";
>             *outputString += to_wstring(files->Size);
>             *outputString += L")\r\n";
>             for (unsigned int j = 0; j < files->Size; j++)
>             {
>                 *outputString += L"     ";
>                 *outputString += files->GetAt(j)->Name->Data();
>                 *outputString += L"\r\n";
>             }
>         }).then([this, outputString]()
>         {
>             m_OutputTextBlock->Text = ref new String((*outputString).c_str());
>         });
>     }    
> });
> ```
> ```cs
> StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
>
> StorageFolderQueryResult queryResult =
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth);
>         
> IReadOnlyList<StorageFolder> folderList =
>     await queryResult.GetFoldersAsync();
>
> StringBuilder outputText = new StringBuilder();
>
> foreach (StorageFolder folder in folderList)
> {
>     IReadOnlyList<StorageFile> fileList = await folder.GetFilesAsync();
>
>     // Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name + " (" + fileList.Count + ")");
>
>     foreach (StorageFile file in fileList)
>     {
>         // Print the name of the file.
>         outputText.AppendLine("   " + file.Name);
>     }
> }
> ```
> ```vb
> Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
> Dim outputText As New StringBuilder
>
> Dim queryResult As StorageFolderQueryResult =
>     picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth)
>
> Dim folderList As IReadOnlyList(Of StorageFolder) =
>     Await queryResult.GetFoldersAsync()
>
> For Each folder As StorageFolder In folderList
>
>     Dim fileList As IReadOnlyList(Of StorageFile) =
>         Await folder.GetFilesAsync()
>
>     ' Print the month and number of files in this group.
>     outputText.AppendLine(folder.Name & " (" & fileList.Count & ")")
>
>     For Each file As StorageFile In fileList
>
>         ' Print the name of the file.
>         outputText.AppendLine("   " & file.Name)
>
>     Next file
>
> Next folder
> ```

该示例的输出与以下内容类似。

``` syntax
July ‎2015 (2)
   MyImage3.png
   MyImage4.png
‎December ‎2014 (2)
   MyImage1.png
   MyImage2.png
```



<!--HONumber=Aug16_HO3-->


