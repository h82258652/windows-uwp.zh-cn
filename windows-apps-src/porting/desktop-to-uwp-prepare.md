---
Description: 本文列出了需要打包桌面应用程序之前需要了解的内容。 你可能不需要执行很多操作即可使应用为打包过程做好准备。
Search.Product: eADQiWindows 10XVcnh
title: 准备打包桌面应用程序 （桌面桥）
ms.date: 05/18/20188
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 49238ba1b72cf3daf46fb076432a478e9f381bc6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600892"
---
# <a name="prepare-to-package-a-desktop-application"></a>准备打包桌面应用程序

本文列出了在对桌面应用打包前你需要知道的事项。 可能无需执行很多以获取你的应用程序准备好进行打包过程中，但如果任何以下各项适用于你的应用程序，则需要打包之前解决。 请记住，Microsoft Store 会为你处理授权和自动更新，使你能够从基本代码中删除与这些任务相关的任何功能。

>[!IMPORTANT]
>在 Windows 10，版本 1607，引入的功能来创建 Windows 应用程序包为桌面应用程序 （也称为桌面桥） 且不能仅用在面向 Windows 10 周年更新 (10.0; 项目Build 14393) 或更高版本在 Visual Studio 中的。

+ __应用程序需要以前版本的.NET 4.6.2__。 您需要确保你的应用程序在.NET 4.6.2 上运行。 你不能需要或重新分发早于 4.6.2 的版本。 这是 Windows 10 周年更新中提供的 .NET 版本。 验证你的应用程序适用于此版本，可以确保你的应用程序将继续与将来的更新的 Windows 10 兼容。  如果你的应用程序面向.NET Framework 4.0 或更高版本，它应在.NET 4.6.2 上运行，但仍应测试它。

+ __使用提升的安全权限运行应用程序始终__。 你的应用程序需要交互式用户身份运行时协同工作。 从 Microsoft Store 安装应用程序的用户可能不是系统管理员，因此需要运行提升的权限意味着它将无法正确运行为标准用户应用程序。 Microsoft Store 中不接受任何功能部分需要提升的应用。

+ __应用程序所需的内核模式驱动程序或 Windows 服务__。 该桥适用于应用，但不支持内核模式驱动程序或需要在系统帐户下运行的 Windows 服务。 使用[后台任务](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)，而不是 Windows 服务。

+ __在进程内将应用的模块加载到不在 Windows 应用包中的进程__。 不允许此操作，这意味着不支持进程中扩展，如 [shell 扩展](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)。 但是，如果你在同一个程序包中有两个应用，则可以在它们之间执行进程间通信。

+ __应用程序使用自定义应用程序用户模型 ID (AUMID)__。 如果您在进程调用[SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx)若要设置其自己 AUMID，则它可能仅使用为其生成的应用程序模型环境/Windows 应用包 AUMID。 无法定义自定义 AUMID。

+ __你的应用程序修改 HKEY_LOCAL_MACHINE (HKLM) 注册表配置单元__。 你的应用程序的任何尝试创建 HKLM 密钥，或打开一个修改时，将导致在拒绝访问失败。 请记住你的应用程序具有的注册表中，其自己专用的虚拟化的视图，因此用户和计算机范围内注册表配置单元 （这是什么是 HKLM） 的概念不适用。 你将需要找到另一种方法来实现 HKLM 的用途，如改为写入 HKEY_CURRENT_USER (HKCU)。

+ __应用程序作为启动另一个应用程序的一种方式使用 ddeexec 注册表子项__。 改为使用[应用程序包清单](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)中的各种可激活*扩展配置的 DelegateExecute 谓词处理程序之一。

