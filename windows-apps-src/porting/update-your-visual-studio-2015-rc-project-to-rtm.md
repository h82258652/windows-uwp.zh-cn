---
author: mcleblanc
description: "如果你具有使用 Visual Studio 2015 RC 创建的 Windows 10 项目，则你在将项目文件更新为适合 Visual Studio 2015 RTM 的格式时拥有两个选项。"
title: "将 UWP Microsoft Visual Studio 2015 RC 项目更新为 RTM"
ms.assetid: 104E36CE-36DE-4E9C-A944-711C200B44EF
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: ad7754a55a34ba3921d9455209cc76ab8ae0757b
ms.lasthandoff: 02/07/2017

---

# <a name="update-your-uwp-microsoft-visual-studio-2015-rc-project-to-rtm"></a>将 UWP Microsoft Visual Studio 2015 RC 项目更新为 RTM

\[ 已针对 Windows 10 上的 UWP 应用更新。 有关 Windows 8.x 的文章，请参阅[存档](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

如果你具有使用 Visual Studio 2015 RC 创建的 Windows 10 项目，则你在将项目文件更新为适合 Visual Studio 2015 RTM 的格式时拥有两个选项。 推荐的方法是在 Visual Studio 2015 RTM 中创建新的 Windows 10 项目，并将文件复制到其中。 或者，可按照高级文档内容，编辑现有项目文件并将它们移动至新的格式。

## <a name="what-you-see-when-you-open-a-windows-10visual-studio-2015-rc-project-in-visual-studio-2015-rtm"></a>在 Visual Studio 2015 RTM 中打开 Windows 10 Visual Studio 2015 RC 项目时看到的内容

在 Visual Studio 2015 RTM 中打开 Windows 10 Visual Studio 2015 RC 项目时，将在**“解决方案资源管理器”**中看到“需要更新”消息。

![需要更新](images/vsrc-to-rtm/solution-explorer.png)

如果在**“解决方案资源管理器”**中访问项目的上下文菜单，并选择**“重新加载项目”**，你将看到此对话框。

![需要 Visual Studio 更新](images/vsrc-to-rtm/reload-project.png)

## <a name="create-a-new-project-and-copy-files-into-it"></a>创建新项目，并将文件复制到其中

1.  启动 Visual Studio 2015 RTM 并创建新的空白应用程序（Windows 通用）项目。 请记住，在默认情况下，新项目将生成面向通用设备系列的应用包（appx 文件）。 如果面向一个或多个特定设备系列，请更改该默认设置。
2.  在 Visual Studio 2015 RC 项目中，标识要复制的所有源代码文件和视觉资源文件。 通过使用文件资源管理器，将数据模型、视图模型、视觉资源、资源词典，文件夹结构和所需的任何其他内容（包括 AssemblyInfo.cs）复制到新项目中。 根据需要在磁盘上复制或创建子文件夹。
3.  还可以将视图（例如，MainPage.xaml 和 MainPage.xaml.cs）复制到新项目中。 同样，也可根据需要创建新的子文件夹，并从项目中删除现有视图。 但在覆盖或删除 Visual Studio 生成的视图之前，请保留一份副本，因为在以后引用它时，这可能会很有用。
4.  在**“解决方案资源管理器”**中，请确保将**“显示所有文件”**切换为打开。 选择要复制的文件，右键单击这些文件，然后单击“包括在项目中”****。 这将自动包括其所包含的文件夹。 然后，可根据需要将**“显示所有文件”**切换为关闭。 备用工作流（如果选择）旨在使用**“添加现有项”**命令，以便在 Visual Studio**“解决方案资源管理器”**中创建任何必要子文件夹。 仔细检查可见资源是否已将**“生成操作”**设置为**“内容”**，并将**“复制到输出目录”**设置为**“不复制”**。
5.  在 RC 项目中向引用的任何扩展 SDK 添加引用，并从之前的 Package.appxmanifest（如声明的任何功能）将任意更改复制到新 RTM 项目的 Package.appxmanifest 中。

## <a name="advanced-edit-your-existing-project-files"></a>高级：编辑现有项目文件

Visual Studio 2015 RC 和 Visual Studio 2015 RTM 之间的 Windows 10 项目格式的明显区别是，RTM 格式使用 [NuGet](http://docs.nuget.org/) 版本 3。 如果计划手动更新项目，请记住这一差异。

如果不希望手动更新项目，或者对了解 Visual Studio 2015 RC 和 Visual Studio 2015 RTM 之间的项目格式差异感兴趣，请参阅[将应用迁移到通用 Windows 平台 (UWP)](http://msdn.microsoft.com/library/mt148501.aspx)。


