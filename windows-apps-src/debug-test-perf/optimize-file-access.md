---
ms.assetid: 40122343-1FE3-4160-BABE-6A2DD9AF1E8E
title: 优化文件访问
description: 创建可高效访问文件系统的通用 Windows 平台 (UWP) 应用，避免因磁盘延迟和内存/CPU 周期而产生的性能问题。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3114bc7a86f7f7f4d22c69c814735c146352efbd
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/29/2020
ms.locfileid: "75681948"
---
# <a name="optimize-file-access"></a>优化文件访问


创建可高效访问文件系统的通用 Windows 平台 (UWP) 应用，避免因磁盘延迟和内存/CPU 周期而产生的性能问题。

如果你希望访问较大的文件集合，并且你希望访问除典型的 Name、FileType 和 Path 属性之外的属性值，可通过创建 [**QueryOptions**](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) 和调用 [**SetPropertyPrefetch**](https://docs.microsoft.com/uwp/api/windows.storage.search.queryoptions.setpropertyprefetch) 来访问它们。 **SetPropertyPrefetch** 方法可显著改善应用的性能，这些应用可显示从文件系统获取的项目集合，如图像集合。 下一组示例介绍了一些访问多个文件的方法。

第一个示例使用 [**Windows.Storage.StorageFolder.GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync) 来检索一组文件的名称信息。 这个方法具有很好的性能，因为该示例仅访问名称属性。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> 
> for (int i = 0; i < files.Count; i++)
> {
>     // do something with the name of each file
>     string fileName = files[i].Name;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) =
>     Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> 
> For i As Integer = 0 To files.Count - 1
>     ' do something with the name of each file
>     Dim fileName As String = files(i).Name
> Next i
> ```

第二个示例使用 [**Windows.Storage.StorageFolder.GetFilesAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefolder.getfilesasync)，然后检索每个文件的图像属性。 这个方法的性能逊色不少。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFolder library = Windows.Storage.KnownFolders.PicturesLibrary;
> IReadOnlyList<StorageFile> files = await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate);
> for (int i = 0; i < files.Count; i++)
> {
>     ImageProperties imgProps = await files[i].Properties.GetImagePropertiesAsync();
> 
>     // do something with the date the image was taken
>     DateTimeOffset date = imgProps.DateTaken;
> }
> ```
> ```vb
> Dim library As StorageFolder = Windows.Storage.KnownFolders.PicturesLibrary
> Dim files As IReadOnlyList(Of StorageFile) = Await library.GetFilesAsync(Windows.Storage.Search.CommonFileQuery.OrderByDate)
> For i As Integer = 0 To files.Count - 1
>     Dim imgProps As ImageProperties =
>         Await files(i).Properties.GetImagePropertiesAsync()
> 
>     ' do something with the date the image was taken
>     Dim dateTaken As DateTimeOffset = imgProps.DateTaken
> Next i
> ```

第三个示例使用 [**QueryOptions**](https://docs.microsoft.com/uwp/api/Windows.Storage.Search.QueryOptions) 来获取关于一组文件的信息。 此方法提供的性能优于前面的示例。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> // Set QueryOptions to prefetch our specific properties
> var queryOptions = new Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, null);
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView, 100,
>         ThumbnailOptions.ReturnOnlyIfCached);
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties, 
>        new string[] {"System.Size"});
> 
> StorageFileQueryResult queryResults = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions);
> IReadOnlyList<StorageFile> files = await queryResults.GetFilesAsync();
> 
> foreach (var file in files)
> {
>     ImageProperties imageProperties = await file.Properties.GetImagePropertiesAsync();
> 
>     // Do something with the date the image was taken.
>     DateTimeOffset dateTaken = imageProperties.DateTaken;
> 
>     // Performance gains increase with the number of properties that are accessed.
>     IDictionary<String, object> propertyResults =
>         await file.Properties.RetrievePropertiesAsync(
>               new string[] {"System.Size" });
> 
>     // Get/Set extra properties here
>     var systemSize = propertyResults["System.Size"];
> }
> ```
> ```vb
> ' Set QueryOptions to prefetch our specific properties
> Dim queryOptions = New Windows.Storage.Search.QueryOptions(CommonFileQuery.OrderByDate, Nothing)
> queryOptions.SetThumbnailPrefetch(ThumbnailMode.PicturesView,
>             100, Windows.Storage.FileProperties.ThumbnailOptions.ReturnOnlyIfCached)
> queryOptions.SetPropertyPrefetch(PropertyPrefetchOptions.ImageProperties,
>                                  New String() {"System.Size"})
> 
> Dim queryResults As StorageFileQueryResult = KnownFolders.PicturesLibrary.CreateFileQueryWithOptions(queryOptions)
> Dim files As IReadOnlyList(Of StorageFile) = Await queryResults.GetFilesAsync()
> 
> 
> For Each file In files
>     Dim imageProperties As ImageProperties = Await file.Properties.GetImagePropertiesAsync()
> 
>     ' Do something with the date the image was taken.
>     Dim dateTaken As DateTimeOffset = imageProperties.DateTaken
> 
>     ' Performance gains increase with the number of properties that are accessed.
>     Dim propertyResults As IDictionary(Of String, Object) =
>         Await file.Properties.RetrievePropertiesAsync(New String() {"System.Size"})
> 
>     ' Get/Set extra properties here
>     Dim systemSize = propertyResults("System.Size")
> 
> Next file
> ```
如果要对 Windows.Storage 对象（如 `Windows.Storage.ApplicationData.Current.LocalFolder`）执行多个操作，请创建一个本地变量以引用该存储源，这样你每次访问它时就不必重新创建中间对象。

## <a name="stream-performance-in-c-and-visual-basic"></a>C# 和 Visual Basic 中的数据流性能

### <a name="buffering-between-uwp-and-net-streams"></a>UWP 与 .NET 数据流之间的缓冲

当你希望将 UWP 数据流（例如 [**Windows.Storage.Streams.IInputStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IInputStream) 或 [**IOutputStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IOutputStream)）转换为 .NET 数据流 ([**System.IO.Stream**](https://docs.microsoft.com/dotnet/api/system.io.stream)) 时，可以使用多种方案。 例如，在编写 UWP 应用并希望将在数据流上可用的现有 .NET 代码用于 UWP 文件系统时，这非常有用。 为了实现此目的，适用于 UWP 应用的 .NET API 提供了多个扩展方法，让你可以在 .NET 与 UWP 流类型之间转换。 有关详细信息，请参阅 [**WindowsRuntimeStreamExtensions**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions)。

在将 UWP 流转换为 .NET 流时，有效地为基础 UWP 流创建适配器。 在某些情况下，在 UWP 数据流上调用方法会产生与此行为关联的运行时成本。 这可能会影响你的应用的速度，尤其在执行若干频繁读或写的小操作时。

为了提高应用速度，UWP 数据流适配器包含一个数据缓冲区。 以下代码示例演示使用带有默认缓冲区大小的 UWP 数据流适配器的小型连续读取操作。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> StorageFile file = await Windows.Storage.ApplicationData.Current
>     .LocalFolder.GetFileAsync("example.txt");
> Windows.Storage.Streams.IInputStream windowsRuntimeStream = 
>     await file.OpenReadAsync();
> 
> byte[] destinationArray = new byte[8];
> 
> // Create an adapter with the default buffer size.
> using (var managedStream = windowsRuntimeStream.AsStreamForRead())
> {
> 
>     // Read 8 bytes into destinationArray.
>     // A larger block is actually read from the underlying 
>     // windowsRuntimeStream and buffered within the adapter.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> 
>     // Read 8 more bytes into destinationArray.
>     // This call may complete much faster than the first call
>     // because the data is buffered and no call to the 
>     // underlying windowsRuntimeStream needs to be made.
>     await managedStream.ReadAsync(destinationArray, 0, 8);
> }
> ```
> ```vb
> Dim file As StorageFile = Await Windows.Storage.ApplicationData.Current -
> .LocalFolder.GetFileAsync("example.txt")
> Dim windowsRuntimeStream As Windows.Storage.Streams.IInputStream =
>     Await file.OpenReadAsync()
> 
> Dim destinationArray() As Byte = New Byte(8) {}
> 
> ' Create an adapter with the default buffer size.
> Dim managedStream As Stream = windowsRuntimeStream.AsStreamForRead()
> Using (managedStream)
> 
>     ' Read 8 bytes into destinationArray.
>     ' A larger block is actually read from the underlying 
>     ' windowsRuntimeStream and buffered within the adapter.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
>     ' Read 8 more bytes into destinationArray.
>     ' This call may complete much faster than the first call
>     ' because the data is buffered and no call to the 
>     ' underlying windowsRuntimeStream needs to be made.
>     Await managedStream.ReadAsync(destinationArray, 0, 8)
> 
> End Using
> ```

