---
author: awkoren
Description: "使用桌面转换扩展使 Windows 桌面应用程序（如 Win32、WPF 和 Windows 窗体）为转换为通用 Windows 平台 (UWP) 应用做好准备。"
Search.Product: eADQiWindows 10XVcnh
title: "将桌面应用程序转换为通用 Windows 平台 (UWP) 应用"
ms.sourcegitcommit: 37d4c8ef6178fe78c7333c47848177b5185c6d61
ms.openlocfilehash: 16d75ebd2499e56af742c1c9994fd92cc320c22b

---

# 将桌面应用程序转换为通用 Windows 平台 (UWP) 应用

\[有些信息与可能在商业发行之前就经过实质性修改的预发布产品相关。 Microsoft 不对此处提供的信息作任何明示或默示的担保。\]

使用桌面转换扩展使 Windows 桌面应用程序（如 Win32、WPF 和 Windows 窗体）为转换为通用 Windows 平台 (UWP) 应用做好准备。

## 将应用程序转换为 UWP 的好处

使用桌面转换扩展的 UWP 是一个桥，使你能够将经典桌面应用程序（如 Win32、Windows 窗体和 WPF）或游戏转换为通用 Windows 平台 (UWP) 应用或游戏。 有关详细信息，请参阅 [UWP 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx)。 转换后，将以面向 Windows 10 桌面版的 UWP 应用包（.appx 或 .appxbundle）的形式打包、维护和部署经典桌面应用。

支持桌面应用转换为 UWP 程序包的技术有两个部分。 第一个部分是桌面应用转换器，可将你的现有二进制文件重新打包为 UWP 程序包。 你的代码仍然相同，只是打包方式不同。 第二个部分由 Windows 周年更新中的运行时技术组成，这些技术使 UWP 程序包可以具有作为完全信任（而不是应用容器）运行的可执行文件。 此技术还为转换的应用提供程序包标识，该标识需要使用某些 UWP API。

以下是转换经典桌面应用的一些好处。

