---
author: normesta
Description: Create a modern Windows app package for your existing Windows Forms, WPF, or Win32 app or game. Add modern experiences for Windows 10 users and simplify deployment and monetization.
Search.Product: eADQiWindows 10XVcnh
title: 打包桌面应用程序
ms.author: normesta
ms.date: 09/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.openlocfilehash: e08856006bc3ed6348a6ded69421ef0be9fc69e5
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2018
ms.locfileid: "5926358"
---
# <a name="package-desktop-applications-desktop-bridge"></a>打包桌面应用程序 （桌面桥）

获取现有桌面应用程序，并为 Windows 10 用户增加现代体验。 然后，将应用发布到 Microsoft Store，以拓展国际市场。 通过利用应用商店中内置的功能，你可以获取你的应用程序利润以更简单的方式。 当然，并非必须使用 Store。 你完全可以使用现有的渠道。

![桌面桥](images/desktop-to-uwp/desktop-bridge-4.png)

在桌面应用程序创建程序包时，你的应用程序将获取标识和桌面应用程序与该标识，已向 Windows 通用平台 (UWP) Api 的访问权限。 你可以使用它们添加具有吸引力的现代体验，例如动态磁贴和通知。  使用简单的条件编译和运行时检查来仅当你的应用程序运行在 Windows 10 上运行 UWP 代码。

除了用于添加 Windows 10 体验的代码中，你的应用程序保持不变并且你可以继续将它分发给你的现有 Windows 7、 Windows Vista 或 Windows XP 用户群。 在 Windows 10 中，你的应用程序继续运行以完全信任用户模式下，就像它如今。

