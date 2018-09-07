---
author: laurenhughes
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: 打包 UWP 应用
description: 要分发或销售你的通用 Windows 平台 (UWP) 应用，你需要为其创建一个应用包。
ms.author: lahugh
ms.date: 06/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: eb930c5e6b2c1c1f864f2e63fbce97c89bb89e1f
ms.sourcegitcommit: 53ba430930ecec8ea10c95b390fe6e654fe363e1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/06/2018
ms.locfileid: "3413692"
---
# <a name="package-a-uwp-app-with-visual-studio"></a>使用 Visual Studio 打包 UWP 应用

若要出售你的通用 Windows 平台 (UWP) 应用或将其分配给其他用户，你需要将其打包。 如果你不想通过 Microsoft Store 分发应用，也可以将应用包直接旁加载到设备上或通过 [Web 安装](installing-UWP-apps-web.md)分发它。 本文介绍使用 Visual Studio 配置、创建和测试 UWP 应用包的过程。 有关管理和部署业务线 (LOB) 应用的详细信息，请参阅[企业应用管理](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management)。

在 Windows 10 中，你可以向 Windows 开发人员中心提交应用包 (.appx)、应用程序包 (.appxbundle) 或完整的应用包上传文件 (.appxupload)。 在上述选项中，提交包上传文件将提供最佳的体验。 

## <a name="types-of-app-packages"></a>应用包类型

- **应用包 (.appx)**  
    包含应用的文件，其格式可以旁加载到设备上。 Visual Studio 创建的任何单一 .appx 包文件都**不适合**提交到开发人员中心，它们只能用于旁加载和测试目的。 如果你想将应用提交到开发人员中心，请使用应用包上传文件。  

- **应用程序包 (.appxbundle)**  
    应用程序包可以包含多个应用包，每个包都构建为支持特定的设备体系结构。 例如，一个应用程序包可以包含三个独立的应用包，它们分别用于 x86、x64 和 ARM 配置。 应尽可能生成应用程序包，因为它们使你的应用能够在尽可能广泛的设备上使用。  

