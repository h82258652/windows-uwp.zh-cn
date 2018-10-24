---
author: laurenhughes
ms.assetid: 4C59D5AC-58F7-4863-A884-E9E54228A5AD
title: 枚举和查询文件和文件夹
description: 访问位于文件夹、库、设备或网络位置的文件和文件夹。 还可以通过构造文件和文件夹查询来查询某个位置的文件和文件夹。
ms.author: lahugh
ms.date: 06/28/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
- vb
ms.openlocfilehash: 312e351a39bf291e1fcd21921230a73ed10cfd17
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2018
ms.locfileid: "5435500"
---
# <a name="enumerate-and-query-files-and-folders"></a>枚举和查询文件和文件夹

访问位于文件夹、库、设备或网络位置的文件和文件夹。 还可以通过构造文件和文件夹查询来查询某个位置的文件和文件夹。

有关如何存储通用 Windows 平台应用数据的指南，请参阅 [ApplicationData](/uwp/api/windows.storage.applicationdata) 类。

> [!NOTE]
> 另请参阅[文件夹枚举示例](http://go.microsoft.com/fwlink/p/?linkid=619993)。

## <a name="prerequisites"></a>先决条件

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C# 或 Visual Basic 编写异步应用，请参阅[使用 C# 或 Visual Basic 调用异步 API](/windows/uwp/threading-async/call-asynchronous-apis-in-csharp-or-visual-basic)。 若要了解如何编写异步应用 C + + /winrt 中，请参阅[并发和异步操作通过 C + + WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency)。 若要了解如何编写异步应用 C + + CX，请参阅[进行异步编程 C + + CX](/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)。

-   **对位置的访问权限**

    例如，这些示例中的代码需要 **picturesLibrary** 功能，但是你的位置可能需要其他功能或根本不需要任何功能。 若要了解详细信息，请参阅[文件访问权限](file-access-permissions.md)。

## <a name="enumerate-files-and-folders-in-a-location"></a>枚举某个位置中的文件和文件夹

> [!NOTE]
> 请记住声明 **picturesLibrary** 功能。

在此示例中我们首先使用[**StorageFolder.GetFilesAsync**](/uwp/api/windows.storage.storagefolder.getfilesasync)方法获取[**KnownFolders.PicturesLibrary**](/uwp/api/windows.storage.knownfolders.pictureslibrary)的根文件夹中的所有文件 （不在子文件夹） 并列出每个文件的名称。 接下来，我们使用[**StorageFolder.GetFoldersAsync**](/uwp/api/windows.storage.storagefolder.getfoldersasync)方法**PicturesLibrary**中获取所有子文件夹并列出每个子文件夹的名称。

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
StringBuilder outputText = new StringBuilder();

IReadOnlyList<StorageFile> fileList = await picturesFolder.GetFilesAsync();

outputText.AppendLine("Files:");
foreach (StorageFile file in fileList)
{
    outputText.Append(file.Name + "\n");
}

IReadOnlyList<StorageFolder> folderList = await picturesFolder.GetFoldersAsync();
           
outputText.AppendLine("Folders:");
foreach (StorageFolder folder in folderList)
{
    outputText.Append(folder.DisplayName + "\n");
}
```

```cppwinrt
// MainPage.h
// In MainPage.xaml: <TextBlock x:Name="OutputTextBlock"/>
#include <winrt/Windows.Storage.h>
#include <sstream>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    // Be sure to specify the Pictures Folder capability in your Package.appxmanifest.
    Windows::Storage::StorageFolder picturesFolder{
        Windows::Storage::KnownFolders::PicturesLibrary()
    };

    std::wstringstream outputString;
    outputString << L"Files:" << std::endl;

    for (auto const& file : co_await picturesFolder.GetFilesAsync())
    {
        outputString << file.Name().c_str() << std::endl;
    }

    outputString << L"Folders:" << std::endl;
    for (auto const& folder : co_await picturesFolder.GetFoldersAsync())
    {
        outputString << folder.Name().c_str() << std::endl;
    }

    OutputTextBlock().Text(outputString.str().c_str());
}
```

```cpp
#include <ppltasks.h>
#include <string>
#include <memory>

using namespace Windows::Storage;
using namespace Platform::Collections;
using namespace concurrency;
using namespace std;

// Be sure to specify the Pictures Folder capability in the appxmanifext file.
StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;

// Use a shared_ptr so that the string stays in memory
// until the last task is complete.
auto outputString = make_shared<wstring>();
*outputString += L"Files:\n";

