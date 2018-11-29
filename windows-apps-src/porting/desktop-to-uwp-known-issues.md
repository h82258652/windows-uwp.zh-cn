---
Description: This article contains known issues with the Desktop Bridge.
Search.Product: eADQiWindows 10XVcnh
title: 已知问题（桌面桥）
ms.date: 06/20/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.localizationpriority: medium
ms.openlocfilehash: d56482ee036eaadbd759de9af22fdd10c652aceb
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/28/2018
ms.locfileid: "7973521"
---
# <a name="known-issues-with-packaged-desktop-applications"></a>已打包的桌面应用程序的已知的问题

本文包含桌面应用程序创建 Windows 应用包时可能发生的已知的问题。

<a id="app-converter" />

## <a name="known-issues-with-the-desktop-app-converter"></a>Desktop App Converter 的已知问题

### <a name="ecreatingisolatedenvfailed-an-estartingisolatedenvfailed-errors"></a>E_CREATING_ISOLATED_ENV_FAILED 和 E_STARTING_ISOLATED_ENV_FAILED 错误    

如果收到任一错误，请确保你正在使用的是从[下载中心](https://aka.ms/converterimages)下载的有效基础映像。
如果使用的是有效的基础映像，请尝试在命令中使用 ``-Cleanup All``。
如果不起作用，请将日志发送至 converter@microsoft.com，以帮助我们进行调查。

### <a name="new-containernetwork-the-object-already-exists-error"></a>New-ContainerNetwork：对象已存在错误

设置新的基础映像时，可能会收到此错误。 如果你在以前安装了 Desktop App Converter 的开发人员计算机上安装有 Windows 预览体验计划外部测试版，则可能会出现这种情况。

若要解决此问题，请尝试从提升的命令提示符中运行命令 `Netsh int ipv4 reset`，然后重启计算机。

### <a name="your-net-application-is-compiled-with-the-anycpu-build-option-and-fails-to-install"></a>你的.NET 应用程序使用"AnyCPU"生成选项编译，并且无法安装

如果将主要可执行文件或任何依赖项放置在 **Program Files** 或 **Windows\System32** 文件夹层次结构中的任意位置，则可能会出现这种情况。

若要解决此问题，请尝试使用你体系结构特定的桌面安装程序（32 位或 64 位）生成 Windows 应用包。

### <a name="publishing-public-side-by-side-fusion-assemblies-wont-work"></a>发布公共并行的 Fusion 程序集不起作用。

 在安装期间，应用程序可以发布公共并行的 Fusion 程序集，这些程序集可供任何其他进程访问。 在进程激活上下文创建期间，这些程序集将由名为 CSRSS.exe 的系统进程检索。 当为已转换的进程执行此操作后，这些程序集的激活上下文创建和模块加载将失败。 并行的 Fusion 程序集注册在以下位置中：
  + 注册表： `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\SideBySide\Winners`
  + 文件系统：%windir%\\SideBySide

这是一个已知限制，目前尚无解决方法。 即，内置程序集（如 ComCtl）随操作系统附带，因此依赖它们是安全的行为。

### <a name="error-found-in-xml-the-executable-attribute-is-invalid---the-value-myappexe-is-invalid-according-to-its-datatype"></a>XML 中发现错误。 “Executable”特性无效 - 根据其数据类型，值“MyApp.EXE”无效

如果应用程序中的可执行文件具有大写的 **.EXE** 扩展名，则可能会出现这种情况。 虽然此扩展名的大小写不应影响，无论你的应用程序运行，这可能导致 DAC 生成此错误。

若要解决此问题，请尝试在打包时指定 **-AppExecutable** 标志，并使用小写的“.exe”作为主要可执行文件的扩展名（例如 MYAPP.exe）。    或者你可以更改为大写小写应用程序中的所有可执行文件的大小写 (例如： 从。EXE.exe)。

### <a name="corrupted-or-malformed-authenticode-signatures"></a>已损坏或格式不正确的验证码签名

本部分包含有关如何在可能包含已损坏或格式不正确的验证码签名的 Windows 应用包中标识可移植可执行 (PE) 文件的问题。 PE 文件中的无效验证码签名可能采用任何二进制格式（如 .exe、.dll、.chm 等），并且会阻止程序包正确签名，从而使其无法从 Windows 应用包部署。

PE 文件的验证码签名的位置由可选头数据目录中的证书表项和关联的属性证书表指定。 在签名验证期间，这些结构中指定的信息用于找到 PE 文件上的签名。 如果这些值损坏，则文件可能看起来无法有效进行签名。

若要使验证码签名正确，验证码签名必须符合以下情况：

- PE 文件中 **WIN_CERTIFICATE** 项的开头不得超过可执行文件的末尾
- 
            **WIN_CERTIFCATE** 项应位于映像的末尾
- 
            **WIN_CERTIFICATE** 项的大小必须为正数
- 对于 32 位可执行文件，**WIN_CERTIFICATE** 项必须在 **IMAGE_NT_HEADERS32** 结构之后开始，对于 64 位可执行文件，必须在 IMAGE_NT_HEADERS64 结构之后开始

有关更多详细信息，请参考[验证码门户可执行文件规范](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx)和 [PE 文件格式规范](https://msdn.microsoft.com/windows/hardware/gg463119.aspx)。

请注意，当尝试对 Windows 应用包进行签名时，SignTool.exe 可能会输出已损坏或格式不正确的二进制文件列表。 若要执行此操作，请通过将环境变量 APPXSIP_LOG 设置为 1（如 ```set APPXSIP_LOG=1```）来启用详细日志记录并重新运行 SignTool.exe。

若要修复这些格式不正确的二进制文件，请确保它们符合上述要求。

## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>你收到错误 MSB4018“GenerateResource”任务意外失败

在尝试将卫星集转换为包资源索引 (PRI) 文件时，会发生这种情况。

我们已注意到此问题，正在努力制定长期解决方案。 作为临时解决方法，你可以通过将此行 XML 添加到托管项目文件的第一个 PropertyGroup 元素中来禁用资源生成器：

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>带有错误代码 0x139 (KERNEL_SECURITY_CHECK_FAILURE) 的蓝屏

在从 Microsoft Store 安装或启动某些应用后，计算机可能由于以下错误而意外重新启动：**0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)**。

已知受影响的应用包括 Kodi、JT2Go、Ear Trumpet、Teslagrad 和其他应用。

[Windows 更新（版本 14393.351 - KB3197954）](https://support.microsoft.com/kb/3197954)于 2016 年 10 月 27 日发布，该更新包含解决此问题的重要修补程序。 如果你遇到此问题，请更新计算机。 如果由于计算机在你可以登录前便重新启动，从而无法更新电脑，应使用系统还原将系统恢复到早于你安装受影响应用之一时的时间点。 有关如何使用系统还原的信息，请参阅 [Windows10 中的恢复选项](https://support.microsoft.com/help/12415/windows-10-recovery-options)。

如果更新未解决问题或者你不确定如何恢复电脑，请联系 [Microsoft 支持](https://support.microsoft.com/contactus/)。

如果是开发人员，可能需要阻止在不包含此更新的 Windows 版本上安装打包应用程序。 请注意，通过执行此操作，你的应用程序不会提供给尚未安装更新的用户。 若要限制用户已安装此更新的应用程序的可用性，请修改 AppxManifest.xml 文件，如下所示：

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

可在以下位置找到关于 Windows 更新的详细信息：
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>对应用进行签名时可能会出现的常见错误

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>发布者和证书不匹配导致 Signtool 错误“错误: SignerSign() 失败”(-2147024885/0x8007000b)

Windows 应用包清单中的发布者条目必须与要用于签名的证书的使用者匹配。  可使用以下任一方法查看证书的使用者。

**选项 1：Powershell**

运行以下 PowerShell 命令。 可以将 .cer 或 .pfx 用作证书文件，因为它们具有相同的发布者信息。

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**选项 2：文件资源管理器**

在文件资源管理器中双击证书、选择*详细信息*选项卡，然后在列表中选择*使用者*字段。 接着，就可以复制内容。

**选项 3：CertUtil**

从命令行运行**certutil** ，对 PFX 文件，并从输出中复制*使用者*字段。

```cmd
certutil -dump <cert_file.pfx>
```

<a id="bad-pe-cert" />

### <a name="bad-pe-certificate-0x800700c1"></a>损坏的 PE 证书 (0x800700C1)

当你的程序包包含已损坏的证书的二进制文件时，会出现此错误。 下面是一些会出现此错误原因的原因：

* 证书的开始菜单不可末尾的图像。  

* 证书的大小不积极。

* 证书开始菜单未在`IMAGE_NT_HEADERS32`结构为一个 32 位可执行文件或之后`IMAGE_NT_HEADERS64`结构的 64 位可执行文件。

* 证书指针未正确对齐 WIN_CERTIFICATE 结构。

若要查找包含损坏的 PE 证书文件，打开**命令提示符**，并设置名为环境变量`APPXSIP_LOG`为 1 的值。

```
set APPXSIP_LOG=1
```

然后，从**命令提示符下**，登录一次，你的应用程序。 例如：

```
signtool.exe sign /a /v /fd SHA256 /f APPX_TEST_0.pfx C:\Users\Contoso\Desktop\pe\VLC.appx
```

包含损坏的 PE 证书的文件的信息将显示在**控制台窗口**中。 例如：

```
...

ERROR: [AppxSipCustomLoggerCallback] File has malformed certificate: uninstall.exe

...   
```
## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。
