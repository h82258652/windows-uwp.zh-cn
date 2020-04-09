---
title: Unity 和 UWP 中缺少的 .NET API
description: 了解在 Unity 中生成 UWP 游戏时缺少的 .NET API 以及常见问题的解决方案。
ms.assetid: 28A8B061-5AE8-4CDA-B4AB-2EF0151E57C1
ms.date: 02/21/2018
ms.topic: article
keywords: windows 10, uwp, 游戏, .net, unity
ms.localizationpriority: medium
ms.openlocfilehash: df93fbeb3a879a84873827a5ead926f96b02adcc
ms.sourcegitcommit: 8ee0752099170aaf96c7cb105f7cc039b6e7ff06
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2020
ms.locfileid: "80968053"
---
# <a name="missing-net-apis-in-unity-and-uwp"></a>Unity 和 UWP 中缺少的 .NET API

在使用 .NET 生成 UWP 游戏时，你可能会发现你在 Unity 编辑器中可能使用的或用于独立电脑游戏的一些 API 对于 UWP 不存在。 这是因为适用于 UWP 应用的 .NET 包括每个命名空间的完整 .NET Framework 中提供的类型子集。

此外，某些游戏引擎使用与适用于 UWP 的 .NET 不完全兼容的不同 .NET 风格，如 Unity 的 Mono。 因此在你编写游戏时，编辑器中可能完全正常，但当你针对 UWP 进行构建时，你可能会看到如下错误：**类型或命名空间“格式化程序”在命名空间“System.Runtime.Serialization”中不存在（你是否缺少程序集引用？）**

幸运的是，Unity 提供这些缺失的 API 的一部分作为扩展方法和替代类型，在[通用 Windows 平台：在 .NET 脚本后端缺少的 .NET 类型](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)对此进行了描述。 但是，如果你需要的功能不在此处，则[适用于 Windows 8.x 应用的 .NET 概述](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))中讨论了转换代码以使用适用于 UWP API 的 WinRT 或 .NET 的方法。 （它讨论的是 Windows 8，但也适用于 Windows 10 UWP 应用。）

## <a name="net-standard"></a>.NET Standard

要了解一些 API 为什么无法工作，须了解不同的 .NET 风格以及 UWP 如何实现 .NET。 [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) 是 .NET API 的正式规范，适用于各个平台，并统一不同的 .NET 风格。 .NET 的每个实现都支持特定版本的 .NET Standard。 你可以参阅 [.NET 实现支持](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support)中的标准和实现表。

UWP SDK 的每个版本都符合不同级别的 .NET Standard。 例如，16299 SDK（秋季创作者更新）支持 .NET Standard 2.0。

