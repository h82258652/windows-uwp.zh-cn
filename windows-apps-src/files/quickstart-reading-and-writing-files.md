---
author: laurenhughes
ms.assetid: 27914C0A-2A02-473F-BDD5-C931E3943AA0
title: "创建、写入和读取文件"
description: "使用 StorageFile 对象读取和写入文件。"
ms.author: lahugh
ms.date: 07/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: d566bac72b9e3a7e00adb9225342017168ca5f43
ms.sourcegitcommit: 90fbdc0e25e0dff40c571d6687143dd7e16ab8a8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/06/2017
---
# <a name="create-write-and-read-a-file"></a>创建、写入和读取文件


\[ 已针对 Windows10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**StorageFolder 类**](https://msdn.microsoft.com/library/windows/apps/br227230)
-   [**StorageFile 类**](https://msdn.microsoft.com/library/windows/apps/br227171)
-   [**FileIO 类**](https://msdn.microsoft.com/library/windows/apps/hh701440)

使用 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象读取和写入文件。

> [!NOTE] 
> 另请参阅[文件访问示例](http://go.microsoft.com/fwlink/p/?linkid=619995)。

## <a name="prerequisites"></a>先决条件

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C# 或 Visual Basic 编写异步应用，请参阅[使用 C# 或 Visual Basic 调用异步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **了解如何获取你希望读取和/或写入的文件**

    可以在[使用选取器打开文件和文件夹](quickstart-using-file-and-folder-pickers.md)中了解如何使用文件选取器获取文件。

## <a name="creating-a-file"></a>创建文件

这里介绍了如何在应用的本地文件夹中创建一个文件。 如果文件存在，我们将替换它。

> [!div class="tabbedCodeSnippets"]
```cs  
// Create sample file; replace if exists.
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.CreateFileAsync("sample.txt",
        Windows.Storage.CreationCollisionOption.ReplaceExisting);
```
```cpp  
// Create a sample file; replace if exists.
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
concurrency::create_task(storageFolder->CreateFileAsync("sample.txt", CreationCollisionOption::ReplaceExisting));
```
```vb  
' Create sample file; replace if exists.
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.CreateFileAsync("sample.txt", CreationCollisionOption.ReplaceExisting)
```

## <a name="writing-to-a-file"></a>写入文件

下面介绍了如何使用 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 类在磁盘上写入可写文件。 每种写入文件（除非你在创建后立即写入文件）的方法的常见第一步是使用 [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 获取文件。

> [!div class="tabbedCodeSnippets"]
```cs  
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```cpp  
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile) 
{
    // Process file
});
```
```vb  
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**将文本写入文件**

通过调用 [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440) 类的 [**WriteTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701505) 方法，将文本写入文件。

> [!div class="tabbedCodeSnippets"]
```cs  
await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow");
```
```cpp 
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile) 
{
    //Write text to a file
    create_task(FileIO::WriteTextAsync(sampleFile, "Swift as a shadow"));
});
```
```vb  
Await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow")
```

**使用缓冲区将字节写入文件（2 步）**

1.  首先，调用 [**ConvertStringToBinary**](https://msdn.microsoft.com/library/windows/apps/br241385) 以获取你想要写入文件的字节（基于随机字符串）的缓冲区。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
            "What fools these mortals be", Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
    ```
    ```cpp  
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        // Create the buffer
        IBuffer^ buffer = CryptographicBuffer::ConvertStringToBinary
        ("What fools these mortals be", BinaryStringEncoding::Utf8);
    });
    ```
    ```vb  
    Dim buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
                        "What fools these mortals be",
                        Windows.Security.Cryptography.BinaryStringEncoding.Utf8)
    ```

2.  然后，通过调用 [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440) 类的 [**WriteBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701490) 方法，将字节从你的缓冲区写入文件。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer);
    ```
    ```cpp  
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        // Create the buffer
        IBuffer^ buffer = CryptographicBuffer::ConvertStringToBinary
        ("What fools these mortals be", BinaryStringEncoding::Utf8);      
        // Write bytes to a file using a buffer
        create_task(FileIO::WriteBufferAsync(sampleFile, buffer));
    });
    ```
    ```vb  
    Await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer)
    ```

**使用流将文本写入文件（4 步）**

1.  首先，调用 [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) 方法打开文件。 打开操作完成后，它将返回文件的内容流。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
    ```
    ```cpp  
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        create_task(sampleFile->OpenAsync(FileAccessMode::ReadWrite)).then([sampleFile](IRandomAccessStream^ stream)
        {
            // Process stream
        });
    });
    ```
    ```vb  
    Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
    ```

2.  接下来，通过从 `stream` 调用 [**GetOutputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241738) 方法获取输出流。 将其放到 **using** 语句中以管理输出流的生存期。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var outputStream = stream.GetOutputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    stream.Dispose(); // Or use the stream variable (see previous code snippet) with a using statement as well.
    ```
    ```cpp 
    // Add to "Process stream" in part 1
    IOutputStream^ outputStream = stream->GetOutputStreamAt(0);
    ```
    ```vb 
    Using outputStream = stream.GetOutputStreamAt(0)
    ' We'll add more code here in the next step.
    End Using
    ```