- **应用包上传文件 (.appxupload)**  
    一个可以包含多个应用包或一个应用程序包以支持各种处理器体系结构的文件。 上传文件还包含一个符号文件，在 Microsoft Store 中发布应用后，你可以使用该符号文件[分析应用性能](https://docs.microsoft.com/windows/uwp/publish/analytics)。 如果你使用 Visual Studio 打包应用并且打算将其提交到开发人员中心进行发布，将自动为你创建该文件。 需要注意的是，这是**唯一**一种能够使用 Visual Studio 创建的有效的应用包开发人员中心提交。

以下是准备和创建应用包的步骤概述：

1.  [在打包应用前](#before-packaging-your-app)。 按照这些步骤来确保你的应用可以随时打包以进行开发人员中心提交。
2.  [配置应用包](#configure-an-app-package)。 使用 Visual Studio 清单设计器配置程序包。 例如，添加磁贴图像并选择应用支持的方向。
3.  [创建应用包上传文件](#create-an-app-package-upload-file)。 使用 Visual Studio 应用包向导创建应用包，然后使用 Windows 应用认证工具包验证程序包。
4.  [旁加载应用包](#sideload-your-app-package)。 将应用旁加载到设备后，你可以测试该应用是否按预期运行。

完成上述步骤之后，你可以随时分发你的应用。 如果你不打算出售仅供内部用户使用的业务线 (LOB) 应用，你可以旁加载该应用以在任一 Windows 10 设备上安装它。

## <a name="before-packaging-your-app"></a>在打包应用前

1.  **测试你的应用。** 在将你的应用打包以进行开发人员中心提交前，请确保它可以在你计划支持的所有设备系列上按预期运行。 这些设备系列可能包括桌面设备、移动设备、Surface Hub、Xbox、IoT 设备或其他设备。
2.  **优化你的应用。** 你可以使用 Visual Studio 的分析和调试工具来优化你的 UWP 应用的性能。 例如，用于 UI 响应能力的时间线工具、内存使用工具、CPU 使用工具等。 有关这些工具的详细信息，请参阅[分析功能教程](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour)主题。
3.  **检查 .NET Native 兼容性（对于 VB 和 C# 应用）。** 在通用 Windows 平台中，有一个本机编译器可以提升应用的运行时性能。 通过这项更改，你应在该编译环境中测试你的应用。 默认情况下，**Release** 版本配置会启用 .NET 本机工具链，因此请务必使用此 **Release** 配置测试应用并检查应用是否按预期运行。 [调试 .NET Native Windows 通用应用](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx)对在使用 .NET Native 时可能遇到的一些常见调试问题做了更详细的说明。

## <a name="configure-an-app-package"></a>配置应用包

应用清单文件 (Package.appxmanifest.xml) 是一个 XML 文件，其中包含创建应用包所需的属性和设置。 例如，应用清单文件中的属性描述了要用作应用的磁贴的映像和应用在用户旋转设备时支持的方向。

Visual Studio 的清单设计器让你能够更新清单文件，而无需编辑文件的原始 XML。

**使用清单设计器配置程序包**

1.  在**解决方案资源管理器**中，展开你的 UWP 应用的项目节点。
2.  双击 **Package.appxmanifest** 文件。 如果清单文件已经在 XML 代码视图中打开，Visual Studio 会提示你关闭该文件。
3.  现在，你可以确定如何配置你的应用。 每个选项卡都包含可以配置的有关应用的信息，以及更多信息的链接（如果需要）。  
    ![Visual Studio 中的清单设计器](images/packaging-screen1.jpg)

    检查是否在**视觉资源**选项卡上拥有 UWP 应用所需的所有图像。

    你可以从**打包**选项卡中输入发布数据。 你可以从此位置选择用于对你的应用进行签名的证书。 所有 UWP 应用都必须使用证书进行签名。 
    
    >[!IMPORTANT]
    >如果你是在 Microsoft Store 中发布应用，将使用你的受信任的证书进行签名。 这让用户能够安装和运行你的应用，而不必安装关联的应用签名证书。 
    
    如果你不打算发布应用，只想旁加载应用包，则你首先需要信任该程序包。 要信任该程序包，必须在用户设备上安装证书。 有关旁加载的详细信息，请参阅[启用设备进行开发](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。

4.  在你对应用进行必要的编辑后，请保存你的 **Package.appxmanifest** 文件。

如果你通过 Microsoft Store 分发应用，Visual Studio 可以将你的程序包关联到 Microsoft Store。 在关联应用时，会自动更新清单设计器的“打包”选项卡中的某些字段。

## <a name="create-an-app-package-upload-file"></a>创建应用包上传文件

要通过 Microsoft Store 分发应用，你必须创建应用包 (.appx)、应用程序包 (.appxbundle) 或上传程序包 (.appxupload)，并[将打包后的应用提交到开发人员中心](https://docs.microsoft.com/windows/uwp/publish/app-submissions)。 虽然可以单独向开发人员中心提交应用包或应用程序包，但我们鼓励你提交上传包。

>[!NOTE]
> 应用包上传文件 (.appxupload) 是**唯一**一种能够使用 Visual Studio 创建的对开发人员中心有效的应用包类型。 其他有效的[可以手动创建的应用包](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool)，无需使用 Visual Studio。 

你可以通过使用**创建应用包**向导执行该操作。 按照以下步骤使用 Visual Studio 来创建适用于开发人员中心提交的程序包。

**创建应用包上传文件**

1.  在**解决方案资源管理器**中，打开适用于你的 UWP 应用项目的解决方案。
2.  右键单击该项目，然后依次选择**应用商店**->**创建应用包**。 如果此选项处于禁用状态或根本没有显示，请检查该项目是否为通用 Windows 项目。  
    ![上下文菜单，可导航到“创建应用包”](images/packaging-screen2.jpg)

    将显示**创建应用包**向导。

3.  在第一个询问是否希望生成要上传到开发人员中心的程序包的对话框中选择“是”，然后单击“下一步”。  
    ![显示的“创建应用包”对话框窗口](images/packaging-screen3.jpg)

    如果选择“否”，Visual Studio 将不会生成用于开发人员中心提交的应用包上传 (.appxupload) 文件。 如果你仅希望旁加载应用以在内部设备上运行它或用于测试目的，那么你可以选择此选项。 有关旁加载的详细信息，请参阅[启用设备进行开发](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development)。
4.  使用你的开发者帐户登录到 Windows 开发人员中心。 如果你还没有开发者帐户，该向导将帮助你创建一个。
5.  为你的程序包选择应用名称，如果你尚未在 Windows 开发人员中心门户中保留应用名称，也可以保留一个新名称。  
    ![使用显示的应用名称选择创建应用包窗口](images/packaging-screen4.jpg)
6.  确保在 **Select and Configure Packages** 对话框中选择全部三种体系结构配置（x86、x64 和 ARM），以确保你的应用能够部署到最广泛的设备上。 在**生成应用程序包**列表框中，选择**始终**。 应用程序包 (.appxbundle) 优于单个应用包 (.appx)，因为它包含为每种类型的处理器体系结构配置的应用包集合。 当你选择生成应用程序包时，应用程序包将包含在最终的应用包上传 (.appxupload) 文件中，并带有调试和崩溃分析信息。 如果你不确定该选择哪种体系结构，或者想了解有关各种设备使用哪种体系结构的详细信息，请参阅[应用包体系结构](https://docs.microsoft.com/windows/uwp/packaging/device-architecture)。  
    ![显示的创建应用包窗口及包配置](images/packaging-screen5.jpg)


7.  发布应用后，请从 Windows 开发人员中心包含完整的 PDB 符号文件，以[分析应用性能](https://docs.microsoft.com/windows/uwp/publish/analytics)。 配置任何其他详细信息，例如版本编号或包输出位置。
9.  单击**创建**生成应用包。 如果你在步骤 3 中选择了**是**，并且正在为开发人员中心提交创建包，则向导将创建包上传 (.appxupload) 文件。 如果在步骤 3 中选择了**否**，向导将根据步骤 6 中的选择创建单个应用包或应用程序包。
10. 应用成功打包后，你将看到下面的对话框。  
    ![显示的程序包创建完成窗口，带有验证选项](images/packaging-screen6.jpg)

    在你将应用提交到开发人员中心以在本地或远程计算机上进行认证之前，验证你的应用。 你只能验证应用包的发布版本而不是调试版本。

11. 若要在本地验证你的应用，请选择**本地计算机**选项，并单击**启动 Windows 应用认证工具包**。 有关使用 Windows 应用认证工具包测试应用的详细信息，请参阅 [Windows 应用认证工具包](https://msdn.microsoft.com/library/windows/apps/Mt186449)。

    Windows 应用认证工具包会执行多种测试并返回结果。 有关更具体的信息，请参阅 [Windows 应用认证工具包测试](https://msdn.microsoft.com/library/windows/apps/mt186450)。

    如果你拥有要用于测试的远程 Windows 10 设备，你将需要在该设备上手动安装 Windows 应用认证工具包。 下一节将指导你完成这些步骤。 完成该操作后，你可以选择**远程计算机**并单击**启动 Windows 应用认证工具包**，以连接到远程设备并运行验证测试。

12. 在 WACK 完成并且你的应用通过认证后，你可以随时将应用提交到开发人员中心。 请确保上传正确的文件。 可以在你的解决方案的根文件夹 `\[AppName]\AppPackages` 中找到文件的默认位置，并且它将以 .appxupload 文件扩展名结束。 如果你选择了应用程序包以及所有包体系结构，则将采用 `[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload` 形式的名称。

有关将应用提交到开发人员中心的详细信息，请参阅[应用提交](https://docs.microsoft.com/windows/uwp/publish/app-submissions)。

**在远程 Windows 10 设备上验证你的应用包**

1.  按照[启用设备进行开发](https://msdn.microsoft.com/library/windows/apps/Dn706236)说明启用 Windows 10 设备进行开发。
    **重要提示**  不能在适用于 Windows 10 的远程 ARM 设备上验证应用包。
2.  下载并安装适用于 Visual Studio 的远程工具。 这些工具用于远程运行 Windows 应用认证工具包。 你可以通过访问[在远程计算机上运行 UWP 应用](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor)获取有关这些工具的详细信息，包括下载位置。
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
> 应用安装程序假定设备信任此应用。 如果你旁加载开发人员或企业应用，则需要向存储在设备上的“Trusted People or Trusted Publishers Certification Authorities”安装签名证书。 如果你不确定如何执行此操作，请参阅[安装测试证书](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates)。

### <a name="sideload-your-app-on-previous-versions-of-windows"></a>在以前版本的 Windows 上旁加载应用
对于 UWP 应用包，应用不会像桌面应用那样安装到设备上。 通常，你会从 Microsoft Store 下载 UWP 应用，这也会将应用安装到你的设备上。 你可以在不将应用发布到 Microsoft Store 的情况下安装应用（旁加载）。 这允许你使用你所创建的应用包 (.appx) 安装和测试应用。 如果你有一款不想在应用商店中出售的应用（如业务线 (LOB) 应用），你可以旁加载该应用，以便你的公司中的其他用户也可以使用它。

下面的列表提供了旁加载应用的要求。

-   你必须[启用你的设备进行开发](https://msdn.microsoft.com/library/windows/apps/Dn706236)。
-   若要在 Windows 10 移动版设备上旁加载你的应用，请使用 [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) 工具。

**将应用旁加载到台式机、笔记本电脑或平板电脑**

1.  将要安装的应用版本的文件夹复制到目标设备。

    如果你已创建应用程序包，那么你将拥有一个基于版本号的文件夹和一个 `*_Test` 文件夹。 例如以下两个文件夹（其中要安装的版本是 1.0.2.0）：

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_Test`

    如果你没有应用程序包，可以复制正确体系结构的文件夹和相应的 `*_Test` 文件夹。 这两个文件夹是具有 x64 体系结构及其 `*_Test` 文件夹的应用包的示例：

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64_Test`

2.  在目标设备上，打开 `*_Test` 文件夹。
3.  右键单击 **Add-AppDevPackage.ps1** 文件。 选择**使用 PowerShell 运行**并按照提示操作。  
    ![显示的文件资源管理器，已导航到 PowerShell 脚本](images/packaging-screen7.jpg)

    安装应用包后，PowerShell 窗口显示消息：**已成功安装你的应用。**

    **使用技巧**：若要打开平板电脑上的快捷方式菜单，请触摸屏幕上你想要右键单击的位置，按住直到出现一个整圆，然后抬起手指。 快捷菜单将在你抬起手指时打开。
4.  单击“开始”按钮，搜索应用名称，然后启动它。
