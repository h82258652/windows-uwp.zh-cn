---
author: normesta
Description: "为现有 Windows 窗体、WPF 或 Win32 应用或游戏创建现代 Windows 应用包。 为 Windows 10 用户增加现代体验，并简化部署和盈利过程。"
Search.Product: eADQiWindows 10XVcnh
title: "桌面桥"
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.openlocfilehash: 1830c1661325afe68e8e7cd32528ec075e098b1d
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2017
---
# <a name="desktop-bridge"></a>桌面桥

使用你现有的桌面应用，并为 Windows 10 用户增加现代体验。 然后，通过 Windows 应用商店发布应用，在更大的国际市场范围中提供。 你可以直接利用应用商店中内置的功能，以更为简单的方式通过你的应用获取利润。 当然，并非必须使用应用商店。 你完全可以使用现有的渠道。
<div style="float: left; padding: 10px">
    ![桌面到 UWP 桥图像](images/desktop-to-uwp/desktop-bridge-4.png)
</div>
桌面到 UWP 桥是我们内置于平台中的基础架构，借助它，你可以使用一个现代的 Windows 应用包来分发你的 Windows 窗体、WPF 或 Win32 桌面应用或游戏。

此包向你的应用提供一个身份，你的桌面应用可以利用此身份获得对 Windows 通用平台 (UWP) API 的访问权限。 你可以使用它们添加具有吸引力的现代体验，例如动态磁贴和通知。  仅当应用在 Windows 10 上运行时，才可使用简单的条件编译和运行时检查来运行 UWP 代码。

除了用于添加 Windows 10 体验的代码之外，你的应用将保持不变，你可以继续将它分发给你现有的 Windows 7、Windows Vista 或 Windows XP 用户群。 在 Windows 10 上，你的应用将继续像如今的情形一样，以完全信任用户模式运行。

> [!NOTE]
> 请观看 Microsoft Virtual Academy 发布的<a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">这一系列</a>的视频短片。 这些视频演示了将桌面应用引入通用 Windows 平台 (UWP) 的整个过程。

## <a name="benefits"></a>好处

以下是为你的 Windows 桌面应用程序创建 Windows 应用包的一些原因：

**简化了部署**。 使用此桥的应用和游戏都具有出色的部署体验。 此体验确保用户可以放心地安装应用，并对其进行更新。 如果用户选择卸载应用，则将完全删除它，不会留下任何痕迹。 这能减少编写设置体验的时间，并使用户使用最新应用。

**自动更新和许可**。 你的应用能够参与 Windows 应用商店的内置许可和自动更新设施。 自动更新是高度可靠和高效的机制，因为仅下载文件的已更改部分。

**扩大了覆盖范围，并简化了盈利**。 选择通过 Windows 应用商店进行分发可将覆盖范围扩大至数以百万的 Windows10 用户，这些用户可以通过当地支付选项获取应用、游戏和应用内购买。

**添加 UWP 功能**。  你可以按照自己的节奏向应用的程序包添加 UWP 功能，如 XAML 用户界面、动态磁贴更新、UWP 后台任务、应用服务以及许多其他功能。

**拓展了跨设备用例**。 通过使用桥，可以将代码逐渐迁移到通用 Windows 平台，以覆盖所有的 Windows10 设备，包括手机、Xbox One 和 HoloLens。

若要查看更完整的优势列表，请参阅[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)。

## <a name="prepare"></a>准备