* 对于你的客户来说，你的应用的安装体验更流畅了。 你可以使用旁加载将其部署到计算机（请参阅[在 Windows 10 中旁加载 LOB 应用](https://technet.microsoft.com/library/mt269549.aspx)），它在卸载后不会留下任何痕迹。 从长远来看，你还能够将应用发布到 Windows 应用商店。

* 由于转换的应用具有程序包标识，因此你甚至可以从完全信任分区调用比以前更多的 UWP API。

* 你可以按照自己的节奏向应用的程序包添加 UWP 功能，如 XAML 用户界面、动态磁贴更新、UWP 后台任务、应用服务以及许多其他功能。 适用于任何其他 UWP 应用的所有功能都适用于你的应用。

* 如果你选择将应用的所有功能移出应用的完全信任分区并移到应用容器分区中，则你的应用将能够在任何 Windows 10 设备上运行。

* 作为 UWP 应用，你的应用能够执行它作为经典桌面应用时可以执行的操作。 它与注册表和文件系统的虚拟化视图交互，该视图与实际的注册表和文件系统之间无法区分。

* 你的应用能够参与 Windows 应用商店的内置许可和自动更新设施。 自动更新是高度可靠和高效的机制，因为仅下载文件的已更改部分。

## 使桌面应用为转换为 UWP 做好准备
你可能不需要执行很多操作即可使应用为转换过程做好准备。 请记住，Windows 应用商店为你处理许可和自动更新，以便你可以从基本代码中删除这些功能。 如果以下任意情况适用于你的应用程序，你需要在转换前解决此问题。

+ __你的应用使用早于 4.6.1 的 .NET 版本__。 仅 .NET 4.6.1 受支持。 在转换前，你必须将应用重定向到 .NET 4.6.1。 

+ __应用始终使用提升的安全权限运行__。 你的应用需要在以交互用户身份运行时工作。 从 Windows 应用商店安装应用的用户可能不是系统管理员，因此需要应用以提升的权限运行意味着它无法为标准用户正确运行。

+ __应用需要内核模式驱动程序或 Windows 服务__。 该桥适用于应用，但不支持内核模式驱动程序或需要在系统帐户下运行的 Windows 服务。 使用[后台任务](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)，而不是 Windows 服务。

+ __应用的模块在进程中加载到不在 AppX 程序包中的进程__。 不允许此操作，这意味着不支持进程中扩展，如 [shell 扩展](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx)。 但是，如果你在同一个程序包中有两个应用，则可以在它们之间执行进程间通信。

+ __应用使用自定义应用程序用户模型 ID (AUMID)__。 如果进程调用 [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) 以设置其自己的 AUMID，则它可能仅使用应用模型环境/AppX 程序包为其生成的 AUMID。 无法定义自定义 AUMID。

+ __应用修改 HKEY_LOCAL_MACHINE (HKLM) 注册表配置单元__。 应用的任何创建 HKLM 键或打开一个键以进行修改的尝试都会导致拒绝访问失败。 请记住，应用具有其自己的注册表专用虚拟化视图，因此用户范围和计算机范围的注册表配置单元（即 HKLM 的本质）的想法不适用。 你将需要找到另一种方法来实现 HKLM 的用途，如改为写入 HKEY_CURRENT_USER (HKCU)。

+ __应用使用 ddeexec 注册表子项作为启动另一个应用的方式__。 改为使用[应用程序包清单](https://msdn.microsoft.com/library/windows/apps/br211474.aspx)中的各种可激活*扩展配置的 DelegateExecute 谓词处理程序之一。

+ __应用写入 AppData 文件夹，目的是与其他应用共享数据__。 转换后，AppData 将重定向到本地应用数据存储，该存储是每个 UWP 应用的专用应用商店。 使用不同的进程间数据共享方式。 有关详细信息，请参阅[存储和检索设置以及其他应用数据](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)。

+ __应用写入应用的安装目录__。 例如，应用写入与你的 exe 放置在同一个目录中的日志文件。 此操作不受支持，因此你需要找到另一个位置，如本地应用数据存储。

+ __应用安装需要用户交互__。 应用安装程序必须能够在无提示的情况下运行，并且它必须安装默认不在干净操作系统映像中的所有先决条件。

+ __应用使用当前工作目录__。 在运行时，转换的应用不会得到你之前在桌面 .LNK 快捷方式中指定的相同工作目录。 如果具有正确的目录对应用正常运行很重要，你需要在运行时更改 CWD。

+ __应用需要 UIAccess__。 如果应用程序在 UAC 清单的 `requestedExecutionLevel` 元素中指定 `UIAccess=true`，则当前不支持转换为 UWP。 有关详细信息，请参阅 [UI 自动化安全概述](https://msdn.microsoft.com/library/ms742884.aspx)。

+ __你的应用公开 COM 对象或 GAC 程序集以供其他进程使用__。 在当前版本中，你的应用无法公开 COM 对象或 GAC 程序集以供来自 AppX 程序包外部的可执行文件的进程使用。 来自程序包内部的进程可以照常注册和使用 COM 对象和 GAC 程序集，但它们在外部不可见。 这意味着，OLE 等互操作方案在被外部进程调用时不起作用。 

+ __你的应用正以不受支持的方式链接 C 运行时库__。 Microsoft C/C++ 运行时库提供用于为 Microsoft Windows 操作系统编程的例程。 这些例程自动执行许多不采用 C 和 C++ 语言提供的常见编程任务。 如果你的应用利用 C/C++ 运行时库，你需要确保它以受支持的方式链接。 
    
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

+ __你的应用从 Windows 并行文件夹安装和加载程序集。__ 例如，你的应用使用 C 运行时库 VC8 或 VC9，并且正在从 Windows 并行文件夹动态链接它们，这意味着你的代码正在使用来自共享文件夹的常见 DLL 文件。 这不受支持。 你将需要静态链接它们，方式是将可再发行库文件直接链接到你的代码中。


## 本节内容

| 主题 | 描述 |
|-------|-------------|
| [桌面应用转换器预览 (Project Centennial)](desktop-to-uwp-run-desktop-app-converter.md) | 介绍如何运行桌面应用转换器。 你可能不需要执行很多操作（如果有）即可使应用为转换过程做好准备。 |
| [将 Windows 桌面应用程序手动转换为通用 Windows 平台 (UWP) 应用](desktop-to-uwp-manual-conversion.md) | 了解如何手动创建应用包和清单。 |
| [部署和调试转换的 UWP 应用](desktop-to-uwp-deploy-and-debug.md) | 包含有助于你在转换后成功部署和调试应用的信息。 此外，如果你对某些桌面转换扩展的内部组件感兴趣，则本主题适合你。 |
| [UWP 代码示例的桌面应用桥](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | GitHub 上演示已转换应用的功能的代码示例。 |



<!--HONumber=Jun16_HO5-->


