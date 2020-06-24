---
title: 进程间通信 (IPC)
description: 本主题介绍在通用 Windows 平台（UWP）应用程序与 Win32 应用程序之间执行进程间通信（IPC）的各种方式。
ms.date: 03/23/2020
ms.topic: article
keywords: windows 10, uwp
ms.openlocfilehash: 5db029db3ffb538802f39aa616c96dbe75601eac
ms.sourcegitcommit: bf7d4f6739aeeaac735aae3dd0dcbda63a8c5e69
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/23/2020
ms.locfileid: "85256377"
---
# <a name="interprocess-communication-ipc"></a>进程间通信 (IPC)

本主题介绍在通用 Windows 平台（UWP）应用程序与 Win32 应用程序之间执行进程间通信（IPC）的各种方式。

## <a name="app-services"></a>应用程序服务

应用服务使应用程序能够公开在后台接受和返回基元（[**ValueSet**](/uwp/api/Windows.Foundation.Collections.ValueSet)）属性包的服务。 如果对其进行[序列化](https://stackoverflow.com/questions/46367985/how-to-make-a-class-that-can-be-added-to-the-windows-foundation-collections-valu)，则可以传递丰富的对象。

应用服务可以在后台任务或前台应用程序[内的](/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)[进程中](/windows/uwp/launch-resume/convert-app-service-in-process)运行。

应用服务最适合用于共享少量的数据，在这种情况下，不需要几乎实时的延迟。

## <a name="com"></a>COM

[COM](/windows/win32/com/component-object-model--com--portal)是一种面向对象的分布式系统，用于创建可进行交互和通信的二进制软件组件。 作为开发人员，使用 COM 为应用程序创建可重用的软件组件和自动化层。 COM 组件可以是进程内或进程外的，它们可以通过[客户端和服务器](/windows/win32/com/com-clients-and-servers)模型进行通信。 进程外 COM 服务器长时间用作[对象间通信](/windows/win32/com/inter-object-communication)的方式。

具有[runFullTrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)功能的打包应用程序可以通过[包清单](/uwp/schemas/appxpackage/uapmanifestschema/element-com-extension)为 IPC 注册进程外 COM 服务器。 这称为 "[打包的 COM](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)"。

## <a name="filesystem"></a>Filesystem

### <a name="broadfilesystemaccess"></a>BroadFileSystemAccess

打包的应用程序可以通过声明[broadFileSystemAccess](/windows/uwp/files/file-access-permissions#accessing-additional-locations)受限功能来使用广泛的文件系统执行 IPC。 此功能授予[Windows 存储](/uwp/api/Windows.Storage)Api 和[xxxFromApp](/previous-versions/windows/desktop/legacy/mt846585(v=vs.85)) Win32 api 对广泛文件系统的访问权限。

默认情况下，使用打包应用程序的文件系统的 IPC 仅限于此部分中所述的其他机制。

### <a name="publishercachefolder"></a>PublisherCacheFolder

[PublisherCacheFolder](/uwp/api/windows.storage.applicationdata.getpublishercachefolder)使打包后的应用程序可以声明其清单中的文件夹，同一发布者可以与其他包共享这些文件夹。

共享存储文件夹具有以下要求和限制：

* 不会备份或漫游共享存储文件夹中的数据。
* 用户可以清除共享存储文件夹的内容。
* 不能使用 "共享存储" 文件夹在不同发布者的应用程序之间共享数据。
* 不能使用共享存储文件夹在不同用户之间共享数据。
* 共享存储文件夹没有版本管理。

如果发布多个应用程序，并且正在寻找一种简单的机制来共享它们之间的数据，则 PublisherCacheFolder 是基于文件系统的简单选项。

### <a name="sharedaccessstoragemanager"></a>SharedAccessStorageManager

[SharedAccessStorageManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager)与应用服务、协议激活（例如 LaunchUriForResultsAsync）等结合使用，以通过令牌共享 StorageFiles。

## <a name="fulltrustprocesslauncher"></a>FullTrustProcessLauncher

利用[runFullTrust](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities)功能，打包应用程序可以在同一包中[启动完全信任进程](/uwp/api/Windows.ApplicationModel.FullTrustProcessLauncher)。

对于包限制是负担或缺少 IPC 选项的情况，应用程序可以使用完全信任进程作为代理来与系统交互，然后使用完全信任进程本身通过应用服务或其他受支持的其他 IPC 机制进行 IPC。

## <a name="launchuriforresultsasync"></a>LaunchUriForResultsAsync

[LaunchUriForResultsAsync](/windows/uwp/launch-resume/how-to-launch-an-app-for-results)用于与实现[ProtocolForResults](/windows/uwp/launch-resume/how-to-launch-an-app-for-results#step-2-override-applicationonactivated-in-the-app-that-youll-launch-for-results)激活协定的其他封装应用程序进行简单（[ValueSet](/uwp/api/Windows.Foundation.Collections.ValueSet)）数据交换。 与通常在后台运行的应用服务不同，后者在前台启动目标应用程序。

可以通过 ValueSet 将[SharedStorageAccessManager](/uwp/api/Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager)令牌传递给应用程序来共享文件。

## <a name="loopback"></a>环回

环回是与侦听 localhost （环回地址）的网络服务器进行通信的过程。

为了保持安全和网络隔离，默认情况下，针对打包的应用程序，会阻止 IPC 的环回连接。 可以使用[功能](/previous-versions/windows/apps/hh770532(v=win.10))和[清单属性](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)在受信任的打包应用程序之间启用环回连接。

* 所有参与环回连接的打包应用程序都需要 `privateNetworkClientServer` 在其[包清单](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)中声明此功能。
* 两个打包的应用程序可以通过环回进行通信，方法是在其包清单中声明[LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules) 。
    * 每个应用程序都必须列出其[LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)中的另一个。 客户端为服务器声明了 "out" 规则，服务器为其支持的客户端声明了 "in" 规则。

> [!NOTE]
> 在开发时，可以通过 Visual Studio 中的包清单编辑器、通过 Microsoft Store 发布的应用程序的[合作伙伴中心](/windows/uwp/publish/view-app-identity-details)或已安装的应用程序的[add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 命令，在这些规则中标识应用程序所需的包系列名称。

未打包的应用程序和服务没有包标识，因此不能在[LoopbackAccessRules](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-loopbackaccessrules)中声明它们。 你可以通过[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))将打包的应用程序配置为通过与未打包的应用程序和服务的环回进行连接，但这仅适用于你对计算机具有本地访问权限的旁加载或调试方案，并且你具有管理员权限。

* 所有参与环回连接的打包应用程序都需要 `privateNetworkClientServer` 在其[包清单](/uwp/schemas/appxpackage/uapmanifestschema/element-capability)中声明功能。
* 如果打包应用程序连接到未打包的应用程序或服务，则运行 `CheckNetIsolation.exe LoopbackExempt -a -n=<PACKAGEFAMILYNAME>` 为打包的应用程序添加环回例外。
* 如果未打包的应用程序或服务正在连接到打包应用程序，请运行 `CheckNetIsolation.exe LoopbackExempt -is -n=<PACKAGEFAMILYNAME>` 以使打包的应用程序能够接收入站环回连接。
    * 打包的应用程序侦听连接时， [CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))必须连续运行。
    * 该 `-is` 标志是在 Windows 10 版本1607（10.0;）中引入的。生成14393）。

> [!NOTE]
> 在 `-n` 开发时，可以通过 Visual Studio 中的包清单编辑器、通过 Microsoft Store 发布的应用程序的[合作伙伴中心](/windows/uwp/publish/view-app-identity-details)或已安装的应用程序的[add-appxpackage](/powershell/module/appx/get-appxpackage?view=win10-ps) PowerShell 命令来查找[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))标志所需的包系列名称。

[CheckNetIsolation.exe](/previous-versions/windows/apps/hh780593(v=win.10))对于[调试网络隔离问题](/previous-versions/windows/apps/hh780593(v=win.10)#debug-network-isolation-issues)也很有用。

## <a name="pipes"></a>管道

[管道](/windows/win32/ipc/pipes)启用了管道服务器与一个或多个管道客户端之间的简单通信。

支持[匿名管道](/windows/win32/ipc/anonymous-pipes)和[命名管道](/windows/win32/ipc/named-pipes)，但存在以下限制：

* 默认情况下，仅在同一包中的进程之间支持打包应用程序中的命名管道，除非进程是完全信任。
* 按照[共享命名对象](/windows/uwp/communication/sharing-named-objects)的准则，可以在包之间共享命名管道。
* 打包的应用程序中的命名管道必须使用 `\\.\pipe\LOCAL\` 管道名称的语法。

## <a name="registry"></a>注册表

通常不建议使用 IPC 的[注册表](/windows/win32/sysinfo/registry-functions)使用情况，但对于现有代码，它是受支持的。 打包的应用程序只能访问他们有权访问的注册表项。

[打包为 .msix 的桌面应用程序](/windows/msix/desktop/desktop-to-uwp-root)利用[注册表虚拟化](/windows/msix/desktop/desktop-to-uwp-behind-the-scenes#registry)，以便将全局注册表写入包含在 .msix 包中的专用 hive。 这可以实现源代码兼容性，同时最大限度地降低全局注册表影响，并且可用于同一包中的进程之间的 IPC。 如果必须使用注册表，此模型是首选模型，而不是操作全局注册表。

## <a name="rpc"></a>RPC

可以使用[RPC](/windows/win32/rpc/rpc-start-page)将打包应用程序连接到 Win32 rpc 终结点，前提是打包的应用程序具有匹配 RPC 终结点上的 acl 的正确功能。

自定义功能使 Oem 和 Ihv 能够[定义任意功能](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#reserving-a-custom-capability)，[使用这些功能作为其 RPC 终结点的 ACL](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#allowing-access-to-an-rpc-endpoint-to-a-uwp-app-using-the-custom-capability)，然后将[这些功能授予授权客户端应用程序](/windows-hardware/drivers/devapps/hardware-support-app--hsa--steps-for-driver-developers#preparing-the-signed-custom-capability-descriptor-sccd-file)。 有关完整的示例应用程序，请参阅[CustomCapability](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomCapability)示例。

RPC 终结点也可列入到特定的打包应用程序，将对终结点的访问限制为仅限这些应用程序，而无需使用自定义功能的管理开销。 可以使用[DeriveAppContainerSidFromAppContainerName](/windows/win32/api/userenv/nf-userenv-deriveappcontainersidfromappcontainername) API 从包系列名称中派生 SID，然后使用 SID 作为 RPC 终结点的 ACL，如[CustomCapability](https://github.com/Microsoft/Windows-universal-samples/blob/master/Samples/CustomCapability/Service/Server/RpcServer.cpp)示例中所示。

## <a name="shared-memory"></a>Shared Memory

[文件映射](/windows/win32/memory/sharing-files-and-memory)可用于在两个或多个进程之间共享文件或内存，但有以下限制：

* 默认情况下，仅在同一包中的进程之间支持打包应用程序中的文件映射，除非进程是完全信任。
* 按照[共享命名对象](/windows/uwp/communication/sharing-named-objects)的准则，可以在包之间共享文件映射。

建议使用共享内存来有效地共享和处理大量数据。
