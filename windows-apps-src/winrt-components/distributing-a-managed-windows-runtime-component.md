---
title: 分发托管的 Windows 运行时组件
description: 可以按文件副本分发 Windows 运行时组件。
ms.assetid: 80262992-89FC-42FC-8298-5AABF58F8212
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1d306c02d4fd99acaa49ec59230181ac0a20c9f3
ms.sourcegitcommit: f561efbda5c1d47b85601d91d70d86c5332bbf8c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690387"
---
# <a name="distributing-a-managed-windows-runtime-component"></a>分发托管的 Windows 运行时组件

可以按文件副本分发 Windows 运行时组件。 但是，如果你的组件包含多个文件，则对于你的用户来说，安装可能会很繁琐。 此外，将文件放入文件或设置引用失败可能会导致这些错误。 可以将复杂组件打包为 Visual Studio 扩展 SDK，以便于安装和使用。 用户只需为整个包设置一个引用。 使用 "**扩展和更新**" 对话框，可以轻松找到并安装组件，如[查找和使用 Visual Studio 扩展](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)中所述。

## <a name="planning-a-distributable-windows-runtime-component"></a>规划可分发的 Windows 运行时组件

为二进制文件（如 winmd 文件）选择唯一名称。 建议采用以下格式来确保唯一性：

``` syntax
company.product.purpose.extension
For example: Microsoft.Cpp.Build.dll
```

你的二进制文件将安装在应用包中，可能包含来自其他开发人员的二进制文件。 请参阅[如何：创建软件开发工具包](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)中的 "扩展 sdk"。

若要确定如何分发组件，请考虑它的复杂程度。 建议在以下情况时使用扩展 SDK 或类似的包管理器：

-   组件包含多个文件。
-   为多个平台（例如 x86 和 ARM）提供组件版本。
-   同时提供组件的调试版本和发布版本。
-   组件具有仅在设计时使用的文件和程序集。

如果有多个条件为 true，则扩展 SDK 特别有用。

> **注意**  对于复杂的组件，NuGet 程序包管理系统提供扩展 sdk 的开放源替代项。 与扩展 Sdk 一样，NuGet 使你能够创建可简化复杂组件的安装的包。 有关 NuGet 包与 Visual Studio 扩展 Sdk 的比较，请参阅[使用 NuGet 和扩展 SDK 添加引用](https://docs.microsoft.com/visualstudio/ide/adding-references-using-nuget-versus-an-extension-sdk?view=vs-2015)。

## <a name="distribution-by-file-copy"></a>按文件副本分发

如果组件包含单个 winmd 文件或一个 winmd 文件和一个资源索引（pri）文件，则您只需使该 winmd 文件可供用户进行复制。 用户可以将文件放在项目中所需的任何位置，使用 "**添加现有项**" 对话框将 winmd 文件添加到项目中，然后使用 "引用管理器" 对话框创建一个引用。 如果包含 pri 文件或 .xml 文件，请指示用户将这些文件放在 winmd 文件中。

> **请注意**，当你生成 Windows 运行时组件时，即使你的项目不包含任何资源，Visual Studio  始终会生成一个 pri 文件。 如果你的组件具有测试应用，则可以通过检查 bin\\调试\\AppX 文件夹中的应用包的内容来确定是否使用该 pri 文件。 如果组件中的 pri 文件未显示在此处，则无需分发。 或者，你可以使用[makepri.exe](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))工具从 Windows 运行时组件项目转储资源文件。 例如，在 "Visual Studio 命令提示符" 窗口中，键入： makepri.exe dump/if MyComponent/of MyComponent，你可以在[资源管理系统（Windows）](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))中详细了解 pri 文件。

## <a name="distribution-by-extension-sdk"></a>通过扩展 SDK 分发

复杂组件通常包括 Windows 资源，但请参阅有关检测上一部分中的空 pri 文件的说明。

**创建扩展 SDK**

1.  请确保已安装 Visual Studio SDK。 你可以从[Visual Studio 下载](https://visualstudio.microsoft.com/downloads/download-visual-studio-vs)页下载 VISUAL studio SDK。
2.  使用 VSIX 项目模板创建一个新项目。 可以在 "扩展性" 类别中C#的 "视觉对象" 或 "Visual Basic" 下找到该模板。 此模板作为 Visual Studio SDK 的一部分安装。 （[演练：使用C#或 Visual Basic 创建 sdk](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-csharp-or-visual-basic?view=vs-2015)或[演练：使用C++创建 sdk ](https://docs.microsoft.com/visualstudio/extensibility/walkthrough-creating-an-sdk-using-cpp?view=vs-2015)，演示如何在非常简单的方案中使用此模板。 ）
3.  确定 SDK 的文件夹结构。 文件夹结构从 VSIX 项目的根级别开始，其中包含**引用**、再**发行**和**DesignTime**文件夹。

    -   **引用**是用户可对其进行编程的二进制文件的位置。 扩展 SDK 在用户的 Visual Studio 项目中创建对这些文件的引用。
    -   "再**发行**" 是指在使用组件创建的应用程序中，必须与二进制文件一起分发的其他文件的位置。
    -   **DesignTime**是仅当开发人员创建使用您的组件的应用程序时使用的文件的位置。

    在其中每个文件夹中，可以创建配置文件夹。 允许的名称为 "调试"、"零售" 和 "CommonConfiguration"。 CommonConfiguration 文件夹适用于无论零售版本或调试版本使用相同的文件。 如果只分发组件的零售版本，可以将所有内容放在 CommonConfiguration 中，并忽略其他两个文件夹。

    在每个配置文件夹中，可以为特定于平台的文件提供体系结构文件夹。 如果对所有平台使用相同的文件，则可以提供名为中性的单个文件夹。 可以在[如何：创建软件开发工具包](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)中查找文件夹结构的详细信息，包括其他体系结构文件夹名称。 （本文讨论了平台 Sdk 和扩展 Sdk。 你可能会发现，折叠有关平台 Sdk 的部分有助于避免混淆。 ）

4.  创建 SDK 清单文件。 该清单指定名称和版本信息、SDK 支持的体系结构、.NET 版本以及有关 Visual Studio 使用 SDK 的方式的其他信息。 可以在[如何：创建软件开发工具包](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)中找到详细信息和示例。
5.  生成和分发扩展 SDK。 有关详细信息，包括对 VSIX 包进行本地化和签名，请参阅[Vsix 部署](https://docs.microsoft.com/visualstudio/misc/how-to-manually-package-an-extension-vsix-deployment?view=vs-2015)。

## <a name="related-topics"></a>相关主题

* [创建软件开发工具包](https://docs.microsoft.com/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015)
* [NuGet 程序包管理系统](https://github.com/NuGet/Home)
* [资源管理系统（Windows）](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10))
* [查找和使用 Visual Studio 扩展](https://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2015)
* [Makepri.exe 命令选项](https://docs.microsoft.com/previous-versions/windows/apps/jj552945(v=win.10))