3.  现在，将此代码添加到现有的 **using** 语句中，以通过创建一个新的 [**DataWriter**](https://msdn.microsoft.com/library/windows/apps/br208154) 对象和调用 [**DataWriter.WriteString**](https://msdn.microsoft.com/library/windows/apps/br241642) 方法写入输出流。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var dataWriter = new Windows.Storage.Streams.DataWriter(outputStream))
    {
        dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
    }
    ```
    ```cpp  
    // Added after code from part 2
    DataWriter^ dataWriter = ref new DataWriter(outputStream);
    dataWriter->WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.");
    ```
    ```vb  
    Dim dataWriter As New DataWriter(outputStream)
    dataWriter.WriteString("DataWriter has methods to write to various types, such as DataTimeOffset.")
    ```

4.  最后，添加此代码（在内部 **using** 语句中）以使用 [**StoreAsync**](https://msdn.microsoft.com/library/windows/apps/br208171) 将文本保存到你的文件并使用 [**FlushAsync**](https://msdn.microsoft.com/library/windows/apps/br241729) 关闭该流。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    await dataWriter.StoreAsync();
        await outputStream.FlushAsync();
    ```
    ```cpp   
    // Added after code from part 3
    dataWriter->StoreAsync();
    outputStream->FlushAsync();
    ```
    ```vb  
    Await dataWriter.StoreAsync()
        Await outputStream.FlushAsync()
    ```

## <a name="reading-from-a-file"></a>从文件读取

下面介绍了如何使用 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 类在磁盘上从文件进行读取。 每种从文件读取的方法的常见第一步是使用 [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 获取文件。

> [!div class="tabbedCodeSnippets"]
```cs  
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
```
```cpp  
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    // Process file
});
```
```vb  
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.GetFileAsync("sample.txt")
```

**从文件读取文本**

通过调用 [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440) 类的 [**ReadTextAsync**](https://msdn.microsoft.com/library/windows/apps/hh701482) 方法，从文件读取文本。

> [!div class="tabbedCodeSnippets"]
```cs  
string text = await Windows.Storage.FileIO.ReadTextAsync(sampleFile);
```
```cpp  
StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
{
    return FileIO::ReadTextAsync(sampleFile);
});
```
```vb  
Dim text As String = Await Windows.Storage.FileIO.ReadTextAsync(sampleFile)
```

**通过使用缓冲区从文件读取文本（2 步）**

1.  首先，调用 [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440) 类的 [**ReadBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701468) 方法。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var buffer = await Windows.Storage.FileIO.ReadBufferAsync(sampleFile);
    ```

    ```cpp  
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        return FileIO::ReadBufferAsync(sampleFile);

    }).then([](Streams::IBuffer^ buffer)
    {
        // Process buffer
    });
    ```

    ```vb  
    Dim buffer = Await Windows.Storage.FileIO.ReadBufferAsync(sampleFile)
    ```

2.  然后，使用 [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) 对象先读取缓冲区的长度，然后读取其内容。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var dataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer))
    {
        string text = dataReader.ReadString(buffer.Length);
    }
    ```
    ```cpp  
    // Add to "Process buffer" section from part 1
    auto dataReader = DataReader::FromBuffer(buffer);
    String^ bufferText = dataReader->ReadString(buffer->Length);
    ```
    ```vb  
    Dim dataReader As DataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer)
    Dim text As String = dataReader.ReadString(buffer.Length)
    ```

**使用流从文件读取文本（4 步）**

1.  通过调用 [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) 方法，为你的文件打开流。 操作完成后，它将返回文件的内容流。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.Read);
    ```
    ```cpp  
    StorageFolder^ storageFolder = ApplicationData::Current->LocalFolder;
    create_task(storageFolder->GetFileAsync("sample.txt")).then([](StorageFile^ sampleFile)
    {
        create_task(sampleFile->OpenAsync(FileAccessMode::Read)).then([sampleFile](IRandomAccessStream^ stream)
        {
            // Process stream
        });
    });
    ```
    ```vb  
    Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.Read)
    ```

2.  获取稍后要使用的流的大小。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    ulong size = stream.Size;
    ```
    ```cpp  
    // Add to "Process stream" from part 1
    UINT64 size = stream->Size;
    ```
    ```vb  
    Dim size = stream.Size
    ```

3.  通过调用 [**GetInputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241737) 方法获取输入流。 将其放到 **using** 语句中以管理流的生存期。 调用 **GetInputStreamAt** 时指定 0，以将位置设置在流的开头。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var inputStream = stream.GetInputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    ```
    ```cpp  
    // Add after code from part 2
    IInputStream^ inputStream = stream->GetInputStreamAt(0);
    auto dataReader = ref new DataReader(inputStream);
    ```
    ```vb  
    Using inputStream = stream.GetInputStreamAt(0)
        ' We'll add more code here in the next step.
    End Using
    ```

4.  最后，将此代码添加到现有的 **using** 语句中以在流上获取 [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) 对象，然后通过调用 [**DataReader.LoadAsync**](https://msdn.microsoft.com/library/windows/apps/br208135) 和 [**DataReader.ReadString**](https://msdn.microsoft.com/library/windows/apps/br208147) 读取文本。

    > [!div class="tabbedCodeSnippets"]
    ```cs  
    using (var dataReader = new Windows.Storage.Streams.DataReader(inputStream))
    {
        uint numBytesLoaded = await dataReader.LoadAsync((uint)size);
        string text = dataReader.ReadString(numBytesLoaded);
    }
    ```
    ```cpp 
    // Add after code from part 3
    create_task(dataReader->LoadAsync(size)).then([sampleFile, dataReader](unsigned int numBytesLoaded)
    {
        String^ streamText = dataReader->ReadString(numBytesLoaded);
    });
    ```
    ```vb  
    Dim dataReader As New DataReader(inputStream)
    Dim numBytesLoaded As UInteger = Await dataReader.LoadAsync(CUInt(size))
    Dim text As String = dataReader.ReadString(numBytesLoaded)
    ```

 

 