// Get a read-only vector of the file objects
// and pass it to the continuation.
create_task(picturesFolder->GetFilesAsync())        
   // outputString is captured by value, which creates a copy
   // of the shared_ptr and increments its reference count.
   .then ([outputString] (IVectorView\<StorageFile^>^ files)
   {        
       for ( unsigned int i = 0 ; i < files->Size; i++)
       {
           *outputString += files->GetAt(i)->Name->Data();
           *outputString += L"\n";
      }
   })
   // We need to explicitly state the return type
   // here: -IAsyncOperation<...>
   .then([picturesFolder]() -IAsyncOperation\<IVectorView\<StorageFolder^>^>^
   {
       return picturesFolder->GetFoldersAsync();
   })
   // Capture "this" to access m_OutputTextBlock from within the lambda.
   .then([this, outputString](IVectorView/<StorageFolder^>^ folders)
   {        
       *outputString += L"Folders:\n";

       for ( unsigned int i = 0; i < folders->Size; i++)
       {
          *outputString += folders->GetAt(i)->Name->Data();
          *outputString += L"\n";
       }

       // Assume m_OutputTextBlock is a TextBlock defined in the XAML.
       m_OutputTextBlock->Text = ref new String((*outputString).c_str());
    });
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim outputText As New StringBuilder

Dim fileList As IReadOnlyList(Of StorageFile) =
    Await picturesFolder.GetFilesAsync()

outputText.AppendLine("Files:")
For Each file As StorageFile In fileList

    outputText.Append(file.Name & vbLf)

Next file

Dim folderList As IReadOnlyList(Of StorageFolder) =
    Await picturesFolder.GetFoldersAsync()

outputText.AppendLine("Folders:")
For Each folder As StorageFolder In folderList

    outputText.Append(folder.DisplayName & vbLf)

Next folder
```

> [!NOTE]
> 在 C# 或 Visual Basic 中，请记得在使用 **await** 运算符的任何方法的方法声明中放入 **async** 关键字。

或者，你可以使用[**StorageFolder.GetItemsAsync**](/uwp/api/windows.storage.storagefolder.getitemsasync)方法获取某个特定位置中的所有项 （文件和子文件夹）。 下面的示例使用**GetItemsAsync**方法 （不在子文件夹） 中获取的所有文件和[**KnownFolders.PicturesLibrary**](/uwp/api/windows.storage.knownfolders.pictureslibrary)的根文件夹中的子文件夹。 然后，该示例会列出每个文件和子文件夹的名称。 如果该项是子文件夹，则该示例会向该名称追加 `"folder"`。

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;
StringBuilder outputText = new StringBuilder();

IReadOnlyList<IStorageItem> itemsList = await picturesFolder.GetItemsAsync();

foreach (var item in itemsList)
{
    if (item is StorageFolder)
    {
        outputText.Append(item.Name + " folder\n");

    }
    else
    {
        outputText.Append(item.Name + "\n");
    }
}
```

```cppwinrt
// MainPage.h
// In MainPage.xaml: <TextBlock x:Name="OutputTextBlock"/>
#include <winrt/Windows.Storage.h>
#include <sstream>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    // Be sure to specify the Pictures Folder capability in your Package.appxmanifest.
    Windows::Storage::StorageFolder picturesFolder{
        Windows::Storage::KnownFolders::PicturesLibrary()
    };

    std::wstringstream outputString;

    for (Windows::Storage::IStorageItem const& item : co_await picturesFolder.GetItemsAsync())
    {
        outputString << item.Name().c_str();

        if (item.IsOfType(Windows::Storage::StorageItemTypes::Folder))
        {
            outputString << L" folder" << std::endl;
        }
        else
        {
            outputString << std::endl;
        }

        OutputTextBlock().Text(outputString.str().c_str());
    }
}
```

```cpp
// See previous example for comments, namespace and #include info.
StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;
auto outputString = make_shared<wstring>();

create_task(picturesFolder->GetItemsAsync())        
    .then ([this, outputString] (IVectorView<IStorageItem^>^ items)
{        
    for ( unsigned int i = 0 ; i < items->Size; i++)
    {
        *outputString += items->GetAt(i)->Name->Data();
        if(items->GetAt(i)->IsOfType(StorageItemTypes::Folder))
        {
            *outputString += L"  folder\n";
        }
        else
        {
            *outputString += L"\n";
        }
        m_OutputTextBlock->Text = ref new String((*outputString).c_str());
    }
});
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim outputText As New StringBuilder

Dim itemsList As IReadOnlyList(Of IStorageItem) =
    Await picturesFolder.GetItemsAsync()

For Each item In itemsList

    If TypeOf item Is StorageFolder Then

        outputText.Append(item.Name & " folder" & vbLf)

    Else

        outputText.Append(item.Name & vbLf)

    End If

Next item
```

## <a name="query-files-in-a-location-and-enumerate-matching-files"></a>查询某个位置中的文件并枚举匹配的文件

在此示例中我们查询按月和此次分组[**KnownFolders.PicturesLibrary**](/uwp/api/windows.storage.knownfolders.pictureslibrary)中的所有文件的示例会递归到子文件夹。 首先，我们调用 [**StorageFolder.CreateFolderQuery**](/uwp/api/windows.storage.storagefolder.createfolderquery) 并将 [**CommonFolderQuery.GroupByMonth**](/uwp/api/windows.storage.search.commonfolderquery) 值传递给该方法。 这向我们提供了一个 [**StorageFolderQueryResult**](/uwp/api/windows.storage.search.storagefolderqueryresult) 对象。

