---
author: jnHs
Description: 按照以下指南准备要提交到 Windows 应用商店的应用包。
title: 应用包要求
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
---

# 应用包要求

按照以下指南准备要提交到 Windows 应用商店的应用包。

## 为 Windows 应用商店生成应用包之前

请确保[使用 Windows 应用认证工具包测试应用](https://msdn.microsoft.com/library/windows/apps/mt186449)。 我们还建议在不同类型的硬件上测试你的应用。 请注意，在我们对你的应用进行认证并在 Windows 应用商店中发布之前，该应用只能在具有开发者许可证的计算机上安装和运行。

## 使用 Microsoft Visual Studio 生成应用包

如果你使用 Microsoft Visual Studio 作为开发环境，则你已经拥有可使创建应用包的过程变得快速而轻松的内置工具。 有关详细信息，请参阅[打包应用](https://msdn.microsoft.com/library/windows/apps/mt270969)。

> **注意** 请确保你的所有文件名都使用 ANSI。 


在 Visual Studio 中创建程序包时，请确保你使用与你的开发者帐户关联的相同 Microsoft 帐户登录。 程序包清单的某些部分具有与你的帐户相关的特定详细信息。 将自动检测和添加此信息。

在生成应用包时，Visual Studio 可以创建 .appx 文件或 .appxupload 文件（对于 Windows Phone 8.1 及更早版本，创建 .xap 文件）。 对于面向 Windows 10 的应用，始终在[程序包](upload-app-packages.md)页面中上传 .appxupload 文件。 有关打包适用于应用商店的 UWP 应用的详细信息，请参阅[打包适用于 Windows 10 的通用 Windows 应用](http://go.microsoft.com/fwlink/p/?LinkId=620193 )。

不必使用来自受信任的证书颁发机构的根证书对你的应用包进行签名。

### 应用程序包

对于面向 Windows 8.1、Windows Phone 8.1 及更高版本的应用，Visual Studio 可以生成应用程序包 (.appxbundle) 以减少用户所下载应用的大小。 仅当已定义特定于语言的资源、大量图像缩放资源或适用于特定版本的 Microsoft DirectX 的资源时，该操作才有用。

> **注意** 一个应用程序包可以包含所有体系结构的程序包。 对于每个目标操作系统，应仅提交一个捆绑包。


使用应用程序包，用户将仅下载相关文件，而不是所有可能的资源。 有关应用程序包的详细信息，请参阅[打包应用](https://msdn.microsoft.com/library/windows/apps/mt270969)和[打包适用于 Windows 10 的通用 Windows 应用](http://go.microsoft.com/fwlink/p/?LinkId=620193 )。

## 手动生成应用包

如果未使用 Visual Studio 创建程序包，则必须[手动创建程序包清单](https://msdn.microsoft.com/library/windows/apps/br211476)。

确保查看[应用包清单](https://msdn.microsoft.com/library/windows/apps/br211474)文档，以获取完整清单的详细信息和要求。 你的清单必须遵循程序包清单架构，才能通过认证。

你的清单必须包含有关你的帐户和应用的某些特定信息。 通过查看仪表板中应用概述页面的**“应用管理”**部分中的[查看应用标识详细信息](view-app-identity-details.md)，可找到此信息。

> **注意** 清单中的值区分大小写。 空格和其他标点符号也必须匹配。 请小心输入值并进行检查，以确保这些值准确无误。


应用程序包使用不同的清单。 查看[捆绑包清单](https://msdn.microsoft.com/library/windows/apps/dn263089)文档，以获取应用程序包清单的详细信息和要求。

> **提示** 在提交你的程序包之前，请确保运行 [Windows 应用认证工具包](https://msdn.microsoft.com/library/windows/apps/mt186449)。 此操作可帮助你确定你的清单是否存在任何可能导致认证或提交失败的问题。


如果你的应用具有多个程序包，这些应用清单元素必须在每个程序包（每个目标操作系统）中保持相同：

-   [**程序包/功能**](https://msdn.microsoft.com/library/windows/apps/br211422)
-   [**程序包/依赖项**](https://msdn.microsoft.com/library/windows/apps/br211428)
-   [**程序包/资源**](https://msdn.microsoft.com/library/windows/apps/br211462)

## 程序包格式要求

你的应用包必须符合这些要求。

| 应用包属性 | 要求                                                          |
|----------------------|----------------------------------------------------------------------|
| 程序包大小         | .appxbundle：每个捆绑包最大为 25 GB <br>面向 Windows 8.1 的 .appx 程序包：每个程序包最大为 8 GB <br> 面向 Windows 8 的 .appx 程序包：每个程序包最大为 2 GB <br> 面向 Windows Phone 8.1 的 .appx 程序包：每个程序包最大为 4 GB <br> .xap 程序包：每个程序包最大为 1 GB                                                                           |
| 块映射哈希     | SHA2-256 算法                                                   |
 

## StoreManifest XML 文件

StoreManifest.xml 是一种可选的配置文件，可包含在应用包中。 在文件旨在支持程序包清单未涵盖的功能，例如将应用声明为 Windows 应用商店设备应用或声明程序包适用于某个设备所依据的要求。 StoreManifest.xml 与应用包一起提交，并且必须在应用的主项目的根文件夹中。 有关详细信息，请参阅 [StoreManifest 架构](https://msdn.microsoft.com/library/windows/apps/mt617325)。

 

 






<!--HONumber=May16_HO2-->


