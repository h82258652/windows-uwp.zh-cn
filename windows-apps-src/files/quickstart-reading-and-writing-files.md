---
ms.assetid: 27914C0A-2A02-473F-BDD5-C931E3943AA0
title: 创建、写入和读取文件
description: 使用 StorageFile 对象读取和写入文件。
---

# 创建、写入和读取文件


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**重要的 API**

-   [**StorageFolder 类**](https://msdn.microsoft.com/library/windows/apps/br227230)
-   [**StorageFile 类**](https://msdn.microsoft.com/library/windows/apps/br227171)
-   [**FileIO 类**](https://msdn.microsoft.com/library/windows/apps/hh701440)

使用 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 对象读取和写入文件。

> **注意** 另请参阅[文件访问示例](http://go.microsoft.com/fwlink/p/?linkid=619995)。

## 先决条件

-   **了解通用 Windows 平台 (UWP) 应用的异步编程**

    若要了解如何使用 C# 或 Visual Basic 编写异步应用，请参阅[使用 C# 或 Visual Basic 调用异步 API](https://msdn.microsoft.com/library/windows/apps/mt187337)。 若要了解如何使用 C++ 编写异步应用，请参阅[使用 C++ 进行异步编程](https://msdn.microsoft.com/library/windows/apps/mt187334)。

-   **了解如何获取你希望读取和/或写入的文件**

    可以在[使用选取器打开文件和文件夹](quickstart-using-file-and-folder-pickers.md)中了解如何使用文件选取器获取文件。

## 创建文件

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
```vb
' Create sample file; replace if exists.
Dim storageFolder As StorageFolder = Windows.Storage.ApplicationData.Current.LocalFolder
Dim sampleFile As StorageFile = Await storageFolder.CreateFileAsync("sample.txt", CreationCollisionOption.ReplaceExisting)
```

## 写入文件


下面介绍了如何使用 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 类在磁盘上写入可写文件。 每种写入文件（除非你在创建后立即写入文件）的方法的常见第一步是使用 [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 获取文件。
> [!div class="tabbedCodeSnippets"]
```cs
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
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
```vb
Await Windows.Storage.FileIO.WriteTextAsync(sampleFile, "Swift as a shadow")
```

**使用缓冲区将字节写入文件（2 个步骤）**

1.  首先，调用 [**ConvertStringToBinary**](https://msdn.microsoft.com/library/windows/apps/br241385) 以获取你想要写入文件的字节（基于随机字符串）的缓冲区。
> [!div class="tabbedCodeSnippets"]
```cs
var buffer = Windows.Security.Cryptography.CryptographicBuffer.ConvertStringToBinary(
        "What fools these mortals be", Windows.Security.Cryptography.BinaryStringEncoding.Utf8);
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
```vb
Await Windows.Storage.FileIO.WriteBufferAsync(sampleFile, buffer)
```

**使用流将文本写入文件（4 个步骤）**

1.  首先，调用 [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) 方法打开文件。 打开操作完成后，它将返回文件的内容流。
> [!div class="tabbedCodeSnippets"]
```cs
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```
```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
```

2.  下一步，通过从 `stream` 调用 [**GetOutputStreamAt**](https://msdn.microsoft.com/library/windows/apps/br241738) 方法获取输出流。 将其放到 **using** 语句中以管理输出流的生存期。
> [!div class="tabbedCodeSnippets"]
```cs
using (var outputStream = stream.GetOutputStreamAt(0))
    {
        // We'll add more code here in the next step.
    }
    stream.Dispose(); // Or use the stream variable (see previous code snippet) with a using statement as well.
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
```vb
    Await dataWriter.StoreAsync()
        Await outputStream.FlushAsync()
```

## 从文件读取


下面介绍了如何使用 [**StorageFile**](https://msdn.microsoft.com/library/windows/apps/br227171) 类在磁盘上从文件进行读取。 每种从文件进行读取的方法的常见第一步是使用 [**StorageFolder.GetFileAsync**](https://msdn.microsoft.com/library/windows/apps/br227272) 获取文件。
> [!div class="tabbedCodeSnippets"]
```cs
Windows.Storage.StorageFolder storageFolder =
    Windows.Storage.ApplicationData.Current.LocalFolder;
Windows.Storage.StorageFile sampleFile =
    await storageFolder.GetFileAsync("sample.txt");
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
```vb
Dim text As String = Await Windows.Storage.FileIO.ReadTextAsync(sampleFile)
```

**通过使用缓冲区从文件读取字节（2 个步骤）**

1.  首先，通过调用 [**FileIO**](https://msdn.microsoft.com/library/windows/apps/hh701440) 类的 [**ReadBufferAsync**](https://msdn.microsoft.com/library/windows/apps/hh701468) 方法从文件的缓冲区读取字节。
> [!div class="tabbedCodeSnippets"]
```cs
var buffer = await Windows.Storage.FileIO.ReadBufferAsync(sampleFile);
```
```vb
Dim buffer = Await Windows.Storage.FileIO.ReadBufferAsync(sampleFile)
```

2.  然后，使用 [**DataReader**](https://msdn.microsoft.com/library/windows/apps/br208119) 对象首先读取缓冲区的长度，然后读取其内容。
> [!div class="tabbedCodeSnippets"]
```cs
using (var dataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer))
    {
        string text = dataReader.ReadString(buffer.Length);
    }
```
```vb
Dim dataReader As DataReader = Windows.Storage.Streams.DataReader.FromBuffer(buffer)
    Dim text As String = dataReader.ReadString(buffer.Length)
```

**使用流从文件读取文本（4 个步骤）**

1.  通过调用 [**StorageFile.OpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn889851) 方法，为你的文件打开流。 操作完成后，它将返回文件的内容流。
> [!div class="tabbedCodeSnippets"]
```cs
var stream = await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite);
```
```vb
Dim stream = Await sampleFile.OpenAsync(Windows.Storage.FileAccessMode.ReadWrite)
```

2.  获取稍后要使用的流的大小。
> [!div class="tabbedCodeSnippets"]
```cs
ulong size = stream.Size;
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
```vb
Dim dataReader As New DataReader(inputStream)
    Dim numBytesLoaded As UInteger = Await dataReader.LoadAsync(CUInt(size))
    Dim text As String = dataReader.ReadString(numBytesLoaded)
```

 

 






<!--HONumber=Mar16_HO1-->


