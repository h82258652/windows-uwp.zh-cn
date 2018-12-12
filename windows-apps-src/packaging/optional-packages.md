---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: 可选包和相关集的创作
description: 可选包中包含可与主要包相集成的内容。 这些内容可用于可下载内容 (DLC)，因为大小限制而划分大型应用，或者用于随附从原始应用中单独分隔出来的任何其他内容。
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10，uwp，可选包，相关的集，包扩展，visual studio
ms.localizationpriority: medium
ms.openlocfilehash: f62d6c99acc75033403fac7a498308cea6f7d3f8
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2018
ms.locfileid: "8945587"
---
# <a name="optional-packages-and-related-set-authoring"></a>可选包和相关集的创作
可选包中包含可与主要包相集成的内容。 这些是可用于可下载内容 (DLC) 划分大型应用大小限制，或者用于随附的任何其他内容从原始应用单独。

相关的集是可选包扩展-它们允许你将在主要和可选包之间实施严格的一组版本。 它们还可以从可选包加载本机代码 （c + +）。 

## <a name="prerequisites"></a>先决条件

- Visual Studio 2017，版本 15.1
- Windows 10 版本 1703
- Windows 10 版本 1703 SDK

若要获取所有最新的开发工具，请参阅[下载和适用于 Windows 10 的工具](https://developer.microsoft.com/windows/downloads)。

> [!NOTE]
> 若要提交到 Microsoft Store 中使用可选包和/或相关的集的应用，你将需要权限。 如果它们不提交到应用商店，可选包和相关的集可以使用适用于合作伙伴中心未经许可的业务线 (LOB) 或企业应用。 请参阅 [Windows 开发人员支持](https://developer.microsoft.com/windows/support)，获取提交使用可选包和相关集的应用的权限。

### <a name="code-sample"></a>代码示例
虽然你正在阅读本文，但建议你遵循[可选包的代码示例](https://github.com/AppInstaller/OptionalPackageSample)在 GitHub 上以便动手了解如何可选包和相关集 Visual Studio 中的工作。

## <a name="optional-packages"></a>可选包
若要在 Visual Studio 中创建可选包，你将需要：
1. 请确保你的应用的**目标平台最小版本**设置为： 10.0.15063.0 或更高版本。
2. 从**主要包**项目中，打开`Package.appxmanifest`文件。 导航到"打包"选项卡并记下你的**程序包系列名称**，这是"_"字符之前的所有内容。
3. 从**可选包**项目中，右键单击`Package.appxmanifest`，然后选择**打开与 > XML （文本） 编辑器**。
4. 找到`<Dependencies>`文件中的元素。 添加以下代码：

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

替换`[MainPackageDependency]`你从第 2 步的**程序包系列名称**。 这将指定**可选包**已取决于你的**主要包**。

一旦你从步骤 1 到 4 设置的程序包依赖项，你可以继续开发像往常一样。 如果你想要将代码从可选包加载到主程序包，你将需要生成的相关的集。 查看[相关设置](#related_sets)部分获取更多详细信息。

可以配置 visual Studio 为可选包部署在你每次重新部署主程序包。 若要在 Visual Studio 中设置生成依赖项，你应该：

- 右键单击可选包项目，然后选择**生成依赖项 > 项目依赖项...**
- 检查主包项目，然后选择"确定"。 

现在，每当输入 F5 或生成可选包项目时，Visual Studio 将首先生成主程序包项目。 这将确保你的主项目和可选项目均保持同步。

## 相关的集<a name="related_sets"></a>

如果你想要将代码从可选包加载到主程序包，你将需要生成的相关的集。 若要生成的相关的集，必须紧密耦合你的主要包和可选包。 .Appxbundle 或.msixbundle 主程序包的文件中指定相关集的元数据。 Visual Studio 可帮助你在你的文件中获取正确的元数据。 若要配置你的应用的解决方案的相关集，使用以下步骤：

1. 右键单击主包项目中，选择**添加 > 新建项目...**
2. 从窗口中，搜索".txt"已安装的模板，并添加新的文本文件。
> [!IMPORTANT]
> 新的文本文件必须命名为： `Bundle.Mapping.txt`。

3. 在`Bundle.Mapping.txt`文件将指定的任何可选包项目或外部包相对路径。 示例`Bundle.Mapping.txt`文件应如下所示：

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

你的解决方案配置时这种方式，Visual Studio 将创建主程序包捆绑包清单所有相关集所需的元数据。 

请注意，如可选包，`Bundle.Mapping.txt`相关集的文件仅适用于 Windows 10 版本 1703年或更高版本。 此外，你的应用的目标平台最小版本应设置为 10.0.15063.0 或更高版本。

## 已知问题<a name="known_issues"></a>

调试相关的集的可选项目是当前不支持在 Visual Studio 中。 若要解决此问题，你可以部署和启动激活 （Ctrl + F5） 和手动附加到进程的调试程序。 附加调试程序，转到 Visual Studio 中的"调试"菜单、 选择"附加到进程..."，并将调试程序附加到**主应用进程**。