如果你要了解特定的 .NET API 在你的目标 UWP 版本中是否受支持，可以查看 [.NET Standard API 参考](https://docs.microsoft.com/dotnet/api/index?view=netstandard-2.0)并选择该 UWP 版本支持的 .NET Standard 版本。

## <a name="scripting-backend-configuration"></a>脚本后端配置

如果你在针对 UWP 进行生成时遇到麻烦，首先应该检查**播放器设置**（**文件 > 生成设置**，并依次选择**通用 Windows 平台**和**播放器设置**）。 在**其他设置 > 配置**下，前三个下拉列表（**脚本运行时版本**、**脚本后端**和 **API 兼容性级别**）都是需要考虑的重要设置。

Unity 脚本后端使用**脚本运行时版本**来允许你获取你选择的 .NET Framework 支持的（大致）等效版本。 但请记住，该版本的 .NET Framework 中的所有 API 并非全部受支持，仅在你的 UWP 的目标 .NET Standard 版本中的 API 受支持。

在新发布的 .NET 中通常会将更多 API 添加到 .NET Standard 中，这可能允许你在独立版和 UWP 中使用相同代码。 例如，.NET Standard 2.0 中引入了 [System.Runtime.Serialization.Json](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json) 命名空间。 如果你将**脚本运行时版本**设置为 **.NET 3.5 等效项**（针对较早版本的 .NET Standard），你在尝试使用 API 时会遇到错误；将其切换到 **.NET 4.6 等效项**（支持 .NET Standard 2.0），该 API 将正常运行。

**脚本后端**可以为 **.NET** 或 **IL2CPP**。 在本主题中，我们假设你已选择 **.NET**，因为在此处讨论的问题是在这种情况下发生的。 请参阅[脚本后端](https://docs.unity3d.com/Manual/windowsstore-scriptingbackends.html)获取详细信息。

最后，你应将 **API 兼容性级别**设置为你要运行游戏的 .NET 版本。 它应该匹配**脚本运行时版本**。

一般情况下，对于**脚本运行时版本**和 **API 兼容性级别**，你应该选择可用的最新版本，以便提高与 .NET Framework 的兼容性，你也可以使用更多的 .NET API。

![配置：脚本运行时版本；脚本后端；API 兼容性级别](images/missing-dot-net-apis-in-unity-1.png)

## <a name="platform-dependent-compilation"></a>依赖于平台的编译

如果你正在为多个平台（包括 UWP）生成 Unity 游戏，你需要使用依赖于平台的编译，以确保仅当作为 UWP 生成游戏时运行适用于 UWP 的代码。 这样一来，你可以对独立桌面和其他平台使用完整的 .NET Framework，对 UWP 使用 WinRT API，不会发生生成错误。

使用以下指令，以便仅在作为 UWP 应用运行时编译代码：

```csharp
#if NETFX_CORE
    // Your UWP code here
#else
    // Your standard code here
#endif
```

> [!NOTE]
> `NETFX_CORE` 仅用于检查是否正在针对 .NET 脚本编写C#后端编译代码。 如果你使用的是不同的脚本后端（例如 IL2CPP），请改用[`ENABLE_WINMD_SUPPORT`](https://docs.unity3d.com/Manual/windowsstore-code-snippets.html) 。

若要获取依赖于平台的编译指令的完整列表，请参阅[依赖于平台的编译](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html)。

## <a name="common-issues-and-workarounds"></a>常见问题和解决方法

以下方案描述当 UWP 子集中缺少 .NET API 时可能出现的常见问题以及解决方法。

### <a name="data-serialization-using-binaryformatter"></a>使用 BinaryFormatter 进行数据序列化

游戏经常序列化保存数据，使玩家无法轻松操控它。 但是，将对象序列化为二进制的 [BinaryFormatter](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.formatters.binary.binaryformatter) 在 .NET Standard 的较早版本（2.0 以前）中不可用。 请考虑改用 [XmlSerializer](https://docs.microsoft.com/dotnet/api/system.xml.serialization.xmlserializer) 或 [DataContractJsonSerializer](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer)。

```csharp
private void Save()
{
    SaveData data = new SaveData(); // User-defined object to serialize

    DataContractJsonSerializer serializer = 
      new DataContractJsonSerializer(typeof(SaveData));

    FileStream stream = 
      new FileStream(Application.persistentDataPath, FileMode.CreateNew);

    serializer.WriteObject(stream, data);
    stream.Dispose();
}
```

### <a name="io-operations"></a>I/O 操作

[System.IO](https://docs.microsoft.com/dotnet/api/system.io) 命名空间中的一些类型，如 [FileStream](https://docs.microsoft.com/dotnet/api/system.io.filestream)，在 .NET Standard 的较早版本中不可用。 但是，Unity 提供[目录](https://docs.microsoft.com/dotnet/api/system.io.directory)、[文件](https://docs.microsoft.com/dotnet/api/system.io.file)和 **FileStream** 类型，以便你可以在游戏中使用它们。

或者，你可以使用 [Windows.Storage](https://docs.microsoft.com/uwp/api/Windows.Storage) API，这些 API 仅适用于 UWP 应用。 但是，这些 API 限制应用写入到它们的特定存储中，且不允许它自由访问整个文件系统。 请参阅[文件、文件夹和库](https://docs.microsoft.com/windows/uwp/files/)获取详细信息。

一个重要说明是[关闭](https://docs.microsoft.com/dotnet/api/system.io.stream.close)方法仅在 .NET Standard 2.0 及更高版本中可用（但 Unity 提供了扩展方法）。 改用[处置](https://docs.microsoft.com/dotnet/api/system.io.stream.dispose)。

### <a name="threading"></a>线程

[System.Threading](https://docs.microsoft.com/dotnet/api/system.threading) 命名空间中的一些类型，如 [ThreadPool](https://docs.microsoft.com/dotnet/api/system.threading.threadpool)，在 .NET Standard 的较早版本中不可用。 在这些情况下，你可以改用 [Windows.System.Threading](https://docs.microsoft.com/uwp/api/windows.system.threading) 命名空间。

下面介绍在 Unity 游戏中如何使用依赖于平台的编译处理线程，以便为 UWP 和非 UWP 平台做准备：

```csharp
private void UsingThreads()
{
#if NETFX_CORE
    Windows.System.Threading.ThreadPool.RunAsync(workItem => SomeMethod());
#else
    System.Threading.ThreadPool.QueueUserWorkItem(workItem => SomeMethod());
#endif
}
```

### <a name="security"></a>安全性

在为 UWP 生成 Unity 游戏时，有些 **System.Security.** * 命名空间（如 [System.Security.Cryptography.X509Certificates](https://docs.microsoft.com/dotnet/api/system.security.cryptography.x509certificates?view=netstandard-2.0)）不可用。 在这些情况下，请使用 **Windows.Security.** * API，其具有许多相同的功能。

下面的示例只是从具有指定名称的证书存储获取证书：

```cs
private async void GetCertificatesAsync(string certStoreName)
    {
#if NETFX_CORE
        IReadOnlyList<Certificate> certs = await CertificateStores.FindAllAsync();
        IEnumerable<Certificate> myCerts = 
            certs.Where((certificate) => certificate.StoreName == certStoreName);
#else
        X509Store store = new X509Store(certStoreName, StoreLocation.CurrentUser);
        store.Open(OpenFlags.OpenExistingOnly);
        X509Certificate2Collection certs = store.Certificates;
#endif
    }
```

有关使用 WinRT 安全 API 的详细信息，请参阅[安全性](https://docs.microsoft.com/windows/uwp/security/)。

### <a name="networking"></a>联网

有些 **System&period;Net.** * 命名空间（如 [System.Net.Mail](https://docs.microsoft.com/dotnet/api/system.net.mail?view=netstandard-2.0)）在为 UWP 生成 Unity 游戏时同样不可用。 对于这些 API 的大多数，请使用相应的 **Windows.Networking.** * 和**Windows.Web.** * WinRT API 来获取类似的功能。 有关详细信息，请参阅[网络和 Web 服务](https://docs.microsoft.com/windows/uwp/networking/)。

对于 **System.Net.Mail**，请使用 [Windows.ApplicationModel.Email](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email) 命名空间。 有关详细信息，请参阅[发送电子邮件](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email)。

## <a name="see-also"></a>另请参阅

* [通用 Windows 平台： .NET 脚本后端缺少 .NET 类型](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)
* [适用于 UWP 应用的 .NET 概述](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))
* [Unity UWP 移植指南](https://unity3d.com/partners/microsoft/porting-guides)