---
author: PatrickFarley
ms.assetid: 1526FF4B-9E68-458A-B002-0A5F3A9A81FD
title: Windows 应用认证工具包测试
description: Windows 应用认证工具包包含大量测试，可以帮助确保你的应用已准备好发布的 Microsoft 应用商店。
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10，uwp，应用认证
ms.localizationpriority: medium
ms.openlocfilehash: 49ecc472c8c1d4adebd8376fce9d2d5e6e2a955e
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2018
ms.locfileid: "3376805"
---
# <a name="windows-app-certification-kit-tests"></a>Windows 应用认证工具包测试


[Windows 应用认证工具包](windows-app-certification-kit.md)包含大量测试，帮助确保你的应用已准备好发布到 Microsoft 应用商店。 这些测试下面列出了自己的条件的详细信息，并建议在发生故障的操作。

## <a name="deployment-and-launch-tests"></a>部署和启动测试

在认证测试期间监视应用，记录它何时崩溃或停止响应。

### <a name="background"></a>背景

停止响应或崩溃的应用可能导致用户丢失数据和拥有糟糕的体验。

我们希望应用应该具有完整的功能，而无需使用 Windows 兼容模式、AppHelp 消息或兼容性修复程序。

应用不得列出要加载到 HKEY\-LOCAL\-MACHINE\\Software\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\AppInit\-DLLs 注册表项中的 DLL。

### <a name="test-details"></a>测试详细信息

在整个认证测试中，我们将测试应用的恢复能力和稳定性。

Windows 应用认证工具包调用 [**IApplicationActivationManager::ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903) 来启动应用。 若要使 **ActivateApplication** 启动应用，必须启用用户帐户控制 (UAC) 并且屏幕分辨率必须至少为 1024 x 768 或 768 x 1024。 如果不满足任何条件，那么你的应用将无法通过此测试。

### <a name="corrective-actions"></a>更正操作

确保在测试计算机上启用 UAC。

确保在屏幕足够大的计算机上运行测试。

如果你的应用无法启动并且你的测试平台满足 [**ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903) 的先决条件，那么你可以通过查看激活事件日志来解决此问题。 若要在事件日志中找到这些条目，请执行以下操作：

1.  打开 eventvwr.exe 并导航至“Application and Services Log\\Microsoft\\Windows\\Immersive-Shell”文件夹。
2.  筛选视图以显示事件 ID：5900-6000。
3.  查看日志条目，了解可能说明了应用为何未启动的信息。

排除文件的问题，识别并修复问题。 重新构建和重新测试应用。 你还可以检查 Windows App 认证工具包日志文件夹中是否已生成转储文件，该文件可用于调试你的应用。

## <a name="platform-version-launch-test"></a>平台版本启动测试

检查 Windows 应用是否能在未来版本的操作系统中运行。 此测试历来都仅适用于桌面应用工作流，但现在可适用于应用商店和通用 Windows 平台工作流。

### <a name="background"></a>背景

操作系统版本信息已限制的 Microsoft 应用商店的使用情况。 这常被应用误用于检查操作系统版本，使得该应用会向用户提供特定于操作系统版本的相关功能。

### <a name="test-details"></a>测试详细信息

Windows App 认证工具包使用 HighVersionLie 来检测应用检查操作系统版本的方式。 如果应用出现崩溃，它将无法通过此测试。

### <a name="corrective-action"></a>更正操作