接下来，我们调用 [**StorageFolderQueryResult.GetFoldersAsync**](/uwp/api/windows.storage.search.storagefolderqueryresult.getfoldersasync)，它将返回表示虚拟文件夹的 [**StorageFolder**](/uwp/api/windows.storage.storagefolder) 对象。 在此示例中，我们按月分组，因此每个虚拟文件夹都表示一组具有相同月份的文件。

```csharp
StorageFolder picturesFolder = KnownFolders.PicturesLibrary;

StorageFolderQueryResult queryResult =
    picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth);
        
IReadOnlyList<StorageFolder> folderList =
    await queryResult.GetFoldersAsync();

StringBuilder outputText = new StringBuilder();

foreach (StorageFolder folder in folderList)
{
    IReadOnlyList<StorageFile> fileList = await folder.GetFilesAsync();

    // Print the month and number of files in this group.
    outputText.AppendLine(folder.Name + " (" + fileList.Count + ")");

    foreach (StorageFile file in fileList)
    {
        // Print the name of the file.
        outputText.AppendLine("   " + file.Name);
    }
}
```

```cppwinrt
// MainPage.h
// In MainPage.xaml: <TextBlock x:Name="OutputTextBlock"/>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Search.h>
#include <sstream>
...
Windows::Foundation::IAsyncAction ExampleCoroutineAsync()
{
    // Be sure to specify the Pictures Folder capability in your Package.appxmanifest.
    Windows::Storage::StorageFolder picturesFolder{
        Windows::Storage::KnownFolders::PicturesLibrary()
    };

    Windows::Storage::Search::StorageFolderQueryResult queryResult{
        picturesFolder.CreateFolderQuery(Windows::Storage::Search::CommonFolderQuery::GroupByMonth)
    };

    std::wstringstream outputString;

    for (Windows::Storage::StorageFolder const& folder : co_await queryResult.GetFoldersAsync())
    {
        auto files{ co_await folder.GetFilesAsync() };
        outputString << folder.Name().c_str() << L" (" << files.Size() << L")" << std::endl;

        for (Windows::Storage::StorageFile const& file : files)
        {
            outputString << L"    " << file.Name().c_str() << std::endl;
        }
    }

    OutputTextBlock().Text(outputString.str().c_str());
}
```

```cpp
#include <ppltasks.h>
#include <string>
#include <memory>
using namespace Windows::Storage;
using namespace Windows::Storage::Search;
using namespace concurrency;
using namespace Platform::Collections;
using namespace Windows::Foundation::Collections;
using namespace std;

StorageFolder^ picturesFolder = KnownFolders::PicturesLibrary;

StorageFolderQueryResult^ queryResult =
    picturesFolder->CreateFolderQuery(CommonFolderQuery::GroupByMonth);

// Use shared_ptr so that outputString remains in memory
// until the task completes, which is after the function goes out of scope.
auto outputString = std::make_shared<wstring>();

create_task( queryResult->GetFoldersAsync()).then([this, outputString] (IVectorView<StorageFolder^>^ view)
{        
    for ( unsigned int i = 0; i < view->Size; i++)
    {
        create_task(view->GetAt(i)->GetFilesAsync()).then([this, i, view, outputString](IVectorView<StorageFile^>^ files)
        {
            *outputString += view->GetAt(i)->Name->Data();
            *outputString += L" (";
            *outputString += to_wstring(files->Size);
            *outputString += L")\r\n";
            for (unsigned int j = 0; j < files->Size; j++)
            {
                *outputString += L"     ";
                *outputString += files->GetAt(j)->Name->Data();
                *outputString += L"\r\n";
            }
        }).then([this, outputString]()
        {
            m_OutputTextBlock->Text = ref new String((*outputString).c_str());
        });
    }    
});
```

```vb
Dim picturesFolder As StorageFolder = KnownFolders.PicturesLibrary
Dim outputText As New StringBuilder

Dim queryResult As StorageFolderQueryResult =
    picturesFolder.CreateFolderQuery(CommonFolderQuery.GroupByMonth)

Dim folderList As IReadOnlyList(Of StorageFolder) =
    Await queryResult.GetFoldersAsync()

For Each folder As StorageFolder In folderList

    Dim fileList As IReadOnlyList(Of StorageFile) =
        Await folder.GetFilesAsync()

    ' Print the month and number of files in this group.
    outputText.AppendLine(folder.Name & " (" & fileList.Count & ")")

    For Each file As StorageFile In fileList

        ' Print the name of the file.
        outputText.AppendLine("   " & file.Name)

    Next file

Next folder
```

该示例的输出与以下内容类似。

```syntax
July ‎2015 (2)
   MyImage3.png
   MyImage4.png
‎December ‎2014 (2)
   MyImage1.png
   MyImage2.png
```
