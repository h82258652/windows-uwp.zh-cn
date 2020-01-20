---
title: 向文件进行写入的最佳做法
description: 了解使用 FileIO 和 PathIO 类的各种文件写入方法的最佳做法。
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: dcbeffc7e3db8f3df9c197e8c388f30faf7ad03d
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685241"
---
# <a name="best-practices-for-writing-to-files"></a>向文件进行写入的最佳做法

**重要的 API**

* [FileIO 类](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 
* [**PathIO 类**](https://docs.microsoft.com/uwp/api/windows.storage.pathio)

开发人员在使用 [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 和 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 类的 **Write** 方法执行文件系统 I/O 操作时，偶尔会遇到一系列常见问题。 例如，这些常见问题包括：

* 不完整地写入某个文件。
* 应用在调用某个方法时收到异常。
* 操作留下一些文件名类似于目标文件名的 .TMP 文件。

[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 和 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 类的 **Write** 方法包括：

* **WriteBufferAsync**
* **WriteBytesAsync**
* **WriteLinesAsync**
* **WriteTextAsync**

 本文将提供有关这些方法的工作原理的详细信息，使开发人员能够更好地了解何时以及如何使用它们。 本文只会提供指导，而不会试图针对所有可能的文件 I/O 问题提供解决方法。 

> [!NOTE]
> 本文会重点介绍示例和讨论内容中的 **FileIO** 方法。 但是，**PathIO** 方法遵循类似的模式，本文中的大部分指导同样适用于这些方法。 

## <a name="convenience-vs-control"></a>便利性与控制度

[**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) 对象并非本机 Win32 编程模型那样的文件句柄。 [**StorageFile**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile) 是文件的一种表示形式，该文件包含用于操作其内容的方法。

使用 **StorageFile** 执行 I/O 时，了解这一概念很有好处。 例如，[写入文件](quickstart-reading-and-writing-files.md#writing-to-a-file)部分演示了写入文件的三种方式：

* 使用 [**FileIO.WriteTextAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writetextasync) 方法。
* 创建一个缓冲区，然后调用 [**FileIO.WriteBufferAsync**](https://docs.microsoft.com/uwp/api/windows.storage.fileio.writebufferasync) 方法。
* 使用流的四步模型：
  1. [打开](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.openasync)文件以获取流。
  2. [获取](https://docs.microsoft.com/uwp/api/windows.storage.streams.irandomaccessstream.getoutputstreamat)输出流。
  3. 创建 [**DataWriter**](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter) 对象并调用相应的 **Write** 方法。
  4. 在数据写入器中[提交](https://docs.microsoft.com/uwp/api/windows.storage.streams.datawriter.storeasync)数据，然后[刷新](https://docs.microsoft.com/uwp/api/windows.storage.streams.ioutputstream.flushasync)输出流。

前两种方案是应用最常用的方案。 以单个操作写入文件更易于编程和维护，同样，使应用不必要应对文件 I/O 存在的多种复杂性。 但是，获得这种便利性也需要付出一定的代价：损失了整个操作的控制度，并且无法捕获特定时间点发生的错误。

## <a name="the-transactional-model"></a>事务模型

[**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 和 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 类的 **Write** 方法通过一个附加层整合了上述第三个写入模型的步骤。 此层封装在存储事务中。

如果在写入数据时出错，为了保护原始文件的完整性，**Write** 方法将通过 [**OpenTransactedWriteAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.opentransactedwriteasync) 打开该文件，以使用事务模型。 此过程将创建 [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) 对象。 创建此事务对象后，API 会按照 [**StorageStreamTransaction**](https://docs.microsoft.com/uwp/api/windows.storage.storagestreamtransaction) 一文中的[文件访问](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileAccess)示例或代码示例所述的类似方式写入数据。

下图演示了成功的写入操作中 **WriteTextAsync** 方法执行的基础任务。 此图提供了操作的简化视图。 例如，它跳过了在不同的线程上执行文本编码和异步完成的步骤。

![用于写入文件的 UWP API 调用顺序图](images/file-write-call-sequence.svg)

使用 [**FileIO**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileIO) 和 [**PathIO**](https://docs.microsoft.com/uwp/api/windows.storage.pathio) 类的 **Write** 方法，而不是使用利用流的更复杂四步模型的优势包括：

* 执行一次 API 调用即可处理所有中间步骤，包括错误处理。
* 出错时可以保留原始文件。
* 尽量保留干净的系统状态。

但是，由于存在许多的潜在中间故障点，发生失败的可能性也会增大。 发生错误时，可能难以了解过程在哪个位置失败。 以下部分介绍了在使用 **Write** 方法时可能会遇到的一些失败，并提供了可能的解决方法。

## <a name="common-error-codes-for-write-methods-of-the-fileio-and-pathio-classes"></a>FileIO 和 PathIO 类的 Write 方法的常见错误代码

下表列出了应用开发人员在使用 **Write** 方法时可能会遇到的常见错误代码。 表中的步骤对应于上图中的步骤。

|  错误名称（值）  |  步骤  |  原因  |  解决方案  |
|----------------------|---------|----------|-------------|
|  ERROR_ACCESS_DENIED (0X80070005)  |  5  |  原始文件可能已标记为删除（可能是在前一操作中标记的）。  |  重试操作。</br>确保对文件的访问权限已同步。  |
|  ERROR_SHARING_VIOLATION (0x80070020)  |  5  |  原始文件已由另一个排他写入操作打开。   |  重试操作。</br>确保对文件的访问权限已同步。  |
|  ERROR_UNABLE_TO_REMOVE_REPLACED (0x80070497)  |  19-20  |  原始文件 (file.txt) 已被使用，因此无法将其替换。 在替换之前，另一个进程或操作已获取了该文件的访问权限。  |  重试操作。</br>确保对文件的访问权限已同步。  |
|  ERROR_DISK_FULL (0x80070070)  |  7、14、16、20  |  事务处理模型创建了额外的文件，这消耗了额外的存储。  |    |
|  ERROR_OUTOFMEMORY (0x8007000E)  |  14、16  |  此错误的原因可能是存在多个未完成的 I/O 操作，或文件很大。  |  以更精细的方法控制流可能会解决该错误。  |
|  E_FAIL (0x80004005) |  Any  |  杂项  |  重试操作。 如果仍然失败，则可能表示平台出错，应用应该终止，因为它处于不一致状态。 |

## <a name="other-considerations-for-file-states-that-might-lead-to-errors"></a>可能导致出错的其他文件状态考虑因素

除了 **Write** 方法返回的错误以外，下面还提供了有关应用在写入文件时预期可能会遇到的问题的指导。

### <a name="data-was-written-to-the-file-if-and-only-if-operation-completed"></a>当且仅当操作完成时，才会将数据写入文件

当操作正在进行时，应用不应该对文件中的数据做出任何假设。 在操作完成之前尝试访问文件可能会导致不一致的数据。 应用应该负责跟踪未完成的 I/O。

### <a name="readers"></a>Readers

如果写入到的文件同时由某个正常的读取器（即，文件是使用 [**FileAccessMode.Read**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileAccessMode) 打开的），则后续读取将会失败并出现错误 ERROR_OPLOCK_HANDLE_CLOSED (0x80070323)。 有时，当 **Write** 操作正在进行时，应用会重试再次打开文件进行读取。 这可能会导致争用状态，使 **Write** 最终在尝试覆盖原始文件时失败，因为无法替换该文件。

### <a name="files-from-knownfolders"></a>KnownFolders 中的文件

你的应用可能不是唯一一个尝试访问任何 [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) 中的文件的应用。 无法保证当操作成功时，应用写入到该文件的内容在下一次尝试读取该文件时保持不变。 此外，在这种情况下，拒绝共享或访问错误会更常见。

### <a name="conflicting-io"></a>有冲突的 I/O

如果应用对其本地数据中的文件使用 **Write** 方法，则可以减少出现并发错误的可能性，但仍需注意某些问题。 如果同时将多个 **Write** 发送到文件，则无法保证该文件中的最终数据是什么。 为了缓解此问题，我们建议让应用将发送到文件的 **Write** 操作序列化。

### <a name="tmp-files"></a>~TMP 文件

有时，如果强制取消操作（例如，如果应用已由 OS 挂起或终止），则不会提交或适当关闭事务。 这可能会留下带有 .~TMP 扩展名的文件。 在处理应用激活时，请考虑删除这些临时文件（如果应用的本地数据中存在这些文件）。

## <a name="considerations-based-on-file-types"></a>文件类型相关的注意事项

根据文件的类型、其访问频率和大小，某些错误可能会变得更加普遍。 通常情况下，应用可以访问三种类别的文件：

* 由用户在应用的本地数据文件夹中创建和编辑的文件。 只能在使用应用时创建和编辑这些文件，并且它们只会在该应用中存在。
* 应用元数据。 应用使用这些文件来跟踪自身的状态。
* 文件系统位置中的其他文件，应用在其中声明了访问功能。 这些文件通常位于某个 [**KnownFolders**](https://docs.microsoft.com/uwp/api/Windows.Storage.KnownFolders) 中。

应用对前两个类别的文件拥有完全控制权，因为这些文件是该应用的包文件的一部分，并由该应用独占访问。 对于最后一个类别中的文件，应用必须注意，其他应用和 OS 服务可能同时正在访问这些文件。

根据具体的应用，对文件的访问因频率而异：

* 非常低。 这些文件通常在应用启动时打开，并在应用挂起时保存。
* 低。 这些文件是用户专门对其执行操作（例如保存或加载）的文件。
* 中等或高。 应用必须在这些文件中持续更新数据（例如，自动保存功能或常量元数据跟踪）。

在文件大小方面，请考虑 **WriteBytesAsync** 方法的以下图表中的性能数据。 该图表比较了在受控的环境中，根据平均性能为每个文件大小 10000 次操作，针对不同的文件大小完成某项操作所需的时间。

![WriteBytesAsync 性能](images/writebytesasync-performance.png)

此图表中有意省略了 Y 轴上的时间值，因为不同的硬件和配置会生成不同的绝对时间值。 但是，我们在测试中观察到了这些趋势的一致性：

* 对于极小的文件 (<= 1 MB)：完成操作所需的时间一贯很短。
* 对于较大的文件 (> 1 MB)：完成操作所需的时间开始呈指数级增长。

## <a name="io-during-app-suspension"></a>应用挂起期间的 I/O

如果你想要保留状态信息或元数据以便在后续会话中使用，则应用必须能够处理挂起。 有关应用挂起的背景信息，请参阅[应用生命周期](../launch-resume/app-lifecycle.md)和[此博客文章](https://blogs.windows.com/buildingapps/2016/04/28/the-lifecycle-of-a-uwp-app/#qLwdmV5zfkAPMEco.97)。

除非 OS 授权长时间执行应用，否则，当应用挂起时，它可以在 5 秒钟内释放其资源并保存其数据。 为获得最佳可靠性和用户体验，请始终假设处理挂起任务的时间是有限的。 在处理挂起任务的 5 秒时限内，请记住以下指导原则：

* 尽量减少 I/O，以避免刷新和释放操作导致争用状态。
* 避免写入需要数百毫秒或更长时间才能写入的文件。
* 如果应用使用 **Write** 方法，请记住这些方法需要的所有中间步骤。

如果应用在挂起期间处理少量的状态数据，则大多数情况下，你都可以使用 **Write** 方法来刷新数据。 但是，如果应用使用大量的状态数据，请考虑使用流来直接存储数据。 这可能有助于减小 **Write** 方法的事务模型造成的延迟。 

有关示例，请参阅 [BasicSuspension](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicSuspension) 示例。

## <a name="other-examples-and-resources"></a>其他示例和资源

下面是适用于特定方案的几个示例和其他资源。

### <a name="code-example-for-retrying-file-io-example"></a>有关重试文件 I/O 的代码示例

以下伪代码示例假设在用户选取要保存的文件后执行写入，演示在这种情况下如何重试写入 (C#)：

```csharp
Windows.Storage.Pickers.FileSavePicker savePicker = new Windows.Storage.Pickers.FileSavePicker();
savePicker.FileTypeChoices.Add("Plain Text", new List<string>() { ".txt" });
Windows.Storage.StorageFile file = await savePicker.PickSaveFileAsync();

Int32 retryAttempts = 5;

const Int32 ERROR_ACCESS_DENIED = unchecked((Int32)0x80070005);
const Int32 ERROR_SHARING_VIOLATION = unchecked((Int32)0x80070020);

if (file != null)
{
    // Application now has read/write access to the picked file.
    while (retryAttempts > 0)
    {
        try
        {
            retryAttempts--;
            await Windows.Storage.FileIO.WriteTextAsync(file, "Text to write to file");
            break;
        }
        catch (Exception ex) when ((ex.HResult == ERROR_ACCESS_DENIED) ||
                                   (ex.HResult == ERROR_SHARING_VIOLATION))
        {
            // This might be recovered by retrying, otherwise let the exception be raised.
            // The app can decide to wait before retrying.
        }
    }
}
else
{
    // The operation was cancelled in the picker dialog.
}
```

### <a name="synchronize-access-to-the-file"></a>同步对文件的访问权限

[使用 .NET 的并行编程博客文章](https://devblogs.microsoft.com/pfxteam/)是一个不错的资源，其中提供了有关并行编程的指导。 [有关 AsyncReaderWriterLock 的文章](https://devblogs.microsoft.com/pfxteam/building-async-coordination-primitives-part-7-asyncreaderwriterlock/)专门介绍了如何保留对文件的独占写入访问权限，同时允许进行并发读取访问。 请注意，序列化 I/O 会影响性能。

## <a name="see-also"></a>另请参阅

* [创建、写入和读取文件](quickstart-reading-and-writing-files.md)
