---
author: laurenhughes
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: "打包 UWP 应用"
description: "要分发或销售你的通用 Windows 平台 (UWP) 应用，你需要为其创建一个应用包。"
ms.author: lahugh
ms.date: 08/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: c6aa08c8c2e62bfed388000944de5520cbe58501
ms.sourcegitcommit: 63c815f8c6665872987b5410cabf324f2b7e3c7c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/10/2017
---
# <a name="package-a-uwp-app-with-visual-studio"></a>使用 Visual Studio 打包 UWP 应用

\[ 已针对 Windows 10 上的 UWP 应用进行更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

要销售你的通用 Windows 平台 (UWP) 应用或将其分发给其他用户，你需要为其创建一个 UWP 应用包。 如果你不想通过应用商店分发应用，也可以将应用包直接旁加载到设备上。 本文介绍使用 Visual Studio 配置、创建和测试 UWP 应用包的过程。 有关旁加载企业和业务线 (LOB) 应用的详细信息，请参阅[在 Windows10 中旁加载 LOB 应用](https://docs.microsoft.com/windows/application-management/sideload-apps-in-windows-10)。

在 Windows 10 中，你可以上传应用包 (.appx)、应用程序包 (.appxbundle) 或完整的上传文件 (.appxupload)。 .appxupload 文件是一个包含应用包或应用程序包及其他重要调试信息的文件夹。 我们强烈建议你提交 .appxupload 文件。 将应用包提交到应用商店后，你的应用就可以在任意 Windows 10 设备上安装和运行了。 

以下是准备和创建应用包的步骤概述。

1.  [在打包应用前](#before-packaging-your-app)。 按照这些步骤来确保你的应用可以随时打包以进行应用商店提交。
2.  [配置应用包](#configure-an-app-package)。 使用清单设计器配置程序包。 例如，添加磁贴图像并选择应用支持的方向。
3.  [创建应用包](#create-an-app-package)。 使用 Visual Studio 应用包向导创建应用包。 然后使用 Windows 应用认证工具包认证你的程序包。
4.  [旁加载应用包](#sideload-your-app-package)。 将应用旁加载到设备后，你可以测试该应用是否能够正常运行。

完成上述步骤之后，你可以随时在应用商店中分发你的应用。 如果你不打算出售仅供内部用户使用的业务线 (LOB) 应用，你可以旁加载该应用以在任一 Windows 10 设备上安装它。

## <a name="before-packaging-your-app"></a>在打包应用前

1.  **测试你的应用。** 在将你的应用打包以进行应用商店提交前，请确保它可以在你计划支持的所有设备系列上按预期运行。 这些设备系列可能包括桌面设备、移动设备、Surface Hub、Xbox、IoT 设备或其他设备。
2.  **优化你的应用。** 你可以使用 Visual Studio 的分析和调试工具来优化你的 UWP 应用的性能。 例如，用于 UI 响应能力的时间线工具、内存使用工具、CPU 使用工具等。 有关这些工具的详细信息，请参阅[运行诊断工具（不调试）](https://msdn.microsoft.com/library/dn957936.aspx)。
3.  **检查 .NET Native 兼容性（对于 VB 和 C# 应用）。** 在通用 Windows 平台中，有一个本机编译器可以提升应用的运行时性能。 通过这项更改，强烈建议你在此编译环境中测试你的应用。 默认情况下，**Release** 版本配置会启用 .NET 本机工具链，因此请务必使用此 **Release** 配置测试应用并检查应用是否按预期运行。 [调试 .NET Native Windows 通用应用](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx)对在使用 .NET Native 时可能遇到的一些常见调试问题做了更详细的说明。

## <a name="configure-an-app-package"></a>配置应用包

应用清单文件 (Package.appxmanifest.xml) 是一个 XML 文件，其中包含创建应用包所需的属性和设置。 例如，清单文件中的属性描述了要用作应用的磁贴的映像和应用在用户旋转设备时支持的方向。

Visual Studio 的清单设计器让你能够更新清单文件，而无需编辑文件的原始 XML。

**使用清单设计器配置程序包**

1.  在**解决方案资源管理器**中，展开你的 UWP 应用的项目节点。
2.  双击 **Package.appxmanifest** 文件。 如果清单文件已经在 XML 代码视图中打开，Visual Studio 会提示你关闭该文件。
3.  现在，你可以确定如何配置你的应用。 每个选项卡都包含可以配置的有关应用的信息，以及更多信息的链接（如果需要）。<br/>
    ![Visual Studio 中的清单设计器](images/packaging-screen1.jpg)

    检查是否在**视觉资源**选项卡上拥有 UWP 应用所需的所有图像。

    你可以从**打包**选项卡中输入发布数据。 你可以从此位置选择用于对你的应用进行签名的证书。 所有 UWP 应用都必须使用证书进行签名。 为了旁加载应用包，你需要信任该程序包。 证书必须安装在该设备上才能信任此程序包。 有关旁加载的详细信息，请参阅[启用设备进行开发](https://msdn.microsoft.com/library/windows/apps/Dn706236)。

4.  在你对应用进行必要的编辑后，请保存你的文件。

Visual Studio 可以将你的程序包与应用商店相关联。 当执行此操作时，会自动更新清单设计器的“打包”选项卡中的某些字段。

## <a name="create-an-app-package"></a>创建应用包

要通过应用商店分发应用，你必须创建应用包 (.appx)、应用程序包 (.appxbundle) 或上传程序包 (.appxupload)。 你可以通过使用**创建应用包**向导执行该操作。 按照以下步骤使用 Visual Studio 来创建适用于应用商店提交的程序包。

**创建应用包**

1.  在**解决方案资源管理器**中，打开适用于你的 UWP 应用项目的解决方案。
2.  右键单击该项目，然后依次选择**应用商店**->**创建应用包**。 如果此选项处于禁用状态或根本没有显示，请检查该项目是否为 UWP 项目。<br/>
    ![上下文菜单，可导航到“创建应用包”](images/packaging-screen2.jpg)

    将显示**创建应用包**向导。

3.  在第一个询问是否希望生成要上传到 Windows 应用商店的程序包的对话框中选择“是”，然后单击“下一步”。<br/>
    ![显示的“创建程序包”对话框窗口](images/packaging-screen3.jpg)

    如果你在此处选择“否”，Visual Studio 将不会生成用于应用商店提交的所需 .appxupload 程序包。 如果你仅希望旁加载应用以在内部设备上运行它，那么你可以选择此选项。 有关旁加载的详细信息，请参阅[启用设备进行开发](https://msdn.microsoft.com/library/windows/apps/Dn706236)。

4.  使用你的开发者帐户登录到 Windows 开发人员中心。 （如果你还没有开发者帐户，该向导将帮助你创建一个。）
5.  为你的程序包选择应用名称，如果你尚未在 Windows 开发人员中心门户中保留应用名称，也可以保留一个新名称。<br/>
    ![使用显示的应用名称选择创建应用包窗口](images/packaging-screen4.jpg)
6.  请确保选择**选择并配置程序包**对话框中的全部三个体系结构配置（x86、x64 和 ARM）。 这样，你的应用就可以部署到尽可能多的设备上。 在**生成应用程序包**列表框中，选择**始终**。 这使应用商店提交流程更加简单，因为你只需要上传一个文件 (.appxupload)。 单个程序包将包含所有要部署到设备（带有各自处理器体系结构）的必需程序包，以及重要的调试和崩溃分析信息。 要了解有关各种设备使用的程序包体系结构的详细信息，请参阅[应用包架构](https://docs.microsoft.com/windows/uwp/packaging/device-architecture)。<br/>
    ![使用显示的程序包配置创建应用包窗口](images/packaging-screen5.jpg)
7.  我们强烈建议你包括完整的 PDB 符号文件，以便从 Windows 开发人员中心获取最佳的[故障分析](http://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/)体验。 你可以通过访问[使用符合调试](https://msdn.microsoft.com/library/windows/desktop/Ee416588)了解有关使用符号调试的详细信息。
8.  现在你可以配置详细信息来创建你的程序包。 当你准备好发布应用时，你可以从输出位置上传程序包。
9.  单击**创建**以生成 appxupload 程序包。
10. 现在，你将看到此对话框。<br/>
    ![显示的程序包创建完成窗口，带有验证选项](images/packaging-screen6.jpg)

    在你将应用提交到应用商店以在本地或远程计算机上进行认证之前，验证你的应用。 你只能验证应用包的发布版本而不是调试版本。

11. 若要在本地进行验证，请选择**本地计算机**选项，并单击**启动 Windows 应用认证工具包**。 有关使用 Windows 应用认证工具包测试应用的详细信息，请参阅 [Windows 应用认证工具包](https://msdn.microsoft.com/library/windows/apps/Mt186449)。

    Windows 应用认证工具包会执行多种测试并返回结果。 有关更具体的信息，请参阅 [Windows 应用认证工具包测试](https://msdn.microsoft.com/library/windows/apps/mt186450)。

    如果你拥有要用于测试的远程 Windows 10 设备，你将需要在该设备上手动安装 Windows 应用认证工具包。 下一节将指导你完成这些步骤。 完成该操作后，你可以选择**远程计算机**并单击**启动 Windows 应用认证工具包**，以连接到远程设备并运行验证测试。

12. 在 WACK 完成并且你的应用通过测试后，你可以随时将应用提交到应用商店。 请确保上传正确的文件。 可以在你的解决方案的根文件夹 \\\[AppName\]\\AppPackages 中找到它，并且它将以 .appxupload 文件扩展名（或 .appx/.appxbundle 扩展名）结束。 名称将采用 \[AppName\]\_\[AppVersion\]\_x86\_x64\_arm\_bundle 格式。

**在远程 Windows 10 设备上验证你的应用包**

1.  按照[启用设备进行开发](https://msdn.microsoft.com/library/windows/apps/Dn706236)说明启用 Windows 10 设备进行开发。
    **重要提示**  不能在适用于 Windows 10 的远程 ARM 设备上验证应用包。
2.  下载并安装适用于 Visual Studio 的远程工具。 这些工具用于远程运行 Windows 应用认证工具包。 你可以通过访问[在远程计算机上运行 Windows 应用商店应用](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor)获取有关这些工具的详细信息，包括下载位置。
3.  下载所需的 [Windows 应用认证工具包](http://go.microsoft.com/fwlink/p/?LinkID=309666)，然后将它安装在远程 Windows 10 设备上。
4.  在向导的**程序包创建已完成**页面上，选择**远程计算机**选项按钮，然后选择**测试连接**按钮旁边的省略号按钮。
    **注意**  仅当你至少选择了一种支持验证的解决方案配置时，**远程计算机**选项按钮才可用。 有关使用 WACK 测试应用的详细信息，请参阅 [Windows 应用认证工具包](https://msdn.microsoft.com/library/windows/apps/Mt186449)。
5.  指定你的子网内部的设备规格，或提供你的子网外部的设备的域名服务器 (DNS) 名称或 IP 地址。
6.  在**身份验证模式**列表中，如果你的设备没有要求你使用 Windows 凭据登录该设备，请选择**无**。
7.  选择**选择**按钮，然后选择**启动 Windows 应用认证工具包**按钮。 如果远程工具正在该设备上运行，Visual Studio 将与其连接，然后执行验证测试。 请参阅 [Windows 应用认证工具包测试](https://msdn.microsoft.com/library/windows/apps/mt186450)。

## <a name="sideload-your-app-package"></a>旁加载应用包

Windows 10 周年更新引入了双击应用包文件安装应用包的简单方法。 你只需要找到应用包 (.appx) 或应用程序包 (.appxbundle) 文件，然后双击它。 应用安装程序启动并提供基本的应用信息及安装按钮、安装进度条和任何相关的错误消息。 

![安装名为 Contoso 的示例应用时的应用安装程序显示](images/appinstaller-screen.png)

> [!NOTE]
> 应用安装程序假定设备信任此应用。 如果你旁加载开发人员或企业应用，则需要向存储在设备上的“受信任的根证书颁发机构”安装签名证书。 如果你不确定如何执行此操作，请参阅[安装测试证书](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates)。

### <a name="sideload-your-app-on-previous-versions-of-windows"></a>在以前版本的 Windows 上旁加载应用
对于 UWP 应用包，应用不会像桌面应用那样安装到设备上。 通常，你会从应用商店下载 UWP 应用，这也会将应用安装到你的设备上。 你可以在不将应用提交到应用商店的情况下安装应用（旁加载）。 这允许你使用你所创建的应用包 (.appx) 安装和测试应用。 如果你有一款不想在应用商店中出售的应用（如业务线 (LOB) 应用），你可以旁加载该应用，以便你的公司中的其他用户也可以使用它。

下面的列表提供了旁加载应用的要求。

-   你必须[启用你的设备进行开发](https://msdn.microsoft.com/library/windows/apps/Dn706236)。
-   若要在 Windows 10 移动版设备上旁加载你的应用，请使用 [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) 工具。

**将应用旁加载到台式机、笔记本电脑或平板电脑**

1.  将要安装的应用版本的文件夹复制到目标设备。

    如果你已创建应用程序包，那么你将拥有一个基于版本号的文件夹和一个 `*\_Test` 文件夹。 例如以下两个文件夹（其中要安装的版本是 1.0.2.0）：

    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0`
    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test`

    如果你没有应用程序包，可以复制正确体系结构的文件夹和相应的 `*\_Test` 文件夹。 这两个文件夹是具有 x64 体系结构及其 `*\_Test` 文件夹的应用包的示例：

    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64`
    -   `C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64\_Test`

2.  在目标设备上，打开 `*\_Test` 文件夹。
3.  右键单击 **Add-AppDevPackage.ps1** 文件。 选择**使用 PowerShell 运行**并按照提示操作。<br/>
    ![显示的文件资源管理器，已导航到 PowerShell 脚本](images/packaging-screen7.jpg)

    安装应用包后，PowerShell 窗口显示消息：**已成功安装你的应用。**

    **使用技巧**：若要打开平板电脑上的快捷方式菜单，请触摸屏幕上你想要右键单击的位置，按住直到出现一个整圆，然后抬起手指。 快捷菜单将在你抬起手指时打开。
4.  单击“开始”按钮，搜索应用名称，然后启动它。