是否计划将应用发布到 [Windows 应用商店](https://www.microsoft.com/store/apps)。 如果是，请先填写[此表单](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge)。 Microsoft 将联系你以开始加入过程。 在此过程中，你需要在应用商店中预留一个名称，并获取创建 Windows 应用包所需的信息。

接下来，请查看文章[准备将桌面应用打包](desktop-to-uwp-prepare.md)，并首先解决应用存在的任何问题，然后再为应用创建 Windows 应用包。 在创建包之前，可能不需要对应用进行很多更改。 但是，在一些情况下可能需要你在创建包之前对应用进行调整。

<span id="convert" />
## <a name="package"></a>包

以下是一些可用于为应用创建 Windows 应用包的工具。

### <a name="desktop-app-converter"></a>Desktop App Converter

尽管此工具名称中出现了“Converter”一词，但它实际上并不转换你的应用。 你的应用将保持不变。 但是，此工具将为你生成 Windows 应用包。 在应用进行大量系统修改的情况下，或者如果你不太确定安装程序要执行的操作，则使用它非常方便。

Desktop App Converter 还可以为你执行一些额外的操作。 以下是其中一些例子。

* 自动注册预览控件、缩略图处理程序、属性处理程序、防火墙规则、URL 标志。

* 自动注册使用户能够使用“文件资源管理器”中的**种类**列对文件进行分组的文件类型映射。

* 注册你的公共 COM 服务器。

* 生成可用于运行应用的证书。

* 根据桌面桥和 Windows 应用商店要求验证你的应用。

请参阅[使用 Desktop App Converter 将应用打包（桌面到 UWP 桥）](desktop-to-uwp-run-desktop-app-converter.md)

### <a name="manual-packaging"></a>手动打包

如果想精细地控制你的转换，则可以创建清单文件，然后运行 **MakeAppx.exe** 工具来创建 Windows 应用包。

如果你熟悉安装程序对系统所做的更改，或者如果你没有安装程序，并且是通过以物理方式将文件复制到文件夹位置或通过使用如 **xcopy** 等命令安装的应用，则可以使用此方法。 不过请尽量避免出现由于缺少安装程序而使你手动将应用打包的情况。 即使没有安装程序，你也可以使用 Desktop App Converter 来将应用打包。

请参阅[手动将应用打包（桌面到 UWP 桥）](desktop-to-uwp-manual-conversion.md)。

### <a name="visual-studio"></a>Visual Studio

除了 Visual Studio 会为你执行一些操作（例如为你的应用生成应用包和可视资源）外，此选项类似于上述的手动选项。 将 Visual Studio 视为可用于手动将应用打包并且能带来一些额外便利的工具。

请参阅[使用 Visual Studio 将 .NET 应用打包（桌面到 UWP 桥）](desktop-to-uwp-packaging-dot-net.md)

### <a name="third-party-installer"></a>第三方安装程序

 多个热门的第三方产品和安装程序现在都支持桌面到 UWP 桥。 只需单击几下即可使用它们来生成 MSI 安装程序或应用包。 虽然我们不生成有关如何使用这些工具的文档，但可以访问它们的网站以了解详细信息。

 以下是一些选项：

#### <a name="advanced-installer"></a>Advanced Installer

Caphyon 提供基于 GUI 的免费桌面应用打包工具。使用此工具，你只需单击几下即可为应用程序生成 Windows 应用包。 它可以使用任何安装程序（甚至是以静默模式运行的安装程序）并执行验证检查，以确定该应用是否适合打包。
<div style="float: left; padding: 10px; width: 20%">
     ![Advanced Installer 徽标](images/desktop-to-uwp/Advanced_Installer_Vertical.png)
</div>
Desktop App Converter 还与 Hyper-V 和 [VMware](http://www.vmware.com/) 集成。 这意味着你可以使用自己的虚拟机，而无需下载大小可能超过 3GB 的匹配的 [Docker](https://docs.docker.com/) 映像。

可以使用 [Advanced Installer](http://www.advancedinstaller.com/) 从现有项目中生成 MSI 和 [Windows 应用包](http://www.advancedinstaller.com/uwp-app-package.html)。 还可以使用 Advanced Installer 导入使用 Microsoft Desktop App Converter 生成的 Windows 应用包。 导入后，可以使用专为 UWP 应用设计的可视化工具对它们进行维护。

Advanced Installer 还提供用于 Visual Studio 2017 和 2015 的扩展，可用于[生成和调试桌面桥应用](http://www.advancedinstaller.com/debug-desktop-bridge-apps.html)。

请观看此[视频](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be)进行简要了解。

#### <a name="cloudhouse-compatibility-containers"></a>Cloudhouse 兼容性容器

对于其业务线应用程序与 Windows 10 和 Windows 10 的不兼容的企业客户，Cloudhouse 的兼容性容器可用来将 Windows XP 和 Windows 7 应用转换为 UWP，然后无需更改源代码就可通过适用于企业的 Windows 应用商店或 Microsoft Intune 交付。
<div style="float: left; padding: 10px; width: 20%">
     ![Cloudhouse 兼容性容器](images/desktop-to-uwp/container.png)
</div>
Cloudhouse 提供了一个自动打包工具，它适用于任何应用程序并可通过在应用当前运行的位置（例如 Windows XP）将其打包来创建兼容性容器。 然后，该容器可通过与桌面桥转换器工具集成以创建 Windows 应用包，从而转换为新的 UWP 格式。

该自动打包工具使用安装/捕获和运行时分析来为应用程序创建一个容器，其中包括应用程序文件、注册表以及允许该应用程序在 Windows 10 上运行的兼容性和重定向引擎。 此外，你还可以包含、隔离运行该应用程序所需的任何运行时或必备软件，以免它影响其他已经安装的应用程序或运行时或与之发生冲突。


请查看其宣布支持通用 Windows 应用和适用于企业的 Windows 应用商店的[博客](http://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp)。

#### <a name="firegiant"></a>FireGiant

[FireGiant Appx 扩展](https://www.firegiant.com/products/wix-expansion-pack/appx)可用来基于相同 WiX 源代码同时创建 Windows 应用包和 MSI 程序包。 每次创建时，对于 Windows 应用包和较早版本的 Windows with MSI，都面向 Windows 10 中的桌面桥进行创建。
<div style="float: left; padding: 10px; width: 20%">
     ![Advanced Installer 徽标](images/desktop-to-uwp/FG3rdPartyLogo.png)
</div>
FireGiant Appx 扩展使用对 WiX 项目的静态分析和智能模拟来创建 Windows 应用包，而无需容器或虚拟机的磁盘空间和运行时开销。

因为 FireGiant Appx 扩展不会通过运行安装程序来转换它，你可以保留 WiX 安装程序，而无需重复将其转换为 Windows 应用包。 你的使用不同版本 Windows 的所有用户都可获取最新改进功能，你无需担心 MSI 和 Windows 应用包不同步。

查看此[视频](https://www.youtube.com/watch?v=AFBpdBiAYQE)，了解 CEO Rob Mensching 如何通过几行 FireGiant 代码创建 Appx（Windows 应用包）版本的热门开源 7-Zip 压缩工具，然后他又如何通过更改相同 WiX 源代码改进了 Windows 应用和 MSI 包。

#### <a name="installaware"></a>InstallAware

Install**Aware** 为 2012-2017 版本的 Visual Studio 提供了免费的 Install**Aware** 扩展。 你可以直接从 [Visual Studio 工具栏](https://www.installaware.com/visual-studio-installer-2015.htm)单击一下，就可使用它们来创建 Windows 应用包。
<div style="float: left; padding: 10px; width: 20%">
    ![FireGiant 徽标](images/desktop-to-uwp/installaware.png)
</div>
即使你没有设置的源代码，你也可以通过使用 Package**Aware** 或数据库导入向导（适用于所有 MSI 安装程序和 MSM 合并模块）导入任何设置（无快照设置捕获）。 你可以使用 [GUI 工具](https://www.installaware.com/scripting-two-way-integrated-ide.htm) 来以可视形式或脚本形式维护和增强你的导入。

[高级 APPX 创建选项](https://www.installaware.com/mhtml5/desktop/appx.htm)可帮助你应对 Windows 应用商店提交，或生成签名 Windows 应用包二进制文件以旁加载形式分发给最终用户。 你甚至可以生成从单个来源定向部署到 **Nano 服务器** 的 **WSA**（Windows Server 应用程序）安装程序包，并且，除了 GUI 之外，还完全支持[命令行自动化](https://www.installaware.com/scripting-automation-interface.htm)。

另外，通过 GNU Affero GPL 许可证安装 **Aware** [开源](https://www.installaware.com/gnu.asp)**APPX 生成器库** 以及命令行小应用程序示例。 这些都是专为用于开源平台（如 WiX）而设计。

#### <a name="installshield"></a>InstallShield

InstallShield 提供单一解决方案来开发 MSI 与 EXE 安装程序、创建通用 Windows 平台 (UWP) 和 Windows Server App (WSA) 包，以及虚拟化应用程序，而所需的脚本编写、编码和改编工作极少。
<div style="float: left; padding: 10px; width: 20%">
    ![InstallShield 徽标](images/desktop-to-uwp/InstallShield-logo.jpg)
</div>
在数秒内扫描你的 InstallShield 项目，通过自动识别你的应用程序与 UWP 及 WSA 包之间潜在的兼容性问题，省去数小时的调查工作。

通过基于现有 InstallShield 项目生成 UWP 应用包，为 Windows 应用商店做准备，并简化软件在 Windows 10 上的安装体验。 生成 Windows Installer 和 UWP 应用包，以支持你的所有客户所需的部署方案。 通过基于现有 InstallShield 项目生成 WSA 包，支持 Nano Server 和 Windows Server 2016 部署。

以模块形式开发你的安装，以便更轻松地进行部署和维护，然后在生成时将各个组件和依赖项合并到单个 UWP 应用包中，通过 Windows 应用商店分发。 要在应用商店之外直接分发，请将你的 UWP 应用包和其他依赖项与套件/高级 UI 安装程序捆绑到一起。

在此[电子书](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0)中了解详细信息。


#### <a name="rad-studio"></a>RAD Studio

了解 [Embarcadero 的 RAD Studio](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

## <a name="integrate"></a>集成

如果你的应用需要与系统集成（例如：建立防火墙规则），请在程序包清单中指出集成任务，系统将完成其余操作。 对于其中大多数任务，你根本不必编写任何代码。 在清单中添加 XML，你就可以执行一些操作，如在用户登录时启动进程、将应用集成到“文件资源管理器”中，以及为应用添加显示在其他应用中的打印目标列表。

请参阅[将应用与 Windows 10 集成（Windows 桌面桥）](desktop-to-uwp-extensions.md)。

## <a name="enhance"></a>增强

将应用打包后，就可以通过添加动态磁贴和推送通知等功能对应用进行完善。 其中一些功能可显著提高应用的参与度，并且只需花费很少的时间即可添加。 某些增强功能需要更多的代码。

请参阅[增强用于 Windows 10 的桌面应用程序](desktop-to-uwp-enhance.md)。

## <a name="extend"></a>扩展

一些 Windows 10 体验（例如：启用触摸功能的 UI 页面）必须在现代应用容器内运行。 一般情况下，首先应确定是否可以通过 UWP API [增强](desktop-to-uwp-enhance.md)现有桌面应用程序来增加你的体验。 如果你必须使用 UWP 组件来可实现此体验，则可以将 UWP 项目添加到你的解决方案中，并使用应用服务在桌面应用和 UWP 组件之间进行通信。

请参阅[使用现代 UWP 组件扩展桌面应用程序](desktop-to-uwp-extend.md)。

## <a name="migrate"></a>迁移

你可将较早的代码逐步迁移到 UWP，同时仍然能够在 Windows 桌面上运行和发布应用。 完全迁移到 UWP（并且应用不再包含任何 WPF/Win32 组件）后，你可以覆盖所有 Windows 设备，包括手机、Xbox One 和 HoloLens。

## <a name="test"></a>测试

若要在准备分发时在现实环境中测试应用，最好对应用进行签名，然后安装它。 请参阅[测试你的应用](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-debug#test-your-app)。

>[!IMPORTANT]
> 如果你打算将应用发布到 Windows 应用商店，请确保它可在运行 Windows 10 S 的设备上正常运行。这是应用商店的要求。 请参阅[测试适用于 Windows 10 S 的 Windows 应用](desktop-to-uwp-test-windows-s.md)。

## <a name="validate"></a>验证

要让你的应用具备在 Windows 应用商店中发布或通过 [Windows 认证](http://go.microsoft.com/fwlink/p/?LinkID=309666)的最大可能性，请在提交应用进行认证之前先在本地进行验证和测试。

如果要使用 DAC 将你的应用打包，则可以使用新的 ``-Verify`` 标志来根据桌面桥和应用商店要求验证应用包。 请参阅[将应用打包、对应用进行签名并为应用商店提交做好准备](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters)。

如果你使用的是 Visual Studio，则可以通过**创建应用包**向导验证应用。 请参阅[创建应用包](../packaging/packaging-uwp-apps.md#create-an-app-package)。

若要手动运行该工具，请参阅 [Windows 应用认证工具包](../debug-test-perf/windows-app-certification-kit.md)。

若要查看 Windows 应用认证用于验证应用的测试列表，请参阅 [Windows 桌面桥应用测试](../debug-test-perf/windows-desktop-bridge-app-tests.md)。

## <a name="distribute"></a>分发

可以通过将应用发布到 Windows 应用商店或将其旁加载到其他系统来分发应用。

请参阅[分发已打包的桌面应用（桌面桥）](desktop-to-uwp-distribute.md)。

## <a name="support-and-feedback"></a>支持和反馈

**查找特定问题的答案**

我们的团队会监控这些 [StackOverflow 标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**提供关于本文的反馈**

请使用下面的评论区。

## <a name="in-this-section"></a>本部分内容

| 主题 | 描述 |
|-------|-------------|
| [准备将应用打包](desktop-to-uwp-prepare.md) | 提供在将应用打包之前要检查的项目列表。 |
| [使用 Desktop App Converter 将应用打包（桌面桥）](desktop-to-uwp-run-desktop-app-converter.md) | 演示如何运行 Desktop App Converter。 |
| [手动将应用打包（桌面桥）](desktop-to-uwp-manual-conversion.md) | 了解如何手动创建应用包和清单。 |
| [使用 Visual Studio 将 .NET 应用打包（桌面桥）](desktop-to-uwp-packaging-dot-net.md)| 演示如何使用 Visual Studio 将应用打包。 |
| [将应用与 Windows 10 集成（桌面桥）](desktop-to-uwp-extensions.md) | 通过在打包项目的程序包清单文件描述任务，将你的应用与 Windows 10 集成。 |
| [增强用于 Windows 10 的桌面应用程序](desktop-to-uwp-enhance.md)| 使用 UWP API 来添加为 Windows 10 用户而设的现代化体验。 |
| [适用于打包桌面应用（桌面桥）的 UWP API](desktop-to-uwp-supported-api.md) | 查看哪些 UWP API 可供已打包的桌面应用使用。 |
| [使用新式 UWP 组件扩展桌面应用程序](desktop-to-uwp-extend.md)| 添加必须在 UWP 应用容器内运行的高级体验。 使用应用服务将你的桌面应用与 UWP 进程连接。|
| [运行、调试和测试打包的桌面应用（桌面桥）](desktop-to-uwp-debug.md) | 说明用于调试已打包应用的选项。 |
| [分发打包的桌面应用（桌面桥）](desktop-to-uwp-distribute.md) | 查看如何向用户分发已转换的应用。  |
| [在桌面桥幕后（桌面桥）](desktop-to-uwp-behind-the-scenes.md) | 深入研究桌面到 UWP 桥的深层次工作原理。 |
| [已知问题（桌面桥）](desktop-to-uwp-known-issues.md) | 列出桌面到 UWP 桥的已知问题。 |
| [桌面桥代码示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | GitHub 上演示已转换应用的功能的代码示例。 |
