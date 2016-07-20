---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: "此路线图提供 Windows 10 通用 Windows 平台 (UWP) 应用的关键企业功能的概述。"
title: "企业"
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 58f7c9739a000b6cac1c000570807587233018ce

---

# 企业


\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

此路线图提供 Windows 10 通用 Windows 平台 (UWP) 应用的关键企业功能的概述。 Windows 10 可使你编写一次即可在所有设备上部署，创建可适应任何设备的单个应用。 这使你可以在对组织所需的安全性、管理和配置提供控制的同时，打造用户期望的出色体验。

**注意** 本文面向编写企业 UWP 应用的开发人员。 有关常规 UWP 开发，请参阅 [Windows 10 应用的操作方法指南](https://msdn.microsoft.com/library/windows/apps/mt244352)。 有关 WPF、Windows 窗体或 Win32 开发，请访问[桌面开发人员中心](https://dev.windows.com/desktop)。 有关 IT 专业人员资源（如部署 Windows 10 或管理企业安全功能），请参阅 [TechNet 上的 Windows 10](https://msdn.microsoft.com/library/dn986868)。

 

## 安全


Windows 10 为应用开发人员提供一套用于保护用户身份、公司网络安全和存储在设备上的任何业务数据的功能。 Windows 10 的新增功能是 Microsoft Passport，它是一种易于部署的双重密码替代项，可通过使用 PIN 或 Windows Hello 访问，提供企业级安全并支持基于指纹、面部和虹膜的识别。

| 主题 | 说明 |
|-------|-------------|
| [安全 Windows 应用开发简介](https://msdn.microsoft.com/library/windows/apps/mt622741) | 本入门文章介绍各个身份验证阶段、未送达数据和静止数据的各种 Windows 安全功能。 它还介绍如何将这些阶段集成到应用中。 它涵盖大量主题，主要旨在帮助应用架构师更好地了解可使创建通用 Windows 平台应用变得快捷的 Windows 功能。 |
| [身份验证和用户身份](https://msdn.microsoft.com/library/windows/apps/mt270184) | 对于本文中所述的用户身份验证，UWP 应用具有多个选项。 对于企业，强烈建议使用新的 Microsoft Passport 功能。 Microsoft Passport 使用强双因素身份验证 (2FA) 来替换密码，方法是验证现有凭据和创建一个受用户手势（基于生物识别或 PIN）保护的特定于设备的凭据，从而创造既便利又高度安全的体验。 |
| [加密](https://msdn.microsoft.com/library/windows/apps/mt270191) | 加密部分概述 UWP 应用可用的加密功能。 文章的范围从有关如何轻松加密敏感业务数据的初级操作实例，一直到操作加密密钥以及使用 MAC、哈希和签名之类的高级主题。 |
| [企业数据保护 (EDP)](edp-hub.md) | 此中心主题涉及企业数据保护 (EDP) 如何关联到文件、缓冲区、剪贴板、网络、后台任务以及锁屏下的数据保护的完整开发人员蓝图。 |

 

## 数据绑定和数据库


数据绑定是你的应用 UI 用于从外部源（如数据库）显示数据以及与该数据保持同步（可选）的一种方法。 借助数据绑定，你可以将关注的数据从关注的 UI 中分离开来，从而可形成一个更简易的概念模型，并且使你的应用拥有更好的可读性、可测试性和可维护性。

| 主题 | 说明 |
|-------|-------------|
| [数据绑定概述](https://msdn.microsoft.com/library/windows/apps/mt269383) | 本主题介绍了如何在通用 Windows 平台 (UWP) 应用中将控件（或其他 UI 元素）绑定到单个项目，或者将项目控件绑定到项目集合。 此外，它还介绍了如何控制项的呈现、基于所选内容实现详细信息视图，以及转换数据以供显示。 |
| [面向 UWP 的 Entity Framework 7](https://msdn.microsoft.com/library/windows/apps/mt592863) | 针对大数据集执行复杂查询通过使用 Entity Framework 7（支持 UWP）得到了大幅简化。 在本操作实例中，你将生成一个使用 Entity Framework 针对本地 SQLite 数据库执行基本数据访问的 UWP 应用。 |
| [SQLite 本地数据库](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | 本视频是使用 SQLite（本地应用数据库的建议解决方案）的全面开发人员指南。 请访问 [SQLite](https://www.sqlite.org/download.html) 以下载适用于 UWP 的最新版本，或使用已经与 Windows 10 SDK 一起提供的版本。 |

 

## 网络和数据序列化


业务线应用经常需要与各种其他系统上的数据通信或存储它们。 这通常通过以下方式实现：连接到网络服务（使用 REST 或 SOAP 等协议），然后将数据序列化或反序列化为通用格式。 在 UWP 应用中使用网络和数据序列化类似于 WPF、WinForms 和 ASP.NET 应用程序。 有关详细信息，请参阅以下文章。

| 主题 | 说明 |
|-------|-------------|
| [网络基础知识](https://msdn.microsoft.com/library/windows/apps/mt280233) | 本操作实例介绍与所有 UWP 应用相关的基本网络概念，不考虑使用何种通信协议。  |
| [选择哪一种网络技术？](https://msdn.microsoft.com/library/windows/apps/mt280235) | 适用于 UWP 应用的网络技术的简短概述，以及关于如何选择最适合自己应用的技术的建议。 |
| [XML 和 SOAP 序列化](https://msdn.microsoft.com/library/90c86ass.aspx) | XML 序列化将对象转换为 XML 流，该流符合特定的 XML 架构定义语言 (XSD)。 若要在 XML 和强类型的类之间进行转换，可以使用本机 [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) 类或外部库。 |
| [JSON 序列化](https://msdn.microsoft.com/library/windows/apps/br240639) | JSON（JavaScript 对象表示法）序列化是用于与 REST API 通信的流行格式。 [Newtonsoft Json.NET](http://www.newtonsoft.com/json)，在 UWP 应用中受到完全支持。 |

 

## 设备


为了与业务线工具（如打印机、条形码扫描仪或智能卡读卡器）集成，你可能会发现有必要将外部设备或传感器集成到应用中。 下面是一些功能示例，你可以使用本节所述技术将其添加到你的应用中。

| 主题  | 说明 |
|--------|-------------|
| [枚举设备](https://msdn.microsoft.com/library/windows/apps/mt187355) | 本文介绍如何使用 [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) 命名空间找到内部连接到系统的、外部连接的或通过无线或网络协议可检测到的设备。 如果你要生成任何使用设备的应用，请从此处开始。 |
| [打印和扫描](https://msdn.microsoft.com/library/windows/apps/mt204544) | 介绍如何从应用打印和扫描，包括连接到和使用业务设备，如销售点 (POS) 系统、收据打印机和高容量送纸扫描仪。 |
| [蓝牙](https://msdn.microsoft.com/library/windows/apps/mt270288) | 除了将传统蓝牙连接用于发送和接收数据或控制设备外，Windows 10 还支持使用低耗电蓝牙 (BTLE) 在后台发送或接收信号。 使用此功能在用户接近或离开特定位置时显示通知或启用功能。 |
| [企业共享存储](enterprise-shared-storage.md) | 在设备锁定方案中，了解数据可如何在相同应用、应用的实例之间甚至是在应用之间共享。 |

 

## 设备定位


如今许多用户携带自己的手机或平板电脑上班，这些设备的外形规格和屏幕大小各异。 使用通用 Windows 平台 (UWP)，你可以编写可在所有不同类型的设备上无缝运行的单个业务线应用，这些设备包括桌面电脑和 PPI 屏幕，从而允许你最大程度地扩大应用的受众和提高代码效率。

| 主题 | 说明 |
|-------|-------------|
| [UWP 应用指南](https://msdn.microsoft.com/library/windows/apps/dn894631) | 在此初级指南中，你将熟悉 Windows 10 UWP 平台，包括：什么是设备系列和如何决定要面向哪个设备、可使你针对不同设备外形规格改编 UI 的新 UI 控件和面板以及如何了解和控制适用于应用的 API 图面。 |
| [自适应 XAML UI 代码示例](http://go.microsoft.com/fwlink/p/?LinkId=619992) | 此代码示例介绍应用的所有可能的布局选项和控件，而不考虑设备类型，并允许你与面板交互以演示如何实现你需要的任何布局。 除了演示每个控件如何响应不同的外形规格外，应用本身也具有响应性，并显示实现自定义 UI 的各种方法。 |

 

## 部署


你可以选择将应用分配给组织的用户。 你可以使用适用于企业的 Windows 应用商店、现有移动设备管理，或者可以将应用旁加载到设备。 你还可以通过发布到 Windows 应用商店向公众提供你的应用。

| 主题 | 说明 |
|-------|-------------|
| [将 LOB 应用分配到企业](https://msdn.microsoft.com/library/windows/apps/mt608995) | 你可以通过适用于企业的 Windows 应用商店向企业直接发布业务线应用以获取批量购置，而无需将应用广泛提供给公众。 |
| [旁加载应用](https://technet.microsoft.com/library/mt269549) | 当你旁加载应用时，你将一个已签名的应用包部署到设备。 你维护这些应用的签名、承载和部署。 旁加载应用的过程已在 Windows 10 中得到简化。             |
| [将应用发布到 Windows 应用商店](https://dev.windows.com/publish) | 利用统一的 Windows 应用商店，你可以针对所有 Windows 设备发布和管理你的所有应用。 通过每个市场的定价、分配和可见性控件以及其他选项自定义应用的可用性。 |

 

## 模式和实践


大规模的企业级应用的基本代码可能变得不实用。 Prism 是用于在 WPF、Windows 10 UWP 和 Xamarin Forms 中生成松散耦合、可维护且可测试的 XAML 应用程序的框架。 Prism 提供设计模式集合的实现，有助于编写结构完善且可维护的 XAML 应用程序，包括 MVVM、依赖关系注入、命令、EventAggregator 等。

有关 Prism 的详细信息，请参阅 [GitHub 存储库](https://github.com/PrismLibrary/Prism)。

 

 






<!--HONumber=Jul16_HO2-->


