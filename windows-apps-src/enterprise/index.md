---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: 此路线图提供适用于 windows 10 和通用 Windows 平台 (UWP) 应用的关键企业功能的概述。
title: 企业版
ms.date: 08/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6cce98591cdaa78a887d7a5fb495e999a4ffc453
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2018
ms.locfileid: "8477735"
---
# <a name="enterprise"></a>企业版

本文提供了适用于 Windows 10 应用中提供由通用 Windows 平台 (UWP) 的关键企业功能的概述。

## <a name="whats-new-and-recent-for-enterprise-applications"></a>新增功能和最近的企业应用程序

> [!div class="checklist"]
> * [Windows Template Studio](#template-studio)
> * [若要创建桌面样式 Ui 的控件](#desktop-style-UI)
> * [控件以支持企业方案](#enterprise)
> * [Windows UI 库](#UI-library)
> * [桌面应用程序中的 UWP 控件](#xaml-islands)
> * [.NET Standard 2.0](#standard)
> * [SQL Server 连接](#sql-server)
> * [MSIX 部署](#MSIX)

有关演示的一些详细介绍这些功能的视频，请参阅[快速构建 LOB 应用程序与 UWP 和 Visual Studio](https://channel9.msdn.com/Events/Build/2018/BRK3502)。

<a id="template-studio" />

### <a name="windows-template-studio"></a>Windows Template Studio

Windows Template Studio 是 Visual Studio 2017 扩展加快了创建新的通用 Windows 平台 (UWP) 应用使用基于向导的体验。 生成的 UWP 项目是标准格式，可读实现成熟的模式和最佳做法同时包含最新的 Windows 10 功能的代码。

![Windows Template Studio](images/windows-template-studio.png)

请参阅[Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio)

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>若要创建桌面样式 Ui 的控件

我们已发布新的 UWP XAML 控件填充传统桌面应用程序 UI 和 UWP UI 之间的差距。

例如，新的[菜单栏](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/menus?branch=jimwalk%2Frs5-menu-bar)、 [DropDownButton](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button)、[拆分按钮](https://docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button)，以及[CommandBarFlyout](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/command-bar-flyout?branch=jimwalk%2Frs5-command-bar-flyout)控件向你提供了更灵活的方式来公开命令，而[EditableComboBox](https://review.docs.microsoft.com/en-us/windows/uwp/design/controls-and-patterns/combo-box?branch=rs5#make-a-combo-box-editable)让用户输入并未列出的值在预定义的选项列表。

![菜单栏](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>控件以支持企业方案

[DataGridView](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/datagrid)提供行和列中显示的数据集合的灵活方法。

[树视图](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view)支持分层列表，带有展开和折叠节点包含嵌套的项。 它可用于说明你的用户界面中的文件夹结构或嵌套关系。

![数据网格控件](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Windows UI 库

Windows UI 库是一组提供适用于 UWP 应用的控件和其他用户界面元素的 NuGet 程序包。 因此即使用户不需要的最新的操作系统，你的应用起作用，它还使低级别与早期版本的 Windows 10 的兼容性。

![Windows UI 库](images/win-ui.png)

请参阅[Windows UI 库 （预览版）](https://docs.microsoft.com/en-us/uwp/toolkits/winui/)。

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications"></a>桌面应用程序中的 UWP 控件

Windows 10 现在可以在 WPF、 Windows 窗体和 c + + Win32 桌面应用程序中使用 UWP 控件。 这意味着你可以增强的外观、 体验和功能的现有桌面应用程序将仅可通过 UWP 控件，如 Windows Ink 和支持 Fluent 设计系统的控件的最新 Windows 10 UI 功能。 此功能称为 XAML 群岛。

了解[UWP 控件的桌面应用程序](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls)。

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard 2.0

在.NET Standard 包含超过 20000 比.NET Standard 的更多 Api 1.x。 这使得变得更加方便迁移现有的.NET Framework 库，然后在不同的.NET 应用程序包括 UWP 应用程序中使用它们。

![net 标准](images/dot-net-standard-project-template.png)

请参阅[桌面应用和 UWP 应用之间共享代码](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate)。

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>SQL Server 连接

通过使用 [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN&view=netframework-4.7.2) 命名空间中的类，你的应用可以直接连接到 SQL Server 数据库然后存储和检索数据。

请参阅[在 UWP 应用中使用 SQL Server 数据库](https://docs.microsoft.com/en-us/windows/uwp/data-access/sql-server-databases)。

<a id="MSIX" />

### <a name="msix-deployment"></a>MSIX 部署

MSIX 是提供对所有 Windows 应用的现代打包体验的 Windows 应用包格式。 MSIX 程序包格式保留现有的应用包的功能，并安装文件除了启用 Win32、 WPF 和 Windows 窗体应用的新的现代打包和部署功能。

MSIX 是一种打包格式，构建为可安全、 安全且可靠，具体取决于.msi、.appx、 APP-V 和 ClickOnce 安装技术组合。

![MSIX 图标](images/MSIX-App-Package.ico)

请参阅[MSIX 文档](https://docs.microsoft.com/windows/msix/)。

<a id="distribution" />

## <a name="security"></a>安全

Windows 10 提供了一套适用于应用开发人员保护其用户、 公司网络安全和存储在设备上的任何业务数据的身份的安全功能。 新增功能的 windows 10 是 Microsoft Passport、 轻松部署的双重密码替代项，是辅助应用通过使用 PIN 或 Windows Hello，提供企业级安全并支持指纹、 面部和虹膜基于识别。

| 主题 | 说明 |
|-------|-------------|
| [安全 Windows 应用开发简介](https://msdn.microsoft.com/library/windows/apps/mt622741) | 本入门文章介绍各个身份验证阶段、未送达数据和静止数据的各种 Windows 安全功能。 它还介绍如何将这些阶段集成到应用中。 它涵盖大量主题，并且主要旨在帮助应用架构师更好地了解使创建通用 Windows 平台应用快速且简单的 Windows 功能。 |
| [身份验证和用户身份](https://msdn.microsoft.com/library/windows/apps/mt270184) | 对于本文中所述的用户身份验证，UWP 应用具有多个选项。 对于企业，强烈建议使用新的 Microsoft Passport 功能。 Microsoft Passport 来替换密码，使用强大的双因素身份验证 (2FA) 验证现有凭据和创建特定于设备的凭据的生物识别或 PIN 基于受用户手势，从而同时方便和高度安全的体验。 |
| [加密](https://msdn.microsoft.com/library/windows/apps/mt270191) | 加密部分概述 UWP 应用可用的加密功能。 文章的范围从有关如何轻松加密敏感业务数据的初级操作实例，一直到操作加密密钥以及使用 MAC、哈希和签名等高级主题。 |
| [Windows 信息保护 (WIP)](wip-hub.md) | 此中心主题涉及关于 Windows 信息保护 (EDP) 与文件、缓冲区、剪贴板、网络、后台任务以及锁屏下的数据保护有何关联的完整开发人员蓝图。 |

## <a name="data-binding-and-databases"></a>数据绑定和数据库

数据绑定是你的应用 UI 用于从外部源（如数据库）显示数据以及与该数据保持同步（可选）的一种方法。 借助数据绑定，你可以将关注的数据从关注的 UI 中分离开来，从而可形成一个更简易的概念模型，并且使你的应用拥有更好的可读性、可测试性和可维护性。

| 主题 | 说明 |
|-------|-------------|
| [数据绑定概述](https://msdn.microsoft.com/library/windows/apps/mt269383) | 本主题介绍了如何将一个控件 （或其他 UI 元素） 绑定到单个项目或将项目控件绑定到通用 Windows 平台 (UWP) 应用中的项目集合。 此外，它还介绍了如何控制项的呈现、基于所选内容实现详细信息视图，以及转换数据以供显示。 |
| [面向 UWP 的 Entity Framework 7](https://msdn.microsoft.com/library/windows/apps/mt592863) | 针对大数据集执行复杂查询通过使用 Entity Framework 7（支持 UWP）得到了大幅简化。 在本演练中，你将生成针对使用 Entity Framework 本地 SQLite 数据库执行基本数据访问的 UWP 应用。 |
| [SQLite 本地数据库](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | 本视频是使用 SQLite（本地应用数据库的建议解决方案）的全面开发人员指南。 请访问 [SQLite](https://www.sqlite.org/download.html) 以下载适用于 UWP 的最新版本，或使用已经与 Windows 10 SDK 一起提供的版本。 |

## <a name="networking-and-data-serialization"></a>网络和数据序列化

业务线应用经常需要与各种其他系统上的数据通信或存储它们。 这通常通过以下方式实现：连接到网络服务（使用 REST 或 SOAP 等协议），然后将数据序列化或反序列化为通用格式。 在 UWP 应用中使用网络和数据序列化类似于 WPF、WinForms 和 ASP.NET 应用程序。 有关详细信息，请参阅以下文章。

| 主题 | 说明 |
|-------|-------------|
| [网络基础知识](https://msdn.microsoft.com/library/windows/apps/mt280233) | 本操作实例介绍与所有 UWP 应用相关的基本网络概念，不考虑使用何种通信协议。  |
| [选择哪一种网络技术？](https://msdn.microsoft.com/library/windows/apps/mt280235) | 适用于 UWP 应用的网络技术的简短概述，以及关于如何选择最适合自己应用的技术的建议。 |
| [XML 和 SOAP 序列化](https://msdn.microsoft.com/library/90c86ass.aspx) | XML 序列化将对象转换为 XML 流符合特定的 XML 架构定义语言 (XSD)。 若要在 XML 和强类型的类之间进行转换，可以使用本机 [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) 类或外部库。 |
| [JSON 序列化](https://msdn.microsoft.com/library/windows/apps/br240639) | JSON （JavaScript 对象表示法） 序列化是用于与 REST Api 通信的流行格式。 [Newtonsoft Json.NET](http://www.newtonsoft.com/json)，在 UWP 应用中受到完全支持。 |

## <a name="devices"></a>设备

为了与业务线工具（如打印机、条形码扫描仪或智能卡读卡器）集成，你可能会发现有必要将外部设备或传感器集成到应用中。 下面是一些功能示例，你可以使用本节所述技术将其添加到你的应用中。

| 主题  | 说明 |
|--------|-------------|
| [枚举设备](https://msdn.microsoft.com/library/windows/apps/mt187355) | 本文介绍如何使用 [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) 命名空间找到内部连接到系统的、外部连接的或通过无线或网络协议可检测到的设备。 如果你要生成任何使用设备的应用，请从此处开始。 |
| [打印和扫描](https://msdn.microsoft.com/library/windows/apps/mt204544) | 介绍如何从你的应用，包括连接到和使用业务设备，如销售点 (POS) 系统、 收据打印机和高容量送纸器扫描仪打印和扫描。 |
| [蓝牙](https://msdn.microsoft.com/library/windows/apps/mt270288) | 除了将传统蓝牙连接用于发送和接收数据或控制设备外，Windows 10 还支持使用低耗电蓝牙 (BTLE) 在后台发送或接收信号。 使用此功能在用户接近或离开特定位置时显示通知或启用功能。 |
| [企业共享存储](enterprise-shared-storage.md) | 在设备锁定方案中，了解数据可如何在相同应用、应用的实例之间甚至是在应用之间共享。 |

## <a name="device-targeting"></a>设备定位

如今许多用户携带自己的手机或平板电脑上班，这些设备的外形规格和屏幕大小各异。 使用通用 Windows 平台 (UWP)，你可以编写可在所有不同类型的设备上无缝运行的单个业务线应用，这些设备包括桌面电脑和 PPI 屏幕，从而允许你最大程度地扩大应用的受众和提高代码效率。

| 主题 | 说明 |
|-------|-------------|
| [UWP 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631) | 在此初级指南中，你将熟悉 Windows 10 UWP 平台，包括：什么是设备系列和如何决定要面向哪个设备、可使你针对不同设备外形规格改编 UI 的新 UI 控件和面板以及如何了解和控制适用于应用的 API 图面。 |
| [自适应 XAML UI 代码示例](http://go.microsoft.com/fwlink/p/?LinkId=619992) | 此代码示例演示了所有可能的布局选项和你的应用，而不考虑设备类型的控件，并允许你与显示了如何实现你正在寻找的任何布局面板进行交互。 除了演示每个控件如何响应不同的外形规格外，应用本身也具有响应性，并显示实现自定义 UI 的各种方法。 |
| [Xamarin 主题]() | 面向手机的 Xamarin |

## <a name="deployment"></a>部署

你可以选择将应用分配给组织的用户。 你可以使用适用于企业，现有的移动设备管理的 Microsoft 应用商店，或者你可以将应用旁加载到设备。 你还可以应用可为常规公共通过发布到 Microsoft Store。

| 主题 | 说明 |
|-------|-------------|
| [将 LOB 应用分配到企业](https://msdn.microsoft.com/library/windows/apps/mt608995) | 无需使应用广泛提供给公众，你可以直接向企业获取批量购置，通过适用于企业的 Microsoft 应用商店发布业务线应用。 |
| [旁加载应用](https://technet.microsoft.com/library/mt269549) | 当你旁加载应用时，你将一个已签名的应用包部署到设备。 你维护这些应用的签名、承载和部署。 旁加载应用的过程已在 Windows 10 中得到简化。             |
| [将应用发布到 Microsoft Store](https://dev.windows.com/publish) | 统一的 Microsoft 应用商店，可以发布和管理你的所有应用适用于所有 Windows 设备。 通过每个市场的定价、分配和可见性控件以及其他选项自定义应用的可用性。 |

## <a name="enterprise-uwp-samples"></a>企业 UWP 示例

| 主题 |  描述 |
|------ |--------------|
| [VanArsdel 清单示例](https://github.com/Microsoft/InventorySample) | 业务线方案中展示了 UWP 示例应用。 该示例都基于创建和管理客户、 订单和产品虚构公司 VanArsdel。 |
| [客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | UWP 示例应用的展示了适用于企业开发人员，如 Azure Active Directory (AAD) 身份验证、 UI 控件 （包括数据网格）、 Sqlite 和 SQL Azure 数据库集成，Entity Framework 和 API 的云服务的功能。 该示例都基于创建和管理客户、 订单和产品虚构公司 Contoso。 |

## <a name="patterns-and-practices"></a>模式和实践

大规模的企业级应用的基本代码可能变得不实用。 Prism 是用于生成松散耦合、 可维护性、 和可测试 XAML 应用程序在 WPF、 windows 10 UWP 和 Xamarin Forms 中的框架。 Prism 提供设计模式集合的实现，有助于编写结构完善且可维护的 XAML 应用程序，包括 MVVM、依赖关系注入、命令、EventAggregator 等。

有关 Prism 的详细信息，请参阅 [GitHub 存储库](https://github.com/PrismLibrary/Prism)。

 

 
