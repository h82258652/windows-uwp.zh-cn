---
Description: Test your app for Windows 10 in S mode.
Search.Product: eADQiWindows 10XVcnh
title: 测试适用于 Windows 10 S 的 Windows 应用
ms.date: 05/11/2017
ms.topic: article
keywords: windows 10 S, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a8b17697612d50d10ecfbb07388207527a4cb39b
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/27/2018
ms.locfileid: "7840903"
---
# <a name="test-your-windows-app-for-windows-10-in-s-mode"></a>在 S 模式下测试适用于 Windows 10 的 Windows 应用

可以对 Windows 应用进行测试，以确保其在以 S 模式运行 Windows 10 的设备上正常运行。 事实上，如果准备将应用发布到 Microsoft Store，则必须这样做，因为这是 Microsoft Store 的一项要求。 若要测试你的应用，可以在运行 Windows 10 专业版的设备上应用 Device Guard 代码完整性策略。

> [!NOTE]
> 应用 Device Guard 代码完整性策略的设备必须运行 Windows 10 创意者版本（10.0；内部版本 15063）或更高版本。

Device Guard 代码完整性策略强制执行应用在 Windows 10 S 上运行所必须遵守的规则。

> [!IMPORTANT]
>我们建议你将这些策略应用于虚拟机，但如果你想要将它们应用于本地计算机，则请确保在应用策略之前，先查看本主题“下一步，安装策略并重启系统”部分中的最佳实践指南。

<a id="choose-policy" />

## <a name="first-download-the-policies-and-then-choose-one"></a>首先，下载这些策略，然后从中选择一个

在[此处](https://go.microsoft.com/fwlink/?linkid=849018)下载 Device Guard 代码完整性策略。

然后，选择对你最有意义的一个策略。 以下是每个策略的摘要。

|策略 |强制 |签名证书 |文件名 |
|--|--|--|--|
|审核模式策略 |日志问题/不阻止 |应用商店 |SiPolicy_Audit.p7b |
|生产模式策略 |是 |应用商店 |SiPolicy_Enforced.p7b |
|具有自签名应用的生产模式策略 |是 |AppX 测试证书  |SiPolicy_DevModeEx_Enforced.p7b |

我们建议你从审核模式策略开始。 你可以查看代码完整性事件日志并使用该信息帮助你对应用进行调整。 然后，当你准备好进行最终测试时，请应用生产模式策略。

以下是有关每个策略的更多信息。

### <a name="audit-mode-policy"></a>审核模式策略
在此模式下，即使你的应用执行 Windows 10 S 上不支持的任务，应用也会运行。Windows 将任何可能已被阻止的可执行文件记录到代码完整性事件日志中。

你可以打开**事件查看器**，然后浏览到此位置：应用程序和服务日志 -> Microsoft -> Windows -> CodeIntegrity-> 运营，从而找到这些日志。

![代码完整性事件日志](images/desktop-to-uwp/code-integrity-logs.png)

此模式为安全模式，不会阻止你的系统启动。

#### <a name="optional-find-specific-failure-points-in-the-call-stack"></a>（可选）查找调用堆栈中的特定故障点
若要在调用堆栈中找到阻止问题发生的特定点，请添加此注册表项，然后[设置内核模式调试环境](https://docs.microsoft.com/windows-hardware/drivers/debugger/getting-started-with-windbg--kernel-mode-#span-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanspan-idsetupakernel-modedebuggingspanset-up-a-kernel-mode-debugging)。

|注册表项|名称|类型|值|
|--|---|--|--|
|HKEY_LOCAL_MACHINE\SYSTEM\CurentControlSet\Control\CI| DebugFlags |REG_DWORD | 1 |


![注册表设置](images/desktop-to-uwp/ci-debug-setting.png)

### <a name="production-mode-policy"></a>生产模式策略
此策略强制执行与 Windows 10 S 匹配的代码完整性规则，使你可以模拟在 Windows 10 S 上运行。这是最严格的策略，非常适合用于最终生产测试。 在此模式下，你的应用受到的限制与在用户设备上受到的限制相同。 若要使用此模式，你的应用必须由 Microsoft Store 签名。

### <a name="production-mode-policy-with-self-signed-apps"></a>具有自签名应用的生产模式策略
此模式类似于生产模式策略，但它也允许通过使用 zip 文件中包含的测试证书进行签名的应用运行。 安装在此 zip 文件的 **AppxTestRootAgency** 文件夹中包含的 PFX 文件。 然后，使用它对你的应用进行签名。 如此一来，你便可以快速进行循环访问，而无需应用商店签名。

由于你的证书的发布者名称必须与你的应用的发布者名称匹配，你将需要暂时将 **Identity** 元素的 **Publisher** 属性更改为“CN = Appx Test Root Agency Ex”。 完成测试后，你可以将该属性更改回其原始值。

## <a name="next-install-the-policy-and-restart-your-system"></a>接下来，安装策略并重启系统

我们建议将这些策略应用于虚拟机，因为这些策略可能导致启动失败。 这是因为这些策略会阻止未经 Microsoft Store 签名的代码执行，包括驱动程序。

如果想要将这些策略应用于本地计算机，最好从审核模式策略开始。 使用此策略，你可以查看代码完整性事件日志，以确保在强制执行的策略中不会阻止任何关键操作。

准备好应用策略时，找到所选策略的 .P7B 文件，将其重命名为 **SIPolicy.P7B**，然后将该文件保存到系统上的此位置：**C:\Windows\System32\CodeIntegrity\\**。

然后，重启系统。

>[!NOTE]
>若要从系统中删除策略，请先删除 .P7B 文件，然后重启系统。

## <a name="next-steps"></a>后续步骤

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

**查看我们的应用咨询团队发布的详细博客文章**

请参阅[使用桌面桥移植和测试 Windows 10 上的经典桌面应用程序](https://blogs.msdn.microsoft.com/appconsult/2017/06/15/porting-and-testing-your-classic-desktop-applications-on-windows-10-s-with-the-desktop-bridge/)。

**了解让测试以 S 模式运行的 Windows 变得更轻松的工具**

请参阅[对 APPX 进行解压缩、修改、重新打包、签名](https://blogs.msdn.microsoft.com/appconsult/2017/08/07/unpack-modify-repack-sign-appx/)。