在将 UWP 数据流转换为 .NET 数据流的大部分情况中，均预期使用此默认缓冲行为。 但是，在某些情况下，你可能希望调整缓冲行为以便提高性能。

### <a name="working-with-large-data-sets"></a>使用大型数据集

在读取或写入较大的数据集时，你可以通过为 [**AsStreamForRead**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstreamforread?view=dotnet-uwp-10.0)、[**AsStreamForWrite**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstreamforwrite?view=dotnet-uwp-10.0) 和 [**AsStream**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstream?view=dotnet-uwp-10.0) 扩展方法提供较大的缓冲区大小来提高读取或写入吞吐量。 这将为数据流适配器提供较大的内部缓冲区大小。 例如，将来自大型文件的数据流传递给 XML 分析程序时，分析程序可以从数据流中执行多个连续的小读取操作。 大型缓冲区可以减少对基础 UWP 流的调用次数并提高性能。

> **注意**：在设置大于 80 KB 的缓冲区大小时应非常小心，因为这可能导致在垃圾回收器堆栈上产生碎片（请参阅  改进垃圾回收性能[）。](improve-garbage-collection-performance.md) 以下代码示例创建具有 81,920 个字节缓冲区的托管流适配器。

> [!div class="tabbedCodeSnippets"]
```csharp
// Create a stream adapter with an 80 KB buffer.
Stream managedStream = nativeStream.AsStreamForRead(bufferSize: 81920);
```
```vb
' Create a stream adapter with an 80 KB buffer.
Dim managedStream As Stream = nativeStream.AsStreamForRead(bufferSize:=81920)
```