>[!IMPORTANT]
>创建 Windows 应用包的桌面应用程序的功能 （Windows 10 版本 1607年中引入了称为桌面桥，否则，它仅用于在项目中面向 Windows 10 周年更新 (10.0;内部版本 14393） 或更高版本的 Visual Studio。

> [!NOTE]
> 请观看 Microsoft Virtual Academy 发布的<a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">这一系列</a>的视频短片。 这些视频演示了将桌面应用程序到通用 Windows 平台 (UWP) 的整个过程。

## <a name="benefits"></a>好处

以下是为你的 Windows 桌面应用程序创建 Windows 应用包的一些原因：

:heavy_check_mark: **简化部署**。 使用此桥的应用和游戏都具有出色的部署体验。 此体验确保用户可以放心地安装应用程序并对其进行更新。 如果用户选择卸载应用，则将完全删除它，不会留下任何痕迹。 这能减少编写设置体验的时间，并使用户使用最新应用。

:heavy_check_mark: **自动更新和许可**。 你的应用程序可以参与 Microsoft Store 的内置许可和自动更新设施。 自动更新是高度可靠和高效的机制，因为仅下载文件的已更改部分。

:heavy_check_mark: **扩大覆盖范围并简化盈利过程**。 选择通过 Microsoft Store 进行分发可将覆盖范围扩大至数以百万的 Windows10 用户，这些用户可以通过当地支付选项获取应用、游戏和应用内商品。

:heavy_check_mark: **添加 UWP 功能**。  你可以按照自己的节奏向应用的程序包添加 UWP 功能，如 XAML 用户界面、动态磁贴更新、UWP 后台任务、应用服务以及许多其他功能。

:heavy_check_mark: **拓展跨设备用例**。 通过使用桥，可以将代码逐渐迁移到通用 Windows 平台，以覆盖所有的 Windows10 设备，包括手机、Xbox One 和 HoloLens。

若要查看更完整的优势列表，请参阅[桌面桥](https://developer.microsoft.com/windows/bridges/desktop)。

## <a name="prepare"></a>准备

首先，准备你的应用程序，查看文章[准备打包桌面应用](desktop-to-uwp-prepare.md)，然后解决任何之前为其创建 Windows 应用包应用于你的应用程序的问题。 你可能不需要创建包之前对应用程序进行很多更改。 但是，有一些情况下可能需要进行调整你的应用程序之前为其创建程序包。

<a id="convert" />

## <a name="package"></a>包

以下是一些可用于为应用创建 Windows 应用包的工具。

### <a name="desktop-app-converter"></a>Desktop App Converter

尽管此工具名称中出现了“Converter”一词，但它实际上并不转换你的应用。 你的应用程序保持不变。 但是，此工具将为你生成 Windows 应用包。 你的应用程序进行了大量系统修改或者如果你不确定你的安装程序的用途，则它可以是在情况下非常方便。

Desktop App Converter 转换到的虚拟文件和注册表系统将使用你的应用程序的打包的版本安装程序的操作。 Desktop App Converter 还可以为你执行一些额外的操作。 以下是其中一些例子。

:heavy_check_mark: 自动注册预览控件、缩略图处理程序、属性处理程序、防火墙规则、URL 标志。

:heavy_check_mark: 自动注册使用户能够使用“文件资源管理器”中的**种类**列对文件进行分组的文件类型映射。

:heavy_check_mark: 为你注册公用 COM 服务器。

: heavy_check_mark： 生成可用于运行你的应用的证书。

: heavy_check_mark： 验证你的应用程序针对已打包的桌面应用程序和 Microsoft Store 的要求。

使用 Desktop App Converter 的另一个绝佳理由是通过除 Visual Studio 之外的不同的开发环境维护你的应用程序。 即使你的应用程序没有安装程序，你可以使用 Desktop App Converter。

查看[程序包的桌面应用程序使用 Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md)

### <a name="visual-studio"></a>Visual Studio

如果你通过除 Visual Studio 之外的开发环境来维护应用，但没有应用安装程序或安装程序无法执行太多复杂任务，请考虑使用 Visual Studio。

借助 Visual Studio 可以非常轻松地创建包。 只需添加打包项目，参考桌面项目，再按 F5 进行应用调试。 无需手动调整。 相比于使用以往版本的 Visual Studio 的体验，新的简洁体验是一个巨大的改进。 以下是你可以利用它执行的一些其他操作。

:heavy_check_mark: 自动生成可视资源。

:heavy_check_mark: 使用可视设计器对清单进行更改。

:heavy_check_mark: 使用向导生成包。

: heavy_check_mark： 轻松地将标识从已保存在 Windows 开发人员中心仪表板中的名称分配给你的应用程序。

请参阅[包使用 Visual Studio 的桌面应用程序](desktop-to-uwp-packaging-dot-net.md)

### <a name="third-party-installer"></a>第三方安装程序

 多个热门的第三方产品和安装程序现在都支持打包的桌面应用程序的功能。 只需单击几下即可使用它们来生成 MSI 安装程序或应用包。 我们不提供有关如何使用这些工具的文档，不过你可以访问它们的网站以了解详细信息。

#### <a name="advanced-installer"></a>Advanced Installer

Caphyon 提供基于 GUI 的免费桌面应用打包工具。使用此工具，你只需单击几下即可为应用程序生成 Windows 应用包。 它可以使用任何安装程序;即使在静默模式下运行，将执行的验证检查来确定是否适用于打包应用程序。
Desktop App Converter 还与 Hyper-V 和 [VMware](http://www.vmware.com/) 集成。 这意味着你可以使用自己的虚拟机，而无需下载大小可能超过 3GB 的匹配的 [Docker](https://docs.docker.com/) 映像。

<img width="20%" src="images/desktop-to-uwp/Advanced_Installer_Vertical.png">

可以使用 [Advanced Installer](http://www.advancedinstaller.com/) 从现有项目中生成 MSI 和 [Windows 应用包](http://www.advancedinstaller.com/uwp-app-package.html)。 还可以使用 Advanced Installer 导入使用 Microsoft Desktop App Converter 生成的 Windows 应用包。 导入后，可以使用专为 UWP 应用设计的可视化工具对它们进行维护。

Advanced Installer 还提供用于 Visual Studio 2017 和 2015 的扩展，可用于[生成和调试桌面桥应用](http://www.advancedinstaller.com/debug-desktop-bridge-apps.html)。

请观看此[视频](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be)进行简要了解。

> [!TIP]
> 请务必检查最近发布的 [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html)。

#### <a name="cloudhouse-compatibility-containers"></a>Cloudhouse 兼容性容器

对于其业务线应用程序与 Windows 10 和 10 S 不兼容的企业客户，Cloudhouse 的兼容性容器支持在 Window 10 上运行 Windows XP 和 7 应用，然后转换为在通用 Windows 平台 (UWP) 上运行，以便无需更改源代码就可通过适用于企业的 Microsoft Store 或 Microsoft InTune 交付。 注册即可[免费试用](http://www.cloudhouse.com/free-trial)。

<img width="20%" src="images/desktop-to-uwp/cloudhouse-container-logo.png">

Cloudhouse 提供了自动打包工具，用于将业务线应用程序打包到应用现在所运行的操作系统（如 Windows XP）上的[兼容性容器](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications)中，并[为转换为 UWP 做好准备](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905)。 该容器随后将通过与 Microsoft 的 Desktop App Converter 工具集成转换为新的 Windows 应用包格式。

该自动打包工具使用安装/捕获和运行时分析来为应用程序创建一个容器，其中包括应用程序文件、注册表、运行时、依赖项以及允许该应用程序在 Windows 10 上运行的兼容性和重定向引擎。 该容器为应用程序及其运行时提供隔离，从而使其影响用户设备上运行的其他应用程序或与之冲突。

有关如何通过适用于企业的 Microsoft Store 提供企业应用程序的详细信息，请阅读[版本博客](http://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp)。

#### <a name="firegiant"></a>FireGiant

[FireGiant Appx 扩展](https://www.firegiant.com/products/wix-expansion-pack/appx)可用来基于相同 WiX 源代码同时创建 Windows 应用包和 MSI 程序包。 每次你生成时，你可以使用 Windows 应用包和早期版本的 Windows msi 面向 Windows 10。

<img width="20%" src="images/desktop-to-uwp/FG3rdPartyLogo.png">

FireGiant Appx 扩展使用对 WiX 项目的静态分析和智能模拟来创建 Windows 应用包，而无需容器或虚拟机的磁盘空间和运行时开销。

因为 FireGiant Appx 扩展不会通过运行安装程序来转换它，你可以保留 WiX 安装程序，而无需重复将其转换为 Windows 应用包。 你的使用不同版本 Windows 的所有用户都可获取最新改进功能，你无需担心 MSI 和 Windows 应用包不同步。

请查看此[视频](https://www.youtube.com/watch?v=AFBpdBiAYQE)，了解如何在代码通过几行 FireGiant CEO Rob Mensching 创建 Appx （Windows 应用包） 版本的热门开源 7-zip 压缩工具，然后他如何提高 Windows 应用程序和 MSI 包相同 WiX 源代码中的更改。

#### <a name="installaware"></a>InstallAware

安装 **Aware** 及快速支持 Microsoft 创新的[跟踪记录](https://www.installaware.com/press-room.htm)，通过单个来源构建 [Windows 应用包（桌面桥）](https://www.installaware.com/appx-builder.htm)、App-V（应用程序虚拟化）、MSI (Windows Installer) 和 EXE（本机代码）包。

<img width="20%" src="images/desktop-to-uwp/installaware.png">

Install**Aware** 为 2012-2017 版本的 Visual Studio 提供了免费的 Install**Aware** 扩展。 你可以直接从 [Visual Studio 工具栏](https://www.installaware.com/visual-studio-installer-2015.htm)单击一下，就可使用它们来创建 Windows 应用包。

即使你没有设置的源代码，你也可以通过使用 Package**Aware** 或数据库导入向导（适用于所有 MSI 安装程序和 MSM 合并模块）导入任何设置（无快照设置捕获）。 你可以使用 [GUI 工具](https://www.installaware.com/scripting-two-way-integrated-ide.htm) 来以可视形式或脚本形式维护和增强你的导入。

[高级 APPX 创建选项](https://www.installaware.com/mhtml5/desktop/appx.htm)可帮助你应对 Microsoft Store 提交，或生成签名 Windows 应用包二进制文件以旁加载形式分发给最终用户。 你甚至可以生成从单个来源定向部署到 **Nano 服务器** 的 **WSA**（Windows Server 应用程序）安装程序包，并且，除了 GUI 之外，还完全支持[命令行自动化](https://www.installaware.com/scripting-automation-interface.htm)。

另外，通过 GNU Affero GPL 许可证安装 **Aware** [开源](https://www.installaware.com/gnu.asp)**APPX 生成器库** 以及命令行小应用程序示例。 这些都是专为用于开源平台（如 WiX）而设计。

#### <a name="installshield"></a>InstallShield

InstallShield 提供单一解决方案来开发 MSI 与 EXE 安装程序、创建通用 Windows 平台 (UWP) 和 Windows Server App (WSA) 包，以及虚拟化应用程序，而所需的脚本编写、编码和改编工作极少。

<img width="20%" src="images/desktop-to-uwp/InstallShield-logo.jpg">

在数秒内扫描你的 InstallShield 项目，通过自动识别你的应用程序与 UWP 及 WSA 包之间潜在的兼容性问题，省去数小时的调查工作。

通过基于现有 InstallShield 项目生成 UWP 应用包，为 Microsoft Store 做准备，并简化软件在 Windows 10 上的安装体验。 生成 Windows Installer 和 UWP 应用包，以支持你的所有客户所需的部署方案。 通过基于现有 InstallShield 项目生成 WSA 包，支持 Nano Server 和 Windows Server 2016 部署。

以模块形式开发你的安装程序，以便更轻松地进行部署和维护，然后在生成时将各个组件和依赖项合并到单个 UWP 应用包中，通过 Microsoft Store 分发。 要在 Store 之外直接分发，请将你的 UWP 应用包和其他依赖项与套件/高级 UI 安装程序捆绑到一起。

在此[电子书](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0)中了解详细信息。

#### <a name="pace-suite"></a>PACE 套件

[PACE 套件](https://pacesuite.com/)是用于将你的桌面应用引入通用 Windows 平台的应用程序打包工具。

<img width="20%" src="images/desktop-to-uwp/PACE.png">

有了 PACE 套件，你无需再准备特殊的打包环境或安装额外的 Windows SDK 组件。 PACE 套件可以在 Windows 10 或 Windows Server 2016 下的标准包装环境中独立构建 Windows 应用包。 查看此[带图示例](https://pacesuite.com/convert-exe-to-appx/)以了解 PACE 套件如何将安装程序重新打包到 Windows 应用包。

除创建 Windows 应用包外，你还可以使用 PACE 套件创建 Windows Installer 包 (MSI)、修补程序 (MSP)、转换程序 (MST) 和 APP-V 包。 针对 MSI 创作，PACE 套件可帮助管理升级、权限设置、自定义操作和脚本等。 你可以将你的应用程序直接发布到 System Center Configuration Manager。

若要查看所有应用程序打包功能，请参阅 [PACE 套件功能](https://pacesuite.com/features/)。

#### <a name="rad-studio"></a>RAD Studio

查看 [Embarcadero 的 RAD Studio](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

#### <a name="raypack-studio"></a>RayPack Studio

Raynet 的打包解决方案， [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio)中，作为一种高效且易于配置的转换和重新打包框架的桌面应用程序支持创建的包。

<img width="20%" src="images/desktop-to-uwp/RaynetLogo_v3.png">

现有的虚拟环境（VMware 工作站、Hyper-V）可在不进行长时间环境设置的情形下用于执行自动/批量转换。 RayPack Studio 的组件，[RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)，可利用预转换屏蔽和兼容性测试来验证可用于转换的合格软件。 此外，用户现在可以对多种版本的 Windows 10（包括周年和创意者更新）进行全面的冲突性和兼容性检查。

除创建 Windows 10 APPX/UWP 格式软件包外，RayPack Studio 还可用于创建经典 Windows Installer 包 (MSI)、修补程序 (MSP)、转换程序 (MST) 和 APP-V 包。 此外，此解决方案附带一组软件产品和专业企业软件包组件。 除了软件打包和虚拟化，RayPack Studio 还可以完成所有打包相关任务：软件应用程序及软件包的冲突性和兼容性检查 ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad))、软件评估 ([RayEval](https://raynet.de/Raynet-Products/RayEval)) 和质量保证 ([RayQC](https://raynet.de/Raynet-Products/RayQC))。

通过与 [RayFlow](https://raynet.de/Raynet-Products/RayFlow)（Raynet 的企业工作流系统）结合使用，用户可以通过完整的企业应用程序生命周期（从程序包订购到评估、分析、打包、质量保证、用户接受度测试和部署）高效地开发软件。 所有的程序包和格式可被直接储存和部署到 SCCM 或其他解决方案中。 RayFlow 会对整个应用程序生命周期进程进行追踪和管理。 除此之外，RayFlow 还可以对 ServiceNow 等所有订单系统进行集成。 Raynet 利用面向服务提供者的工具在世界各地构建软件打包工厂。

说服自己获取 Raynet 提供的 RayPack Studio 和 RayFlow [免费试用版许可](https://raynet.de/contact?init=license)。 有关详细信息，请访问 [www.raynet.de](https://raynet.de/home)。

**相关链接**：

* Raynet：[https://raynet.de/home](https://raynet.de/home)
* RayPack Studio：[https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow：[https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval：[https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC：[https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced：[https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* 免费试用版许可：[https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)

### <a name="manual-packaging"></a>手动打包

作为最后一个选项，你可以不使用上述任何工具的情况下转换你的应用程序。 如果想精细地控制你的转换，则可以创建清单文件，然后运行 **MakeAppx.exe** 工具来创建 Windows 应用包。

请参阅[手动打包的桌面应用程序](desktop-to-uwp-manual-conversion.md)。

## <a name="integrate"></a>集成

如果你的应用程序需要与系统集成 (例如： 建立防火墙规则)，你的应用程序的程序包清单中指出，系统将完成其余部分。 对于其中大多数任务，你根本不必编写任何代码。 在清单中的 XML 位，你可以执行操作像在用户登录时启动进程、 将你的应用程序集成到文件资源管理器，并添加你的应用程序的其他应用中显示的打印目标列表。

请参阅[Windows 10 已打包的桌面应用程序集成](desktop-to-uwp-extensions.md)。

## <a name="enhance"></a>增强

将应用打包后，就可以通过添加动态磁贴和推送通知等功能对应用进行完善。 其中一些功能可显著提高你的应用程序的参与度，并且只需花费很少的时间来添加。 某些增强功能需要更多的代码。

请参阅[增强用于 Windows 10 的桌面应用程序](desktop-to-uwp-enhance.md)。

## <a name="extend"></a>扩展

一些 Windows 10 体验（例如：启用触摸功能的 UI 页面）必须在现代应用容器内运行。 一般情况下，首先应确定是否可以通过 UWP API [增强](desktop-to-uwp-enhance.md)现有桌面应用程序来增加你的体验。 如果你有使用 UWP 组件，以实现此体验，你可以将 UWP 项目添加到你的解决方案和使用应用服务在桌面应用程序和 UWP 组件之间进行通信。

请参阅[使用现代 UWP 组件扩展桌面应用程序](desktop-to-uwp-extend.md)。

## <a name="migrate"></a>迁移

虽然没有工具可以将桌面应用程序转换为 UWP 应用，但是，你可以重复使用大量现有代码来降低构建代码的成本。 你可以将尽可能多的业务逻辑移动到 .NET Standard 2.0 库，以便执行该操作。

.NET Standard 2.0 中包含数目大幅增加的 .NET API 以及适用于你最常用的 NuGet 程序包以及第三方库的兼容性填充码。

你可以将代码迁移到 .NET Standard 库中，然后创建通用 Windows 平台 (UWP) 应用以覆盖所有 Windows 10 设备。

请参阅[在桌面应用和 UWP 应用之间共享代码](desktop-to-uwp-migrate.md)


## <a name="test"></a>测试

若要在准备分发，请在现实环境中测试你的应用程序，最好是对你的应用程序进行签名，然后安装它。 请参阅[测试你的应用](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-debug#test-your-app)。

>[!IMPORTANT]
> 如果你打算发布到 Microsoft 应用商店应用程序，请确保你的应用程序在 S 模式下运行 Windows 10 的设备上正常运行。 这是 Microsoft Store 的要求。 请参阅[测试 Windows 应用是否适用于 S 模式下的 Windows 10](desktop-to-uwp-test-windows-s.md)。

## <a name="validate"></a>验证

若要为你的应用程序在 Microsoft 应用商店发布的最佳机会或[Windows 认证](http://go.microsoft.com/fwlink/p/?LinkID=309666)，验证在本地和测试之前提交应用进行认证。

如果你使用 DAC 打包应用，你可以使用新``-Verify``标志来根据的已打包的桌面应用程序和应用商店要求验证应用。 请参阅[将应用打包、对应用进行签名并为 Store 提交做好准备](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters)。

如果你使用 Visual Studio，你可以验证你的应用程序，从**创建应用包**向导。 请参阅[创建应用包上传文件](../packaging/packaging-uwp-apps.md#create-an-app-package-upload-file)。

若要手动运行该工具，请参阅 [Windows 应用认证工具包](../debug-test-perf/windows-app-certification-kit.md)。

若要查看 Windows 应用认证用于验证应用的测试列表，请参阅 [Windows 桌面桥应用测试](../debug-test-perf/windows-desktop-bridge-app-tests.md)。

## <a name="distribute"></a>分发

你可以分配你的应用程序通过发布到 Microsoft Store 或通过旁加载到其他系统它。

请参阅[分发打包的桌面应用](desktop-to-uwp-distribute.md)。

## <a name="support-and-feedback"></a>支持和反馈

**查找问题的答案**

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。

**提供反馈或提出功能建议**

请参阅 [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)。

## <a name="in-this-section"></a>本部分内容

| 主题 | 描述 |
|-------|-------------|
| [准备将应用打包](desktop-to-uwp-prepare.md) | 提供在将应用打包之前要检查的项目列表。 |
| [使用 Desktop App Converter 将应用打包](desktop-to-uwp-run-desktop-app-converter.md) | 演示如何运行 Desktop App Converter。 |
| [手动打包的桌面应用程序](desktop-to-uwp-manual-conversion.md) | 了解如何手动创建应用包和清单。 |
| [使用 Visual Studio 将桌面应用程序打包](desktop-to-uwp-packaging-dot-net.md)| 向你显示如何使用 Visual Studio 打包的桌面应用程序。 |
| [与 Windows 10 集成桌面应用程序](desktop-to-uwp-extensions.md) | 通过描述打包项目的程序包清单文件中的任务将与 Windows 10 集成你的应用程序。 |
| [增强用于 Windows 10 的桌面应用程序](desktop-to-uwp-enhance.md)| 使用 UWP API 来添加为 Windows 10 用户而设的现代化体验。 |
| [适用于已打包的桌面应用程序的 UWP Api](desktop-to-uwp-supported-api.md) | 查看哪些 UWP Api 可用于在已打包的桌面应用程序使用。 |
| [使用新式 UWP 组件扩展桌面应用程序](desktop-to-uwp-extend.md)| 添加必须在 UWP 应用容器内运行的高级体验。 通过使用应用服务连接与 UWP 进程的桌面应用程序。|
| [运行、 调试和测试打包的桌面应用程序](desktop-to-uwp-debug.md) | 说明用于调试已打包应用的选项。 |
| [分发打包的桌面应用程序 ](desktop-to-uwp-distribute.md) | 请参阅可如何分配给用户转换的应用程序。  |
| [已知的 Issues(desktop-to-uwp-known-issues.md) | 列出的已知问题的打包桌面应用程序。 |
