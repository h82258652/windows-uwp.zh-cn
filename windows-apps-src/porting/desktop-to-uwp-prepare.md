---
author: normesta
Description: This article lists things you need to know before packaging your desktop application. You may not need to do much to get your app ready for the packaging process.
Search.Product: eADQiWindows 10XVcnh
title: 准备打包的桌面应用程序 （桌面桥）
ms.author: normesta
ms.date: 05/18/20188
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 3a0b3a9f5ce7c03b8add9cc459bade684b9daf21
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5886682"
---
# <a name="prepare-to-package-a-desktop-application"></a>准备打包的桌面应用程序

本文列出了在对桌面应用打包前你需要知道的事项。 你可能不需要执行很多准备你的应用程序打包过程，但如果任一以下项适用于你的应用程序，则需要进行打包前解决该问题。 请记住，Microsoft Store 会为你处理授权和自动更新，使你能够从基本代码中删除与这些任务相关的任何功能。

>[!IMPORTANT]
>创建 Windows 应用包的桌面应用程序的功能 （Windows 10 版本 1607年中引入了称为桌面桥，否则，它仅用于在项目中面向 Windows 10 周年更新 (10.0;内部版本 14393） 或更高版本的 Visual Studio。

+ __你的应用程序需要.NET 早于 4.6.2 的版本__。 你需要确保你的应用程序在.NET 4.6.2 上运行。 你不能需要或重新分发早于 4.6.2 的版本。 这是 Windows 10 周年更新中提供的 .NET 版本。 确认你的应用程序可在此版本中工作将确保你的应用程序将继续与未来的 Windows 10 更新兼容。  如果你的应用程序面向.NET Framework 4.0 或更高版本，它预期在.NET 4.6.2 上运行，但你仍应进行测试。

+ __你的应用程序始终使用提升的安全权限运行__。 你的应用程序需要以交互用户身份运行时工作。 从 Microsoft Store 安装应用程序用户可能不是系统管理员，因此需要你的应用程序运行提升的意味着它无法为标准用户运行正常。 Microsoft Store 中不接受任何功能部分需要提升的应用。

+ __你的应用程序需要在内核模式驱动程序或 Windows 服务__。 该桥适用于应用，但不支持内核模式驱动程序或需要在系统帐户下运行的 Windows 服务。 使用[后台任务](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)，而不是 Windows 服务。

+ __在进程内将应用的模块加载到不在 Windows 应用包中的进程__。 不允许此操作，这意味着不支持进程中扩展，如 [shell 扩展](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)。 但是，如果你在同一个程序包中有两个应用，则可以在它们之间执行进程间通信。

+ __你的应用程序使用自定义应用程序用户模型 ID (AUMID)__。 如果进程调用[SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx)以设置其自己的 AUMID，则它仅可以使用应用模型环境 /windows 应用程序包为其生成的 AUMID。 无法定义自定义 AUMID。

+ __你的应用程序修改 HKEY_LOCAL_MACHINE (HKLM) 注册表配置单元__。 任何你的应用程序创建 HKLM 键，或以尝试打开另一个用于修改，将导致拒绝访问失败。 请记住你的应用程序具有其自己的注册表的专用虚拟化的视图，因此用户和计算机范围注册表配置单元 （即 HKLM 的新增功能） 的概念不适用。 你将需要找到另一种方法来实现 HKLM 的用途，如改为写入 HKEY_CURRENT_USER (HKCU)。

+ __你的应用程序使用 ddeexec 注册表子项作为启动另一个应用一种__。 改为使用[应用程序包清单](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)中的各种可激活*扩展配置的 DelegateExecute 谓词处理程序之一。

+ __你的应用程序写入 AppData 文件夹或注册表，目的是共享数据与其他应用__。 转换后，AppData 将重定向到本地应用数据存储，该存储是每个 UWP 应用的专用应用商店。

  你的应用程序写入 HKEY_LOCAL_MACHINE 注册表配置单元的所有条目重都定向到隔离的二进制文件和应用程序写入 HKEY_CURRENT_USER 注册表配置单元的任何条目都放入专用每用户、 每应用位置。 有关文件和注册表重定向的更多详细信息，请参阅[在桌面桥幕后](desktop-to-uwp-behind-the-scenes.md)。  

  使用不同的进程间数据共享方式。 有关详细信息，请参阅[存储和检索设置以及其他应用数据](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

+ __你的应用程序写入到你的应用的安装目录__。 例如，你的应用程序写入到日志文件放置在与你的 exe 的同一目录中。 此操作不受支持，因此你需要找到另一个位置，如本地应用数据存储。

+ __你的应用程序安装需要用户交互__。 应用程序安装程序必须能够采用静默方式运行，并且它必须安装所有不在默认情况下上一个全新的操作系统映像, 及其先决条件。

+ __你的应用程序使用当前工作目录__。 在运行时，你已打包的桌面应用程序不会得到你之前在桌面中指定的相同工作目录。LNK 快捷方式。 你需要更改 CWD 在运行时，如果具有正确的目录很重要的应用程序才能正常工作。

+ __你的应用程序需要 UIAccess__。 如果应用程序在 UAC 清单的 `requestedExecutionLevel` 元素中指定 `UIAccess=true`，则当前不支持转换为 UWP。 有关详细信息，请参阅 [UI 自动化安全概述](https://msdn.microsoft.com/library/ms742884.aspx)。

+ __你的应用程序公开 COM 对象__。 来自程序包内的进程和扩展可以注册并使用 COM 和 OLE 服务器，进程内和进程外 (OOP) 皆可。   创意者更新添加了打包的 COM 支持，它提供注册 OOP COM 和 OLE 服务器（现在这些服务器在包外部可见）的功能。  请参阅[对桌面桥的 COM 服务器和 OLE 文档支持](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97)。

   打包的 COM 支持适用于现有的 COM API，但不适用于依赖直接读取注册表的应用程序扩展，因为打包的 COM 位于一个专用位置。

+ __你的应用程序公开 GAC 程序集以供其他进程使用__。 在当前版本中，你的应用程序无法供来自 Windows 应用包外部的可执行文件的进程公开 GAC 程序集以供使用。 来自程序包内的进程可以照常注册和使用 GAC 程序集，但它们在外部将不可见。 这意味着，OLE 等互操作方案在被外部进程调用时不起作用。

+ __你的应用程序正在链接 C 运行时库 (CRT) 不受支持的方式__。 Microsoft C/C++ 运行时库提供用于为 Microsoft Windows 操作系统编程的例程。 这些例程自动执行许多不采用 C 和 C++ 语言提供的常见编程任务。 如果你的应用程序利用 C/c + + 运行时库，你需要确保它以受支持的方式链接。

    对于最新版本的 CRT，Visual Studio 2017 即支持静态和动态链接，以允许你的代码使用常见 DLL 文件，也支持静态链接，以将库直接链接到你的代码。 如果可能，我们建议你的应用程序将动态链接与 VS 2017 一起使用。

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

    注意：在所有情况下，你必须链接到最新公开提供的 CRT。

+ __你的应用程序安装和加载程序集以从 Windows 并行的文件夹__。 例如，你的应用程序使用 C 运行时库 VC8 或 VC9，并且正在动态链接它们从 Windows 并行的文件夹，这意味着你的代码正在使用来自共享文件夹的常见 DLL 文件。 这不受支持。 你将需要静态链接它们，方法是将可再发行库文件直接链接到你的代码中。

+ __你的应用程序使用 System32/SysWOW64 文件夹中的依赖项__。 若要使这些 DLL 有效，必须将其包含在 Windows 应用包的虚拟文件系统部分中。 这可确保应用程序的行为就像 Dll 已安装在**System32**/**SysWOW64**文件夹。 在程序包的根目录中，创建一个名为 **VFS** 的文件夹。 在该文件夹内创建 **SystemX64** 和 **SystemX86** 文件夹。 然后，将 DLL 的 32 位版本放置在 **SystemX86** 文件夹，并将 64 位版本放置在 **SystemX64** 文件夹。

+ __你的应用使用 VCLibs 框架程序包__。 如果 VCLibs 库被定义为 Windows 应用包中的依赖项，则可以直接从 Microsoft Store 中安装。 例如，如果你的应用程序使用 Dev11 VCLibs 程序包，使到你的应用程序的程序包清单的以下更改： 在`<Dependencies>`节点中，添加：  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
在从 Microsoft Store 安装期间，将在安装应用之前先安装 VCLibs 框架的适当版本（x86 或 x64）。  
如果通过旁加载安装应用程序将不安装这些依赖项。 若要在计算机上手动安装这些依赖项，必须下载并安装桌面桥的相应 VCLibs 框架包。 有关这些方案的详细信息，请参阅 [在 Centennial 项目中使用 Visual C++ 运行时](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/)。

  **框架包**：

  * [适用于桌面桥的 VC 14.0 框架包](https://www.microsoft.com/download/details.aspx?id=53175)
  * [适用于桌面桥的 VC 12.0 框架包](https://www.microsoft.com/download/details.aspx?id=53176)
  * [适用于桌面桥的 VC 11.0 框架包](https://www.microsoft.com/download/details.aspx?id=53340)


+ __你的应用程序包含自定义跳转列表__。 使用跳转列表时需要注意几个问题和注意事项。

    - __你的应用的体系结构与操作系统不匹配。__  跳转列表目前不能正常的应用程序和操作系统体系结构不匹配 (例如，x86 应用程序运行在 x64 Windows)。 此时，没有解决方法而不重新编译你的应用程序匹配体系结构。

    - __你的应用程序创建跳转列表条目，并调用[ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx)或[SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__。 不要在代码中以编程方式设置你的 AppID。 否则，将会导致不显示你的跳转列表条目。 如果你的应用程序需要自定义 Id，使用指定的清单文件。 有关说明，请参阅[手动打包的桌面应用程序](desktop-to-uwp-manual-conversion.md)。 应用程序的 AppID 在 *YOUR_PRAID_HERE* 部分中指定。

    - __你的应用程序添加跳转列表 shell 链接引用程序包中的可执行文件__。 你不能通过跳转列表直接启动程序包中的可执行文件（应用自身 .exe 的绝对路径除外）。 相反，注册应用执行别名 （这允许你已打包的桌面应用程序以通过关键字启动，就像它在该路径上一样），并改为设置到别名的链接目标路径。 有关如何使用 appExecutionAlias 扩展的详细信息，请参阅[Windows 10 的桌面应用程序集成](desktop-to-uwp-extensions.md)。 注意，如果要求跳转列表中的链接资源与原始 .exe 匹配，你将需要使用 [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) 和 PKEY_Title 的显示名称设置图标等资源，就像设置其他自定义条目那样。

    - __你的应用程序添加跳转列表条目通过绝对路径的应用的程序包中的资源引用__。 应用程序的安装路径可能会更改时应用的程序包更新，从而更改资源 （如图标、 文档、 可执行文件等） 的位置。 如果跳转列表条目通过绝对路径引用此类资源，则应用程序应定期刷新其跳转列表 （例如，应用程序启动） 以确保正确解析路径。 或者，改用 UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx) API，使你可以使用程序包相对的 ms-resource URI 方案（也与语言、DPI 和高对比度相关）来引用字符串和图像资源。

+ __你的应用程序启动一个实用工具以执行任务__。 避免启动 PowerShell 和 Cmd.exe 等命令实用工具。 事实上，如果用户安装到运行 Windows 10 S 的系统上的应用程序，然后你的应用程序将无法启动它们。 这可能阻止你无法提交至 Microsoft Store 的应用程序，因为提交到 Microsoft Store 的所有应用都必须与 Windows 10 S 兼容

启动实用工具通常可以提供一种方便的方法，用于从操作系统获取信息、访问注册表或访问系统功能。 但是，你可以改为使用 UWP API 完成这些类型的任务。 这些 Api 非常性能更佳，因为它们无需单独的可执行文件运行，但更重要的是，它们会一直到达程序包外部应用程序。 应用的设计保持一致隔离、 信任和附带的应用程序打包，并且你的应用程序将按预期在运行 Windows 10 s。 的系统上的安全

+ __你的应用程序主机的加载项、 插件或扩展__。   在许多情况下，只要尚未对扩展打包并且该扩展以完全信任方式安装，COM 样式的扩展则可能会继续工作。 这是因为那些安装程序可以使用其完全信任的功能修改注册表，并将扩展文件放置都会在主机应用程序能够找到它们。

   但是，如果这些扩展都打包在一起，并且然后安装为 Windows 应用包，它们不起作用，因为每个包 （主机应用程序和扩展） 将彼此相互隔离。 若要了解有关应用程序的方式从系统隔离，请参阅[桌面桥幕后](desktop-to-uwp-behind-the-scenes.md)。

 用户安装到运行 Windows 10 S 的系统上的所有应用程序和扩展都必须作为 Windows 应用包安装。 因此如果你想要对扩展打包，或者打算鼓励你的参与者对其打包，请考虑如何能够促进主机应用程序包和任意扩展包之间的通信。 能够做到这点的一种方法是使用[应用服务](../launch-resume/app-services.md)。

+ __你的应用程序生成代码__。 你的应用程序可以在内存中，生成它使用的代码，但避免将生成的代码写入磁盘，因为 Windows 应用认证过程无法验证该代码在应用提交之前。 此外，向磁盘写入代码的应用不能正常运行在系统运行 Windows 10 s。这可能阻止你无法提交至 Microsoft Store 的应用程序，因为提交到 Microsoft Store 的所有应用都必须与 Windows 10 S 兼容

>[!IMPORTANT]
> 创建 Windows 应用包后，请测试你的应用程序，以确保它正常系统上运行 Windows 10 s。提交到 Microsoft Store 的所有应用都必须符合应用商店中不接受不兼容的 Windows 10 s。 应用。 请参阅[测试适用于 Windows 10 S 的 Windows 应用](desktop-to-uwp-test-windows-s.md)。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**为桌面应用创建 Windows 应用包**

请参阅[创建 Windows 应用包](desktop-to-uwp-root.md#convert)。
