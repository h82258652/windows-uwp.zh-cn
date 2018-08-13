---
author: laurenhughes
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: 可选包和相关集的创作
description: 可选包中包含可与主要包相集成的内容。 这些内容可用于可下载内容 (DLC)，因为大小限制而划分大型应用，或者用于随附从原始应用中单独分隔出来的任何其他内容。
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10、 uwp、 可选包、 相关的一组、 包扩展、 visual studio
ms.localizationpriority: medium
ms.openlocfilehash: d66a511211396190393e31bfd553149a1e89fad0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2018
ms.locfileid: "406077"
---
# <a name="optional-packages-and-related-set-authoring"></a>可选包和相关集的创作
可选包中包含可与主要包相集成的内容。 这些可获取可下载内容 (DLC) 除以大小约束的大型应用程序或的传送任何其他内容分隔从原始应用程序。

相关的设置是可选的程序包的扩展--他们允许您跨主和可选的包中实施一组严格的版本。 此外可以从可选包加载本机代码 （c + +）。 

## <a name="prerequisites"></a>必备条件

- Visual Studio 2017，15.1 版
- Windows 10，版本 1703
- 第 10 Windows 版本 1703 SDK

若要获取所有的最新的开发工具，请参阅[下载和工具的 Windows 10](https://developer.microsoft.com/windows/downloads)。

> [!NOTE]
> 若要提交到 Microsoft 存储使用可选包和/或相关的设置的应用程序，您需要的权限。 如果未将可选包和相关集提交到 Microsoft Store，则无需开发人员中心权限即可以将它们用于业务线 (LOB) 或企业应用。 请参阅 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)，获取提交使用可选包和相关集的应用的权限。

### <a name="code-sample"></a>代码示例
阅读本文时建议您遵循以及[可选的程序包代码示例](https://github.com/AppInstaller/OptionalPackageSample)在 GitHub 如何可选包动手了解和相关设置 Visual Studio 中的工作。

## <a name="optional-packages"></a>可选包
若要在 Visual Studio 中创建可选包，您将需要：
1. 确保您的应用程序的**目标平台 Min 版本**设置为： 10.0.15063.0。
2. 从**主包**项目中，打开`Package.appxmanifest`文件。 导航到"打包"选项卡，并记下您的**程序包系列名称**，即"_"字符之前的所有内容。
3. 从**可选软件包**项目中，右键单击`Package.appxmanifest`，然后选择**使用打开 > XML （文本） 编辑器**。
4. 找到`<Dependencies>`元素文件中的。 添加以下项目：

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

替换`[MainPackageDependency]`从步骤 2 您**包系列名称**。 这将指定您的**可选包**不依赖于在您的**主包**。

从步骤 1 到 4 设置对包依赖项后，您可以继续开发像平常一样。 如果您希望将代码从可选包加载到主包，您需要生成一组相关。 请参阅[相关设置](#related_sets)部分中的详细信息。

Visual Studio 可以配置重新部署您的主包部署可选软件包每次。 若要在 Visual Studio 中设置生成依赖项，您应该：

- 右键单击可选包项目并选择**构建依赖项 > 项目依赖项...**
- 检查主包项目并选择"OK"。 

现在，每次输入 F5，或生成可选软件包项目，Visual Studio 将首先生成主包项目。 这将确保您的主项目和可选项目同步。

## 相关的设置<a name="related_sets"></a>

如果您希望将代码从可选包加载到主包，您需要生成一组相关。 若要生成一组相关，必须紧密耦合您的主包和可选的程序包。 相关的设置的元数据中指定`.appxbundle`的主程序包的文件。 Visual Studio 将帮助您在文件中获取正确的元数据。 若要配置相关设置的应用程序的解决方案，请使用以下步骤：

1. 右键单击主包项目中，选择**添加 > 新建项目...**
2. 从窗口中，搜索为".txt"已安装的模板，然后添加一个新文本文件。
> [!IMPORTANT]
> 必须命名为新的文本文件： `Bundle.Mapping.txt`。
3. 在`Bundle.Mapping.txt`，您将指定的任何可选软件包项目或外部包的相对路径的文件。 示例`Bundle.Mapping.txt`文件应如下所示：

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

当您的解决方案配置这种方式时，Visual Studio 将创建一个绑定清单主包所有相关的设置的必需元数据。 

请注意，如可选包、`Bundle.Mapping.txt`文件相关的设置仅适用于 Windows 10，版本 1703年。 此外，您的应用程序的目标平台 Min 版本应设置为 10.0.15063.0。

## 已知问题<a name="known_issues"></a>

调试相关的一组可选项目目前不支持在 Visual Studio 中。 若要解决此问题，可以部署和启动激活 （Ctrl + F5），然后手动将调试器附加到进程。 将调试器附加，转 Visual Studio 中的"Debug"菜单、 选择"附加到进程..."，并将调试器附加到**主应用程序的过程**。