+ __你的应用程序写入到 AppData 文件夹或与其他应用共享数据的目的注册表__。 转换后，AppData 将重定向到本地应用数据存储，该存储是每个 UWP 应用的专用应用商店。

  你的应用程序将写入到 HKEY_LOCAL_MACHINE 注册表配置单元的所有项会重都定向到独立的二进制文件和任何应用程序写入 HKEY_CURRENT_USER 注册表配置单元的条目放入专用每个用户，每个应用的位置。 有关文件和注册表重定向的更多详细信息，请参阅[在桌面桥幕后](desktop-to-uwp-behind-the-scenes.md)。  

  使用不同的进程间数据共享方式。 有关详细信息，请参阅[存储和检索设置以及其他应用数据](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

+ __你的应用程序将写入到您的应用程序的安装目录__。 例如，你的应用程序写入日志文件在 exe 所在的同一目录中放置了。 此操作不受支持，因此你需要找到另一个位置，如本地应用数据存储。

+ __应用程序安装需要用户交互__。 应用程序安装程序必须能够以无提示方式，运行并且它必须安装所有必备条件，不在默认情况下在干净的操作系统映像。

+ __应用程序使用当前工作目录__。 在运行时，您打包桌面应用程序不会获得你以前在您的桌面中指定的相同工作目录。LNK 的快捷方式。 需要更改你 CWD 在运行时，如果具有正确的目录非常重要的应用程序才能正常工作。

+ __应用程序需要 UIAccess__。 如果应用程序在 UAC 清单的 `requestedExecutionLevel` 元素中指定 `UIAccess=true`，则当前不支持转换为 UWP。 有关详细信息，请参阅 [UI 自动化安全概述](https://msdn.microsoft.com/library/ms742884.aspx)。

+ __你的应用程序公开的 COM 对象__。 来自程序包内的进程和扩展可以注册并使用 COM 和 OLE 服务器，进程内和进程外 (OOP) 皆可。   创意者更新添加了打包的 COM 支持，它提供注册 OOP COM 和 OLE 服务器（现在这些服务器在包外部可见）的功能。  请参阅[对桌面桥的 COM 服务器和 OLE 文档支持](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97)。

   打包的 COM 支持适用于现有的 COM API，但不适用于依赖直接读取注册表的应用程序扩展，因为打包的 COM 位于一个专用位置。

+ __你的应用程序公开其他进程使用的 GAC 程序集__。 在当前版本中，你的应用程序不能公开使用的 GAC 程序集源自外部可执行文件到你的 Windows 应用程序包的进程。 来自程序包内的进程可以照常注册和使用 GAC 程序集，但它们在外部将不可见。 这意味着，OLE 等互操作方案在被外部进程调用时不起作用。

+ __你的应用程序不受支持的方式链接 C 运行时库 (CRT)__。 Microsoft C/C++ 运行时库提供用于为 Microsoft Windows 操作系统编程的例程。 这些例程自动执行许多不采用 C 和 C++ 语言提供的常见编程任务。 如果你的应用程序中使用 C/c + + 运行时库，必须确保链接中受支持的方式。

    对于最新版本的 CRT，Visual Studio 2017 即支持静态和动态链接，以允许你的代码使用常见 DLL 文件，也支持静态链接，以将库直接链接到你的代码。 如果可能，我们建议你的应用程序使用动态链接与 VS 2017。

    以前版本的 Visual Studio 中的支持各有不同。 有关详细信息，请参阅下表：

    <table>
    <th>Visual Studio 版本</td><th>动态链接</th><th>静态链接</th></th>
    <tr><td>2005 (VC 8)</td><td>不支持</td><td>支持</td>
    <tr><td>2008 (VC 9)</td><td>不支持</td><td>支持</td>
    <tr><td>2010 (VC 10)</td><td>支持</td><td>支持</td>
    <tr><td>2012 (VC 11)</td><td>支持</td><td>不支持</td>
    <tr><td>2013 (VC 12)</td><td>支持</td><td>不支持</td>
    <tr><td>2015 和 2017 (VC 14)</td><td>支持</td><td>支持</td>
    </table>

    注意：必须在所有情况下，链接到最新公开发布 CRT。

+ __你的应用程序安装并从 Windows 的并行文件夹加载程序集__。 例如，你的应用程序使用 C 运行时库 VC8 或 vc9 版本且和动态从 Windows 的并行文件夹中，这意味着你的代码使用常用 DLL 文件从共享文件夹链接它们。 这不受支持。 你将需要静态链接它们，方法是将可再发行库文件直接链接到你的代码中。

+ __你的应用程序使用 System32/SysWOW64 文件夹中的依赖项__。 若要使这些 DLL 有效，必须将其包含在 Windows 应用包的虚拟文件系统部分中。 这可确保应用程序行为就像这些 Dll 安装在**System32**/**SysWOW64**文件夹。 在程序包的根目录中，创建一个名为 **VFS** 的文件夹。 在该文件夹内创建 **SystemX64** 和 **SystemX86** 文件夹。 然后，将 DLL 的 32 位版本放置在 **SystemX86** 文件夹，并将 64 位版本放置在 **SystemX64** 文件夹。

+ __你的应用使用 VCLibs 框架程序包__。 如果 VCLibs 库被定义为 Windows 应用包中的依赖项，则可以直接从 Microsoft Store 中安装。 例如，如果你的应用程序使用 Dev11 VCLibs 包，请到应用程序的包清单中进行以下更改：下`<Dependencies>`节点，添加：  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
在从 Microsoft Store 安装期间，将在安装应用之前先安装 VCLibs 框架的适当版本（x86 或 x64）。  
如果安装了旁加载应用程序将无法安装依赖项。 若要在计算机上手动安装这些依赖项，必须下载并安装桌面桥的相应 VCLibs 框架包。 有关这些方案的详细信息，请参阅 [在 Centennial 项目中使用 Visual C++ 运行时](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/)。

  **框架包**：

  * [桌面桥的 VC 14.0 框架包](https://www.microsoft.com/download/details.aspx?id=53175)
  * [桌面桥的 VC 12.0 framework 包](https://www.microsoft.com/download/details.aspx?id=53176)
  * [桌面桥的 VC 11.0 framework 包](https://www.microsoft.com/download/details.aspx?id=53340)


+ __你的应用程序包含自定义的跳转列表__。 使用跳转列表时需要注意几个问题和注意事项。

    - __应用程序的体系结构与 OS 不匹配。__  跳转列表目前不能正常如果应用程序和操作系统体系结构不匹配 (例如，x86 应用程序运行在 x64 上 Windows)。 在此期间，没有解决方法而不需重新编译应用程序以便对匹配的体系结构。

    - __应用程序创建跳转列表项和调用[ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx)或[SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__。 不要在代码中以编程方式设置你的 AppID。 否则，将会导致不显示你的跳转列表条目。 如果你的应用程序需要自定义 Id，指定它使用清单文件。 请参阅[手动打包桌面应用程序](desktop-to-uwp-manual-conversion.md)有关的说明。 应用程序的 AppID 在 *YOUR_PRAID_HERE* 部分中指定。

    - __你的应用程序中添加引用你的包中的可执行文件的跳转列表 shell 链接__。 你不能通过跳转列表直接启动程序包中的可执行文件（应用自身 .exe 的绝对路径除外）。 相反，注册应用程序执行别名 （它允许您打包的桌面应用程序，就好像它是在路径上通过一个关键字开始） 和设置链接目标路径的别名而不是。 有关如何使用 appExecutionAlias 扩展的详细信息，请参阅[桌面应用程序集成，使用 Windows 10](desktop-to-uwp-extensions.md)。 注意，如果要求跳转列表中的链接资源与原始 .exe 匹配，你将需要使用 [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) 和 PKEY_Title 的显示名称设置图标等资源，就像设置其他自定义条目那样。

    - __你的应用程序添加的绝对路径引用应用程序的包中的资产跳转列表项__。 其包已更新，更改资产 （如图标、 文档、 可执行文件，等等） 的位置时，可能会更改应用程序的安装路径。 如果跳转列表项的绝对路径，引用此类资产，则应用程序应刷新其跳转列表会定期 （例如，在应用程序上启动） 以确保路径正确解析。 或者，改用 UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx) API，使你可以使用程序包相对的 ms-resource URI 方案（也与语言、DPI 和高对比度相关）来引用字符串和图像资源。

+ __启动应用程序用于执行任务的实用工具__。 避免启动 PowerShell 和 Cmd.exe 等命令实用工具。 事实上，如果用户安装应用程序转移到运行 Windows 10 S 的系统，然后你的应用程序将无法在所有启动它们。 这可以阻止从提交到 Microsoft Store 应用程序，因为所有提交到 Microsoft Store 的应用必须符合 Windows 10 s。

启动实用工具通常可以提供一种方便的方法，用于从操作系统获取信息、访问注册表或访问系统功能。 但是，你可以改为使用 UWP API 完成这些类型的任务。 这些 Api 存在更高的性能，因为它们不需要单独的可执行文件运行，但更重要的是，他们将保持到达软件包外部的应用程序。 应用程序的设计保持一致使用隔离、 信任和附带的已打包和你的应用程序会像预期在运行 Windows 10 s。 系统上的应用程序的安全

+ __你的应用程序承载外接程序、 插件或扩展__。   在许多情况下，只要尚未对扩展打包并且该扩展以完全信任方式安装，COM 样式的扩展则可能会继续工作。 这是因为这些安装程序可以使用其完全信任功能来修改注册表，并将扩展文件，只要宿主应用程序期望找到它们。

   但是，如果这些扩展都是打包，，然后安装为 Windows 应用包，它们不会工作因为每个包 （主机应用程序和扩展） 将是相互隔离的。 若要详细了解如何应用程序是独立于系统，请参阅[桌面桥在后台](desktop-to-uwp-behind-the-scenes.md)。

 用户安装到运行 Windows 10 S 的系统上的所有应用程序和扩展都必须作为 Windows 应用包安装。 因此，如果你想要打包你的扩展，或你打算鼓励您的参与者以将它们打包，考虑如何可能会促进主机应用程序包与任何扩展包之间的通信。 能够做到这点的一种方法是使用[应用服务](../launch-resume/app-services.md)。

+ __你的应用程序生成代码__。 你的应用程序可以在内存中，生成该服务要消耗的代码但避免因为 Windows 应用程序认证过程中无法验证该代码，然后再应用提交到磁盘的编写生成代码。 此外，将代码写入到磁盘的应用程序不能正常运行在运行 Windows 10 s。 系统上这可以阻止从提交到 Microsoft Store 应用程序，因为所有提交到 Microsoft Store 的应用必须符合 Windows 10 s。

>[!IMPORTANT]
> 创建 Windows 应用包后，请测试你的应用程序以确保它正常运行系统上运行 Windows 10 s。必须符合所有提交到 Microsoft Store 的应用不兼容的 Windows 10 s。 应用不会接受存储区中。 请参阅[测试适用于 Windows 10 S 的 Windows 应用](desktop-to-uwp-test-windows-s.md)。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**创建桌面应用程序的 Windows 应用包**

请参阅[创建 Windows 应用包](desktop-to-uwp-root.md#convert)。