[  **Stream.CopyTo**](https://docs.microsoft.com/dotnet/api/system.io.stream.copyto) 和 [**CopyToAsync**](https://docs.microsoft.com/dotnet/api/system.io.stream.copytoasync) 方法还会分配本地缓冲区以用于流之间的复制操作。 与 [**AsStreamForRead**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstreamforread?view=dotnet-uwp-10.0) 扩展方法一样，你可以通过覆盖默认缓冲区大小来为大型流复制操作获取较好的性能。 以下代码示例演示了更改 **CopyToAsync** 调用的默认缓冲区大小。

> [!div class="tabbedCodeSnippets"]
> ```csharp
> MemoryStream destination = new MemoryStream();
> // copies the buffer into memory using the default copy buffer
> await managedStream.CopyToAsync(destination);
> 
> // Copy the buffer into memory using a 1 MB copy buffer.
> await managedStream.CopyToAsync(destination, bufferSize: 1024 * 1024);
> ```
> ```vb
> Dim destination As MemoryStream = New MemoryStream()
> ' copies the buffer into memory using the default copy buffer
> Await managedStream.CopyToAsync(destination)
> 
> ' Copy the buffer into memory using a 1 MB copy buffer.
> Await managedStream.CopyToAsync(destination, bufferSize:=1024 * 1024)
> ```

此示例使用 1 MB 的缓冲区大小，此大小大于前面推荐的 80 KB。 使用此类大型缓冲区可以提高极大数据集（即，几百兆字节）的复制操作的吞吐量。 但是，此类缓冲区将在大型对象堆栈上进行分配并可能会降低垃圾回收性能。 只应在大型缓冲区可以显著提高应用性能时使用此类缓冲区大小。

同时使用大量流时，你可能希望减少或消除缓冲区的内存开销。 你可以指定较小的缓冲区，或者将 *bufferSize* 参数设置为 0 以完全关闭该流适配器的缓冲。 如果对托管流执行大型读取和写入操作，仍可以不通过缓冲而获得不错的吞吐量性能。

### <a name="performing-latency-sensitive-operations"></a>执行延迟敏感操作

如果你希望低延迟地读取和写入并且不希望在基础 UWP 数据流之外的大型数据块中进行读取操作，则也可能希望避免使用缓冲。 例如，如果你使用数据流进行网络通信，则可能希望低延迟地读取和写入。

在聊天应用中，你可以使用数据流通过网络接口以便发送来往消息。 在这种情况下，你希望在准备好后立即发送消息，而不等待填满缓冲区。 如果在调用 [**AsStreamForRead**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstreamforread?view=dotnet-uwp-10.0)、[**AsStreamForWrite**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstreamforwrite?view=dotnet-uwp-10.0) 和 [**AsStream**](https://docs.microsoft.com/dotnet/api/system.io.windowsruntimestreamextensions.asstream?view=dotnet-uwp-10.0) 扩展方法时将缓冲区大小设置为 0，则所得的适配器将不分配缓冲区，并且所有调用直接操作基础 UWP 数据流。


