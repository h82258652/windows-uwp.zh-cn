---
author: awkoren
Description: "本文列出了在使用桌面到 UWP 桥转换应用前你需要知道的事项。 你可能不需要执行很多操作即可使应用为转换过程做好准备。"
Search.Product: eADQiWindows 10XVcnh
title: "为桌面到 UWP 桥准备应用"
translationtype: Human Translation
ms.sourcegitcommit: f7a8b8d586983f42fe108cd8935ef084eb108e35
ms.openlocfilehash: 81a2485d5be22dd392c21aaff281c1c9263883a9

---

# <a name="prepare-an-app-for-conversion-with-the-desktop-bridge"></a>准备应用以使用桌面桥转换

本文列出了在使用桌面到 UWP 桥转换应用前你需要知道的事项。 你可能不需要执行很多操作即可使应用为转换过程准备就绪，但是如果以下任意一项适用于你的应用程序，则你需要在转换前解决该问题。 请记住，Windows 应用商店为你处理许可和自动更新，以便你可以从基本代码中删除这些功能。

+ __你的应用使用早于 4.6.1 的 .NET 版本__。 仅 .NET 4.6.1 受支持。 在转换前，你必须将应用重定向到 .NET 4.6.1。 

+ __应用始终使用提升的安全权限运行__。 你的应用需要在以交互用户身份运行时工作。 从 Windows 应用商店安装应用的用户可能不是系统管理员，因此需要应用以提升的权限运行意味着它无法为标准用户正确运行。

+ __应用需要内核模式驱动程序或 Windows 服务__。 该桥适用于应用，但不支持内核模式驱动程序或需要在系统帐户下运行的 Windows 服务。 使用[后台任务](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)，而不是 Windows 服务。

+ __应用的模块在进程中加载到不在 AppX 程序包中的进程__。 不允许此操作，这意味着不支持进程中扩展，如 [shell 扩展](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)。 但是，如果你在同一个程序包中有两个应用，则可以在它们之间执行进程间通信。

+ __你的应用调用 [SetDllDirectory](https://msdn.microsoft.com/library/windows/desktop/ms686203) 或 [AddDllDirectory](https://msdn.microsoft.com/library/windows/desktop/hh310513)__。 这些函数当前不受已转换的应用支持。 我们正致力于在将来版本中添加支持。 作为解决方法，你可以使用这些函数将已找到的任何 .dll 复制到你的程序包根。 

+ __应用使用自定义应用程序用户模型 ID (AUMID)__。 如果进程调用 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) 以设置其自己的 AUMID，则它可能仅使用应用模型环境/AppX 程序包为其生成的 AUMID。 无法定义自定义 AUMID。

+ __应用修改 HKEY_LOCAL_MACHINE (HKLM) 注册表配置单元__。 应用的任何创建 HKLM 键或打开一个键以进行修改的尝试都会导致拒绝访问失败。 请记住，应用具有其自己的注册表专用虚拟化视图，因此用户范围和计算机范围的注册表配置单元（即 HKLM 的本质）的想法不适用。 你将需要找到另一种方法来实现 HKLM 的用途，如改为写入 HKEY_CURRENT_USER (HKCU)。

+ __应用使用 ddeexec 注册表子项作为启动另一个应用的方式__。 改为使用[应用程序包清单](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)中的各种可激活*扩展配置的 DelegateExecute 谓词处理程序之一。

