---
author: msatranjr
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: 打包 UWP 应用
description: 若要出售你的通用 Windows 平台 (UWP) 应用或将其分配给其他用户，你需要为其创建一个 appxupload 程序包。
---
# 打包 UWP 应用

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

若要出售你的通用 Windows 平台 (UWP) 应用或将其分配给其他用户，你需要为其创建一个 appxupload 程序包。 当你创建 appxupload 时，将生成另一个 appx 程序包用于测试和旁加载。 你可以通过将 appx 程序包旁加载到设备来直接分配你的应用。 本文介绍了配置、创建和测试 UWP 应用包的过程。 有关旁加载的详细信息，请参阅[使用 DISM 旁加载应用](http://go.microsoft.com/fwlink/?LinkID=231020)。

在 Windows 10 中，生成一个可上传到 Windows 应用商店的程序包 (.appxupload)。 然后，你的应用就可以在所有 Windows 10 设备上安装和运行。 下面是创建应用包的步骤。

1.  [在打包应用前](#before-packaging-your-app)。 按照这些步骤来确保你的应用可以随时打包以进行应用商店提交。
2.  [配置应用包](#configure-an-app-package)。 使用清单设计器配置程序包。 例如，添加磁贴图像并选择应用支持的方向。
3.  [创建应用包](#create-an-app-package)。 使用 Microsoft Visual Studio 中的向导创建应用包，然后使用 Windows 应用认证工具包验证你的程序包。
4.  [旁加载应用包](#sideload-your-app-package)。 在将应用旁加载到设备后，你可以测试其是否正常运行。

完成上述步骤之后，你可以随时在应用商店中出售你的应用。 如果你不打算出售仅供内部用户使用的业务线” (LOB) 应用，你可以旁加载该应用以在任一 Windows 10 设备上安装它。

## 在打包应用前

1.  测试你的应用。 在将你的应用打包以进行应用商店提交前，请确保它可以在你计划支持的所有设备系列上按预期运行。 这些设备系列可能包括桌面设备、移动设备、Surface Hub、XBOX、IoT 设备或其他设备。
2.  优化你的应用。 你可以使用 Visual Studio 的分析和调试工具来优化你的 UWP 应用的性能。 例如，用于 UI 响应能力的时间线工具、内存使用工具、CPU 使用工具等。 有关这些工具的详细信息，请参阅[运行诊断工具（不调试）](https://msdn.microsoft.com/library/dn957936.aspx)。
3.  检查 .NET 本机兼容性（针对 VB 和 C# 应用）。 在 UWP 中，现在有新的本机编译器，用于提升你的应用的运行时性能。 通过这项更改，强烈建议你在此编译环境中测试你的应用。 默认情况下，**Release** 版本配置会启用 .NET 本机工具链，因此请务必使用此 **Release** 配置测试应用并检查应用是否按预期运行。 对于一些可能在 .NET 本机中发生的常见调试问题，在[此处](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx)进行了详细说明。

## 配置应用包

应用清单文件 (package.appxmanifest.xml) 中包含创建应用包所需的属性和设置。 例如，清单文件中的属性描述了要用作应用的磁贴的映像和应用在用户旋转设备时支持的方向。

Visual Studio 的清单设计器使你可以轻松地更新清单文件，而无需编辑文件的原始 XML。

Visual Studio 可以将你的程序包与应用商店相关联。 当执行此操作时，会自动更新清单设计器的“打包”选项卡中的某些字段。

**使用清单设计器配置程序包**

1.  在“解决方案资源管理器”****中，展开你的 UWP 应用的项目节点。
2.  双击“Package.appxmanifest”****文件。 如果清单文件已经在 XML 代码视图中打开，Visual Studio 会提示你关闭该文件。
3.  现在，你可以确定如何配置你的应用。 每个选项卡都包含你可以配置的有关你的应用的信息，以及更多信息的链接（如果需要）。<br/>
    ![](images/packaging-screen1.jpg)

    检查你是否在“视觉资源”****选项卡上拥有 UWP 应用所需的所有图像。

    你可以从“打包”****选项卡中输入发布数据。 你可以从此位置选择用于对你的应用进行签名的证书。 所有 UWP 应用都必须使用证书进行签名。 为了旁加载应用包，你需要信任该程序包。 证书必须安装在该设备上才能信任此程序包。 有关旁加载的详细信息，请参阅[启用设备进行开发](https://msdn.microsoft.com/library/windows/apps/Dn706236)。

4.  在你对应用进行必要的编辑后，请保存你的文件。

## 创建应用包

若要通过应用商店分配应用，你必须创建 appxupload 程序包。 你可以通过使用“创建应用包”****向导执行该操作。 按照以下步骤使用 Microsoft Visual Studio 2015 来创建适用于应用商店提交的程序包。

**创建应用包**

1.  在“解决方案资源管理器”****中，打开适用于你的 UWP 应用项目的解决方案。
2.  右键单击该项目，然后依次选择“应用商店”****->“创建应用包”****。 如果此选项处于禁用状态或根本没有显示，请检查该项目是否为 UWP 项目。<br/>
    ![](images/packaging-screen2.jpg)

    将显示“创建应用包”****向导。

3.  在第一个询问你是否希望生成要上传到 Windows 应用商店的程序包的对话框中选择“是”，然后单击“下一步”。<br/>
    ![](images/packaging-screen3.jpg)

    如果你在此处选择“否”，Visual Studio 将不会生成用于应用商店提交的所需 .appxupload 程序包。 如果你仅希望旁加载应用以在内部设备上运行它，那么你可以选择此选项。 有关旁加载的详细信息，请参阅[启用设备进行开发](https://msdn.microsoft.com/library/windows/apps/Dn706236)。

4.  使用你的开发者帐户登录到 Windows 开发人员中心。 （如果你还没有开发者帐户，该向导将帮助你创建一个。）
5.  为你的程序包选择应用名称，如果你尚未在 Windows 开发人员中心门户中保留应用名称，也可以保留一个新名称。<br/>
    ![](images/packaging-screen4.jpg)
6.  请确保选择“选择并配置程序包”****对话框中的全部三个体系结构配置（x86、x64 和 ARM）。 这样，你的应用就可以部署到尽可能多的设备上。 在“生成应用程序包”****列表框中，选择“始终”****。 这使应用商店提交流程更加简单，因为你只需要上传一个文件 (.appxupload)。 单个捆绑包将包含所有要部署到设备（带有各自处理器体系结构）的必需程序包。<br/>
    ![](images/packaging-screen5.jpg)
7.  最好包括完整的 PDB 符号文件，以便从 Windows 开发人员中心获取最佳的[故障分析](http://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/)体验。 你可以通过访问[使用符合调试](https://msdn.microsoft.com/library/windows/desktop/Ee416588)了解有关使用符号调试的详细信息。
8.  现在你可以配置详细信息来创建你的程序包。 当你准备好发布应用时，你可以从输出位置上传程序包。
9.  单击“创建”****以生成 appxupload 程序包。
10. 现在，你将看到此对话框。<br/>
    ![](images/packaging-screen6.jpg)

    在你将应用提交到应用商店以在本地或远程计算机上进行认证之前，验证你的应用。 （你只能验证应用包的发布版本而不是调试版本。）

11. 若要在本地进行验证，请选择“本地计算机”****选项，并单击“启动 Windows 应用认证工具包”****。 有关使用 Windows 应用认证工具包测试应用的详细信息，请参阅 [Windows 应用认证工具包](https://msdn.microsoft.com/library/windows/apps/Mt186449)。

    Windows 应用认证工具包会执行测试，并向你显示结果。 请参阅 [Windows 应用认证工具包测试](https://msdn.microsoft.com/library/windows/apps/mt186450)。

    如果你拥有要用于测试的远程 Windows 10，你将需要在该设备上手动安装 Windows 应用认证工具包。 下一节将指导你完成这些步骤。 完成该操作后，你可以选择“远程计算机”****并单击“启动 Windows 应用认证工具包”****，以连接到远程设备并运行验证测试。

12. 在 WACK 完成并且你的应用通过后，你可以随时上传到应用商店。 请确保上传正确的文件。 可以在你的解决方案的根文件夹 \\\[AppName\]\\AppPackages 中找到它，并且它将以 .appxupload 文件扩展名结束。 名称将采用 \[AppName\]\_\[AppVersion\]\_x86\_x64\_arm\_bundle 格式。

**在远程 Windows 10 设备上验证你的应用包**

1.  按照[启用设备进行开发](https://msdn.microsoft.com/library/windows/apps/Dn706236)说明启用 Windows 10 设备进行开发。
    **重要提示** 你不可以在适用于 Windows 10 的远程 ARM 设备上验证你的应用包。
2.  下载并安装适用于 Visual Studio 的远程工具。 这些工具用于远程运行 Windows 应用认证工具包。 你可以通过访问[在远程计算机上运行 Windows 应用商店应用](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor)获取有关这些工具的详细信息，包括下载位置。
3.  下载所需的 [Windows 应用认证工具包](http://go.microsoft.com/fwlink/p/?LinkID=309666)，然后将它安装在远程 Windows 10 设备上。
4.  在向导的“程序包创建已完成”****页面上，选择“远程计算机”****选项按钮，然后选择“测试连接”****按钮旁边的省略号按钮。
    **注意** 只有当你至少选择了一种支持验证的解决方案配置时，“远程计算机”****选项按钮才可用。 有关使用 WACK 测试应用的详细信息，请参阅 [Windows 应用认证工具包](https://msdn.microsoft.com/library/windows/apps/Mt186449)。
5.  指定你的子网内部的设备规格，或提供你的子网外部的设备的域名服务器 (DNS) 名称或 IP 地址。
6.  在“身份验证模式”****列表中，如果你的设备没有要求你使用 Windows 凭据登录该设备，请选择“无”****。
7.  选择“选择”****按钮，然后选择“启动 Windows 应用认证工具包”****按钮。 如果远程工具正在该设备上运行，Visual Studio 将与其连接，然后执行验证测试。 请参阅 [Windows 应用认证工具包测试](https://msdn.microsoft.com/library/windows/apps/mt186450)。

## 旁加载应用包

如果使用 UWP 应用包，则无法仅向设备安装一个应用，如桌面应用。 通常情况下，从应用商店下载这些应用，这也是在你的设备上安装它们的方式。 但你可以将应用旁加载到你的设备，而无需将它们提交到应用商店。 这允许你使用你所创建的应用包 (.appx) 安装和测试它们。 如果你有一款不想在应用商店中出售的应用（如业务线 (LOB) 应用），你可以旁加载该应用，以便你的公司中的其他用户也可以使用它。

下面的列表提供了旁加载应用的要求。

-   必须[启用设备进行开发](https://msdn.microsoft.com/library/windows/apps/Dn706236)。
-   若要在 Windows 10 移动版设备上旁加载你的应用，则必须使用 [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) 工具。

**将应用旁加载到台式机、笔记本电脑或平板电脑**

1.  将你想要安装的版本的文件夹复制到目标设备。

    如果你已创建应用程序包，那么你将拥有一个基于版本号的文件夹和一个 \_test 文件夹。 例如以下两个文件夹（其中要安装的版本是 1.0.2）：

    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0
    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test

    如果你没有应用程序包，那么你仅可以复制正确体系结构的文件夹和相应的测试文件夹。 例如以下两个文件夹。

    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64
    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64\_Test
2.  在目标设备上，打开此测试文件夹。 例如，C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test
3.  右键单击“Add-AppDevPackage.ps1”****文件，然后选择“使用 PowerShell 运行”****并按照提示进行操作。<br/>
    ![](images/packaging-screen7.jpg)

    安装应用包后，你将在 PowerShell 窗口中看到以下消息：已成功安装你的应用。

    **注意** 若要打开平板电脑上的快捷菜单，触摸你想要右键单击的屏幕、按住直到出现一个整圆，然后抬起手指。 快捷菜单将在你抬起手指后显示。
4.  单击“开始”按钮，然后键入你的应用名称来启动它。

 

 






<!--HONumber=May16_HO2-->