应用应使用版本 API 帮助程序函数来检查此版本。 有关详细信息，请参阅[操作系统版本](https://msdn.microsoft.com/library/windows/desktop/ms724832)。

## <a name="background-tasks-cancellation-handler-validation"></a>后台任务取消处理程序验证

这将验证应用是否具有一个可用于已声明后台任务的取消处理程序。 在该任务被取消后，将需要一个用于调用的专用函数。 此测试仅适用于已部署的应用。

### <a name="background"></a>后台

Windows 应用可注册一个在后台运行的进程。 例如，电子邮件应用有时可能会对服务器执行 ping 操作。 但是，如果操作系统需要这些资源，它将取消该后台任务，并且应用应当能正常处理此取消操作。 不具有取消处理程序的应用可能会出现崩溃，或者在用户试图关闭应用时无法将其关闭。

### <a name="test-details"></a>测试详细信息

应用启动后，其中的已暂停和非后台部分将被终止。 随后，将取消与此应用关联的后台任务。 检查应用的状态，如果它仍在运行，将无法通过此测试。

### <a name="corrective-action"></a>更正操作

将取消处理程序添加到你的应用。 有关详细信息，请参阅[使用后台任务支持应用](https://msdn.microsoft.com/library/windows/apps/Mt299103)。

## <a name="app-count"></a>应用计数

这将验证应用包（APPX 应用程序包）中是否包含某个应用程序。 该测试在工具包中已更改为一个独立的测试。

### <a name="background"></a>后台

此测试依据应用商店策略进行实现。

### <a name="test-details"></a>测试详细信息

对于 Windows Phone 8.1 应用，测试将验证程序包中 appx 包的总数是否 &lt; 512、程序包中是否只有一个主程序包，以及程序包中的主程序包体系结构是标记为 ARM 还是中性。

对于 Windows 10 应用，测试将验证程序包版本中的修订号是否设置为 0。

### <a name="corrective-action"></a>更正操作

确保应用包和程序包满足上述测试详细信息中的要求。

## <a name="app-manifest-compliance-test"></a>应用部件清单合规性测试

测试应用部件清单 (manifest) 的内容，确保它的内容是正确的。

### <a name="background"></a>背景

应用必须拥有格式正确的应用部件清单 (manifest)。

### <a name="test-details"></a>测试详细信息

检查应用清单验证内容是否正确，如[应用包要求](https://msdn.microsoft.com/library/windows/apps/Mt148525)中所述。

-   **文件后缀名和协议**

    你的应用可以声明要与其关联的文件扩展名。 使用不当时，应用会声明大量文件扩展名（大多数扩展名甚至可能不会使用），从而导致较差的用户体验。 此测试将添加检查以限制可与应用关联的文件扩展名的数量。

-   **框架依赖关系规则**

    此测试强制要求应用采取对 UWP 的适当依赖关系。 如果存在不适当的依赖关系，该测试将失败。

    如果应用适用的操作系统版本和框架依赖关系采用的操作系统版本不匹配，该测试将失败。 如果应用引用了任何预览版的框架 DLL，该测试也将失败。

-   **进程间通信 (IPC) 验证**

    此测试强制要求的 UWP 应用不在应用容器外部与桌面组件通信。 进程间通信仅适用于旁加载应用。 使用等效于“DesktopApplicationPath”的名称指定 [**ActivatableClassAttribute**](https://msdn.microsoft.com/library/windows/apps/BR211414) 的应用无法通过此测试。

### <a name="corrective-action"></a>更正操作

针对[应用包要求](https://msdn.microsoft.com/library/windows/apps/Mt148525)中所述的要求检查应用清单。

## <a name="windows-security-features-test"></a>Windows 安全功能测试

### <a name="background"></a>背景

更改默认的 Windows 安全保护可能增加客户的风险。

### <a name="test-details"></a>测试详细信息

通过运行 [BinScope Binary Analyzer](#binscope-binary-analyzer-tests) 来测试应用的安全性。

BinScope Binary Analyzer 测试检查应用的二进制文件，以检查使应用不容易被攻击或被用作攻击平台的编码和构建实践。

BinScope Binary Analyzer 测试检查对以下安全相关功能的正确使用。

-   BinScope Binary Analyzer 测试
-   私有代码签名

### <a name="binscope-binary-analyzer-tests"></a>BinScope Binary Analyzer 测试

[BinScope Binary Analyzer](https://www.microsoft.com/en-us/download/details.aspx?id=44995) 测试检查应用的二进制文件，以检查使应用不容易被攻击或被用作攻击平台的编码和生成做法。

BinScope Binary Analyzer 测试检查对这些安全相关功能的正确使用：

-   [AllowPartiallyTrustedCallersAttribute](#binscope-1)
-   [/SafeSEH 异常处理保护](#binscope-2)
-   [数据执行保护](#binscope-3)
-   [地址空间布局随机化](#binscope-4)
-   [读取/写入共享 PE 部分](#binscope-5)
-   [AppContainerCheck](#appcontainercheck)
-   [ExecutableImportsCheck](#binscope-7)
-   [WXCheck](#binscope-8)

### <a name="span-idbinscope-1spanallowpartiallytrustedcallersattribute"></a><span id="binscope-1"></span>AllowPartiallyTrustedCallersAttribute

**Windows 应用认证工具包错误消息：** APTCACheck 测试失败

AllowPartiallyTrustedCallersAttribute (APTCA) 属性可以从签名程序集中的部分信任代码访问完全信任的代码。 当你将 APTCA 属性应用到程序集时，部分信任的调用方便可以在该程序集的寿命内访问它，这样可能会危及安全性。

**在应用未通过此测试时怎么办**

不要使用强命名程序集上的 APTCA 属性，除非你的项目需要它且已充分了解风险。 为防不时之需，请确保所有 API 都在相应的代码访问安全性要求下受到了保护。 当程序集包含在通用 Windows 平台 (UWP) 应用中时，APTCA 不会生效。

**备注**

仅在托管代码（C#、.NET 等）上执行此测试。

### <a name="span-idbinscope-2spansafeseh-exception-handling-protection"></a><span id="binscope-2"></span>/SafeSEH 异常处理保护

**Windows 应用认证工具包错误消息：** SafeSEHCheck 测试失败

异常处理程序在应用遇到异常情况时运行，例如被零除错误。 因为在调用函数时异常处理程序的地址存储在堆栈上，所以如果某个恶意软件要覆盖堆栈，可能会易于受到缓冲区溢出攻击者的攻击。

**在应用未通过此测试时怎么办**

生成应用时，在链接器命令中启用 /SAFESEH 选项。 默认情况下，此选项处于打开状态（位于 Visual Studio 的“发布”配置中）。 确认在生成指令中为应用中的所有可执行模块启用了此选项。

**备注**

不在 64 位二元文件或 ARM 芯片集二元文件上执行此测试，因为它们不将异常处理程序地址存储在堆栈上。

### <a name="span-idbinscope-3spandata-execution-prevention"></a><span id="binscope-3"></span>数据执行保护

**Windows 应用认证工具包错误消息：** NXCheck 测试失败

此测试验证应用是否未运行存储在数据段中的代码。

**在应用未通过此测试时怎么办**

生成应用时，在链接器命令中启用 /NXCOMPAT 选项。 默认情况下，此选项在支持数据执行保护 (DEP) 的链接器版本中处于打开状态。

**备注**

我们建议，在支持 DEP 的 CPU 上测试你的应用，并修复你发现的由于 DEP 所导致的任何故障。

### <a name="span-idbinscope-4spanaddress-space-layout-randomization"></a><span id="binscope-4"></span>地址空间布局随机化

**Windows 应用认证工具包错误消息：** DBCheck 测试失败

地址空间布局随机化 (ASLR) 将可执行文件映像加载到内存中不可预测的位置，从而使预期某个程序在特定虚拟地址加载的恶意软件更难以按可预见的方式运行。 你的应用及其使用的所有组件必须支持 ASLR。

**在应用未通过此测试时怎么办**

在生成应用时，在链接器命令中启用 /DYNAMICBASE 选项。 验证你的应用使用的所有模块也使用此链接器选项。

**备注**

通常，ASLR 不影响性能。 但在某些情况下，在 32 位系统上有少许的性能改进。 原因可能是，在许多不同内存位置加载许多映像的高拥堵系统中，性能可能会降级。

此测试仅在使用非托管语言（如使用 C 或 C++）编写的应用上执行。

### <a name="span-idbinscope-5spanreadwrite-shared-pe-section"></a><span id="binscope-5"></span>读取/写入共享 PE 部分

**Windows 应用认证工具包错误消息：** SharedSectionsCheck 测试失败。

如果二进制文件包含标记为共享的可写入节，那么它就是一种安全威胁。 除非必须，否则不要构建包含共享的可写入节的应用。 使用 [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) 或 [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761) 创建受到适当保护的共享内存对象。

**在应用未通过此测试时怎么办**

从应用中删除任何共享节，使用合适的安全属性调用 [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) 或 [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761) 来创建共享内存对象，然后重新生成你的应用。

**备注**

此测试仅在使用非托管语言（如使用 C 或 C++）编写的应用上执行。

### <a name="appcontainercheck"></a>AppContainerCheck

**Windows 应用认证工具包错误消息：** AppContainerCheck 测试失败。

AppContainerCheck 验证一个可执行二进制文件的可移植可执行 (PE) 头文件中的 **appcontainer** 位是否已设置。 应用必须在所有 .exe 文件和所有非托管 DLL 上设置了 **appcontainer** 位才能正确执行。

**在应用未通过此测试时怎么办**

如果原生的可执行文件未通过测试，请确保你使用了最新的编译器和链接器来生成文件，并在链接器上使用了 */appcontainer* 标记。

如果托管的可执行文件未通过测试，请确保你使用的最新的编译器和链接器，如 Microsoft Visual Studio 中，生成 UWP 应用。

**备注**

此测试在所有 .exe 文件和非托管 DLL 上执行。

### <a name="span-idbinscope-7spanexecutableimportscheck"></a><span id="binscope-7"></span>ExecutableImportsCheck

**Windows 应用认证工具包错误消息：** ExecutableImportsCheck 测试失败。

如果可移植可执行 (PE) 映像的导入表放在了一个可执行代码节中，该映像将无法通过此测试。 如果通过将 Visual C++ 链接器的 */merge* 标记设置为 */merge:.rdata=.text*来为 PE 映像启用了 .rdata 合并，则可能发生此情形。

**在应用未通过此测试时怎么办**

不要将导入表合并到可执行代码节中。 确保 Visual C++ 链接器的 */merge* 标记未设置为将“.rdata”节合并到代码节中。

**备注**

此测试在除纯粹的托管程序集以外的所有二进制代码上执行。

### <a name="span-idbinscope-8spanwxcheck"></a><span id="binscope-8"></span>WXCheck

**Windows 应用认证工具包错误消息：** WXCheck 测试失败。

此项检查有助于确保二进制文件不包含任何映射为可写和可映射的页面。 如果二进制文件包含可写和可执行的部分，或者如果二进制文件的 *SectionAlignment* 小于 *PAGE\-SIZE*，则可能会发生这种情况。

**在应用未通过此测试时怎么办**

确保二进制文件不包含可写或可执行部分，并且二进制文件的 *SectionAlignment* 值至少等于 *PAGE\-SIZE*。

**备注**

此测试在所有 .exe 文件和本机非托管 DLL 上执行。

如果已在启用“编辑”和“继续”(/ZI) 的情况下构建可执行文件，则其中可能包含可写和可执行部分。 禁用“编辑”和“继续”将导致无效部分无法呈现。

*PAGE\-SIZE* 是可执行文件的默认 *SectionAlignment* 值。

### <a name="private-code-signing"></a>私有代码签名

测试应用程序包中是否存在私有代码签名二进制文件。

### <a name="background"></a>背景

私有代码签名文件应该保持私有，因为在泄露这些文件的事件中，它们可能会被恶意使用。

### <a name="test-details"></a>测试详细信息

在应用程序包中测试扩展名为 .pfx 或 .snk 的文件，这指示其中包含私有签名密钥。

### <a name="corrective-actions"></a>更正操作

从该程序包中删除任何私有代码签名密钥（例如 .pfx 和 .snk 文件）。

## <a name="supported-api-test"></a>支持的 API 测试

测试应用是否使用了任何不兼容的 API。

### <a name="background"></a>背景

应用必须使用适用于 UWP 应用 （Windows 运行时或受支持的 Win32 Api） 的 Microsoft 应用商店认证的 Api。 此测试还识别托管二进制文件依赖于批准的配置文件以外功能的情形。

### <a name="test-details"></a>测试详细信息

-   验证，应用包中的每个二进制文件均不依赖于通过检查二进制文件的导入地址表不支持用于 UWP 应用开发的 Win32 API。
-   验证应用包中的每个托管二进制文件是否均不依赖于批准的配置文件以外的功能。

### <a name="corrective-actions"></a>更正操作

确保应用编译为一个发行版本，而不是调试版本。

> **注意** 应用的调试版本将无法通过此测试，即使该应用使用仅[适用于 UWP 应用的 Api](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx)。

检查错误消息，识别应用所用的不是[适用于 UWP 应用的 API](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx)的 API。

> **注意** 内置于该调试配置中的 c + + 应用将无法通过此测试，即使配置仅适用于 UWP 应用使用 Windows SDK 中的 Api。 请参阅，有关详细信息的[UWP 应用中的 Windows Api 的替代项](http://go.microsoft.com/fwlink/p/?LinkID=244022)。

## <a name="performance-tests"></a>性能测试

为了提供快速而流畅的用户体验，应用必须迅速响应用户交互和系统命令。

执行测试的计算机的特性会影响测试结果。 设置应用认证的性能测试阈值，以便低能耗计算机能够满足客户快速流畅的体验预期。 为了确定你的应用性能，我们建议你在低能耗计算机上测试应用，例如基于 Intel Atom 处理器的计算机，屏幕分辨率为 1366x768（或更高），旋转硬盘驱动器（相对于固态硬盘驱动器）。

### <a name="bytecode-generation"></a>字节码的生成

作为一种性能优化，为缩短 JavaScript 执行时间，在部署应用时，以 .js 扩展名结尾的 JavaScript 文件将生成字节码。 这可以显著缩短 JavaScript 操作的启动和持续执行时间。

### <a name="test-details"></a>测试详细信息

检查应用部署，验证所有 .js 文件已转换为字节码。

### <a name="corrective-action"></a>更正操作

如果此测试失败，在解决此问题时请考虑以下方法：

-   验证是否启用了事件日志记录。
-   验证所有 JavaScript 文件的语法是否有效。
-   确认已经卸载该应用的所有以前版本。
-   从应用包中排除识别的文件。

### <a name="optimized-binding-references"></a>优化的绑定引用

在使用绑定时，WinJS.Binding.optimizeBindingReferences 应设置为 True 才能优化内存使用情况。

### <a name="test-details"></a>测试详细信息

验证 WinJS.Binding.optimizeBindingReferences 的值。

### <a name="corrective-action"></a>更正操作

在应用 JavaScript 中，将 WinJS.Binding.optimizeBindingReferences 设置为 **true**。

## <a name="app-manifest-resources-test"></a>应用清单资源测试

### <a name="app-resources-validation"></a>应用资源验证

如果应用部件清单 (manifest) 中声明的字符串或图像不正确，应用可能未安装。 如果安装应用时出现了这些错误，应用的徽标或应用使用的其他图像可能无法正确显示。

### <a name="test-details"></a>测试详细信息

检查应用部件清单 (manifest) 中定义的资源，确保它们是最新且有效的。

### <a name="corrective-action"></a>更正操作

使用下表作为指导。

<table>
<tr><th>错误消息</th><th>备注</th></tr>
<tr><td>
<p>图像 {image name} 定义 Scale 和 TargetSize 限定符；一次只能定义一个限定符。</p>
</td><td>
<p>可以自定义不同分辨率的图像。</p>
<p>在实际消息中，{image name} 包含有错误的图像名称。</p>
<p> 确保每个图像都将 Scale 或 TargetSize 定义为限定符。</p>
</td></tr>
<tr><td>
<p>图像 {image name} 不符合大小限制。</p>
</td><td>
<p>确保所有应用图像都符合相应的大小限制。</p>
<p>在实际消息中，{image name} 包含有错误的图像名称。</p>
</td></tr>
<tr><td>
<p>程序包中缺少图像 {image name}。</p>
</td><td>
<p>缺少所需的图像。</p>
<p>在实际消息中，{image name} 包含缺少的图像名称。</p>
</td></tr>
<tr><td>
<p>图像 {image name} 不是有效的图像文件。</p>
</td><td>
<p>确保所有应用图像都符合相应的文件格式类型限制。</p>
<p>在实际消息中，{image name} 包含无效的图像名称。</p>
</td></tr>
<tr><td>
<p>图像“BadgeLogo”在位置 (x, y) 上具有一个无效的 ABGR 值 {value}。 像素必须为白色 (##FFFFFF) 或透明 (00######)</p>
</td><td>
<p>锁屏提醒徽标是显示在锁屏提醒通知旁边的图像，用于在锁屏上标识应用。 该图像必须是单色图像（它只能包含白色和透明像素）。</p>
<p>在实际消息中，{value} 在图像中包含无效的颜色值。</p>
</td></tr>
<tr><td>
<p>图像“BadgeLogo”在位置 (x, y) 上具有一个对高对比度白色图像来说无效的 ABGR 值 {value}。 像素必须为 (##2A2A2A) 或更暗，或者透明 (00######)。</p>
</td><td>
<p>锁屏提醒徽标是显示在锁屏提醒通知旁边的图像，用于在锁屏上标识应用。   因为当处于高对比度白色背景中时锁屏提醒徽标出现在白色背景上，因此它必须是较暗版本的正常锁屏提醒徽标。 在高对比度白色背景中，锁屏提醒徽标只能包含比 (##2A2A2A) 暗或透明的像素。</p>
<p>在实际消息中，{value} 在图像中包含无效的颜色值。</p>
</td></tr>
<tr><td>
<p>图像必须至少定义一个没有 TargetSize 限定符的变量。 它必须定义一个 Scale 限定符或者保持 Scale 和 TargetSize 为未指定状态，默认值为 Scale-100。</p>
</td><td>
<p>有关详细信息，请参阅 <a href="https://msdn.microsoft.com/library/windows/apps/xaml/dn958435.aspx">UWP 应用的响应式设计基础</a>和<a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">应用资源指南</a>。</p>
</td></tr>
<tr><td>
<p>该程序包缺少一个“resources.pri”文件。</p>
</td><td>
<p>如果你在应用清单中包含可本地化的内容，请确保你的应用包包含有效的 resources.pri 文件。</p>
</td></tr>
<tr><td>
<p>“resources.pri”文件必须包含一个其名称与程序包名称 {package full name} 相匹配的资源映射</p>
</td><td>
<p>如果清单发生更改并且 resources.pri 中的资源映射名称不再与清单中的程序包名称相匹配，你将遇到此错误。</p>
<p>在实际消息中，{package full name} 包含 resources.pri 必须包含的程序包名称。</p>
<p>为了解决此问题，你需要重新构建 resources.pri，而这样做的最简单方法就是重新构建应用包。</p>
</td></tr>
<tr><td>
<p>“resources.pri”文件不得启用 AutoMerge。</p>
</td><td>
<p>MakePRI.exe 支持一个名为 <strong>AutoMerge</strong> 的选项。 <strong>AutoMerge</strong> 的默认值为 <strong>off</strong>。 启用后，<strong>AutoMerge</strong> 在运行时将应用的语言包资源合并到一个 resources.pri 中。 我们不建议执行此操作适用于要通过 Microsoft 应用商店分发的应用。 通过 Microsoft 应用商店分发应用的 resources.pri 必须为应用的程序包的根目录中，并包含应用支持的所有语言参考。</p>
</td></tr>
<tr><td>
<p>字符串 {string} 不符合 {number} 个字符的最大长度限制。</p>
</td><td>
<p>请参阅<a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">应用包要求</a>。</p>
<p>在实际消息中，{string} 替换为有错误的字符串并且 {number} 包含最大长度。</p>
</td></tr>
<tr><td>
<p>字符串 {string} 不得包含前导空格/尾随空格。</p>
</td><td>
<p>应用部件清单 (manifest) 中元素的架构不允许前导空格或尾随空格字符。</p>
<p>在实际消息中，{string} 替换为有错误的字符串。</p>
<p>确保 resources.pri 中清单字段的任何本地化值都没有前导空格或尾随空格字符。</p>
</td></tr>
<tr><td>
<p>字符串必须非空（长度大于零）</p>
</td><td>
<p>有关详细信息，请参阅<a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">应用包要求</a>。</p>
</td></tr>
<tr><td>
<p>“resources.pri”文件中没有指定的默认资源。</p>
</td><td>
<p>有关详细信息，请参阅<a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">应用资源指南</a>。</p>
<p>在默认版本配置中，Visual Studio 在生成捆绑包时仅在应用包中包含比例为 200 的图像资源，从而将其他资源放入资源包中。 确保你包含比例为 200 的图像资源或将项目配置为包含你拥有的资源。</p>
</td></tr>
<tr><td>
<p>“resources.pri”文件中没有指定的资源值。</p>
</td><td>
<p>确保应用清单具有在 resources.pri 中定义的有效资源。</p>
</td></tr>
<tr><td>
<p>图像文件 {filename} 必须小于 204800 字节。\*\*</p>
</td><td>
<p>减小指示图像的大小。</p>
</td></tr>
<tr><td>
<p>{filename} 文件不能包含反向映射部分。\*\*</p>
</td><td>
<p>如果反向映射是在调用 makepri.exe 时在 Visual Studio 的 F5 调试期间生成的，通过在生成 pri 文件时运行 makepri.exe（不具有 /m 参数）可删除该反向映射。</p>
</td></tr>
<tr><td colspan="2">
<p>\*\* 指示某个测试已添加到适用于 Windows 8.1 的 Windows 应用认证工具包 3.3 中，并且仅在使用该版本或更高版本的工具包时适用。</p>
</td></tr>
</table>



 

### <a name="branding-validation"></a>品牌验证

UWP 应用应该完整并且功能齐全。 使用默认图像（来自模板或 SDK 示例）的应用会带来很差的用户体验，且无法在应用商店目录中方便地标识。

### <a name="test-details"></a>测试详细信息

该测试将验证应用使用的图像不是 SDK 示例或 Visual Studio 中的默认图像。

### <a name="corrective-actions"></a>更正操作

将默认图像替换为更能区别和代表该应用的图像。

## <a name="debug-configuration-test"></a>调试配置测试

测试应用，确保它不是一个调试版本。

### <a name="background"></a>背景

要通过 Microsoft 应用商店的认证，应用必须不编译为调试，且不得引用可执行文件的调试版本。 此外，你必须生成优化代码才能使应用通过此测试。

### <a name="test-details"></a>测试详细信息

测试应用，确保它不是调试版本并且未链接到任何调试框架。

### <a name="corrective-actions"></a>更正操作

-   提交到 Microsoft 应用商店之前，将应用编译为发行版本。
-   确保你安装了正确版本的 .NET Framework。
-   确保该应用未链接到框架的调试版本，并使用发布版本构建。 如果此应用包含 .NET 组件，请确保安装了正确的 .NET Framework 版本。

## <a name="file-encoding-test"></a>文件编码测试

### <a name="utf-8-file-encoding"></a>UTF-8 文件编码

### <a name="background"></a>背景

HTML、CSS 和 JavaScript 文件必须使用带有相应字节顺序标记 (BOM) 的 UTF-8 格式进行编码，以便从字节码缓存中获益并避免某些运行时错误情况。

### <a name="test-details"></a>测试详细信息

测试应用包的内容，确保它们使用了正确的文件编码。

### <a name="corrective-action"></a>更正操作

在 Visual Studio 中打开受影响的文件，并从“文件”**** 菜单中选择“另存为”****。 选择“保存”**** 按钮旁边的下拉控件，并选择“编码保存”****。 从“高级”**** 保存选项对话框中，选择 Unicode（带签名的 UTF-8）选项，并单击“确定”****。

## <a name="direct3d-feature-level-test"></a>Direct3D 功能级别测试

### <a name="direct3d-feature-level-support"></a>Direct3D 功能级别支持

对 Microsoft Direct3D 应用进行测试，以确保它们不会在使用旧版图形硬件的设备上崩溃。

### <a name="background"></a>后台

Microsoft 应用商店要求使用 Direct3D 正确呈现或正常功能级别 9 \-1 图形卡上的所有应用程序。

由于用户可能会在安装应用后更换设备中的图形硬件，因此如果你选择高于 9\-1 的最低功能级别，你的应用必须在启动时检测当前的硬件是否满足最低要求。 如果不满足最低要求，则应用必须向用户显示一条消息，以详细说明 Direct3D 要求。 此外，如果应用下载到不兼容的设备上，应用应当在启动时检测出该问题并向用户显示一条消息，详细说明这些要求。

### <a name="test-details"></a>测试详细信息

该测试将验证应用是否在功能级别 9\-1 上准确地呈现。

### <a name="corrective-action"></a>更正操作

确保你的应用在 Direct3D 功能级别 9\-1 上正确呈现，即使你希望它在更高的功能级别上运行。 有关详细信息，请参阅[针对不同 Direct3D 功能级别开发](http://go.microsoft.com/fwlink/p/?LinkID=253575)。

### <a name="direct3d-trim-after-suspend"></a>Direct3D 暂停后修正

> **注意** 此测试仅适用于 Windows 8.1 及更高版本开发 UWP 应用。

### <a name="background"></a>后台

如果该应用不在其 Direct3D 设备上调用 [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346)，则该应用不会释放为其早期 3D 工作分配的内存。 这将增加由于系统内存压力而终止应用的风险。

### <a name="test-details"></a>测试详细信息

检查应用是否符合 d3d 要求，并确保应用在 Suspend 回调时调用新的 [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) API。

### <a name="corrective-action"></a>更正操作

每当应用即将暂停时应在其 [**IDXGIDevice3**](https://msdn.microsoft.com/library/windows/desktop/Dn280345) 接口上调用 [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) API。

## <a name="app-capabilities-test"></a>应用功能测试

### <a name="special-use-capabilities"></a>专用许可范围

### <a name="background"></a>背景

专用许可范围专用于非常特定的场景。 仅允许公司帐户使用这些功能。

### <a name="test-details"></a>测试详细信息

验证应用是否在声明以下任何功能：

-   EnterpriseAuthentication
-   SharedUserCertificates
-   DocumentsLibrary

如果声明了其中任一功能，该测试将向用户显示警告。

### <a name="corrective-actions"></a>更正操作

如果你的应用不需要特殊的使用功能，请考虑将其删除。 此外，使用这些功能应接受其他板载策略审查。

## <a name="windows-runtime-metadata-validation"></a>Windows 运行时元数据验证

### <a name="background"></a>背景

确保随应用发送的组件符合 UWP 类型系统。

### <a name="test-details"></a>测试详细信息

验证程序包中的 **.winmd** 文件是否符合 UWP 规则。

### <a name="corrective-actions"></a>更正操作

-   **ExclusiveTo 属性测试：** 确保 UWP 类未实现标记为 ExclusiveTo 其他类的接口。
-   **类型位置测试：** 确保所有 UWP 类型的元数据位于在应用包中具有最长命名空间匹配名称的 winmd 文件中。
-   **类型名称区分大小写测试：** 确保所有 UWP 类型在应用包中具有不区分大小写的唯一名称。 还要确保没有任何 UWP 类型名称在应用包中用作命名空间名称。
-   **类型名称正确性测试：** 确保在全局命名空间或 Windows 顶级命名空间中没有 UWP 类型。
-   **一般元数据正确性测试：** 确保用于生成相应类型的编译器符合最新的 UWP 规范。
-   **属性测试：** 确保 UWP 类上的所有属性都具有 get 方法（set 方法可选）。 对于 UWP 类型上的所有属性，确保 get 方法返回值的类型与 set 方法输入参数的类型匹配。

## <a name="package-sanity-tests"></a>程序包健全性测试

### <a name="platform-appropriate-files-test"></a>平台的相应文件测试

安装混合二进制文件的应用可能崩溃或者不能正确运行，具体取决于用户的处理器体系结构。

### <a name="background"></a>背景

此测试验证应用包中的二进制文件，确定是否存在体系结构冲突。 应用包不得包含无法在部件清单 (manifest) 指定的处理器体系结构上使用的二进制文件。 包含不受支持的二进制文件可能会导致应用发生崩溃，或造成应用包大小出现不必要的增加。

### <a name="test-details"></a>测试详细信息

在与应用程序包处理器体系结构声明交叉引用时，验证 PE 标头中每个文件的“位元”是否适当。

### <a name="corrective-action"></a>更正操作

遵循以下指南，确保你的应用包仅包含应用部件清单 (manifest) 中指定的体系结构支持的文件。

-   如果应用的目标处理器体系结构为非特定处理器类型，则应用包不能包含 x86、x64 或 ARM 二进制文件或图像类型文件。

-   如果应用的目标处理器体系结构为 x86 处理器类型，则应用包必须仅包含 x86 二进制文件或图像类型文件。 如果应用包包含 x64 或 ARM 二进制文件或图像类型文件，将无法通过这项测试。

-   如果应用的目标处理器体系结构为 x64 处理器类型，则应用包必须包含 x64 二进制文件或图像类型文件。 请注意，在这种情况下，应用包还可以包含 x86 文件，但主要应用体验应使用 x64 二进制文件。

    但是，如果应用包包含 ARM 二进制文件或图像类型文件，或者仅包含 x86 二进制文件或图像类型文件，将无法通过这项测试。

-   如果应用的目标处理器体系结构为 ARM 处理器类型，则应用包必须仅包含 ARM 二进制文件或图像类型文件。 如果应用包包含 x64 或 x86 二进制文件或图像类型文件，将无法通过这项测试。

### <a name="supported-directory-structure-test"></a>支持的目录结构测试

验证应用程序未创建长度超过 MAX\-PATH 的子目录来作为安装的一部分。

### <a name="background"></a>后台

操作系统组件（包括 Trident、WWAHost 等）内部限制在文件系统路径的 MAX\-PATH，对于较长的路径将不会正确运行。

### <a name="test-details"></a>测试详细信息

验证应用安装目录中没有超过 MAX\-PATH 的路径。

### <a name="corrective-action"></a>更正操作

使用较短的目录结构和/或文件名称。

## <a name="resource-usage-test"></a>资源使用测试

### <a name="winjs-background-task-test"></a>WinJS 后台任务测试

WinJS 后台任务测试可确保 JavaScript 应用具有适当的 close 语句，因此应用不消耗电池电量。

### <a name="background"></a>后台

具有 JavaScript 后台任务的应用在后台任务中需要调用 Close() 作为最后的语句。 不执行此操作的应用可能会使系统无法返回到连接的待机模式，并导致消耗电池电量。

### <a name="test-details"></a>测试详细信息

如果应用没有清单中指定的后台任务文件，该测试将会通过。 否则，测试将解析应用包中指定的 JavaScript 后台任务文件，并查找 Close() 语句。 如果找到，测试将通过；否则测试将失败。

### <a name="corrective-action"></a>更正操作

更新后台 JavaScript 代码以正确调用 Close()。


## <a name="related-topics"></a>相关主题

* [Windows 桌面桥应用测试](windows-desktop-bridge-app-tests.md)
* [Microsoft Store 策略](https://msdn.microsoft.com/library/windows/apps/Dn764944)
 