+ __应用写入 AppData 文件夹，目的是与其他应用共享数据__。 转换后，AppData 将重定向到本地应用数据存储，该存储是每个 UWP 应用的专用应用商店。 使用不同的进程间数据共享方式。 有关详细信息，请参阅[存储和检索设置以及其他应用数据](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

+ __应用写入应用的安装目录__。 例如，应用写入与你的 exe 放置在同一个目录中的日志文件。 此操作不受支持，因此你需要找到另一个位置，如本地应用数据存储。

+ __应用安装需要用户交互__。 应用安装程序必须能够在无提示的情况下运行，并且它必须安装默认不在干净操作系统映像中的所有先决条件。

+ __应用使用当前工作目录__。 在运行时，转换的应用不会得到你之前在桌面 .LNK 快捷方式中指定的相同工作目录。 如果具有正确的目录对应用正常运行很重要，你需要在运行时更改 CWD。

+ __应用需要 UIAccess__。 如果应用程序在 UAC 清单的 `requestedExecutionLevel` 元素中指定 `UIAccess=true`，则当前不支持转换为 UWP。 有关详细信息，请参阅 [UI 自动化安全概述](https://msdn.microsoft.com/library/ms742884.aspx)。

+ __你的应用公开 COM 对象或 GAC 程序集以供其他进程使用__。 在当前版本中，你的应用无法公开 COM 对象或 GAC 程序集以供来自 AppX 程序包外部的可执行文件的进程使用。 来自程序包内部的进程可以照常注册和使用 COM 对象和 GAC 程序集，但它们在外部不可见。 这意味着，OLE 等互操作方案在被外部进程调用时不起作用。 

+ __你的应用正以不受支持的方式链接 C 运行时库 (CRT)__。 Microsoft C/C++ 运行时库提供用于为 Microsoft Windows 操作系统编程的例程。 这些例程自动执行许多不采用 C 和 C++ 语言提供的常见编程任务。 如果你的应用利用 C/C++ 运行时库，你需要确保它以受支持的方式链接。 
    
    对于最新版本的 CRT，Visual Studio 2015 即支持动态链接，以允许你的代码使用常见 DLL 文件，也支持静态链接，以将库直接链接到你的代码。 如果可能，我们建议你的应用将动态链接与 VS 2015 一起使用。 

    以前版本的 Visual Studio 中的支持各有不同。 有关详细信息，请参阅下表： 

    <table>
    <th>Visual Studio 版本</td><th>动态链接</th><th>静态链接</th></th>
    <tr><td>2005 (VC 8)</td><td>不支持</td><td>支持</td>
    <tr><td>2008 (VC 9)</td><td>不支持</td><td>支持</td>
    <tr><td>2010 (VC 10)</td><td>支持</td><td>支持</td>
    <tr><td>2012 (VC 11)</td><td>支持</td><td>不支持</td>
    <tr><td>2013 (VC 12)</td><td>支持</td><td>不支持</td>
    <tr><td>2015 (VC 14)</td><td>支持</td><td>支持</td>
    </table>
    
    注意：在所有情况下，你必须链接到最新公开提供的 CRT。

+ __你的应用从 Windows 并行文件夹安装和加载程序集。__ 例如，你的应用使用 C 运行时库 VC8 或 VC9，并且正在从 Windows 并行文件夹动态链接它们，这意味着你的代码正在使用来自共享文件夹的常见 DLL 文件。 这不受支持。 你将需要静态链接它们，方法是将可再发行库文件直接链接到你的代码中。

+ __你的应用使用 System32/SysWOW64 文件夹中的依赖项__。 若要使这些 DLL 有效，必须将其包含在 AppX 程序包的虚拟文件系统部分中。 这可确保应用的行为就像 DLL 已安装在 **System32**/**SysWOW64** 文件夹中一样。 在程序包的根目录中，创建一个名为 **VFS** 的文件夹。 在该文件夹内创建 **SystemX64** 和 **SystemX86** 文件夹。 然后，将 DLL 的 32 位版本放置在 **SystemX86** 文件夹，并将 64 位版本放置在 **SystemX64** 文件夹。

+ __你的应用使用 Dev11 VCLibs 框架程序包__。 如果 VCLibs 11 库被定义为 AppX 程序包中的依赖项，则可以直接从 Windows 应用商店中安装。 若要执行此操作，请对应用包清单进行以下更改：在 `<Dependencies>` 节点下，添加：  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
在从 Windows 应用商店安装期间，将在安装应用之前先安装 VCLibs 11 框架的适当版本（x86 或 x64）。  
如果通过旁加载安装应用，将不安装这些依赖项。 若要在计算机上手动安装这些依赖项，必须下载并安装[桌面应用桥的 VC 11.0 框架包](https://www.microsoft.com/download/details.aspx?id=53340&WT.mc_id=DX_MVP4025064)。 有关这些方案的详细信息，请参阅[在 Centennial 项目中使用 Visual C++ 运行时](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/)。


<!--HONumber=Dec16_HO1-->


